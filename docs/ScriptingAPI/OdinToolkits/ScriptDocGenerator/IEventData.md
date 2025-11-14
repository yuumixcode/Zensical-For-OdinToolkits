# `IEventData`

## 介绍

- 种类: `interface`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator`

``` csharp
public interface IEventData : Yuumix.OdinToolkits.ScriptDocGenerator.IDerivedMemberData
```

### 注释

- 事件数据接口，继承自 IDerivedMemberData

## 属性

### 声明的属性

| 属性签名 | 注释 |
| :--- | :--- |
| `public Type EventType { get; }` | 事件类型 |
| `public string EventTypeFullName { get; }` | 事件类型的完整名称，包括命名空间 |
| `public string EventTypeName { get; }` | 事件类型名称 |

## 额外说明

> 首个 `## 额外说明` 是增量生成文档标识符，请勿修改标题级别和内容！本文档由 [`Odin Toolkits For Unity`](https://github.com/yuumixcode/OdinToolkits-For-Unity) 辅助生成。
