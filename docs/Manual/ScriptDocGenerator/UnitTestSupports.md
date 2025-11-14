---
icon: lucide/test-tube
comments: true
tags:
  - Script Doc Generator
  - UnitTest
---

# Script Doc Generator 单元测试

## 概述

TypeAnalyzer 类型解析器单元测试，共 143 个测试用例，其中 `TypeData` 14 个，`ConstructorData` 2 个，`EventData` 6 个，`MethodData` 23 个，`PropertyData`  13 个，`FieldData` 85 个，基本覆盖大部分类型解析情况。

## `TypeData`

!!! note "`TypeData` 单元测试须知"

    TypeData 一共有 14 个测试用例。
    
    类型解析支持包括 `class`、`struct`、`enum`、`interface`、`delegate`、`nested type`、`record`。

???+ success "14 个测试用例"

    ``` csharp title="No.1 - ITestInterface 源码声明"
    public interface ITestInterface { }
    ``` 

    ``` markdown title="解析结果"
    public interface ITestInterface
    ```
    ``` csharp title="No.2 - TestAbstractClass 源码声明"
    public abstract class TestAbstractClass { }
    ``` 
    ``` markdown title="解析结果"
    public abstract class TestAbstractClass
    ```
    ``` csharp title="No.3 - TestSealedClass 源码声明"
    public sealed class TestSealedClass { }
    ``` 
    ``` markdown title="解析结果"
    public sealed class TestSealedClass
    ```
    ``` csharp title="No.4 - TestStaticClass 源码声明"
    public static class TestStaticClass { }
    ``` 
    ``` markdown title="解析结果"
    public static class TestStaticClass
    ```

    ``` csharp title="No.5 - TestGenericClass<T> 源码声明"
    public class TestGenericClass<T> where T : class
    {
        public T Owner;
    }
    ```
    ``` markdown title="解析结果"
    public class TestGenericClass<T> where T : class
    ```
    ``` csharp title="No.6 - UnitTestTypes 源码声明"
    public abstract class TestAbstractClass { }

    public class UnitTestTypes : TestAbstractClass { }
    ```
    ``` markdown title="解析结果"
    public class UnitTestTypes : Yuumix.OdinToolkits.Tests.Editor.TestAbstractClass
    ```
    
    ``` csharp title="No.7 - TestClassWithAttribute 源码声明"
    [Summary("支持解析特性")]
    [ReferenceLinkURL("https://learn.microsoft.com/en-us/dotnet/api/system.object?view=net-9.0")]
    public class TestClassWithAttribute { }
    ``` 

    ``` markdown title="解析结果"
    [ReferenceLinkURL("https://learn.microsoft.com/en-us/dotnet/api/system.object?view=net-9.0")]
    public class TestClassWithAttribute
    ```

    ``` csharp title="No.8 - TestDelegate 源码声明"
    public delegate void TestDelegate();
    ``` 
    ``` markdown title="解析结果"
    public delegate void TestDelegate()
    ``` 

    ``` csharp title="No.9 - TestDelegateHasParameters 源码声明"
    public delegate void TestDelegateHasParameters(int a, List<string> b);
    ``` 
    ``` markdown title="解析结果"
    public delegate void TestDelegateHasParameters(int a, List<string> b)
    ``` 

    ``` csharp title="No.10 - TestDelegateHasReturnType 源码声明"
    public delegate bool TestDelegateHasReturnType(float a, int[] b);
    ``` 
    ``` markdown title="解析结果"
    public delegate bool TestDelegateHasReturnType(float a, int[] b)
    ``` 
    ``` csharp title="No.11 - TestRecord 源码声明"
    public record TestRecord;
    ``` 
    ``` markdown title="解析结果"
    [NullableContext(NullableContextOptions.flow)]
    [Nullable(NullableContextOptions.flow)]
    public record TestRecord : System.IEquatable<TestRecord>
    ```

    ``` csharp title="No.12 - TestStruct 源码声明"
    public struct TestStruct { }
    ``` 
    ``` markdown title="解析结果"
    public struct TestStruct : System.ValueType
    ``` 

    ``` csharp title="No.13 - ScriptDocGeneratorTestEnum 源码声明"
    public enum ScriptDocGeneratorTestEnum
    {
        Value1,
        Value2,
        Value3
    }
    ```

    ``` markdown title="解析结果"
    public enum ScriptDocGeneratorTestEnum : System.Enum, 
    System.IFormattable, 
    System.IComparable, 
    System.IConvertible
    ```

    ``` csharp title="No.14 - NestedClass 源码声明"
    public class UnitTestTypes
    {
        class NestedClass { }
    }
    ```

    ``` markdown title="解析结果"
    private class UnitTestTypes.NestedClass
    ```
