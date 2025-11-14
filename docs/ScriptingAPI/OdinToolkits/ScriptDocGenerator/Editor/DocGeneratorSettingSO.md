# `DocGeneratorSettingSO`

## 介绍

- 种类: `abstract class`
- 所在程序集: `Yuumix.OdinToolkits.Editor`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator.Editor`

``` csharp
public abstract class DocGeneratorSettingSO : UnityEngine.ScriptableObject, 
Yuumix.OdinToolkits.Core.IOdinToolkitsEditorReset
```

### 注释

- 文档生成器设置抽象类

## 方法

### 所有方法签名总览

| 方法完整签名 |
| :--- | 
| `public Type GetType()` |
| `public abstract string GetGeneratedDoc(ITypeData data)` |
| `public int GetInstanceID()` |
| `public override bool Equals(object other)` |
| `public override int GetHashCode()` |
| `public override string ToString()` |
| `public void EditorReset()` |
| `protected object MemberwiseClone()` |
| `protected virtual void Finalize()` |
| `public void SetDirty()` |

### 声明的普通方法

| 普通方法名称 | 注释 |
| :--- | :--- | 
| `public abstract string GetGeneratedDoc` | 通过 TypeData 实例对象，生成文档内容。注意：不要在此方法中添加增量生成标识符 |
| `public void EditorReset` | 重置文档生成器设置 |

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
| `public bool customizeDocFileExtensionName;` | 是否自定义文档扩展名 |
| `public bool generateIdentifier;` | 是否自动生成增量标识符 |
| `public bool generateNamespaceFolder;` | 是否按命名空间生成文件夹 |
| `public string docFileExtensionName;` | 设置的文档扩展名 |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
