# `IConstructorData`

## 介绍

- 种类: `interface`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator`

``` csharp
public interface IConstructorData : Yuumix.OdinToolkits.ScriptDocGenerator.IDerivedMemberData
```

### 注释

- 构造方法数据接口，继承自 IDerivedMemberData

## 属性

### 声明的属性

| 属性签名 | 注释 |
| :--- | :--- |
| `public string ParametersDeclaration { get; }` | 方法的参数声明字符串，包含参数名称和类型 |
| `public string SignatureWithoutParameters { get; }` | 不包含参数的简单方法签名 |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
