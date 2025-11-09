---
id: use-llms-and-mcp
title: 在 Grafana 應用程式插件中使用 LLM 和 Grafana MCP
description: 學習如何將大型語言模型 (LLM) 和 Grafana 模型情境協定伺服器整合至您的 Grafana 應用程式插件中，以實現 AI 驅動的功能。
keywords:
  - grafana
  - plugin development
  - app plugin
  - llm
  - large language model
  - mcp
  - model context protocol
  - ai
  - artificial intelligence
  - agent
---

# 在 Grafana 應用程式插件中使用 LLM 和 Grafana MCP

本指南將說明如何將大型語言模型 (LLM) 和 Grafana [模型情境協定][mcp] (MCP) 伺服器整合至您的 Grafana 應用程式插件中，以新增 AI 驅動的功能。

## 先決條件

在開始之前，請確保您具備：

- 已安裝並啟用 [Grafana LLM 應用程式] 插件（MCP 功能需要 0.22 或更新版本）
- 已設定 [Grafana 應用程式插件開發環境][app-plugin-dev]
- TypeScript 和 React 的基本知識
- 在您的 Grafana 執行個體中設定了 LLM 供應商
- 已安裝 Node.js 和 npm 以進行套件管理

## 關於 Grafana 插件中的 LLM 和 MCP

Grafana 管理員可以安裝 [Grafana LLM 應用程式] 插件，以集中管理 Grafana 中的 LLM 存取。作為插件作者，您可以使用 [`@grafana/llm`][npm] npm 套件，透過 Grafana 的插件後端向已設定的 LLM 發出安全請求。

Grafana LLM 應用程式還提供對 Grafana MCP 伺服器的存取。模型情境協定 (MCP) 提供了可用於 LLM 請求的實用工具，讓它們能夠在類似代理的對話中執行動作和收集資訊。

## 在您的插件中使用 LLM

若要將 LLM 功能整合至您的 Grafana 應用程式插件：

### 1. 安裝必要的套件

```bash
npm install @grafana/llm
```

### 2. 匯入 LLM 模組

```typescript
import { llm } from '@grafana/llm';
```

### 3. 檢查 LLM 可用性並發出請求

以下範例展示了一個完整的函式，它會安全地向 LLM 請求建議。此模式可確保您的插件在 LLM 服務不可用時能優雅地處理。

```typescript
import { llm } from '@grafana/llm';

async function getLLMResponse(): Promise<string> {
  try {
    // 在發出請求前，請務必驗證服務可用性
    // 這可以防止在未設定 LLM 的環境中發生執行階段錯誤
    const enabled = await llm.enabled();
    if (!enabled) {
      throw new Error('LLM 服務未設定或未啟用');
    }

    // 使用 OpenAI 聊天完成格式來建構訊息
    // 系統訊息定義了 LLM 的角色和專業知識
    // 使用者訊息包含實際的查詢
    const messages: llm.Message[] = [
      {
        role: 'system',
        content: '您是一位經驗豐富、稱職的 SRE，對 PromQL、LogQL 和 Grafana 有深入的了解。'
      },
      {
        role: 'user',
        content: '我應該使用哪個指標來監控容器的 CPU 使用率？'
      },
    ];

    // 使用基本模型傳送請求（最具成本效益的選項）
    const response = await llm.chatCompletions({
      model: llm.Model.BASE,
      messages,
    });

    // 擷取並傳回 LLM 的回應文字
    // 請務必檢查 choices 是否存在且包含內容
    return response.choices[0]?.message?.content || '未收到回應';
  } catch (error) {
    console.error('取得 LLM 回應失敗：', error);
    throw new Error(`LLM 請求失敗：${error.message}`);
  }
}
```

您的插件現在已具備一個可運作的 LLM 整合，可以優雅地處理成功的回應和錯誤情況。

### 4. 使用串流回應

對於較長的 LLM 輸出，串流回應可提供更好的使用者體驗。以下範例顯示如何設定串流，並為 React 元件提供適當的 Observable 處理：