---

## `ConstructorData`

!!! note "`ConstructorData` 单元测试须知"

    `ConstructorData` 一共有 2 个测试用例。

    只解析公共实例构造方法。

    构造方法属于特殊的方法成员，部分可参考 `MethodData` 解析.

???+ success "2 个测试用例"

    ``` csharp title="源码声明"
    public abstract class TestClassAbstract
    {
        protected TestClassAbstract(int a) { }

        protected TestClassAbstract() { }
    }

    public class TestClass : TestClassAbstract
    {
        public TestClass() { }

        public TestClass(bool b, int a) : base(a) { }

        TestClass(string s) { }
    }
    ```
    
    | 解析后的构造方法完整签名 |
    | :---------- |
    | `public UnitTestConstructorsCommon.TestClass()` |
    | `public UnitTestConstructorsCommon.TestClass(bool b, int a)` |

---

## `EventData`

!!! note "`EventData` 单元测试须知"

    `EventData` 一共有 6 个测试用例。

    只解析使用 `event` 关键字声明的事件，简单委托属于字段。
    
    事件相当于在字段的声明中添加了 `event` 关键字，部分可参考 `FieldData` 解析.

???+ success "6 个测试用例"

    ``` csharp
    public event Action ActionEvent;
    public event Action<int, string> ActionWithParamsEvent;
    public event Func<int, string, bool> FuncWithParamsEvent;
    public event Predicate<int> PredicateEvent;
    public event Comparison<string> ComparisonEvent;
    public static event Action<bool> StaticActionEvent;
    ```
    
    | 解析后的事件完整签名 |
    | :---------- |
    | `public event Action ActionEvent;` |
    | `public event Action<int, string> ActionWithParamsEvent;` |
    | `public event Func<int, string, bool> FuncWithParamsEvent;` |
    | `public event Predicate<int> PredicateEvent;` |
    | `public event Comparison<string> ComparisonEvent;` |
    | `public static event Action<bool> StaticActionEvent;` |

---

## `MethodData`

!!! note "`MethodData` 单元测试须知"

    `MethodData` 一共有 23 个测试用例。
    
    `MethodData` 不包括构造函数，仅包括普通方法。

### 非继承方法测试

