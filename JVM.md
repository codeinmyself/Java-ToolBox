## JVM
运行Java字节码的虚拟机，针对不同系统有特定实现（windows，Linux，macOS）
目的是使用相同的字节码，给出相同的结果。
字节码和不同系统的JVM是Java语言实现‘一次编译，随处可以运行’的关键所在

## JDK （开发+运行）
Java Development Kit ，功能齐全的Java SDK。
拥有JRE的一切，还有编译器（javac）和工具（JavaDoc和job），能够创建和编译程序

## JRE （运行）
Java运行时环境，运行已编译Java程序所需的所有内容的集合，包括Java虚拟机，Java类库等，不能用于创建新程序

Java通过字节码的方式，在一定程度上解决了传统解释型语言执行效率低的问题，同时又保留了解释型语言可移植的特点

JVM类加载器首先加载字节码文件，然后通过解释器逐行解释执行。有些方法和代码快时经常被调用的（热点代码），所以引入
JIT运行时编译。当JIT完成第一次编译后，会将字节码对应的机器码保存下来，下次可以直接使用。`这也就是编译和解释共存的原因`

## JIT
just-in-time compilation编译器，属于运行时编译。机器码的运行效率肯定高于Java解释器。
HotSpot采用惰性评估的做法，根据二八定律，消耗大部分系统资源的只有热点代码，而这也是JIT需要编译的部分。
JVM会格局每次被执行的情况收集并相应做出一些优化，因此执行次数越多，速度越快。

## AOT
JDK 9 引入了新的编译模式AOT（Ahead of Time Compilation），它是直接将字节码编译成机器码，这样就避免了JIT预热等各方面的开销。
JDK支持分层编译和AOT协作使用。

### 为什么不全部使用AOT呢
方便支持Java语言的动态特性。
CGLIB动态代理使用的ASM技术，运行时直接在内存中生成并加载修改后的字节码文件，如果全部使用ATO提前编译，也就无法使用ASM技术。

### 为什么说Java语言“编译和解释共存”
Java程序经过先编译，后解释两个步骤。

### 几乎虽有对象实例都存在于堆中
HotSpot虚拟机引入JIT之后，会对对象进行逃逸分析，如果发现某一个对象并没有逃逸到方法外部，name就可能通过标量替换来实现栈上分配，而避免堆上分配内存。

### 基本数据类型的成员变量，存放在Java虚拟机的`堆`中
### 基本数据类型的局部变量，存放在Java虚拟机的`局部变量表`中