```typescript
import { llm } from '@grafana/llm';
import { Observable } from 'rxjs';

async function getStreamingLLMResponse(): Promise<Observable<string>> {
  try {
    // 首先驗證服務可用性
    const enabled = await llm.enabled();
    if (!enabled) {
      throw new Error('LLM 服務未設定或未啟用');
    }

    // 使用與非串流請求相同的訊息格式
    const messages: llm.Message[] = [
      {
        role: 'system',
        content: '您是一位經驗豐富、稱職的 SRE，對 PromQL、LogQL 和 Grafana 有深入的了解。'
      },
      {
        role: 'user',
        content: '我應該使用哪個指標來監控容器的 CPU 使用率？'
      },
    ];

    // 建立串流連線 - 傳回回應區塊的 Observable
    const stream = llm.streamChatCompletions({
      model: llm.Model.BASE,
      messages,
    });

    // accumulateContent 輔助函式會從區塊中建構完整的文字
    // 這非常適合在您的 UI 中顯示漸進式文字更新
    const accumulatedStream = stream.pipe(llm.accumulateContent());

    // 如何在 React 元件中使用串流的範例
    accumulatedStream.subscribe({
      next: (content) => {
        // 使用累積的內容更新您的元件狀態
        console.log('串流內容：', content);
      },
      error: (error) => {
        console.error('串流錯誤：', error);
        // 在您的 UI 中處理錯誤（顯示錯誤訊息、重試按鈕等）
      },
      complete: () => {
        console.log('串流完成');
        // 在您的 UI 中將回應標示為完成
      }
    });

    return accumulatedStream;
  } catch (error) {
    console.error('啟動 LLM 串流失敗：', error);
    throw new Error(`LLM 串流失敗：${error.message}`);
  }
}
```

您的插件現在支援即時 AI 回應。使用者將會看到文字逐漸出現，為較長的解釋或複雜的分析創造更具回應性的體驗。

## 將 MCP 工具與 LLM 搭配使用

模型情境協定 (MCP) 允許 LLM 使用工具來執行動作和收集資訊。這需要 Grafana LLM 應用程式插件 0.22 或更新版本。

### 1. 設定 MCP 用戶端

此範例會建立一個可重複使用的 MCP 用戶端，您的插件可以在其整個生命週期中使用。用戶端會管理與 Grafana 的 MCP 伺服器的連線，並提供對可觀察性工具的存取：

```typescript
import { llm, mcp } from '@grafana/llm';

async function setupMCPClient(): Promise<InstanceType<typeof mcp.Client>> {
  try {
    // 驗證兩個服務都可用 - MCP 需要基礎的 LLM 服務
    const enabled = await llm.enabled();
    if (!enabled) {
      throw new Error('LLM 服務未設定或未啟用');
    }

    const mcpEnabled = await mcp.enabled();
    if (!mcpEnabled) {
      throw new Error('MCP 服務未啟用或未設定');
    }

    // 使用您的插件身分建立用戶端
    // 使用您實際的插件名稱和版本以利於偵錯
    const mcpClient = new mcp.Client({
      name: 'my-monitoring-plugin', // 以您的插件名稱取代
      version: '1.0.0',              // 以您的插件版本取代
    });

    // 建立與 Grafana 的 MCP 伺服器的 HTTP 連線
    // streamableHTTPURL() 會自動提供正確的端點
    const transport = new mcp.StreamableHTTPClientTransport(mcp.streamableHTTPURL());
    await mcpClient.connect(transport);

    // 透過列出可用的工具來驗證連線
    const toolsResponse = await mcpClient.listTools();
    console.log(`已連線至 MCP 伺服器，共有 ${toolsResponse.tools.length} 個可用工具`);

    return mcpClient;
  } catch (error) {
    console.error('設定 MCP 用戶端失敗：', error);
    throw new Error(`MCP 設定失敗：${error.message}`);
  }
}
```

### 2. 將 MCP 工具與 LLM 請求搭配使用

若要將 MCP 工具與 LLM 請求結合使用，請遵循以下步驟：

此範例展示了完整的代理模式，其中 LLM 可以呼叫工具並使用其結果來提供有根據的回應。這對於可觀察性使用案例特別強大：

