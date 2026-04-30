---
{"dg-publish":true,"permalink":"/Java/基础/String/"}
---

```java
package org.example.core;  
  
public class StringDemo {  
    // 一个比较触及灵魂的问题  
    public void a () {  
        // 这一行到底发生了什么？  
        // 首先 声明了变量a只能指向一个String对象,不能指向其他类型的对象了。  
        // 然后我们常说到的String不可变实际上是说“1”是不能变的，只要被创建出来就永远不能变了，  
        // 但是 a 是可以通过=运算符来指向其他的String对象的，比如 a = "2"        String a = "1";  
        // 首先JVM会先从字符串常量池中查找"1"，如果找到就直接在栈区存入a,其值为堆区的引用。  
        // 如果没有找到，则在堆中创建一个String对象（其值为“1”），并把这个引用放入字符串常量中。  
  
        // 介绍一下StringTable的结构  
        // 它是一个链地址hash表，  
    }  
}
```
