---
id: advanced-data
title: 自訂場景物件中的資料和時間範圍
---

自訂場景物件可以使用新增至場景的資料和時間範圍資訊來執行額外操作。本主題說明如何在渲染器和自訂物件類別中使用這些屬性。

若要深入了解資料和時間範圍設定，請先參閱[資料和時間範圍](./core-concepts#data-and-time-range)。

## 使用資料

在自訂場景物件中，使用 `sceneGraph.getData(model)` 呼叫來尋找並訂閱最接近且具有 `SceneDataProvider` 的父物件。這表示它會使用在其自身層級設定的 `$data`，或者如果 `$data` 設定在任何父層級，則會與其他同層級物件和場景物件共用資料。

### 在渲染器中使用資料

在您的自訂場景物件渲染器中，您可以使用 `sceneGraph.getData` 公用程式來訂閱最接近的 `SceneDataProvider`：

```ts
import { sceneGraph, SceneObjectState, SceneObjectBase, SceneComponentProps } from '@grafana/scenes';

interface CustomObjectState extends SceneObjectState {}

class CustomObject extends SceneObjectBase<CustomObjectState> {
  static Component = CustomObjectRenderer;
}

function CustomObjectRenderer({ model }: SceneComponentProps<CustomObject>) {
  const data = sceneGraph.getData(model).useState();

  return (
    <div>
      <pre>時間範圍: {JSON.stringify(data.data?.timeRange)}</pre>
      <pre>資料: {JSON.stringify(data.data?.series)}</pre>
    </div>
  );
}
```

### 在自訂物件類別中使用資料

您也可以在自訂物件類別中使用資料。為此，請使用[啟動處理常式](./advanced-activation-handlers.md)。在啟動處理常式中，使用 `sceneGraph.getData(this)` 取得最接近的 `SceneDataProvider`。然後，使用 `SceneObjectBase` 的 `subscribeToState` 方法訂閱 `SceneDataProvider` 的狀態變更：

```ts
class CustomObject extends SceneObjectBase<CustomObjectState> {
  static Component = CustomObjectRenderer;

  constructor() {
    super({});
    this.addActivationHandler(() => this.activationHandler());
  }

  private activationHandler() {
    const sourceData = sceneGraph.getData(this);

    this._subs.add(sourceData.subscribeToState((state) => console.log(state)));
  }
}
```

:::note
從 `sourceData.subscribeToState` 傳回的訂閱會新增至 `this._subs`。因此，當自訂物件被銷毀時，您不需要執行任何清理工作，因為程式庫會負責取消訂閱。
:::

## 使用時間範圍

與資料類似，您可以使用 `sceneGraph.getTimeRange(model)` 在自訂場景物件中使用最接近的時間範圍。如先前在[使用資料](#use-data)一節中所述，此方法可以在自訂物件類別和渲染器中使用。

## 共用相同的資料提供者

如果您需要在許多不同的場景物件之間共用相同的資料提供者，並且無法透過將 `$data` 放在共用的共同祖先上來實現，您可以使用 `DataProviderSharer`。這是一個可以從另一個資料提供者共用/轉發資料的資料提供者。

## 原始碼

[檢視範例原始碼](https://github.com/grafana/scenes/tree/main/docusaurus/docs/advanced-data.tsx)