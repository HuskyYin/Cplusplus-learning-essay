# Windows API
<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Windows API](#windows-api)
	- [1. CreateProcess](#1-createprocess)
		- [函数原型：](#函数原型)
		- [返回值](#返回值)
		- [pszImageName参数：](#pszimagename参数)
		- [pszCmdLine参数：](#pszcmdline参数)

<!-- /TOC -->
## 1. CreateProcess
&emsp;&emsp;用以启动另一个指定进程的Windows API。

### 函数原型：
```
    BOOL CreateProcess(
      LPCWSTR pszImageName,
      LPCWSTR pszCmdLine,
      LPSECURITY_ATTRIBUTES psaProcess,
      LPSECURITY_ATTRIBUTES psaThread,
      BOOL fInheritHandles,
      DWORD fdwCreate,
      LPVOID pvEnvironment,
      LPWSTR pszCurDir,
      LPSTARTUPINFOW psiStartInfo,
      LPPROCESS_INFORMATION pProcInfo
    );
```

### 返回值
&emsp;&emsp;若启动成功返回true,若启动失败则返回false。

### pszImageName参数：
&emsp;&emsp;**pszImageName**可以用来指定待启动进程的文件位置或者文件名。其可以指定绝对路径名如：

>pszImageName = "D:\\项目\\bd_pub\\libroc\\test\\testngesapi\\x64\\Debug\\testtradeapi.exe";

或者只指定文件名，比如:

>pszImageName = "testtradeapi.exe";

&emsp;&emsp;一般来说将该参数设置为NULL比较合适，这也是大多数情况下调用此API时的设置，启动进程的文件位置可以作为**pszCmdLine**参数中的第一个字符串指定，在此处不需要进行额外设置。

### pszCmdLine参数：
&emsp;&emsp;**pszCmdLine**用以传递启动进程的参数，与以命令行启动程序相似，需要将文件位置作为第一个参数，将其他参数按照次序拼接在一起，每一个参数之间需要以空格隔开。一个标准的**pszCmdLine**应该设置成如下形式：

>pszCmdLine = "D:\\项目\\bd_pub\\libroc\\test\\testngesapi\\x64\\Debug\\testtradeapi.exe cmd1 cmd2"

&emsp;&emsp;如此待启动程序启动时的**argc**将会为3，于此对应的，如果在**pszImageName**指定了路径而在**pszCmdLine**不指定路径只指定参数，待启动程序依然可以正常运行但**argc**将会为2。这个数量与通常情况下以**cmd**启动程序时传入参数个数不符（以**cmd**启动程序时会将文件路径作为第一个参数传入），当启动程序解析参数时可能会发生错误，为了避免此类错误发生，应该尽量仅使用**pszCmdLine**参数以保持与以**cmd**启动行为一致。
