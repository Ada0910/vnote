# 1. javac

      javac 是java语言编程编译器。全称java compiler。
      javac工具读由java语言编写的类和接口的定义，并将它们编译成字节代码的class文件。
      javac 可以隐式编译一些没有在命令行中提及的源文件。用 -verbose 选项可跟踪自动编译。
  当编译源文件时，编译器常常需要它还没有识别出的类型的有关信息。对于源文件中使用、扩展或实现的每个类或接口，编译器都需要其类型信息。这包括在源文件中没有明确提及、但通过继承提供信息的类和接口。