???+ success "11 个测试用例"

    ``` csharp title="No.1 - EmptyParamMethod 方法源码声明"
    public void EmptyParamMethod()
    {
        Debug.Log("EmptyParamMethod");
    }
    ```

    ``` markdown title="解析结果"
    无参数短方法签名：public void EmptyParamMethod
    完整方法签名：public void EmptyParamMethod()
    ```

    ``` csharp title="No.2 - OneIntParamMethod 方法源码声明"
    public void OneIntParamMethod(int param)
    {
        Debug.Log(param);
    }
    ```

    ``` markdown title="解析结果"
    无参数短方法签名：public void OneIntParamMethod
    完整方法签名：public void OneIntParamMethod(int param)
    ```

    ``` csharp title="No.3 - TwoParamMethod 方法源码声明"
    public void TwoParamMethod(string param1, bool param2)
    {
        Debug.Log(param1 + param2);
    }
    ```

    ``` markdown title="解析结果"
    无参数短方法签名：public void TwoParamMethod
    完整方法签名：public void TwoParamMethod(string param1, bool param2)
    ```

    ``` csharp title="No.4 - GenericMethod 方法源码声明"
    public void GenericMethod<T>(T param)
    {
        Debug.Log(param);
    }
    ```

    ``` markdown title="解析结果"
    无参数短方法签名：public void GenericMethod<T>
    完整方法签名：public void GenericMethod<T>(T param)
    ```

    ``` csharp title="No.5 - TwoParamsGenericMethod 方法源码声明"
    public void TwoParamsGenericMethod<T, T1>(T param1, List<T1> param2)
    {
        Debug.Log(param1);
        Debug.Log(param2);
    }
    ```

    ``` markdown title="解析结果"
    无参数短方法签名：public void TwoParamsGenericMethod<T, T1>
    完整方法签名：public void TwoParamsGenericMethod<T, T1>(T param1, List<T1> param2)
    ```

    ``` csharp title="No.6 - IndefiniteParamsMethod 方法源码声明"
    public void IndefiniteParamsMethod(string param1, params bool[] param2)
    {
        Debug.Log(param1 + param2);
    }
    ```
 
    ``` markdown title="解析结果"
    无参数短方法签名：public void IndefiniteParamsMethod
    完整方法签名：public void IndefiniteParamsMethod(string param1, params bool[] param2)
    ```
 
    ``` csharp title="No.7 - HasReturnStringMethod 方法源码声明"
    public string HasReturnStringMethod(float param1)
    {
        Debug.Log(param1);
        return "HasReturnStringMethod";
    }
    ```

    ``` markdown title="解析结果"
    无参数短方法签名：public string HasReturnStringMethod
    完整方法签名：public string HasReturnStringMethod(float param1)
    ```

    ``` csharp title="No.8 - StaticMethod 方法源码声明"
    public static void StaticMethod()
    {
        Debug.Log("StaticMethod");
    }
    ```

    ``` markdown title="解析结果"
    无参数短方法签名：public static void StaticMethod
    完整方法签名：public static void StaticMethod()
    ```

    ``` csharp title="No.9 - HasReturnBoolMethodWithDefault 方法源码声明"
    public static bool HasReturnBoolMethodWithDefault(float param1 = 5f, bool param2 = false,
        string param3 = "Hello World", int param4 = 0,
        ScriptDocGeneratorTestEnum param5 = ScriptDocGeneratorTestEnum.Value3)
    {
        Debug.Log(param3 + param4);
        return true;
    }
    ```

    ``` markdown title="解析结果"
    无参数短方法签名：public static bool HasReturnBoolMethodWithDefault
    完整方法签名：public static bool HasReturnBoolMethodWithDefault(float param1 = 5f, 
    bool param2 = false, string param3 = "Hello World", int param4 = 0,
    ScriptDocGeneratorTestEnum param5 = ScriptDocGeneratorTestEnum.Value3)
    ```

    ``` csharp title="No.10 - AsyncMethod 方法源码声明"
    public static async Task AsyncMethod()
    {
        await Task.Delay(1);
    }
    ```

    ``` markdown title="解析结果"
    无参数短方法签名：public static async Task AsyncMethod
    完整方法签名：public static async Task AsyncMethod()
    ```

    ``` csharp title="No.11 - AsyncMethodWithReturnValue 方法源码声明"
    public async Task<int> AsyncMethodWithReturnValue()
    {
        await Task.Delay(1);
        return 42;
    }
    ```

    ``` markdown title="解析结果"
    无参数短方法签名：public async Task<int> AsyncMethodWithReturnValue
    完整方法签名：public async Task<int> AsyncMethodWithReturnValue()
    ```

### 继承、接口方法测试

???+ success "4 个测试用例"

    ``` csharp
    public interface IInterface
    {
        void InterfaceMethod();
    }
    
    public abstract class TestClassAbstract
    {
        public virtual void OverrideVirtualMethod() { }
        public abstract void OverrideAbstractMethod();
    }
    
    public class TestClassImplement : TestClassAbstract, IInterface
    {
        public override void OverrideAbstractMethod() { }
        public override void OverrideVirtualMethod() { 
        public void InterfaceMethod()
        {
            Debug.Log("InterfaceMethod");
        }
    }
    ```
    
    | 解析后的方法完整签名 |
    | :---------- |
    | `public virtual void OverrideVirtualMethod()` |
    | `public override void OverrideAbstractMethod()` |
    | `public override void OverrideVirtualMethod()` |
    | `public void InterfaceMethod()` |

### 运算符方法测试

