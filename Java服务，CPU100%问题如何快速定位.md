### 简介

1. 找到最消耗CPU的进程
2. 找到最消耗CPU的线程
3. 查看堆栈，定位线程在干嘛，定位对应的代码

### 步骤一 找到最消耗CPU的进程

使用top命令

	* 执行top -c 命令显示运行进程列表
	* 键入P,进程按照CPU使用率排序

### 步骤二 找到最消耗CPU的线程

使用top命令

* top -Hp 进程ID,显示一个进程的线程运行列表
* 键入P，按照CPU的使用率排序

### 步骤三 查看堆栈，定位线程在干嘛，定位代码

使用print命令

* print "%x\n" 线程ID ,将10进制转为16进制

* 使用jstack， jstack 进程ID | grep  -20 线程ID(16进制)

  









