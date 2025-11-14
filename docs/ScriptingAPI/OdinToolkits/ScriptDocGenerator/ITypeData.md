# `ITypeData`

## 介绍

- 种类: `interface`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator`

``` csharp
public interface ITypeData : Yuumix.OdinToolkits.ScriptDocGenerator.IDerivedMemberData
```

### 注释

- 类型解析数据接口，继承自 IDerivedMemberData 接口

## 属性

### 声明的属性

| 属性签名 | 注释 |
| :--- | :--- |
| `public Assembly Assembly { get; }` | 类型所在的程序集 |
| `public IAnalysisDataFactory DataFactory { get; }` | 分析数据工厂实例对象 |
| `public IConstructorData[] RuntimeReflectedConstructorsData { get; }` | 类型的构造函数解析数据数组，只包含公共构造函数，GetConstructors() 方法 |
| `public IEventData[] RuntimeReflectedEventsData { get; }` | 类型的事件解析数据数组，GetRuntimeEvents() 方法 |
| `public IFieldData[] RuntimeReflectedFieldsData { get; }` | 类型的字段解析数据数组，GetUserDefinedFields() 方法 |
| `public IMethodData[] RuntimeReflectedMethodsData { get; }` | 类型的方法解析数据数组，GetRuntimeMethods() 方法 |
| `public IPropertyData[] RuntimeReflectedPropertiesData { get; }` | 类型的属性解析数据数组，GetRuntimeProperties() 方法 |
| `public TypeCategory TypeCategory { get; }` | Type 种类 |
| `public bool IsAbstract { get; }` | 是否为抽象类型 |
| `public bool IsGenericType { get; }` | 是否为泛型类型 |
| `public bool IsSealed { get; }` | 是否为密封类型 |
| `public string AssemblyName { get; }` | 类型所在的程序集名称 |
| `public string NamespaceName { get; }` | 类型所在的命名空间 |
| `public string[] InheritanceChain { get; }` | 类型的继承链数组 |
| `public string[] InterfaceArray { get; }` | 类型的接口数组 |
| `public string[] ReferenceWebLinkArray { get; }` | 类型的引用链接数组 |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