???+ success "7 个测试用例"

    ``` csharp
    public static TestClass operator +(TestClass a, TestClass b) => new TestClass();
    public static TestClass operator -(TestClass a, TestClass b) => new TestClass();
    public static TestClass operator *(TestClass a, TestClass b) => new TestClass();
    public static TestClass operator /(TestClass a, TestClass b) => new TestClass();
    public static TestClass operator %(TestClass a, TestClass b) => new TestClass();
    public static implicit operator TestClass(int a) => new TestClass();
    public static explicit operator float(TestClass a) => 1f;
    ```
    
    | 解析后的运算符方法完整签名 |
    | :---------- |
    | `public static UnitTestMethodsOperator.TestClass operator +(UnitTestMethodsOperator.TestClass a, UnitTestMethodsOperator.TestClass b)` |
    | `public static UnitTestMethodsOperator.TestClass operator -(UnitTestMethodsOperator.TestClass a, UnitTestMethodsOperator.TestClass b)` |
    | `public static UnitTestMethodsOperator.TestClass operator *(UnitTestMethodsOperator.TestClass a, UnitTestMethodsOperator.TestClass b)` |
    | `public static UnitTestMethodsOperator.TestClass operator /(UnitTestMethodsOperator.TestClass a, UnitTestMethodsOperator.TestClass b)` |
    | `public static UnitTestMethodsOperator.TestClass operator %(UnitTestMethodsOperator.TestClass a, UnitTestMethodsOperator.TestClass b)` |
    | `public static implicit operator UnitTestMethodsOperator.TestClass(int a)` |
    | `public static explicit operator float(UnitTestMethodsOperator.TestClass a)` |

### 静态扩展方法测试

???+ success "1 个测试用例"

    ``` csharp
    public static class TestStaticExtension
    {
        public static int StaticMethod(this UnitTestMethodsStaticExtension.TestClass t) => 0;
    }
    ```
    
    | 解析后的静态扩展方法完整签名 |
    | :---------- |
    | `public static int StaticMethod(this UnitTestMethodsStaticExtension.TestClass t)` |

---

## `PropertyData`

!!! note "`PropertyData` 测试须知"

    `PropertyData` 一共有 13 个测试用例。
    
    属性相当于在字段的声明中添加了 `get` 和 `set` 访问器，部分属性参考字段解析。
    
    `PropertyData` 自定义默认值解析仅支持静态属性。

!!! danger "不支持属性的访问修饰符与 get 和 set 访问器都不同的情况"

    ``` csharp
    protected bool BoolPropertyPrivateGetPublicSet { private get; set; }
    ```

### 实例和静态基础类型属性单元测试

???+ success "8 个测试用例"

    ``` csharp
    public int IntPropertyPublicGetPublicSet { get; set; }
    public string StringPropertyPublicGetInternalSet { get; internal set; }
    public float FloatPropertyPublicGetProtectedSet { get; protected set; }
    public bool BoolPropertyPublicGetPrivateSet { get; private set; }
    public int IntPropertyInternalGetPublicSet { internal get; set; }
    public float FloatPropertyProtectedGetPublicSet { protected get; set; }
    public bool BoolPropertyPrivateGetPublicSet { private get; set; }
    public static int StaticIntPropertyPublicGetPublicSet { get; set; }
    ```
    
    | 解析后的属性完整签名 |
    | :---------- |
    | `public int IntPropertyPublicGetPublicSet { get; set; }` |
    | `public string StringPropertyPublicGetInternalSet { get; internal set; }` |
    | `public float FloatPropertyPublicGetProtectedSet { get; protected set; }` |
    | `public bool BoolPropertyPublicGetPrivateSet { get; private set; }` |
    | `public int IntPropertyInternalGetPublicSet { internal get; set; }` |
    | `public float FloatPropertyProtectedGetPublicSet { protected get; set; }` |
    | `public bool BoolPropertyPrivateGetPublicSet { private get; set; }` |
    | `public static int StaticIntPropertyPublicGetPublicSet { get; set; }` |

### 自定义默认值属性单元测试

???+ success "5 个测试用例"

    ``` csharp
    public static int StaticIntPropertyWithDefaultValue { get; set; } = 1;
    public static float StaticFloatPropertyWithDefaultValue { get; set; } = 1f;
    public static bool StaticBoolPropertyWithDefaultValue { get; set; } = true;
    public static string StaticStringPropertyWithDefaultValue { get; set; } = "Hello";
    public static TestEnum StaticEnumPropertyWithDefaultValue { get; set; } = TestEnum.B;
    ```
    
    | 解析后的属性完整签名 |
    | :---------- |
    | `public static int StaticIntPropertyWithDefaultValue { get; set; } = 1;` |
    | `public static float StaticFloatPropertyWithDefaultValue { get; set; } = 1f;` |
    | `public static bool StaticBoolPropertyWithDefaultValue { get; set; } = true;` |
    | `public static string StaticStringPropertyWithDefaultValue { get; set; } = "Hello";` |
    | `public static TestEnum StaticEnumPropertyWithDefaultValue { get; set; } = TestEnum.B;` |

