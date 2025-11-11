# 身份驗證與授權

本文件描述了 Argo CD 中身份驗證 (authn) 和授權 (authz) 的實作方式。程式碼庫中明確區分了這兩個安全性概念的強制執行時機和方式。

## 邏輯層次

下圖呈現了 Argo CD API 伺服器內部的 4 個不同邏輯層次（由 4 個方塊表示：HTTP、gRPC、AuthN 和 AuthZ），它們協同合作以提供身份驗證和授權。

-   **HTTP**：HTTP 層次包含了處理 HTTP 請求的*邏輯元件*。每個傳入的請求都會到達同一個 HTTP 伺服器的同一個連接埠 (8080)。此伺服器將分析請求標頭，並將其分派到適當的內部伺服器：gRPC 或標準 HTTP。

-   **gRPC**：[gRPC][4] 層次包含了負責 gRPC 實作的邏輯元件。

-   **AuthN**：AuthN 代表負責身份驗證的層次。

-   **AuthZ**：AuthZ 代表負責授權的層次。

![Argo CD 架構](../../assets/argocd-arch-authn-authz.jpg)

## 邏輯元件

邏輯元件（由數字標識）可以代表程式碼庫中的一個物件、一個函式或一個元件。請注意，此特定區別並未在圖中表示。

傳入的請求可以從網頁 UI 以及 `argocd` CLI 到達 Argo CD API 伺服器。所代表元件的職責如下所述，並附有其各自的編號：

1.  **Cmux**：使用 [cmux][1] 函式庫提供連接多工處理能力，使得可以使用同一個連接埠來處理標準 HTTP 以及 gRPC 請求。它負責檢查傳入的請求，並將其分派到適當的內部伺服器。如果請求版本是 `http1.x`，它將委派給 *http mux*。如果請求版本是 `http2` 並且具有 `content-type: application/grpc` 標頭，它將委派給 *gRPC 伺服器*。

2.  **HTTP mux**：一個[標準的 HTTP 多工器][8]，將處理非 gRPC 請求。它負責向網頁 UI 提供一個統一的 [REST API][3]，公開所有 gRPC 和非 gRPC 服務。

3.  **gRPC-gateway**：使用 [grpc-gateway][2] 函式庫來轉換內部 gRPC 服務，並將其公開為 [REST API][3]。Argo CD 中的絕大多數 API 服務都是以 gRPC 實作的。grpc-gateway 使得可以從網頁 UI 存取 gRPC 服務。

4.  **伺服器**：負責處理 gRPC 請求的內部 gRPC 伺服器。

5.  **AuthN**：負責叫用身份驗證邏輯。它被註冊為一個 gRPC 攔截器，將會針對每個 gRPC 請求自動觸發。

6.  **會話管理器**：負責管理 Argo CD API 伺服器會話的物件。它提供了驗證請求中所提供之身份驗證權杖有效性的功能。根據 Argo CD 的設定方式，它可能會或可能不會委派給外部 AuthN 提供者來驗證權杖。

7.  **AuthN 提供者**：描述了可以插入 Argo CD API 伺服器以提供身份驗證功能的元件，例如登入和權杖驗證過程。

8.  **服務方法**：代表實作所請求之業務邏輯（核心功能）的方法。業務邏輯的一個例子是：`列出應用程式`。服務方法也負責叫用 [RBAC][7] 強制執行函式，以驗證經過身份驗證的使用者是否有權限執行此方法。

9.  **RBAC**：一組函式的集合，提供驗證使用者是否有權限在 Argo CD 中執行特定動作的能力。它透過將傳入的請求動作與可以在 Argo CD API 伺服器以及 Argo CD `Project` CRD 中設定的預定義 [RBAC][7] 規則進行驗證來達成此目的。

10. **Casbin**：使用 [Casbin][5] 函式庫來強制執行 [RBAC][7] 規則。

11. **AuthN 中介軟體**：一個 [HTTP 中介軟體][6]，設定為叫用邏輯以驗證非 gRPC 實作且需要身份驗證的 HTTP 服務的權杖。

12. **HTTP 處理常式**：代表負責叫用所請求之業務邏輯（核心功能）的 http 處理常式。業務邏輯的一個例子是：`列出應用程式`。Http 處理常式也負責叫用 [RBAC][7] 強制執行函式，以驗證經過身份驗證的使用者是否有權限執行此業務邏輯。

[1]: https://github.com/soheilhy/cmux
[2]: https://github.com/grpc-ecosystem/grpc-gateway
[3]: https://en.wikipedia.org/wiki/Representational_state_transfer
[4]: https://grpc.io/
[5]: https://casbin.org/
[6]: https://go.dev/wiki/LearnServerProgramming#middleware
[7]: https://en.wikipedia.org/wiki/Role-based_access_control
[8]: https://pkg.go.dev/net/http#ServeMux
