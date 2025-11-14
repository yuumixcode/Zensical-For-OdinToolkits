# `TypeAnalyzerStaticExtensions`

## 介绍

- 种类: `static class`
- 所在程序集: `Yuumix.OdinToolkits.Runtime`
- 所在命名空间: `Yuumix.OdinToolkits.ScriptDocGenerator`

``` csharp
[Extension]
public static class TypeAnalyzerStaticExtensions
```

### 注释

- 类型分析器静态扩展类，统一管理类型分析器有关的静态扩展方法

## 方法

### 所有方法签名总览

| 方法完整签名 |
| :--- | 
| `public Type GetType()` |
| `public virtual bool Equals(object obj)` |
| `public virtual int GetHashCode()` |
| `public virtual string ToString()` |
| `[Ext] public static AccessModifierType GetEventAccessModifierType(this EventInfo eventInfo)` |
| `[Ext] public static AccessModifierType GetFieldAccessModifier(this FieldInfo fieldInfo)` |
| `[Ext] public static AccessModifierType GetMethodAccessModifierType(this MethodBase method)` |
| `[Ext] public static AccessModifierType GetPropertyAccessModifierType(this PropertyInfo propertyInfo)` |
| `[Ext] public static AccessModifierType GetTypeAccessModifier(this Type type)` |
| `[Ext] public static FieldInfo[] GetUserDefinedFields(this Type type)` |
| `[Ext] public static TypeCategory GetTypeCategory(this Type type)` |
| `[Ext] public static bool IsAbstractOrInterface(this Type type)` |
| `[Ext] public static bool IsApiMember(this IDerivedMemberData derivedMemberData)` |
| `[Ext] public static bool IsAsyncMethod(this MethodBase methodBase)` |
| `[Ext] public static bool IsDelegate(this Type type)` |
| `[Ext] public static bool IsDynamicField(this FieldInfo fieldInfo)` |
| `[Ext] public static bool IsFromInheritance(this MemberInfo member)` |
| `[Ext] public static bool IsFromInterfaceImplementMethod(this MethodBase method)` |
| `[Ext] public static bool IsInheritedOverrideFromAncestor(this MethodInfo method, Type currentType)` |
| `[Ext] public static bool IsOperatorMethod(this MethodBase methodInfo)` |
| `[Ext] public static bool IsOverrideMethod(this MethodInfo methodInfo)` |
| `[Ext] public static bool IsRecord(this Type type)` |
| `[Ext] public static bool IsRecordStruct(this Type type)` |
| `[Ext] public static bool IsReferenceTypeExcludeString(this Type type)` |
| `[Ext] public static bool IsStaticProperty(this PropertyInfo propertyInfo)` |
| `[Ext] public static bool TryAsIMemberData(this IDerivedMemberData derivedMemberData, out ref IMemberData memberData)` |
| `[Ext] public static bool TryGetFieldCustomDefaultValue(this FieldInfo fieldInfo, out ref Object defaultValue)` |
| `[Ext] public static bool TryGetPropertyCustomDefaultValue(this PropertyInfo propertyInfo, out ref Object defaultValue)` |
| `[Ext] public static string GetAttributesDeclarationWithMultiLine(this MemberInfo member, IAttributeFilter filter = null)` |
| `[Ext] public static string GetMethodNameAndParameters(this MethodBase method)` |
| `[Ext] public static string GetParametersNameWithDefaultValue(this MethodBase method)` |
| `[Ext] public static string GetReadableTypeName(this Type type, bool useFullName = false)` |
| `[Ext] public static string GetTypeDeclaration(this Type type)` |
| `[Ext] public static string[] GetInheritanceChain(this Type type)` |
| `[Ext] public static string[] GetInterfaceArray(this Type type)` |
| `[Ext] public static string[] GetReferenceLinks(this Type type)` |
| `protected object MemberwiseClone()` |
| `protected virtual void Finalize()` |

### 声明的普通方法

