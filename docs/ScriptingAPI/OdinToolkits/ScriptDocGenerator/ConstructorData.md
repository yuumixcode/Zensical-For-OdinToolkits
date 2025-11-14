# `ConstructorData`

## 介绍

- 种类: `class`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator`

``` csharp
[Serializable]
public class ConstructorData : Yuumix.OdinToolkits.ScriptDocGenerator.MemberData, 
Yuumix.OdinToolkits.ScriptDocGenerator.IConstructorData, 
Yuumix.OdinToolkits.ScriptDocGenerator.IDerivedMemberData, 
Yuumix.OdinToolkits.ScriptDocGenerator.IMemberData
```

### 注释

- 构造方法解析数据

## 构造方法

| 构造方法签名 [仅包含公共实例方法] | 注释 |
| :--- | :--- |
| `public ConstructorData(ConstructorInfo constructorInfo, IAttributeFilter filter = null)` |  |

## 方法

### 所有方法签名总览

| 方法完整签名 |
| :--- | 
| `public Type GetType()` |
| `public virtual bool Equals(object obj)` |
| `public virtual int GetHashCode()` |
| `public virtual string ToString()` |
| `protected object MemberwiseClone()` |
| `protected virtual void Finalize()` |

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
| `public AccessModifierType AccessModifier { get; }` | 构造方法的访问修饰符类型 |
| `public MemberTypes MemberType { get; }` | 构造方法的成员类型 |
| `public bool IsStatic { get; }` | 是否为静态构造方法 |
| `public string AccessModifierName { get; }` | 构造方法的访问修饰符名称字符串 |
| `public string FullDeclarationWithAttributes { get; }` | 包含特性和签名的完整构造方法声明 |
| `public string MemberTypeName { get; }` | 构造方法的成员类型名称字符串 |
| `public string ParametersDeclaration { get; }` | 构造方法的参数声明字符串，包含参数名称和类型 |
| `public string Signature { get; }` | 构造方法的完整签名 |
| `public string SignatureWithoutParameters { get; }` | 不包含参数的简单构造方法签名 |

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
