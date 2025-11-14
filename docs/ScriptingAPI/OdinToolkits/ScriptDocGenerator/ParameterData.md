# `ParameterData`

## 介绍

- 种类: `class`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator`

``` csharp
[Serializable]
public class ParameterData : Yuumix.OdinToolkits.ScriptDocGenerator.IParameterData
```

### 注释

- 参数信息解析数据

## 构造方法

| 构造方法签名 [仅包含公共实例方法] | 注释 |
| :--- | :--- |
| `public ParameterData(ParameterInfo parameterInfo)` |  |

## 方法

### 所有方法签名总览

| 方法完整签名 |
| :--- | 
| `public Type GetType()` |
| `public string GetFormattedString()` |
| `public virtual bool Equals(object obj)` |
| `public virtual int GetHashCode()` |
| `public virtual string ToString()` |
| `protected object MemberwiseClone()` |
| `protected virtual void Finalize()` |

### 声明的普通方法

| 普通方法名称 | 注释 |
| :--- | :--- | 
| `public string GetFormattedString` | 生成格式化的参数字符串 |

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
| `public ParameterDirection Direction { get; }` | 参数方向（in/out/ref） |
| `public Type ParameterType { get; }` | 参数类型 |
| `public bool HasDefaultValue { get; }` | 是否有默认值 |
| `public bool IsParams { get; }` | 是否为 params 参数 |
| `public object DefaultValue { get; }` | 默认值 |
| `public string Name { get; }` | 参数名称 |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
