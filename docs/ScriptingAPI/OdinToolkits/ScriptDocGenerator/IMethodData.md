# `IMethodData`

## 介绍

- 种类: `interface`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator`

``` csharp
public interface IMethodData : Yuumix.OdinToolkits.ScriptDocGenerator.IDerivedMemberData
```

### 注释

- 方法数据接口，继承自 IDerivedMemberData

## 属性

### 声明的属性

| 属性签名 | 注释 |
| :--- | :--- |
| `public Type ReturnType { get; }` | 方法的返回类型 |
| `public bool IsAbstract { get; }` | 是否带有抽象属性，不一定声明时带有 abstract 关键字 |
| `public bool IsAsync { get; }` | 是否为异步方法 |
| `public bool IsFromAncestor { get; }` | 此方法继承自祖先（不是直接的基类，而是基类的上层） |
| `public bool IsFromInterfaceImplement { get; }` | 此方法继承自接口，在该类中实现 |
| `public bool IsOperator { get; }` | 是否为运算符方法 |
| `public bool IsOverloadMethodInDeclaringType { get; set; }` | 此方法在当前类中存在重载方法，需要在 Type 解析时进行处理 |
| `public bool IsOverride { get; }` | 是否属于重写的方法，不一定带有 override 关键字 |
| `public bool IsVirtual { get; }` | 是否带有虚方法属性，不一定声明时带有 virtual 关键字 |
| `public string ParametersDeclaration { get; }` | 方法的参数声明字符串，包含参数名称和类型 |
| `public string ReturnTypeFullName { get; }` | 方法的返回类型的完整名称 |
| `public string ReturnTypeName { get; }` | 方法的返回类型名称 |
| `public string SignatureWithoutParameters { get; }` | 不包含参数的简单方法签名 |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
