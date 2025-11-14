# `IPropertyData`

## 介绍

- 种类: `interface`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator`

``` csharp
public interface IPropertyData : Yuumix.OdinToolkits.ScriptDocGenerator.IDerivedMemberData
```

### 注释

- 属性数据接口，继承自 IDerivedMemberData，包含属性特有的数据信息和方法，派生类的通用数据信息和方法

## 属性

### 声明的属性

| 属性签名 | 注释 |
| :--- | :--- |
| `public Type PropertyType { get; }` | 属性类型 |
| `public object DefaultValue { get; }` | 自定义默认值，如果没有自定义默认值，则为 null |
| `public string PropertyTypeFullName { get; }` | 属性类型的完整名称，包括命名空间 |
| `public string PropertyTypeName { get; }` | 属性类型名称 |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
