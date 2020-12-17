<!-- TOC -->

- [摘要](#摘要)
  - [枚举类型](#枚举类型)

<!-- /TOC -->

## 枚举类型
&emsp;&emsp;枚举类型需定义在.h文件中，每一个使用枚举类型的编译单元都应该包含枚举类型的应用，在如下情形下：
```
//a.h
enum OperationType;

//a.cpp
enum OperationType
{
	MoveFileOperation = 1,
	CopyFileOperation = 2,
};

//main.cpp
//testcase_1
OperationType test;
//testcase_2
OperationType test = MoveFileOperation;
//testcase_3
OperationType test = OperationType::MoveFileOperation;
```
&emsp;&emsp;在testcase_1情形下，程序是可以编译通过的，在testcase_2和testcase_3下程序不能编译通过，会提示没有`MoveFileOperation`的定义。<br/>
&emsp;&emsp;这是因为C++编译器在编译期替换枚举变量所表示的值，而编译期时`.h`文件只是将`OperationType`的声明包含进来，所以无法得知`MoveFileOperation`的信息。
&emsp;&emsp;所以有结论：当定义枚举类型时，需要将它的声明和定义全部放在`.h`文件中，这样一个编译单元（`.cpp`）包含这个`.h`文件并使用枚举类型的时候就可以正常使用枚举成员的值了。
