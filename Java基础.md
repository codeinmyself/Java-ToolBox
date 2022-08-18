## 构造方法是否可以被override？
构造方法不能被override（重写），但可以被overload（重载）
## 接口和抽象类有什么共同点和区别？
### 共同点
不能被实例化
可以包含抽象方法
可以有默认实现的方法
### 区别
接口针对类的行为进行约束，抽象类主要用于代码复用
一个类只能集成一个类，但是可以实现多个接口
接口中的成员变量只能是`public static final`类型，不能被修改且必须有初始值，而抽象类的成员变量默认default，可在子类中被重新定义，也可以被重新赋值

## 深拷贝和浅拷贝区别了解吗？什么是引用拷贝？
### 浅拷贝
#### 定义
浅拷贝会在堆上创建一个新的对象（区别于引用拷贝的一点），不过，如果原对象内部的属性是引用类型的话，浅拷贝会直接复制内部对象的引用地址，也就是说拷贝对象和原对象共用同一个内部对象。
#### 方法
实现Cloneable接口，直接使用Object的clone方法
### 深拷贝 
#### 定义
深拷贝会完全复制整个对象，包括这个对象所包含的内部对象。
#### 方法
实现Cloneable接口，重写clone方法

### 引用拷贝和浅拷贝、深拷贝的区别
https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/github/javaguide/java/basis/shallow&deep-copy.jpg

## Object的11个主要方法
getClass
hashCode
equals
clone
toString
notify
notifyAll
wait
wait
wait
finalize

## equals和==的区别
== 对于基本类型和引用类型的作用效果是不同的：
对于基本数据类型来说，== 比较的是值。
对于引用数据类型来说，== 比较的是对象的内存地址。
因为 Java 只有值传递，所以，对于 == 来说，不管是比较基本数据类型，还是引用数据类型的变量，其本质比较的都是值，只是引用类型变量存的值是对象的地址。
equals() 方法存在两种使用情况：
类没有重写 equals()方法 ：通过equals()比较该类的两个对象时，等价于通过“==”比较这两个对象，使用的默认是 Object类equals()方法。
类重写了 equals()方法 ：一般我们都重写 equals()方法来比较两个对象中的属性是否相等；若它们的属性相等，则返回 true(即，认为这两个对象相等)。

## 为什么要有 hashCode？
当你把对象加入 HashSet 时，HashSet 会先计算对象的 hashCode 值来判断对象加入的位置，同时也会与其他已经加入的对象的 hashCode 值作比较，如果没有相符的 hashCode，HashSet 会假设对象没有重复出现。但是如果发现有相同 hashCode 值的对象，这时会调用 equals() 方法来检查 hashCode 相等的对象是否真的相同。如果两者相同，HashSet 就不会让其加入操作成功。如果不同的话，就会重新散列到其他位置。这样我们就大大减少了 equals 的次数，相应就大大提高了执行速度。

## 为什么重写 equals() 时必须重写 hashCode() 方法？
因为两个相等的对象的 hashCode 值必须是相等。也就是说如果 equals 方法判断两个对象是相等的，那这两个对象的 hashCode 值也要相等。
如果重写 equals() 时没有重写 hashCode() 方法的话就可能会导致 equals 方法判断是相等的两个对象，hashCode 值却不相等。

## String、StringBuffer、StringBuilder 的区别？
可变性、线程安全性、性能
## String 为什么是不可变的?
1、保存字符串的数组被 final 修饰且为私有的，并且String 类没有提供/暴露修改这个字符串的方法。
2、String 类被 final 修饰导致其不能被继承，进而避免了子类破坏 String 不可变。

## 循环里用StringBuilder替代+
## String#equals() 和 Object#equals() 有何区别？
## 字符串常量池的作用了解吗？
字符串常量池 是 JVM 为了提升性能和减少内存消耗针对字符串（String 类）专门开辟的一块区域，主要目的是为了避免字符串的重复创建。

## String s1 = new String("abc");这句话创建了几个字符串对象？
会创建 1 或 2 个字符串对象。
## intern 方法有什么作用?
将指定的字符串对象的引用保存在字符串常量池中
## String 类型的变量和常量做“+”运算时发生了什么？
对于编译期可以确定值的字符串，也就是常量字符串 ，jvm 会将其存入字符串常量池。并且，字符串常量拼接得到的字符串常量在编译阶段就已经被存放字符串常量池，这个得益于编译器的优化。



