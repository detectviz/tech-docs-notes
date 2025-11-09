---
id: degregate-ui
title: 在使用擴充點時保護您的 UI
sidebar_label: 保護您的 UI
description: 在使用擴充點時保護您的 UI。
keywords:
  - grafana
  - plugins
  - plugin
  - extensions
  - ui-extensions
  - degregate
sidebar_position: 90
---

在使用 Grafana 中的 UI 擴充功能開發功能時，請考慮呈現的內容可能可用或不可用的情境。這可讓您建立一個有彈性的 UI，無論擴充功能的可用性或數量如何，都能保持正常運作，並確保無縫的使用者體驗。

## 內容無法在我的擴充點中呈現

如果您已建立擴充點但尚未呈現任何內容，請確保：

- 如果沒有呈現的內容，UI 不會顯示此區段。
- 如果功能在沒有呈現內容的情況下仍然有用，請提供備用 UI 或預留位置訊息。

例如：

```
{extensions.length > 0 ? (
  extensions.map((Ext, index) => <Ext key={index} />)
) : (
  <DefaultComponent />
)}
```

## 我的擴充點支援多個元素

您可以建立一個允許 多個插件貢獻元素的擴充點。在這種情況下，請確保 UI：

- 可以在不破壞版面的情況下呈現多個擴充功能。
- 使用適當的間距和排序。
- 處理衝突，例如衝突的樣式或重複的內容。

例如，您可以使用具有受控版面的容器：

```
<div className="extensions-container">
  {extensions.map((Ext, index) => (
    <div key={index} className="extension-item">
      <Ext />
    </div>
  ))}
</div>
```

## 我的擴充點使用外部插件

如果您正在使用外部插件來擴充您的 UI，請考慮以下事項：

- 安全性和驗證：確保擴充功能不會引入漏洞，例如透過清理使用者產生的內容。
- 共用資料限制：僅與外部元素共用所需的最少量資料。您隨時可以稍後擴充，這比移除更容易。
- 限制插件：您可以決定只允許某些插件為您的擴充點提供內容。

例如，您可以依插件 ID 限制擴充功能：

```
const allowedExtensions = extensions.filter(ext => allowedPluginIds.includes(ext.pluginId));
```