---

## `FieldData`

!!! note "`FieldData` 单元测试须知"

    `FieldData` 一共有 85 个测试用例。

### 不同访问修饰符和复合关键字

???+ success "6 个测试用例"

    ``` csharp
    int _privateField;
    internal int InternalField;
    private protected int PrivateProtectedField;
    protected int ProtectedField;
    protected internal int ProtectedInternalField;
    public int PublicField;
    ```
    
    | 解析后的字段完整签名 |
    | :----------|
    | `private int _privateField;` |
    | `internal int InternalField;` |
    | `private protected int PrivateProtectedField;` |
    | `protected int ProtectedField;` |
    | `protected internal int ProtectedInternalField;` |
    | `public int PublicField;` |

???+ success "4 个测试用例"

    ``` csharp
    public const int CONST_FIELD = 42;
    public static readonly int StaticReadOnlyField;
    public static int StaticField;
    public readonly int ReadOnlyField;
    ```
    
    | 解析后的字段完整签名 |
    | :---------- |
    | `public const int CONST_FIELD = 42;` |
    | `public static readonly int StaticReadOnlyField;` |
    | `public static int StaticField;` |
    | `public readonly int ReadOnlyField;` |

### 常量字段

!!! note "常量字段单元测试须知"

    1. 常量字段自定义设置默认值的解析测试只包括基础值类型、枚举类型、特殊的 string 引用类型。
       
    2. 常量字段部分类型解析限制如下：
    
        a. 长整型常量字段，long，解析后的字段数据设置为以 'L' 字符结尾。
    
        b. 无符号长整型常量字段，ulong，解析后的字段数据设置为以 'ul' 字符结尾。
    
        c. 无符号整型常量字段，uint，解析后的字段数据设置为以 'u' 字符结尾。
    
        d. 双精度浮点型常量字段，double，解析后的字段数据设置为以 'd' 字符结尾，且为了保证精准，位数不能超过 15 位，这里的 15 位是指有效数字的位数，不只是小数点后的位数。
    
        e. 十进制浮点型常量字段，decimal，解析后的字段数据设置为以 'm' 字符结尾。
    
        f. 嵌套类枚举常量字段，UnitTestFieldsIsConstantWithDefaultValue.TestEnum，解析后的字段数据的字段类型会显示嵌套路径。

???+ success "16 个测试用例"

    ``` csharp
    public const string STRING_CONST_FIELD = "Hello, World!";
    public const int INT_CONST_FIELD = 2147483647;
    public const float FLOAT_CONST_FIELD = 3.14159f;
    public const bool BOOLEAN_CONST_FIELD = true;
    public const char CHAR_CONST_FIELD = 'A';
    public const byte BYTE_CONST_FIELD = 255;
    public const sbyte SBYTE_CONST_FIELD = -128;
    public const short SHORT_CONST_FIELD = 32767;
    public const ushort USHORT_CONST_FIELD = 65535;
    ```
    
    | 解析后的字段完整签名 |
    | :---------- |
    | `public const string STRING_CONST_FIELD = "Hello, World!";` |
    | `public const int INT_CONST_FIELD = 2147483647;` |
    | `public const float FLOAT_CONST_FIELD = 3.14159f;` |
    | `public const bool BOOLEAN_CONST_FIELD = true;` |
    | `public const char CHAR_CONST_FIELD = 'A';` |
    | `public const byte BYTE_CONST_FIELD = 255;` |
    | `public const sbyte SBYTE_CONST_FIELD = -128;` |
    | `public const short SHORT_CONST_FIELD = 32767;` |
    | `public const ushort USHORT_CONST_FIELD = 65535;` |

    ``` csharp
    public const long LONG_CONST_FIELD = 9223372036854775807L;
    public const ulong ULONG_CONST_FIELD = 18446744073709551615ul;
    public const uint UINT_CONST_FIELD = 4294967295u;
    public const double DOUBLE_CONST_FIELD = 2.71828182845904d; 
    public const decimal DECIMAL_CONST_FIELD = 123.456m;
    public const ScriptDocGeneratorTestEnum ENUM_CONST_FIELD = ScriptDocGeneratorTestEnum.Value1;

    // 嵌套类枚举常量字段，UnitTestFieldsIsConstantWithDefaultValue.TestEnum
    public const TestEnum NESTED_ENUM_CONST_FIELD = TestEnum.Value3;
    ```
    
    | 解析后的字段完整签名 |
    | :---------- |
    | `public const long LONG_CONST_FIELD = 9223372036854775807L;` |
    | `public const ulong ULONG_CONST_FIELD = 18446744073709551615ul;` |
    | `public const uint UINT_CONST_FIELD = 4294967295u;` |
    | `public const double DOUBLE_CONST_FIELD = 2.71828182845904d;` |
    | `public const decimal DECIMAL_CONST_FIELD = 123.456m;` |
    | `public const ScriptDocGeneratorTestEnum ENUM_CONST_FIELD = ScriptDocGeneratorTestEnum.Value1;` |
    | `public const UnitTestFieldsIsConstantWithDefaultValue.TestEnum NESTED_ENUM_CONST_FIELD = TestEnum.Value3;` |

