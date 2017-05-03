---
layout: post
title: 【Java】向下类型转换
---

> お久しぶりです。

预留。

```java
public class Test {
    public static void main(String[] args) {
        A a = new AA();
        // C c = (C)a; // incompatible type
        AA1 c = (AA1)a; // java.lang.ClassCastException
        c.show();
    }
}
class A {}
class AA extends A {
    public void show() {
        System.out.println("--- AA ---");
    }
}
class AA1 extends A {
    public void show() {
        System.out.println("--- AA1 ---");
    }
}
class C {
    public void show() {
        System.out.println("--- c ---");
    }
}
```

**总结**：尝试对父类引用（指向子类对象）进行向下类型转换：

0. 不可以转换为不在父类继承关系链上的其他类型（编译时期会进行类型检查）。
1. 可以转换为在父类继承关系链上的其他类型（编译通过）。
2. 可以转换为不在子类继承关系链上的其他类型，但会出现运行时异常。
