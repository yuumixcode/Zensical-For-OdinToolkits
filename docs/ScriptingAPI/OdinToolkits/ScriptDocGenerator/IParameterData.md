# `IParameterData`

## 介绍

- 种类: `interface`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator`

``` csharp
public interface IParameterData
```

### 注释

- 参数信息解析数据接口

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
