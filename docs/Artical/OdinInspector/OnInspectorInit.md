---
tags:
  - odin-inspector
authors: 
  - Yuumix
comments: true
---

# `[OnInspectorInit]` 特性

??? note "官方英文原文"

    The OnInspectorInit attribute takes in an action string as an argument (typically the name of a method to be invoked, or an expression to be executed), and executes that action when the property's drawers are initialized in the inspector. Initialization will happen at least once during the first drawn frame of any given property, but may also happen several times later, most often when the type of a polymorphic property changes and it refreshes its drawer setup and recreates all its children.

OnInspectorInit 特性填写一个方法，或者一个表达式，当被挂载此特性的 Property[^1] 初始化时，执行对应的方法或者表达式。

[^1]: Inspector 面板中显示的每一个内容在编辑器中被称为一个 Property，这里的 Property 和逻辑代码中说的属性(Property) 不是相同概念。
