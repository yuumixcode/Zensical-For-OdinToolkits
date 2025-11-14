# `TypeData`

## 介绍

- 种类: `class`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator`

``` csharp
[Serializable]
public class TypeData : Yuumix.OdinToolkits.ScriptDocGenerator.MemberData, 
Yuumix.OdinToolkits.ScriptDocGenerator.ITypeData, 
Yuumix.OdinToolkits.ScriptDocGenerator.IDerivedMemberData, 
Yuumix.OdinToolkits.ScriptDocGenerator.IMemberData
```

### 注释

- 类型解析数据类，存储类型的各种成员的解析数据

## 构造方法

| 构造方法签名 [仅包含公共实例方法] | 注释 |
| :--- | :--- |
| `public TypeData(Type type, IAttributeFilter filter = null, IAnalysisDataFactory factory = null)` |  |

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
| `public AccessModifierType AccessModifier { get; }` | 访问修饰符 |
| `public Assembly Assembly { get; }` | 类型所在的程序集 |
| `public IAnalysisDataFactory DataFactory { get; }` | 分析数据工厂实例对象 |
| `public IConstructorData[] RuntimeReflectedConstructorsData { get; }` | 声明的构造方法解析数据数组，只包含公共构造函数，GetConstructors() 方法 |
| `public IEventData[] RuntimeReflectedEventsData { get; }` | 声明的事件解析数据数组，GetRuntimeEvents() 方法 |
| `public IFieldData[] RuntimeReflectedFieldsData { get; }` | 类型的字段解析数据数组，GetUserDefinedFields() 方法 |
| `public IMethodData[] RuntimeReflectedMethodsData { get; }` | 声明的方法解析数据数组，GetRuntimeMethods() 方法 |
| `public IPropertyData[] RuntimeReflectedPropertiesData { get; }` | 声明的属性解析数据数组，GetRuntimeProperties() 方法 |
| `public MemberTypes MemberType { get; }` | 成员类型 |
| `public TypeCategory TypeCategory { get; }` | Type 种类 |
| `public bool IsAbstract { get; }` | 是否为抽象类 |
| `public bool IsGenericType { get; }` | 是否为泛型类型 |
| `public bool IsSealed { get; }` | 是否为密封类 |
| `public bool IsStatic { get; }` | 是否为静态类型 |
| `public string AccessModifierName { get; }` | 访问修饰符名称 |
| `public string AssemblyName { get; }` | 程序集名称 |
| `public string FullDeclarationWithAttributes { get; }` | 完整类型声明 - 包含特性和签名 - 默认剔除 [Summary] 特性 |
| `public string MemberTypeName { get; }` | 成员类型名称 |
| `public string NamespaceName { get; }` | 命名空间名称 |
| `public string Signature { get; }` | 类型签名，不包含特性声明 |
| `public string[] InheritanceChain { get; }` | 继承链数组 |
| `public string[] InterfaceArray { get; }` | 接口列表数组 |
| `public string[] ReferenceWebLinkArray { get; }` | 引用链接数组 |

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
