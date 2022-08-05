在Kotlin中，提供了 Sequence 相关的技术，类似 Java 的 Stream ，是一种惰性处理数据流（PipeLines）的解决方案，这篇文档假定你已经学会使用 Sequence 技术，我们讨论这项技术使用时的注意事项。 
# Sequence 装箱和拆箱成本
众所周知，Java 的泛型处理原生 int,long 类型时，是要拆箱和装箱的，这个技术债务 Kotlin 一样要背，Java Stream 为此单独设计了 IntStream ，而 Kotlin 似乎没有专门的IntSequence。  
```kotlin
val t = intArrayOf(100,2000,3000).asSequence().filter { it > 999 } 
```
上面的代码每次从 IntArray 中迭代一笔记录，就发生一次装箱， 而 filter 内部的表达式 it > 999 要完成功能，从生成的字节码应该是也会发生拆箱，返回值又发生装箱，但我不完全确定，因为Kotlin的编译器有很多优化技术。
```
    INVOKEVIRTUAL java/lang/Number.intValue ()I
    INVOKEVIRTUAL D5$test2$t$1.invoke (I)Z
    INVOKESTATIC java/lang/Boolean.valueOf (Z)Ljava/lang/Boolean;
```
所以我建议，如果你处理大量的原生数据流，建议还是用 Java Stream。
# Sequence 在异常和流中断时的行为
