# `TypesCacheSO`

## 介绍

- 种类: `class`
- 所在程序集: `Yuumix.OdinToolkits.Editor`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator.Editor`

``` csharp
public class TypesCacheSO : Sirenix.OdinInspector.SerializedScriptableObject, 
UnityEngine.ISerializationCallbackReceiver
```

### 注释

- 存储 Type 的资源文件，提供给脚本文档生成工具复用，用户无需每次重新选择 Type

## 构造方法

| 构造方法签名 [仅包含公共实例方法] | 注释 |
| :--- | :--- |
| `public TypesCacheSO()` |  |

## 方法

### 所有方法签名总览

| 方法完整签名 |
| :--- | 
| `public Type GetType()` |
| `public int GetInstanceID()` |
| `public override bool Equals(object other)` |
| `public override int GetHashCode()` |
| `public override string ToString()` |
| `protected object MemberwiseClone()` |
| `protected virtual void Finalize()` |
| `protected virtual void OnAfterDeserialize()` |
| `protected virtual void OnBeforeSerialize()` |
| `public void SetDirty()` |

### 继承的普通方法

| 普通方法名称 | 注释 | 声明方法的类 |
| :--- | :--- | :--- |
| `public Type GetType` |  | `System.Object` |
| `public int GetInstanceID` |  | `UnityEngine.Object` |
| `public override bool Equals` |  | `UnityEngine.Object` |
| `public override int GetHashCode` |  | `UnityEngine.Object` |
| `public override string ToString` |  | `UnityEngine.Object` |
| `protected object MemberwiseClone` |  | `System.Object` |
| `protected virtual void Finalize` |  | `System.Object` |
| `protected virtual void OnAfterDeserialize` |  | `Sirenix.OdinInspector.SerializedScriptableObject` |
| `protected virtual void OnBeforeSerialize` |  | `Sirenix.OdinInspector.SerializedScriptableObject` |
| `public void SetDirty` |  | `UnityEngine.ScriptableObject` |

## 属性

### 继承的属性

| 属性签名 | 注释 | 声明属性的类 | 
| :--- | :--- | :--- |
| `public HideFlags hideFlags { get; set; }` |  | `UnityEngine.Object` |
| `public string name { get; set; }` |  | `UnityEngine.Object` |

## 字段

### 声明的普通字段

| 字段名称 | 注释 | 
| :--- | :--- | 
| `public List<Type> Types;` | 存储 Type 的列表 |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