| 普通方法名称 | 注释 |
| :--- | :--- | 
| `[Ext] public static AccessModifierType GetEventAccessModifierType` | 获取事件的访问修饰符类型 |
| `[Ext] public static AccessModifierType GetFieldAccessModifier` | 获取字段访问修饰符 |
| `[Ext] public static AccessModifierType GetMethodAccessModifierType` | 获取方法的访问修饰符类型 |
| `[Ext] public static AccessModifierType GetPropertyAccessModifierType` | 获取属性的访问修饰符类型 |
| `[Ext] public static AccessModifierType GetTypeAccessModifier` | 获取类型的访问修饰符 |
| `[Ext] public static FieldInfo[] GetUserDefinedFields` | 获取开发者声明的字段，剔除自动属性生成的字段 |
| `[Ext] public static TypeCategory GetTypeCategory` | 获取类型的种类 |
| `[Ext] public static bool IsAbstractOrInterface` | 判断一个类型是否为抽象类或接口 |
| `[Ext] public static bool IsApiMember` | 判断是否为 API 成员，返回 true 表示是 API 成员，返回 false 表示不是。API 成员指的是公共成员或受保护成员。 |
| `[Ext] public static bool IsAsyncMethod` | 判断方法是否是异步方法 |
| `[Ext] public static bool IsDelegate` | 判断指定类型是否为委托类型 |
| `[Ext] public static bool IsDynamicField` | 判断是否为动态字段 |
| `[Ext] public static bool IsFromInheritance` | 判断成员是否从继承中获取，这里的成员不包括 Type 类型 |
| `[Ext] public static bool IsFromInterfaceImplementMethod` | 判断是否为接口的实现方法 |
| `[Ext] public static bool IsInheritedOverrideFromAncestor` | 判断方法是否为从祖先类继承的重写方法，重写声明不是在当前类中 |
| `[Ext] public static bool IsOperatorMethod` | 判断方法是否是运算符方法 |
| `[Ext] public static bool IsOverrideMethod` | 方法是否具有 override 的特性 |
| `[Ext] public static bool IsRecord` | 判断指定类型是否为 record（包括 record class 和 record struct） |
| `[Ext] public static bool IsRecordStruct` | 判断类型是否为 record struct（值类型 record） |
| `[Ext] public static bool IsReferenceTypeExcludeString` | 判断一个类型是否为非字符串的引用类型（非值类型） |
| `[Ext] public static bool IsStaticProperty` | 判断是否为静态属性 |
| `[Ext] public static bool TryAsIMemberData` | 将 IDerivedMemberData 转换为 IMemberData，转换成功返回 true，转换失败返回 false |
| `[Ext] public static bool TryGetFieldCustomDefaultValue` | 获取字段的自定义默认值，不能获取到值则返回 null。只获取静态字段和常量字段的默认值。 |
| `[Ext] public static bool TryGetPropertyCustomDefaultValue` | 获取属性的自定义默认值，不能获取到值则返回 null，只获取静态属性的默认值。 |
| `[Ext] public static string GetAttributesDeclarationWithMultiLine` | 获取特性声明字符串，多行显示 |
| `[Ext] public static string GetMethodNameAndParameters` | 获取方法名称和参数列表，不包含返回值和修饰符 |
| `[Ext] public static string GetParametersNameWithDefaultValue` | 获取方法的参数签名，包含默认值 |
| `[Ext] public static string GetReadableTypeName` | 将反射获取到的系统类型名称转换为人类可读的 C# 风格类型名称 |
| `[Ext] public static string GetTypeDeclaration` | 获取类型声明字符串 |
| `[Ext] public static string[] GetInheritanceChain` | 获取一个类型的继承链，不包括接口 |
| `[Ext] public static string[] GetInterfaceArray` | 获取一个类型继承的所有接口 |
| `[Ext] public static string[] GetReferenceLinks` | 获取一个数组，内容是所有的 ReferenceLinkURL 特性中的网页链接字符串 |

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
