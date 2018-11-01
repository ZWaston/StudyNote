**静态代码块**：用staitc声明，jvm加载类时执行，仅执行一次 

**构造代码块**：类中直接用{}定义，每一次创建对象时执行。 

执行顺序优先级：静态块, main()，函数，构造块，构造方法。

# 构造函数

```java
public Test(){//构造函数
}
```

注意点：

1. 一旦开始创建对象，会调用对应的构造函数；
2. 构造函数的作用是：给对象进行初始化；
3. 构造函数在一个对象中只会被调用一次，而普通方法可以被调用多次。

# 构造代码块

```java
public class Test{
    {
        //构造代码块
    }
}
```

注意点：

1. 作用也是对对象进行初始化；
2. 构造代码块 先于 构造函数 执行，且都是在有对象创建时才会执行。
3. 构造函数 与 构造代码块 的区别：
   1. 构造代码块是统一给所有对象初始化，而构造函数是给对应的对象初始化。
   2. 因为构造函数可以有多个（但对象只调用其中一个），但是构造代码块是所有对象创建都会调用的。

# 静态代码块

```java
public class Test{
    static{
        //静态代码块
    }
}
```

注意点：

1. 静态代码块是由类调用的。类调用时，先执行静态代码块，然后才执行主函数的。
2. 静态代码块其实就是给类初始化的，而构造代码块是给对象初始化的。
3. 一个类中可以有多个静态代码块。
4. 静态代码块中的变量是局部变量，与普通函数中的局部变量性质没有区别。

```java
public class Test{
   staitc int cnt=6;
   static{
         cnt+=9;
   }
  public static void main(String[] args) {
         System.out.println(cnt);
  }
   static{
         cnt/=3;
   }
}
/*
* 运行结果：
* 5
*/
```

# 案例分析

## 例子1：

```java
public class HelloA {
    public HelloA(){//构造函数
        System.out.println("A的构造函数");   
    }
    {//构造代码块
        System.out.println("A的构造代码块");  
    }
    static {//静态代码块
        System.out.println("A的静态代码块");      
    }
    public static void main(String[] args) {
    }
}
运行结果：
A的静态代码块
```

## 例子2：

```java
public class HelloA {
    public HelloA(){//构造函数
        System.out.println("A的构造函数");   
    }
    {//构造代码块
        System.out.println("A的构造代码块");  
    }
    static {//静态代码块
        System.out.println("A的静态代码块");      
    }
    public static void main(String[] args) {
        HelloA a=new HelloA();  
    }
}
运行结果：
A的静态代码块
A的构造代码块
A的构造函数
```

## 例子3：

```java
public class HelloA {
    public HelloA(){//构造函数
        System.out.println("A的构造函数");   
    }
    {//构造代码块
        System.out.println("A的构造代码块");  
    }
    static {//静态代码块
        System.out.println("A的静态代码块");      
    }
    public static void main(String[] args) {
        HelloA a=new HelloA();
        HelloA b=new HelloA();
    }
}
运行结果：
A的静态代码块
A的构造代码块
A的构造函数
A的构造代码块
A的构造函数
```

## 涉及继承例子4：

```java
public class HelloA {
    public HelloA(){//构造函数
        System.out.println("A的构造函数");   
    }
    {//构造代码块
        System.out.println("A的构造代码块");  
    }
    static {//静态代码块
        System.out.println("A的静态代码块");      
    }
}
public class HelloB extends HelloA{
    public HelloB(){//构造函数
        System.out.println("B的构造函数");   
    }
    {//构造代码块
        System.out.println("B的构造代码块");  
    }
    static {//静态代码块
        System.out.println("B的静态代码块");      
    }
    public static void main(String[] args) {
        HelloB b=new HelloB();      
    }
}
运行结果：
A的静态代码块
B的静态代码块
A的构造代码块
A的构造函数
B的构造代码块
B的构造函数
```

当涉及到继承时，按照如下顺序执行：

1. 执行父类的静态代码块，并初始化父类静态成员变量
2. 执行子类的静态代码块，并初始化子类静态成员变量
3. 执行父类的构造代码块，执行父类的构造函数，并初始化父类普通成员变量
4. 执行子类的构造代码块， 执行子类的构造函数，并初始化子类普通成员变量



参考：


- https://www.jianshu.com/p/8a3d0699a923