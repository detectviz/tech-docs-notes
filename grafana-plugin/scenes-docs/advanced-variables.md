---
id: advanced-variables
title: 自訂場景物件中的變數
---

[變數](./variables.md) 為互動式儀表板奠定了基礎。它們允許動態設定查詢的資料。

除了標準的變數支援外，Scenes 還提供了一個 API，讓[自訂場景物件](./advanced-custom-scene-objects.md)能與變數搭配運作。這個 API 為儀表板建立者提供了更多的可能性。

## 在自訂場景物件中使用變數

請依照以下步驟，讓自訂場景物件對變數產生反應。

### 步驟 1. 建置自訂場景物件

首先，建置一個會顯示所提供文字的自訂場景物件。

此物件將：

1.  具有一個包含字串值 (`text` 屬性) 的簡單狀態。
2.  渲染一個用於修改狀態的 `textarea`，以及一個用於顯示 `text` 狀態目前值的預格式化文字區塊。

```tsx
import { SceneObjectState, SceneObjectBase, SceneComponentProps } from '@grafana/scenes';
import { TextArea } from '@grafana/ui';

interface TextInterpolatorState extends SceneObjectState {
  text: string;
}

class TextInterpolator extends SceneObjectBase<TextInterpolatorState> {
  static Component = TextInterpolatorRenderer;

  constructor(text: string) {
    super({ text });
  }

  onTextChange = (text: string) => {
    this.setState({ text });
  };
}

function TextInterpolatorRenderer({ model }: SceneComponentProps<TextInterpolator>) {
  const { text } = model.useState();
  return (
    <div>
      <div style={{ marginBottom: 8 }}>
        <TextArea defaultValue={text} onBlur={(e) => model.onTextChange(e.currentTarget.value)} />
      </div>
      <pre>{model.state.text}</pre>
    </div>
  );
}
```

### 步驟 2. 使用 `TextInterpolator` 建置場景

使用 `TextInterpolator` 建立一個簡單的場景：

```tsx
const scene = new EmbeddedScene({
  body: new SceneFlexLayout({
    direction: 'column',
    children: [
      new SceneFlexItem({
        minHeight: 300,
        body: new TextInterpolator('Hello world'),
      }),
    ],
  }),
});
```

### 步驟 3. 將變數新增至場景

定義一個自訂變數並將其新增至場景：

```tsx
const greetingsVar = new CustomVariable({
  name: 'greetings',
  query: 'Hello , Hola , Bonjour , Ahoj',
});

const scene = new EmbeddedScene({
  $variables: new SceneVariableSet({ variables: [greetingsVar] }),
  controls: [new VariableValueSelectors({})],
  body: new SceneFlexLayout({
    direction: 'column',
    children: [
      new SceneFlexItem({
        minHeight: 300,
        body: new TextInterpolator('Hello world'),
      }),
    ],
  }),
});
```

### 步驟 4. 將變數支援新增至 `TextInterpolator` 物件

使用 `VariableDependencyConfig` 讓 `TextInterpolator` 對變數變更產生反應。在 `TextInterpolator` 中定義一個 `protected _variableDependency` 實例屬性，該屬性是 `VariableDependencyConfig` 的實例：

```tsx
class TextInterpolator extends SceneObjectBase<TextInterpolatorState> {
  static Component = TextInterpolatorRenderer;

  protected _variableDependency = new VariableDependencyConfig(this, {
    statePaths: ['text'],
  });

  constructor(text: string) {
    super({ text });
  }

  onTextChange = (text: string) => {
    this.setState({ text });
  };
}
```

`VariableDependencyConfig` 接受一個具有以下設定選項的物件：

-   `statePaths` - 設定物件狀態的哪些屬性可以包含變數。使用 `['*']` 來參照物件狀態的任何屬性。
-   `onReferencedVariableValueChanged` - 設定一個回呼函式，當物件所依賴的變數變更時，將會執行此回呼函式。

:::note
如果未為 `VariableDependencyConfig` 指定 `onReferencedVariableValueChanged`，則物件預設會在變數變更時重新渲染。
:::

### 步驟 5. 在元件中內插 `text` 屬性

在 `TextInterpolatorRenderer` 元件中，使用 `sceneGraph.interpolate` 輔助函式在變數變更時取代 `text` 屬性中的變數：

```tsx
function TextInterpolatorRenderer({ model }: SceneComponentProps<TextInterpolator>) {
  const { text } = model.useState();
  const interpolatedText = sceneGraph.interpolate(model, text);

  return (
    <div>
      <div style={{ marginBottom: 8 }}>
        <TextArea defaultValue={text} onBlur={(e) => model.onTextChange(e.currentTarget.value)} />
      </div>
      <pre>{interpolatedText}</pre>
    </div>
  );
}
```

