# API 文件

您可以在您的 Argo CD UI 中將路徑設定為 `/swagger-ui` 來找到 Swagger 文件。例如 [http://localhost:8080/swagger-ui](http://localhost:8080/swagger-ui)。

## 授權

您需要使用 bearer token 來授權您的 API。要取得一個 token：

```bash
$ curl -H "Content-Type: application/json" $ARGOCD_SERVER/api/v1/session -d $'{"username":"admin","password":"password"}'
{"token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpYXQiOjE1Njc4MTIzODcsImlzcyI6ImFyZ29jZCIsIm5iZiI6MTU2NzgxMjM4Nywic3ViIjoiYWRtaW4ifQ.ejyTgFxLhuY9mOBtKhcnvobg3QZXJ4_RusN_KIdVwao"} 
```

然後透過 HTTP `Authorization` 標頭傳遞，並在前面加上 `Bearer `：

```bash
$ curl $ARGOCD_SERVER/api/v1/applications -H "Authorization: Bearer $ARGOCD_TOKEN" 
{"metadata":{"selfLink":"/apis/argoproj.io/v1alpha1/namespaces/argocd/applications","resourceVersion":"37755"},"items":...}
```

## 服務

### 應用程式 API

#### 如何避免因應用程式遺失而產生 403 錯誤

應用程式 API 的所有端點都接受一個可選的 `project` 查詢字串參數。如果指定了該參數，
且指定的應用程式不存在，API 將回傳 `404` 錯誤。

此外，如果指定了 `project` 查詢字串參數，且應用程式存在但不在
給定的 `project` 中，API 將回傳 `403` 錯誤。這是為了防止向沒有
存取權限的使用者洩漏有關應用程式存在的資訊。