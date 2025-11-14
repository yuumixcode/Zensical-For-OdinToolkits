# `DefaultAttributeFilter`

## 介绍

- 种类: `class`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator`

``` csharp
public class DefaultAttributeFilter : Yuumix.OdinToolkits.ScriptDocGenerator.IAttributeFilter
```

### 注释

- 默认特性过滤器，构造函数中传入需要排除的 Attribute 类型

## 构造方法

| 构造方法签名 [仅包含公共实例方法] | 注释 |
| :--- | :--- |
| `public DefaultAttributeFilter(Type[] excludeTypes)` |  |

## 方法

### 所有方法签名总览

| 方法完整签名 |
| :--- | 
| `public Type GetType()` |
| `public bool ShouldFilterOut(Type type)` |
| `public virtual bool Equals(object obj)` |
| `public virtual int GetHashCode()` |
| `public virtual string ToString()` |
| `protected object MemberwiseClone()` |
| `protected virtual void Finalize()` |

### 声明的普通方法

| 普通方法名称 | 注释 |
| :--- | :--- | 
| `public bool ShouldFilterOut` | 判断传入的特性类型是否应该被过滤掉 |

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
| `public Type[] ExcludeTypes { get; }` | 排除的特性类型 |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
