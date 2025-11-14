# `IAnalysisDataFactory`

## 介绍

- 种类: `interface`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator`

``` csharp
public interface IAnalysisDataFactory
```

### 注释

- 解析数据工厂接口，自定义扩展解析数据工厂

## 方法

### 所有方法签名总览

| 方法完整签名 |
| :--- | 
| `public abstract IConstructorData CreateConstructorData(ConstructorInfo constructorInfo, IAttributeFilter filter = null)` |
| `public abstract IEventData CreateEventData(EventInfo eventInfo, IAttributeFilter filter = null)` |
| `public abstract IFieldData CreateFieldData(FieldInfo fieldInfo, IAttributeFilter filter = null)` |
| `public abstract IMethodData CreateMethodData(MethodInfo methodInfo, IAttributeFilter filter = null)` |
| `public abstract IPropertyData CreatePropertyData(PropertyInfo propertyInfo, IAttributeFilter filter = null)` |
| `public abstract ITypeData CreateTypeData(Type type, IAnalysisDataFactory factory = null, IAttributeFilter filter = null)` |

### 声明的普通方法

| 普通方法名称 | 注释 |
| :--- | :--- | 
| `public abstract IConstructorData CreateConstructorData` | 创建构造函数数据 |
| `public abstract IEventData CreateEventData` | 创建事件数据 |
| `public abstract IFieldData CreateFieldData` | 创建字段数据 |
| `public abstract IMethodData CreateMethodData` | 创建方法数据 |
| `public abstract IPropertyData CreatePropertyData` | 创建属性数据 |
| `public abstract ITypeData CreateTypeData` | 创建类型数据 |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