```typescript
async function useMCPWithLLM(): Promise<string> {
  try {
    const mcpClient = await setupMCPClient();

    // 從可能需要使用工具的對話開始
    const messages: llm.Message[] = [
      {
        role: 'system',
        content: '您是一位經驗豐富、稱職的 SRE，對 PromQL、LogQL 和 Grafana 有深入的了解。在提供建議之前，請使用可用的工具收集有關系統的即時資訊。'
      },
      {
        role: 'user',
        content: '我的系統中目前有哪些警示正在觸發？'
      },
    ];

    // 擷取並轉換可用的工具以供 LLM 使用
    const toolsResponse = await mcpClient.listTools();
    const tools = mcp.convertToolsToOpenAI(toolsResponse.tools);

    console.log(`可用工具：${tools.map(t => t.function.name).join(', ')}`);

    // 傳送帶有可用工具的初始請求
    let response = await llm.chatCompletions({
      model: llm.Model.BASE,
      messages,
      tools,
    });

    // 處理 LLM 想要進行的任何工具呼叫
    while (response.choices[0].message.tool_calls) {
      // 將 LLM 的回應（帶有工具呼叫）新增至對話
      messages.push(response.choices[0].message);

      // 執行 LLM 請求的每個工具呼叫
      for (const toolCall of response.choices[0].message.tool_calls) {
        try {
          console.log(`正在執行工具：${toolCall.function.name}`);

          const result = await mcpClient.callTool({
            name: toolCall.function.name,
            arguments: JSON.parse(toolCall.function.arguments),
          });

          // 將工具的結果新增回對話
          messages.push({
            role: 'tool',
            content: JSON.stringify(result.content),
            tool_call_id: toolCall.id,
          });
        } catch (toolError) {
          console.error(`工具呼叫失敗：${toolError.message}`);
          // 包含錯誤資訊，以便 LLM 可以適當地處理
          messages.push({
            role: 'tool',
            content: `執行 ${toolCall.function.name} 時發生錯誤：${toolError.message}`,
            tool_call_id: toolCall.id,
          });
        }
      }

      // 取得 LLM 結合工具呼叫結果的回應
      response = await llm.chatCompletions({
        model: llm.Model.BASE,
        messages,
        tools,
      });
    }

    return response.choices[0].message.content || '未收到回應';
  } catch (error) {
    console.error('使用 MCP 搭配 LLM 失敗：', error);
    throw new Error(`MCP + LLM 請求失敗：${error.message}`);
  }
}
```

您現在已經建立了一個完整的 AI 代理，可以主動與您的 Grafana 環境互動。您的 LLM 可以查詢真實資料、檢查系統狀態，並根據即時資訊提供情境式建議。

## 在 React 元件中使用 MCP 用戶端

您可以使用 `mcp.MCPClientProvider` 元件和 `useMCPClient` hook 從 React 元件存取 MCP 用戶端。此方法會處理檢查可用性和初始化用戶端的樣板程式碼。

### 1. 設定 MCP 提供者

MCP 用戶端初始化是異步的，如果服務不可用，可能會失敗。使用 `Suspense` 來處理用戶端連線時的載入狀態，並使用 `ErrorBoundary` 來優雅地處理任何連線失敗或設定問題。

若要在您的 React 應用程式中設定 MCP 提供者，請遵循以下步驟：

```tsx
import React, { Suspense } from 'react';
import { mcp } from '@grafana/llm';
import { ErrorBoundary, Spinner } from '@grafana/ui';

function App() {
  return (
    <Suspense fallback={<Spinner />}>
      <ErrorBoundary>
        {({ error }) => {
          if (error) {
            return <div>MCP 發生錯誤：{error.message}</div>;
          }
          return (
            <mcp.MCPClientProvider
              appName="my-app"
              appVersion="1.0.0"
            >
              <MyComponent />
            </mcp.MCPClientProvider>
          );
        }}
      </ErrorBoundary>
    </Suspense>
  );
}
```

### 2. 在元件中使用 MCP 用戶端

若要在您的 React 元件中使用 MCP 用戶端，請遵循以下步驟：

此元件展示了如何在 React 中安全地存取 MCP 功能。`useMCPClient` hook 會自動處理用戶端初始化並提供適當的錯誤邊界：

```tsx
import React from 'react';
import { mcp } from '@grafana/llm';
import { Alert, LoadingPlaceholder } from '@grafana/ui';
import { useAsync } from 'react-use';

function MyComponent() {
  // useMCPClient hook 提供了一個立即可用的 MCP 用戶端
  // 它會自動處理所有初始化和錯誤狀態
  const { client, enabled } = mcp.useMCPClient();

  // 使用適當的相依性追蹤異步擷取可用的工具
  const { loading, error, value: toolsResponse } = useAsync(async () => {
    if (!enabled || !client) {
      return null;
    }
    return await client.listTools();
  }, [client]);

  // 正在擷取工具時顯示載入狀態
  if (loading) {
    return <LoadingPlaceholder label="正在載入 MCP 工具..." />;
  }

  // 顯示帶有可操作資訊的錯誤狀態
  if (error) {
    return (
      <Alert title="載入 MCP 工具失敗" severity="error">
        {error.message}
        <br />
        確保 Grafana LLM 應用程式已正確設定並啟用 MCP。
      </Alert>
    );
  }

  const tools = toolsResponse?.tools ?? [];

  return (
    <div>
      <h3>可用的 MCP 工具</h3>
      {tools.length === 0 ? (
        <Alert title="沒有可用的工具" severity="info">
          目前沒有可用的 MCP 工具。請檢查您的 Grafana LLM 應用程式設定。
        </Alert>
      ) : (
        <div>
          <p>找到 {tools.length} 個可用的工具：</p>
          <ul>
            {tools.map((tool, index) => (
              <li key={tool.name || index}>
                <strong>{tool.name}</strong>
                {tool.description && (
                  <>: {tool.description}</>
                )}
              </li>
            ))}
          </ul>
        </div>
      )}
    </div>
  );
}
```