### 静态字段

!!! note "静态字段单元测试须知"

    1. 静态字段自定义设置默认值的解析测试只包括基础值类型、枚举类型、特殊的 string 引用类型。
        
    2. 静态字段部分类型解析限制和常量一致

???+ success "16 个测试用例"

    ``` csharp
    public static string StringStaticField = "Hello, World!";
    public static int INTStaticField = 2147483647;
    public static float FloatStaticField = 3.14159f;
    public static bool BooleanStaticField = true;
    public static char CharStaticField = 'A';
    public static byte ByteStaticField = 255;
    public static sbyte SbyteStaticField = -128;
    public static short ShortStaticField = 32767;
    public static ushort UshortStaticField = 65535;
    ```
    
    | 解析后的字段完整签名 |
    | :---------- |
    | `public static string StringStaticField = "Hello, World!";` |
    | `public static int INTStaticField = 2147483647;` |
    | `public static float FloatStaticField = 3.14159f;` |
    | `public static bool BooleanStaticField = true;` |
    | `public static char CharStaticField = 'A';` |
    | `public static byte ByteStaticField = 255;` |
    | `public static sbyte SbyteStaticField = -128;` |
    | `public static short ShortStaticField = 32767;` |
    | `public static ushort UshortStaticField = 65535;` |

    ``` csharp
    public static long LongStaticField = 9223372036854775807L;
    public static ulong UlongStaticField = 18446744073709551615ul;
    public static uint UintStaticField = 4294967295u;
    public static double DoubleStaticField = 2.71828182845904d;
    public static decimal DecimalStaticField = 123.456m;
    public static ScriptDocGeneratorTestEnum EnumStaticField = ScriptDocGeneratorTestEnum.Value2;
    public static TestEnum NestedEnumStaticField = TestEnum.Value3;
    ```
    
    | 解析后的字段完整签名 |
    | :---------- |
    | `public static long LongStaticField = 9223372036854775807L;` |
    | `public static ulong UlongStaticField = 18446744073709551615ul;` |
    | `public static uint UintStaticField = 4294967295u;` |
    | `public static double DoubleStaticField = 2.71828182845904d;` |
    | `public static decimal DecimalStaticField = 123.456m;` |
    | `public static ScriptDocGeneratorTestEnum EnumStaticField = ScriptDocGeneratorTestEnum.Value2;` |
    | `public static UnitTestFieldsIsStaticWithDefaultValue.TestEnum NestedEnumStaticField = TestEnum.Value3;` |

### 实例字段

!!! note "实例字段单元测试须知"

    1. 普通实例字段若成功解析，则只读实例字段也可以成功解析，不做另外测试。

???+ success "16 个测试用例"

    ``` csharp
    class TestClass
    {
        public string StringField;
        public int IntField;
        public float FloatField;
        public bool BooleanField;
        public char CharField;
        public byte ByteField;
        public sbyte SbyteField;
        public short ShortField;
        public ushort UshortField;
        public long LongField;
        public ulong UlongField;
        public uint UintField;
        public double DoubleField;
        public decimal DecimalField;
        public ScriptDocGeneratorTestEnum EnumField;
        public TestEnum NestedEnumField;
    }

    enum TestEnum
    {
        Value1,
        Value2,
    }
    ```
    
    | 解析后的字段完整签名 |
    | :---------- |
    | `public string StringField;` |
    | `public int IntField;` |
    | `public float FloatField;` |
    | `public bool BooleanField;` |
    | `public char CharField;` |
    | `public byte ByteField;` |
    | `public sbyte SbyteField;` |
    | `public short ShortField;` |
    | `public ushort UshortField;` |
    | `public long LongField;` |
    | `public ulong UlongField;` |
    | `public uint UintField;` |
    | `public double DoubleField;` |
    | `public decimal DecimalField;` |
    | `public ScriptDocGeneratorTestEnum EnumField;` |
    | `public TestEnum NestedEnumField;` |

