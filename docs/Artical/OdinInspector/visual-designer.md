---
tags:
  - odin-inspector
authors: 
  - Yuumix
comments: true
---

# Visual Designer 设计

Odin Inspector 4.0 Visual Designer 定义了一个新的文本文件类型，`ovdf`。这个文件类型用于定义可视化设计师的布局和行为。

1. 用户可以发布包含 `ovdf` 文件的插件，而无需任何代码依赖。此类型的文本不会影响项目，只有在用户安装了 Odin Inspector 4.0 或更高版本时，才会使得 `ovdf` 文件生效。这样的话，使用或者支持 Odin Inspector 4.0 变得更加简单和灵活，无需依赖任何代码。

2. 可视化设计器，默认是不允许修改 Unity 拥有自定义编辑器的脚本的样式的，不打算支持修改自定义编辑器的脚本的样式，未来会确保无法编辑带有自定义编辑器的类型，这样可以避免一些潜在的问题。
