# 自定义主题

::: tip 提示
主题组件同样受到[浏览器 API 访问限制](./using-vue.md#browser-api-access-restrictions)。
:::

VuePress 使用 Vue 单个文件组件来定制主题。要使用自定义布局，请在文档根目录下创建一个 `.vuepress/theme` 目录，然后创建一个 `Layout.vue` 文件：

```
.
└─ .vuepress
   └─ theme
      └─ Layout.vue
```

这儿和开发一个正常的 Vue 应用程序是一样的。完全取决于你如何组织你的主题。

## 网站和网页元数据（Site and Page Metadata）

对于 `docs` 中的每个 `.md` 文件，`Layout`组件将只被调用一次，并且整个站点和该特定页面的元数据将分别暴露为 `this.$site` 和 `this.$page` 属性被注入到应用程序中的每个组件里。

这是这个网站的 `$site` 值：

``` json
{
  "title": "VuePress",
  "description": "Vue-powered Static Site Generator",
  "base": "/",
  "pages": [
    {
      "path": "/",
      "title": "VuePress",
      "frontmatter": {}
    },
    ...
  ]
}
```

`title`，`description` 和 `base` 从 `.vuepress/config.js` 中的相应字段复制而来。 `pages` 包含每个页面的元数据对象数组，包括其路径，页面标题（在 YAML frontmatter 中显式指定或从页面上的第一个标题推断）以及该文件中的任意 YAML frontmatter 数据。

这是你正在查看的此页面的 `$page` 对象：

``` json
{
  "path": "/custom-themes.html",
  "title": "自定义主题",
  "headers": [/* ... */],
  "frontmatter": {}
}
```

如果用户在 `.vuepress/config.js` 中提供 `themeConfig`，它也可以用 `$site.themeConfig`。你可以使用它来允许用户自定义主题的行为 - 例如，指定类别和页面顺序。然后，你可以将这些数据与 `$site.pages` 一起使用来动态构建导航链接。

最后，别忘了 `this.$route` 和 `this.$router` 也可作为 Vue Router API 的一部分。

## 内容出口（Content Outlet）

所渲染的当前 `.md` 文件的编译内容将作为特殊的 `<Content/>` 全局组件来使用。你需要将其呈现在布局的某个位置以显示页面的内容。最简单的主题可以是一个带有以下内容的 `Layout.vue` 组件：

``` html
<template>
  <div class="theme-container">
    <Content/>
  </div>
</template>
```

## 使用来自依赖项的主题（Using Theme from a Dependency）

主题可以在 npm 上以原始 Vue SFC 格式发布为 `vuepress-theme-xxx`。

要使用 npm 依赖项的主题，请在 `.vuepress/config.js` 中提供一个 `theme` 选项：

``` js
module.exports = {
  theme: 'awesome'
}
```

VuePress 将尝试定位并使用 `node_modules/vuepress-theme-awesome/Layout.vue`。

***

> 原文：[https://vuepress.vuejs.org/guide/custom-themes.html](https://vuepress.vuejs.org/guide/custom-themes.html)

***