上述程式碼將渲染一個具有範本變數、文字輸入和預格式化文字區塊的場景。將文字輸入中的文字修改為 `${greetings} World!`，預格式化文字方塊將會更新。變更場景頂部的變數值，這也會更新預格式化文字區塊。

### 自訂變數宏

您可以使用 `sceneUtils.registerVariableMacro` 註冊自訂變數宏。對於您希望根據某些情境動態評估的變數表達式，變數宏非常有用。作為宏實作的核心變數範例。

-   `${__url.params:include:var-from,var-to}`
-   `${__user.login}`

範例：

```tsx
export function getVariablesSceneWithCustomMacro() {
  const scene = new EmbeddedScene({
    // 將行為附加到註冊和取消註冊宏的 SceneApp 或頂層場景物件
    $behaviors: [registerMacro],
    controls: [new VariableValueSelectors({})],
    body: new SceneFlexLayout({
      direction: 'column',
      children: [
        new SceneFlexItem({
          minHeight: 300,
          body: new TextInterpolator('測試我的宏 ${__sceneInfo.key}'),
        }),
      ],
    }),
  });

  return scene;
}

/**
 * 支援 ${__sceneInfo.<stateKey>} 的宏，它將評估為
 * 內插字串的情境場景物件的狀態鍵值。
 */
export class MyCoolMacro implements FormatVariable {
  public state: { name: string; type: string };

  public constructor(name: string, private _context: SceneObject) {
    this.state = { name: name, type: '__sceneInfo' };
  }

  public getValue(fieldPath?: string) {
    if (fieldPath) {
      return (this._context.state as any)[fieldPath];
    }

    return this._context.state.key!;
  }

  public getValueText?(): string {
    return '';
  }
}

function registerMacro() {
  const unregister = sceneUtils.registerVariableMacro('__sceneInfo', MyCoolMacro);
  return () => unregister();
}
```

### 等待變數

當您有依賴於變數的狀態邏輯時，您可以使用 `sceneGraph.hasVariableDependencyInLoadingState` 檢查所有變數相依性是否已就緒 (非載入狀態)。如果任何相依性處於載入狀態，這將傳回 true，包括檢查完整的相依性鏈。

對於同時訂閱時間和變數的物件，我們建議使用 `VariableDependencyConfig` 及其 `onVariableUpdateCompleted` 回呼和 `hasDependencyInLoadingState` 函式。由於變數也可以根據時間做出反應和變更，並且為了避免雙重反應，`VariableDependencyConfig` 具有內部狀態來記住場景物件正在等待變數。若要利用此功能，請指定 `onVariableUpdateCompleted` 回呼。每當相依性變更值時，或者如果場景物件正在等待變數，當變數更新程序完成時，都會呼叫此回呼。

設定範例：

變數：A、B、C (B 相依於 A，C 相依於 B)。A 相依於時間範圍，因此每當時間範圍變更時，它都會載入新值，這可能會導致新值 (然後會導致 B 和 C 也更新)。

具有相依於變數 C 的查詢的 SceneQueryRunner

-   1. 時間範圍變更值
-   2. 變數 A 開始載入
-   3. SceneQueryRunner 回應時間範圍變更，嘗試啟動新查詢，但在發出新查詢之前呼叫 `variableDependency.hasDependencyInLoadingState`。這會檢查變數 C 是否正在載入，但它沒有，所以接著檢查變數 B 是否正在載入 (因為它是 C 的相依性)，它也沒有，所以接著檢查 A，A 正在載入，所以它傳回 true，SceneQueryRunner 將跳過發出新查詢。發生這種情況時，VariableDependencyConfig 將設定一個內部旗標，表示它正在等待變數相依性，這可確保下一個變數完成時呼叫 onVariableUpdateCompleted (無論完成的變數是否為直接相依性，或者它是否已變更值，我們只關心它已完成載入)。
-   4. 變數 A 完成載入。選項 (可能的值) 相同，因此沒有變更值。
-   5. SceneQueryRunner 的 VariableDependencyConfig 收到變數 A 已完成其載入階段的通知，由於它處於等待變數狀態，因此即使 A 不是直接相依性且其值未變更，它也會呼叫 onVariableUpdateCompleted 回呼。

## 原始碼

[檢視範例原始碼](https://github.com/grafana/scenes/tree/main/docusaurus/docs/advanced-variables.tsx)