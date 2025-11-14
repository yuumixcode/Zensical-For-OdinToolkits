---
icon: lucide/book-text
---

# Quick Generate SO

## 简介

Quick Generate SO 是一个用于快速生成 ScriptableObject 资源的工具。

## 开始使用

  1. 选择继承了 `ScriptableObject` 的脚本
  2. 右键选择 `Create SO Asset From Selected`
  3. 在当前目录生成一个 SO 资源

!!! tip "提示"

    1. 支持单选和多选。
         1. 单选 SO 脚本生成时可以立即设置文件名。
         2. 多选 SO 脚本生成时将直接生成资源文件，使用默认命名。
    2. 只有当选择的资源中包含有继承 `ScriptableObject` 的脚本文件时才可以点击 `Create SO Asset From Selected`。

## 菜单项

![Generate SO Asset From Selected](QuickGenerateSO.assets/QuickGenerateSO菜单项.png)

## 已知限制

!!! warning

    为确保准确，一个脚本文件中只包含一个 public 的 class，并且继承 `ScriptableObject`。 
