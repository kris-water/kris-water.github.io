---
{"dg-publish":true,"permalink":"/Java/Collection/Map/"}
---


### 为什么需要Map?

>[!概念]
>因为数据结构的不同，map是通过hash表来实现的，查找一个元素的时间复杂度为O(1)，而List为线性表，时间复杂度为O(n)，Map的查询效率更高

> [!warning] 缺点
> 查询效率高的同时，还是有代价的
> - 首先存入的数据不保证顺序
> - key不允许重复
> - 占用的内存比List大很多，List只存储数据

---

```java
class Maps {   
    public static void main(String[] args) {  
        Map<String, Person> map1 = new HashMap<>();  
        Person p1 = new Person("a", 18);  
        Person p2 = new Person("a", 18);  
        Person p3 = new Person("a", 18);  
        Person p4 = new Person("a", 100);  
        map1.put("p1", p1);  
        map1.put("p2", p2);  
        map1.put("p3", p3);  
        map1.put("p1", p4);  
        // 相同的key会覆盖所以p1就变成了p4  
        // 但是value可以重复  
        System.out.println(map1.get("p1").name);  
        System.out.println(map1.get("p1").age);  
        System.out.println(map1.get("p2").name);  
        System.out.println(map1.get("p2").age);  
        System.out.println(map1.get("p3").name);  
        System.out.println(map1.get("p3").age);  
    }  
}  
  
class Person {  
    public String name;  
    public int age;  
    Person(String initName, int initAge) {  
        this.name = initName;  
        this.age = initAge;  
    }  
}
```