您已成功將 LLM 和 MCP 功能與適當的錯誤處理和載入狀態整合至 React 元件中。

## 疑難排解

以下偵錯策略可協助您識別和解決 LLM 和 MCP 整合的常見問題。

### LLM 服務不可用

**問題**：`llm.enabled()` 傳回 `false` 或擲回錯誤。

**偵錯步驟**：
1. **檢查插件安裝**：導覽至 **管理** > **插件** 並驗證 Grafana LLM 應用程式已安裝並啟用
2. **驗證 LLM 設定**：在 LLM 應用程式設定中，確保至少設定了一個具有有效憑證的 LLM 供應商
3. **測試連線**：使用 LLM 應用程式的內建連線測試來驗證您的供應商設定

**程式碼偵錯**：
```typescript
// 新增詳細記錄以了解失敗原因
try {
  console.log('正在檢查 LLM 可用性...');
  const enabled = await llm.enabled();
  console.log('LLM 啟用狀態：', enabled);

  if (!enabled) {
    // 記錄詳細的錯誤資訊
    console.error('LLM 服務不可用 - 請檢查插件設定');
    return;
  }
} catch (error) {
  console.error('LLM 可用性檢查失敗：', error);
  // 檢查是否為網路錯誤、權限問題等
}
```

**常見解決方案**：
- 安裝 LLM 插件後重新啟動 Grafana 伺服器
- 檢查瀏覽器網路標籤中是否有失敗的 `/api/plugins/grafana-llm-app/` API 請求
- 驗證您的插件在其 `plugin.json` 中是否具有必要的功能

### MCP 連線失敗

**問題**：MCP 用戶端連線失敗或 `mcp.enabled()` 傳回 `false`。

**偵錯步驟**：
1. **版本檢查**：確保您使用的是 Grafana LLM 應用程式 0.22 或更新版本
2. **網路偵錯**：開啟瀏覽器開發人員工具並檢查是否有失敗的 WebSocket 或 HTTP 連線
3. **服務狀態**：透過檢查 LLM 應用程式狀態頁面來驗證 MCP 伺服器是否正在執行

**程式碼偵錯**：
```typescript
// 新增連線偵錯
async function debugMCPConnection() {
  try {
    console.log('正在檢查 MCP 可用性...');
    const mcpEnabled = await mcp.enabled();
    console.log('MCP 已啟用：', mcpEnabled);

    if (!mcpEnabled) {
      console.error('MCP 不可用 - 請檢查 LLM 應用程式版本 >= 0.22');
      return;
    }

    const client = new mcp.Client({
      name: 'debug-client',
      version: '1.0.0',
    });

    const transport = new mcp.StreamableHTTPTransport(mcp.streamableHTTPUrl());
    console.log('MCP URL：', mcp.streamableHTTPUrl());

    await client.connect(transport);
    console.log('MCP 連線成功');

    // 測試基本功能
    const capabilities = await client.getServerCapabilities();
    console.log('伺服器功能：', capabilities);

  } catch (error) {
    console.error('MCP 連線失敗：', {
      error: error.message,
      stack: error.stack,
      url: mcp.streamableHTTPUrl()
    });
  }
}
```

**常見解決方案**：
- 將 Grafana LLM 應用程式更新至最新版本
- 檢查代理或防火牆設定是否封鎖 WebSocket 連線
- 驗證您的瀏覽器是否可以存取 MCP 伺服器 URL

### 工具呼叫失敗

**問題**：LLM 嘗試呼叫工具但呼叫失敗。

