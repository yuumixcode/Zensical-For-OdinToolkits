# `MethodData`

## 介绍

- 种类: `class`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator`

``` csharp
[Serializable]
public class MethodData : Yuumix.OdinToolkits.ScriptDocGenerator.MemberData, 
Yuumix.OdinToolkits.ScriptDocGenerator.IDerivedMemberData, 
Yuumix.OdinToolkits.ScriptDocGenerator.IMemberData, 
Yuumix.OdinToolkits.ScriptDocGenerator.IMethodData
```

### 注释

- 方法解析数据类，用于存储 MethodInfo 的解析结果

## 构造方法

| 构造方法签名 [仅包含公共实例方法] | 注释 |
| :--- | :--- |
| `public MethodData(MethodInfo memberInfo, IAttributeFilter filter = null)` |  |

## 方法

### 所有方法签名总览

| 方法完整签名 |
| :--- | 
| `public Type GetType()` |
| `public virtual bool Equals(object obj)` |
| `public virtual int GetHashCode()` |
| `public virtual string ToString()` |
| `public void AddOverloadPrefix()` |
| `public static string GetMethodKeywordSnippet(MethodInfo methodInfo)` |
| `protected object MemberwiseClone()` |
| `protected virtual void Finalize()` |

### 声明的普通方法

| 普通方法名称 | 注释 |
| :--- | :--- | 
| `public void AddOverloadPrefix` | 为方法添加重载前缀（[Overload]） |
| `public static string GetMethodKeywordSnippet` | 获取方法的关键字片段字符串 |

### 继承的普通方法

| 普通方法名称 | 注释 | 声明方法的类 |
| :--- | :--- | :--- |
| `public Type GetType` |  | `System.Object` |
| `public virtual bool Equals` |  | `System.Object` |
| `public virtual int GetHashCode` |  | `System.Object` |
| `public virtual string ToString` |  | `System.Object` |
| `protected object MemberwiseClone` |  | `System.Object` |
| `protected virtual void Finalize` |  | `System.Object` |

## 属性

### 声明的属性

| 属性签名 | 注释 |
| :--- | :--- |
| `public AccessModifierType AccessModifier { get; }` | 方法的访问修饰符类型 |
| `public MemberTypes MemberType { get; }` | 方法的成员类型 |
| `public Type ReturnType { get; }` | 方法的返回类型 |
| `public bool IsAbstract { get; }` | 是否为抽象方法（abstract） |
| `public bool IsAsync { get; }` | 是否为异步方法（async） |
| `public bool IsFromAncestor { get; }` | 是否从祖先类继承的重写方法，在该子类的方法签名中不一定带有 override 关键字 |
| `public bool IsFromInterfaceImplement { get; }` | 是否从接口实现的方法 |
| `public bool IsOperator { get; }` | 是否为运算符方法（operator） |
| `public bool IsOverloadMethodInDeclaringType { get; set; }` | 是否为声明类型中的重载方法 |
| `public bool IsOverride { get; }` | 是否有重写方法（override）的特性，方法签名中不一定带有 override 关键字 |
| `public bool IsStatic { get; }` | 是否为静态方法（static） |
| `public bool IsVirtual { get; }` | 是否有虚拟方法（virtual）的特性，方法签名中不一定带有 virtual 关键字 |
| `public string AccessModifierName { get; }` | 方法的访问修饰符名称字符串 |
| `public string FullDeclarationWithAttributes { get; }` | 包含特性的完整方法声明字符串，包含特性声明和方法签名 |
| `public string MemberTypeName { get; }` | 方法的成员类型名称字符串 |
| `public string ParametersDeclaration { get; }` | 方法的参数声明字符串，包含参数名称和类型 |
| `public string ReturnTypeFullName { get; }` | 方法的返回值完整类型名称字符串 |
| `public string ReturnTypeName { get; }` | 方法的返回类型名称字符串 |
| `public string Signature { get; private set; }` | 方法的签名字符串 |
| `public string SignatureWithoutParameters { get; }` | 不包含参数的简单方法签名 |

### 继承的属性

| 属性签名 | 注释 | 声明属性的类 | 
| :--- | :--- | :--- |
| `public Type DeclaringType { get; }` | 声明此成员的类型 | `Yuumix.OdinToolkits.ScriptDocGenerator.MemberData` |
| `public Type ReflectedType { get; }` | 通过反射获取该成员的类型 | `Yuumix.OdinToolkits.ScriptDocGenerator.MemberData` |
| `public bool IsFromInheritance { get; }` | 成员是否从继承中获取，这里的成员不包括 Type 类型 | `Yuumix.OdinToolkits.ScriptDocGenerator.MemberData` |
| `public bool IsObsolete { get; }` | 是否已过时 | `Yuumix.OdinToolkits.ScriptDocGenerator.MemberData` |
| `public string AttributesDeclaration { get; }` | 特性声明字符串 | `Yuumix.OdinToolkits.ScriptDocGenerator.MemberData` |
| `public string DeclaringTypeFullName { get; }` | 声明类型的完整名称，包括命名空间 | `Yuumix.OdinToolkits.ScriptDocGenerator.MemberData` |
| `public string DeclaringTypeName { get; }` | 声明类型的名称 | `Yuumix.OdinToolkits.ScriptDocGenerator.MemberData` |
| `public string Name { get; }` | 成员名称 | `Yuumix.OdinToolkits.ScriptDocGenerator.MemberData` |
| `public string ReflectedTypeFullName { get; }` | 通过反射获取该成员的类型的完整名称，包括命名空间 | `Yuumix.OdinToolkits.ScriptDocGenerator.MemberData` |
| `public string ReflectedTypeName { get; }` | 通过反射获取该成员的类型名称 | `Yuumix.OdinToolkits.ScriptDocGenerator.MemberData` |
| `public string SummaryAttributeValue { get; }` | 注释 | `Yuumix.OdinToolkits.ScriptDocGenerator.MemberData` |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
