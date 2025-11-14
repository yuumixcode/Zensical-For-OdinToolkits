# `TypeCategory`

## 介绍

- 种类: `enum`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator`

``` csharp
public enum TypeCategory : System.Enum, 
System.IFormattable, 
System.IComparable, 
System.IConvertible
```

## 方法

### 所有方法签名总览

| 方法完整签名 |
| :--- | 
| `public Type GetType()` |
| `public bool HasFlag(Enum flag)` |
| `public override bool Equals(object obj)` |
| `public override int GetHashCode()` |
| `public override string ToString()` |
| `public string ToString(string format)` |
| `public virtual TypeCode GetTypeCode()` |
| `public virtual int CompareTo(object target)` |
| `protected object MemberwiseClone()` |
| `protected virtual void Finalize()` |
| `[Overload] [Overload] public virtual string ToString(IFormatProvider provider)` |
| `[Overload] [Overload] public virtual string ToString(string format, IFormatProvider provider)` |

### 继承的普通方法

| 普通方法名称 | 注释 | 声明方法的类 |
| :--- | :--- | :--- |
| `public Type GetType` |  | `System.Object` |
| `public bool HasFlag` |  | `System.Enum` |
| `public override bool Equals` |  | `System.Enum` |
| `public override int GetHashCode` |  | `System.Enum` |
| `public override string ToString` |  | `System.Enum` |
| `public string ToString` |  | `System.Enum` |
| `public virtual TypeCode GetTypeCode` |  | `System.Enum` |
| `public virtual int CompareTo` |  | `System.Enum` |
| `protected object MemberwiseClone` |  | `System.Object` |
| `protected virtual void Finalize` |  | `System.Object` |
| `public virtual string ToString` |  | `System.Enum` |
| `public virtual string ToString` |  | `System.Enum` |

## 字段

### 常量字段

| 字段完整签名 | 注释 |
| :--- | :--- |
| `public const TypeCategory Class;` |  |
| `public const TypeCategory Delegate;` |  |
| `public const TypeCategory Enum;` |  |
| `public const TypeCategory Interface;` |  |
| `public const TypeCategory Record;` |  |
| `public const TypeCategory Struct;` |  |
| `public const TypeCategory Unknown;` |  |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
