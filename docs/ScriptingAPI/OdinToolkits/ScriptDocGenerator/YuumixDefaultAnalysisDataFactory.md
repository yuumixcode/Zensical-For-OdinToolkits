# `YuumixDefaultAnalysisDataFactory`

## 介绍

- 种类: `class`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator`

``` csharp
[Serializable]
public class YuumixDefaultAnalysisDataFactory : Yuumix.OdinToolkits.ScriptDocGenerator.IAnalysisDataFactory
```

### 注释

- Yuumix 默认提供的解析数据工厂实现类

## 构造方法

| 构造方法签名 [仅包含公共实例方法] | 注释 |
| :--- | :--- |
| `public YuumixDefaultAnalysisDataFactory()` |  |

## 方法

### 所有方法签名总览

| 方法完整签名 |
| :--- | 
| `public IConstructorData CreateConstructorData(ConstructorInfo constructorInfo, IAttributeFilter filter = null)` |
| `public IEventData CreateEventData(EventInfo eventInfo, IAttributeFilter filter = null)` |
| `public IFieldData CreateFieldData(FieldInfo fieldInfo, IAttributeFilter filter = null)` |
| `public IMethodData CreateMethodData(MethodInfo methodInfo, IAttributeFilter filter = null)` |
| `public IPropertyData CreatePropertyData(PropertyInfo propertyInfo, IAttributeFilter filter = null)` |
| `public ITypeData CreateTypeData(Type type, IAnalysisDataFactory factory = null, IAttributeFilter filter = null)` |
| `public Type GetType()` |
| `public virtual bool Equals(object obj)` |
| `public virtual int GetHashCode()` |
| `public virtual string ToString()` |
| `protected object MemberwiseClone()` |
| `protected virtual void Finalize()` |

### 声明的普通方法

| 普通方法名称 | 注释 |
| :--- | :--- | 
| `public IConstructorData CreateConstructorData` | 创建构造函数数据 |
| `public IEventData CreateEventData` | 创建事件数据 |
| `public IFieldData CreateFieldData` | 创建字段数据 |
| `public IMethodData CreateMethodData` | 创建方法数据 |
| `public IPropertyData CreatePropertyData` | 创建属性数据 |
| `public ITypeData CreateTypeData` | 创建类型数据 |

### 继承的普通方法

| 普通方法名称 | 注释 | 声明方法的类 |
| :--- | :--- | :--- |
| `public Type GetType` |  | `System.Object` |
| `public virtual bool Equals` |  | `System.Object` |
| `public virtual int GetHashCode` |  | `System.Object` |
| `public virtual string ToString` |  | `System.Object` |
| `protected object MemberwiseClone` |  | `System.Object` |
| `protected virtual void Finalize` |  | `System.Object` |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