### 集合字段

???+ success "16 个测试用例"

    ``` csharp
    public int[] ArrayField;
    public int[,] MultiArrayField;
    public int[][] JaggedArrayField;
    public List<string> ListField;
    public Dictionary<string, int> DictionaryField;
    public HashSet<string> HashSetField;
    public SortedDictionary<string, int> SortedDictionaryField;
    public SortedList<string, int> SortedListField;
    public Stack<string> StackField;
    public Queue<int> QueueField;
    public LinkedList<string> LinkedListField;
    public System.Collections.ArrayList ArrayListField;
    public System.Collections.Hashtable HashtableField;
    public IReadOnlyList<string> ReadOnlyListField;
    public IReadOnlyDictionary<string, int> ReadOnlyDictionaryField;
    public ConcurrentDictionary<string, int> ConcurrentDictionaryField;
    ```
    
    | 解析后的字段完整签名 |
    | :---------- |
    | `public int[] ArrayField;` |
    | `public int[,] MultiArrayField;` |
    | `public int[][] JaggedArrayField;` |
    | `public List<string> ListField;` |
    | `public Dictionary<string, int> DictionaryField;` |
    | `public HashSet<string> HashSetField;` |
    | `public SortedDictionary<string, int> SortedDictionaryField;` |
    | `public SortedList<string, int> SortedListField;` |
    | `public Stack<string> StackField;` |
    | `public Queue<int> QueueField;` |
    | `public LinkedList<string> LinkedListField;` |
    | `public System.Collections.ArrayList ArrayListField;` |
    | `public System.Collections.Hashtable HashtableField;` |
    | `public IReadOnlyList<string> ReadOnlyListField;` |
    | `public IReadOnlyDictionary<string, int> ReadOnlyDictionaryField;` |
    | `public ConcurrentDictionary<string, int> ConcurrentDictionaryField;` |

### Unity 特有字段

???+ success "7 个测试用例"

    ``` csharp
    public GameObject gameObjectField;
    public Transform transformField;
    public Rigidbody rigidbodyField;
    public Vector3 vector3Field;
    ```
    
    | 完整声明为单行的字段，包含特性 |
    | :---------- |
    | `public GameObject gameObjectField;` |
    | `public Transform transformField;` |
    | `public Rigidbody rigidbodyField;` |
    | `public Vector3 vector3Field;` |

    ``` csharp title="Quaternion 字段源码声明"
    [SerializeField]
    [Tooltip("This is a tooltip")]
    [UnityEngine.Range(0, 100)]
    public Quaternion quaternionField;
    ```

    ``` markdown title="解析结果"
    [SerializeField]
    [UnityEngine.Tooltip("This is a tooltip")]
    [UnityEngine.Range(0, 100)]
    public Quaternion quaternionField;
    ```

    ``` csharp title="Color 字段源码声明"
    [ColorUsage(true, true)]
    public Color colorField = Color.white;
    
    ```
 
    ``` markdown title="解析结果"
    [UnityEngine.ColorUsage(true, true)]
    public Color colorField;
    ```

    ``` csharp title="LayerMask 字段源码声明"
    [Obsolete("Use newField instead")]
    public LayerMask layerMaskField;
    ```

    ``` markdown title="解析结果"
    [Obsolete("Use newField instead")]
    public LayerMask layerMaskField;
    ```

### 杂项

???+ success "4 个测试用例"

    ``` csharp
    public TestAbstractClass AbstractField;
    public dynamic DynamicField;
    public ITestInterface InterfaceField;
    public int? NullableField;
    ```
    
    | 解析后的字段完整签名 |
    | :---------- |
    | `public TestAbstractClass AbstractField;` |
    | `public dynamic DynamicField;` |
    | `public ITestInterface InterfaceField;` |
    | `public int? NullableField;` |

