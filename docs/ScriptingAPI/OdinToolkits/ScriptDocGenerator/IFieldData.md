# `IFieldData`

## 介绍

- 种类: `interface`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator`

``` csharp
public interface IFieldData : Yuumix.OdinToolkits.ScriptDocGenerator.IDerivedMemberData
```

### 注释

- 字段数据接口，继承自 IDerivedMemberData

## 属性

### 声明的属性

| 属性签名 | 注释 |
| :--- | :--- |
| `public Type FieldType { get; }` | 字段的类型 |
| `public bool IsConstant { get; }` | 是否为常量字段（const） |
| `public bool IsDynamic { get; }` | 是否为动态字段（dynamic） |
| `public bool IsReadOnly { get; }` | 是否为只读字段（readonly） |
| `public object DefaultValue { get; }` | 字段的默认值 |
| `public string FieldTypeFullName { get; }` | 这个字段的类型的完整名称 |
| `public string FieldTypeName { get; }` | 这个字段的类型的名称 |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
