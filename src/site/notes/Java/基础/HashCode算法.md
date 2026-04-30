---
{"dg-publish":true,"permalink":"/Java/基础/HashCode算法/"}
---

# Java hashCode 使用的算法

Java 的 `hashCode()` 方法是一个本地方法，核心目标是**将对象的关键特征转换成一个 32 位的有符号整数**。不同的类有不同的实现，下面分别说明。

---

## 1. Object 类

`Object` 的 `hashCode()` 默认实现通常**与对象的内存地址相关**。  
在 HotSpot 虚拟机中，它可能直接使用对象头中的标记字（Mark Word）来生成，或结合随机数等因素，以保证不同对象的哈希值大概率唯一。

---

## 2. String 类

`String` 类的 `hashCode()` 使用的是 **BKDRHash** 算法，计算公式为：
h = 31 × h（上一次计算的结果）+ 当前字符的码点值（UTF-16的字符编码值）

**所以“1”的hashcode为 31 × 0 + 49 = 49**

那我们可以尝试计算一次"ab"的哈希值
首先算出来a的哈希值 31 × 0 + 97 = 97 再根据上一次的结果累加运算 31 × 97 + 98 = 3105
所以“ab”的哈希值为3105

选择 **31** 作为乘子的原因：

- **31 是奇素数**，能让哈希值分布更均匀，减少冲突。
    
- **31 可被编译器优化**：`31 * h` 等价于 `(h << 5) - h`，只涉及移位和减法，比一般的乘法快。

这样设计的最终目的：让任何字符串都能算出一个唯一的、尽量不冲突的整数指纹，方便在字符串常量池里做 O(1) 查找。
## 3. 基本类型包装类

|包装类|hashCode 实现|说明|
|---|---|---|
|`Integer`|返回 `intValue()` 本身|例如 `Integer.valueOf(100).hashCode()` 就是 `100`|
|`Byte`、`Short`、`Character`|返回所包装的数值|`(int) value`|
|`Long`|`(int)(value ^ (value >>> 32))`|高 32 位与低 32 位异或|
|`Float`|`Float.floatToIntBits(value)`|将其 IEEE 754 位模式转为 int|
|`Double`|`Long.hashCode(Double.doubleToLongBits(value))`|先转为 long 位，再按 Long 的方式处理|
|`Boolean`|`value ? 1231 : 1237`|固定质数（1231 和 1237）|

---

## 4. 自定义对象与 Objects 工具类

对于开发者自定义的类，通常需要重写 `hashCode()`，常见写法是使用 `Objects.hash(...)` 辅助方法：

java

@Override
public int hashCode() {
    return Objects.hash(field1, field2, field3);
}

其内部会调用 `Arrays.hashCode`，算法同样是经典的：

text

result = 1;
result = 31 * result + field1Hash;
result = 31 * result + field2Hash;
...

利用 **31** 继续作为乘子，逐步混合各字段的哈希值。

---

## 5. HashMap 中的扰动函数（JDK 8+）

在 `HashMap` 中，直接使用 `key.hashCode()` 的低位来定位桶时，若哈希分布不均，容易冲突。  
因此 JDK 8 增加了一个 **扰动函数**，让高位特征也能影响低位：

java

static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}

这样先让高 16 位与低 16 位异或，再配合桶数组大小是 2 的幂，使用位与 `(n - 1) & hash` 代替取模，最终的散列效果更好。

---

## 总结

- **Object**：默认与内存地址相关。
    
- **String**：BKDRHash，乘子为 31。
    
- **包装类**：直接返回数值或进行高低位混合。
    
- **自定义类**：通常使用 31 乘子组合各字段哈希。
    
- **HashMap 内部**：额外扰动 `hashCode` 高低位，使分布更均匀。