# [0001. Custom New Tab URL](https://github.com/tnotesjs/TNotes.chrome/tree/main/notes/0001.%20Custom%20New%20Tab%20URL)

<!-- region:toc -->

- [1. 📝 概述](#1--概述)
- [2. 💻 核心原理简介](#2--核心原理简介)

<!-- endregion:toc -->

## 1. 📝 概述

- Custom New Tab URL
  - 自定义你的 Chrome 新标签页
  - 应用商店链接：https://chromewebstore.google.com/detail/custom-new-tab-url/mmjbdbjnoablegbkcklggeknkfcjkjia
  - ![图 0](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-06-26-20-08-23.png)
- 配置页面：
  - 及其简洁，你只需要将 Enabled 开关打开，并输入新标签页的 URL 保存即可。
  - 接下来你每次打开新标签页，比如点击标签栏右侧的 ➕ 号图标或者通过快捷方式 `CTRL/CMD T` 打开新页面，都会自动跳转到你配置的页面。
  - ![图 1](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2025-06-26-20-18-30.png)
  - 比如图片中就是将新标签页定位到 TNotes 主页。

## 2. 💻 核心原理简介

- **通过 `chrome.tabs` API 拦截导航**
  - 监听新标签页创建事件，动态修改目标 URL：

```javascript
chrome.tabs.onCreated.addListener((tab) => {
  if (tab.url === 'chrome://newtab/') {
    chrome.tabs.update(tab.id, { url: customUrl })
  }
})
```

- **配置持久化存储**
  - 使用 `chrome.storage.sync` 保存用户设置的 URL，实现跨设备同步：

```javascript
chrome.storage.sync.set({ redirectUrl: newUrl })
```
