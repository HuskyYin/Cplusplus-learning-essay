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
&emsp;&emsp;这是因为C++编译器在编译期替换枚举变量所表示的值，而编译期时`.h`文件只是将`OperationType`的声明包含进来，所以无法得知`MoveFileOperation`的信息。<br/>
&emsp;&emsp;所以有结论：当定义枚举类型时，需要将它的声明和定义全部放在`.h`文件中，这样一个编译单元（`.cpp`）包含这个`.h`文件并使用枚举类型的时候就可以正常使用枚举成员的值了。<br/>

## 临时变量生命周期
情景：封装一个回调函数log打印时候碰到的问题，代码大致如下：<br/>
```
//PrintField.h
#define CALLBACKBLOCKPRINT_1 CallBackBlockPrint blockprint(__FUNCTION__);

#define CALLBACKBLOCKPRINT_2 CallBackBlockPrint(__FUNCTION__);

class CallBackBlockPrint
{
public:
	CallBackBlockPrint(const char* pfuncname) :ptr_funcname(pfuncname)
	{
		printf("---------------%s BEGIN---------------\n", pfuncname);
	}
	~CallBackBlockPrint(){
		printf("---------------%s END---------------\n", ptr_funcname);
	}
private:
	const char* ptr_funcname;
};

//main.cpp
static void restart_nges3b()
{
	CALLBACKBLOCKPRINT_1
  //CALLBACKBLOCKPRINT_2
	stop_nges3b();
	Execute_bat_Byname("start.bat");
	Sleep(12000);
}
```
使用`CALLBACKBLOCKPRINT_1`回调函数像预期一样在block开始和结束的地方打印函数名；而使用`CALLBACKBLOCKPRINT_2`则会立即打印`BEGIN`和`END`两条信息。经过测试发现是因为`CALLBACKBLOCKPRINT_2`定义中使用的是没有名称的临时变量，临时变量的生命周期只存在于本表达式。因此在执行到下一条语句时，对象将被销毁同时执行析构函数中的`print`。

## 宏定义获取形参名
情景：函数调试需要打印传入形参（`double`类型）的名称和值。<br/>
利用宏函数和辅助函数实现：<br/>
```
//PrintField.h

#define PRINT_DOUBLE(xx) print_double_equal_DBL_MAX(#xx, xx)

void print_double_equal_DBL_MAX(const char* name, double value)
{
	if (DBL_MAX != value)
	{
		printf("%s = [%lf].\n", name, value);
	}
	else{
		printf("%s = DBL_MAX.\n", name);
	}
}
```
符号#表示“字符串化”的意思，就是把跟在后面的参数转换成一个字符串
