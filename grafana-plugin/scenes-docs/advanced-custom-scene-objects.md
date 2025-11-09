---
id: advanced-custom-scene-objects
title: 自訂場景物件
---

Scenes 的設計考量了擴充性。除了程式庫的基本功能外，您還可以建置自己的自訂場景物件，以擴充程式庫的基本功能。本主題說明如何建立自訂物件。

## 建立自訂場景物件

請依照以下步驟建立自訂場景物件。

### 步驟 1. 定義自訂物件的狀態類型

首先為您的自訂物件定義狀態類型。此介面必須擴充 `SceneObjectState` 介面：

```ts
interface CounterState extends SceneObjectState {
  count: number;
}
```

### 步驟 2. 實作自訂物件類別

為自訂場景物件實作一個類別。此類別必須擴充 `SceneObjectBase` 類別：

```ts
export class Counter extends SceneObjectBase<CounterState> {
  public constructor(state?: Partial<CounterState>) {
    super({ count: 0, ...state });
  }
}
```

### 步驟 3. 實作自訂物件渲染器

實作一個 React 元件，當在場景中使用自訂物件時，將會顯示此元件。此元件的 props 必須使用 `SceneComponentProps<T extends SceneObjectBase>` 類型：

```ts
function CounterRenderer(props: SceneComponentProps<Counter>) {
  return <div>計數器</div>;
}
```

使用 `static Component` 屬性為 `Counter` 自訂物件設定渲染器：

```ts
export class Counter extends SceneObjectBase<CounterState> {
  static Component = CounterRenderer;
}
```

### 步驟 4. 在渲染器中使用自訂物件狀態

使用傳遞給元件的 `model` 屬性，並使用 `model.useState()` 掛鉤訂閱其狀態。物件狀態的任何變更都會重新渲染元件：

```ts
function CounterRenderer({ model }: SceneComponentProps<Counter>) {
  const { count } = model.useState();

  return (
    <div>
      <div>計數器: {count}</div>
    </div>
  );
}
```

### 步驟 5. 從元件修改自訂物件的狀態

在自訂場景物件中定義狀態修改方法 (`onIncrement`)：

```ts
export class Counter extends SceneObjectBase<CounterState> {
  public static Component = CounterRenderer;

  public onIncrement = () => {
    this.setState({ count: this.state.count + 1 });
  };
}
```

在渲染器中使用 `onIncrement` 方法：

```ts
function CounterRenderer({ model }: SceneComponentProps<Counter>) {
  const { count } = model.useState();

  return (
    <div>
      <div>計數器: {count}</div>
      <button onClick={model.onIncrement}>遞增計數器</button>
    </div>
  );
}
```

### 步驟 6. 在場景中使用自訂物件

現在您的自訂場景物件 `Counter` 已準備好在場景中使用。建立一個使用它的場景：

```ts
const myScene = new EmbeddedScene({
  body: new SceneFlexLayout({
    children: [
      new SceneFlexItem({
        body: new Counter(),
      }),
    ],
  }),
});
```

## 原始碼

[檢視範例原始碼](https://github.com/grafana/scenes/tree/main/docusaurus/docs/advanced-custom-scene-objects.tsx)