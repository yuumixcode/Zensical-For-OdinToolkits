# `TypeAnalyzerUtility`

## 介绍

- 种类: `static class`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator`

``` csharp
public static class TypeAnalyzerUtility
```

### 注释

- 类型分析器工具类

## 方法

### 所有方法签名总览

| 方法完整签名 |
| :--- | 
| `public Type GetType()` |
| `public virtual bool Equals(object obj)` |
| `public virtual int GetHashCode()` |
| `public virtual string ToString()` |
| `public static bool TreatedAsTypeDefaultValue(object value, Type type)` |
| `public static bool TryGetFormatedAttributeWithFullParameter(object attrInstance, out ref string attributeFullSignature)` |
| `public static string GetAttributeNameWithoutSuffix(string attributeName)` |
| `public static string GetFieldKeywordSnippet(bool isConst, bool isStatic, bool isReadOnly)` |
| `public static string GetFormattedDefaultValue(Type memberType, object value)` |
| `protected object MemberwiseClone()` |
| `protected virtual void Finalize()` |

### 声明的普通方法

| 普通方法名称 | 注释 |
| :--- | :--- | 
| `public static bool TreatedAsTypeDefaultValue` | 提供的值被视为类型的默认值，返回 true 表示被视为类型的默认值，返回 false 表示不是。 |
| `public static bool TryGetFormatedAttributeWithFullParameter` | 获取格式化的完整特性签名字符串，返回 true 表示该特性支持格式化为完整特性签名字符串，返回 false 表示不支持。 |
| `public static string GetAttributeNameWithoutSuffix` | 获取没有后缀的 Attribute 名称 |
| `public static string GetFieldKeywordSnippet` | 获取字段的关键字片段字符串 |
| `public static string GetFormattedDefaultValue` | 获取格式化的默认值字符串，用于生成签名 |

### 继承的普通方法

| 普通方法名称 | 注释 | 声明方法的类 |
| :--- | :--- | :--- |
| `public Type GetType` |  | `System.Object` |
| `public virtual bool Equals` |  | `System.Object` |
| `public virtual int GetHashCode` |  | `System.Object` |
| `public virtual string ToString` |  | `System.Object` |
| `protected object MemberwiseClone` |  | `System.Object` |
| `protected virtual void Finalize` |  | `System.Object` |

## 字段

### 声明的普通字段

| 字段名称 | 注释 | 
| :--- | :--- | 
| `public static readonly IReadOnlyDictionary<Type, string> TypeAliasMap;` | 将系统类型名称映射到其 C# 别名的字典 |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