**偵錯步驟**：
1. **驗證工具可用性**：在進行呼叫前列出可用的工具
2. **檢查引數格式**：確保工具引數符合預期的結構描述
3. **監控工具執行**：在工具呼叫周圍新增記錄以識別失敗點

**程式碼偵錯**：
```typescript
// 新增全面的工具呼叫偵錯
async function debugToolCalls(mcpClient: mcp.Client) {
  try {
    // 首先，列出可用的工具
    const toolsResponse = await mcpClient.listTools();
    console.log('可用工具：', toolsResponse.tools.map(t => ({
      name: t.name,
      description: t.description,
      inputSchema: t.inputSchema
    })));

    // 測試特定的工具呼叫
    const toolName = 'your-tool-name';
    const args = { /* 您的引數 */ };

    console.log(`正在呼叫工具：${toolName}`, args);
    const result = await mcpClient.callTool({
      name: toolName,
      arguments: args
    });

    console.log('工具呼叫結果：', result);

  } catch (error) {
    console.error('工具呼叫偵錯失敗：', {
      error: error.message,
      toolName: toolName,
      arguments: args,
      stack: error.stack
    });
  }
}
```

**常見解決方案**：
- 在呼叫前根據工具的輸入結構描述驗證工具引數
- 使用適當的重試邏輯處理工具呼叫逾時
- 檢查 Grafana 記錄以取得詳細的 MCP 伺服器錯誤訊息

### React 元件錯誤

**問題**：使用 MCP hook 的 React 元件擲回錯誤。

**偵錯步驟**：
1. **檢查提供者階層**：確保 `MCPClientProvider` 包裹了所有使用 MCP hook 的元件
2. **驗證 hook 用法**：確認您是在函式元件內部使用 hook
3. **新增錯誤邊界**：在您的元件樹中實作適當的錯誤處理

**程式碼偵錯**：
```typescript
// 偵錯 React 元件問題
function DebugMCPComponent() {
  const mcpClient = mcp.useMCPClient();

  // 新增記錄以了解用戶端狀態
  React.useEffect(() => {
    console.log('MCP 用戶端狀態：', {
      client: mcpClient,
      isConnected: mcpClient ? '可用' : '不可用'
    });
  }, [mcpClient]);

  if (!mcpClient) {
    console.warn('MCP 用戶端不可用 - 請檢查 MCPClientProvider 包裹器');
    return <div>MCP 用戶端不可用</div>;
  }

  // 您的元件邏輯在此處
  return <div>MCP 用戶端已就緒</div>;
}

// 適當的錯誤邊界實作
class MCPErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, errorInfo) {
    console.error('MCP 元件錯誤：', {
      error: error.message,
      stack: error.stack,
      componentStack: errorInfo.componentStack
    });
  }

  render() {
    if (this.state.hasError) {
      return (
        <Alert severity="error">
          <h4>MCP 元件錯誤</h4>
          <p>{this.state.error?.message}</p>
          <button onClick={() => this.setState({ hasError: false, error: null })}>
            再試一次
          </button>
        </Alert>
      );
    }

    return this.props.children;
  }
}
```

**常見解決方案**：
- 請務必使用 `MCPClientProvider` 和錯誤邊界來包裹 MCP 元件
- 使用條件式呈現來處理載入和錯誤狀態
- 新增適當的 TypeScript 類型以獲得更好的偵錯支援
- 獨立測試您的元件以找出特定的失敗點

## 後續步驟

在您的插件中實作 LLM 和 MCP 整合後，您已建構了可以：
- 使用 LLM 提出智慧建議
- 串流回應以獲得更好的使用者體驗
- 透過 MCP 工具執行實際動作
- 在生產環境中優雅地處理錯誤

若要進一步擴充您的整合：

- 深入了解 [Grafana LLM 應用程式文件][Grafana LLM app] 以取得進階設定選項
- 查看 [模型情境協定規範][mcp] 以掌握 MCP 概念
- 實驗不同的 LLM 供應商和模型，找出最適合的方案
- 使用穩健的載入狀態和錯誤處理來完善您的整合
- 為失敗的請求新增重試邏輯以提高可靠性
- 與 Grafana 社群聯繫以取得插件開發技巧和支援

[mcp]: https://modelcontextprotocol.io/
[Grafana LLM app]: https://grafana.com/grafana/plugins/grafana-llm-app/
[npm]: https://www.npmjs.com/package/@grafana/llm
[app-plugin-dev]: https://grafana.com/developers/plugin-tools/tutorials/build-an-app-plugin