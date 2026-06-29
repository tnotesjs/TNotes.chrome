# [0003. 在 GitHub 上预览自己的笔记时处理一些默认样式](https://github.com/tnotesjs/TNotes.chrome/tree/main/notes/0003.%20%E5%9C%A8%20GitHub%20%E4%B8%8A%E9%A2%84%E8%A7%88%E8%87%AA%E5%B7%B1%E7%9A%84%E7%AC%94%E8%AE%B0%E6%97%B6%E5%A4%84%E7%90%86%E4%B8%80%E4%BA%9B%E9%BB%98%E8%AE%A4%E6%A0%B7%E5%BC%8F)

<!-- region:toc -->

- [1. 概述](#1-概述)
- [2. js 实现脚本](#2-js-实现脚本)

<!-- endregion:toc -->

## 1. 概述

- 分享个人在预览 github 上的 TNotes 笔记时添加的自定义 JS 脚本。
- 脚本的功能很简单，主要是针对图片的大小和超链接的默认样式做了些修改。

## 2. js 实现脚本

```js
var style = document.createElement('style')

style.innerHTML = `
  .js-snippet-clipboard-copy-unpositioned img {
    /* width: 50% !important; */
  }
  .js-snippet-clipboard-copy-unpositioned a {
    text-decoration: none !important;
  }
`

var head = document.head || document.getElementsByTagName('head')[0]

head.appendChild(style)

// 选择具有特定类名的所有div元素
const divs = document.querySelectorAll(
  '.js-snippet-clipboard-copy-unpositioned',
)

divs.forEach((div) => {
  // 在每个div中查找所有的img标签
  const images = div.querySelectorAll('img')

  images.forEach((img) => {
    // 确保图片已经加载完成
    if (img.complete) {
      resizeImage(img)
    } else {
      img.onload = function () {
        resizeImage(img)
      }
    }
  })
})

function resizeImage(image) {
  // 获取原始宽度和高度
  const originalWidth = image.naturalWidth
  const originalHeight = image.naturalHeight

  // 设置新的宽度为原始宽度的50%
  const newWidth = originalWidth * 0.5
  // 保持宽高比的情况下设置新高度
  const newHeight = originalHeight * 0.5

  // 应用新的尺寸
  image.style.width = newWidth + 'px'
  image.style.height = newHeight + 'px'
}
```

- **脚本功能简介**
  - 图片尺寸设置为原分辨率的一半大小。
    - 使用的显示器的分辨率是 4k，截出来的图片在 github 上展示的时候，尺寸是实际截屏尺寸的两倍。为了在预览图片的时候，能够和截图时的尺寸一致，因此做了缩小处理。
  - 超链接去掉下划线。
    - 感觉去掉下划线更美观。
- **脚本应用**
  - 可以将脚本丢到 Scripty 插件中，简单配置一下自动加载规则，即可在访问 https://github.com/Tdahuyou/ 自己的 github 仓库数据时自动运行。
  - ![](https://cdn.jsdelivr.net/gh/tnotesjs/imgs@main/2024-11-29-23-07-30.png)
  - 也可以丢到油猴插件中，并添加简单的配置即可使用。
