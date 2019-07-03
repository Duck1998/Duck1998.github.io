---
layout: post
title: "实验二 - 进程控制"
subtitle: "操作系统实践编程作业"
date: 2019-07-03
author: "Duck"
tags:
    - 操作系统实践
    - 编程作业
---
## 实验要求
设计并实现类似 Unix 的 “time” 命令：“mytime”。“mytime” 命令通过命令行参数接收要运行的程序，创建一个独立的进程来运行该程序，并记录该程序运行的时间。  
实现 Windows 版本和 Linux 版本。

### 提示
#### 在 Windows 下实现
- 使用 `CreateProcess()` 来创建进程  
- 使用 `WaitForSingleObject()` 在 “mytime” 命令和新创建的进程之间同步  
- 调用 `GetSystemTime()` 来获取时间

#### 在 Linux 下实现
- 使用 `fork()` / `execv()` 来创建进程运行程序  
- 使用 `wait()` 等待新创建的进程结束  
- 调用 `gettimeofday()` 来获取时间

### 示例用法
Windows：`mytime "ping baidu.com"`  
Linux: `./mytime apt-get moo`

## Windows 版
```cpp
#include <windows.h>
#include <stdio.h>
#include <tchar.h>
#include <locale.h>

int _tmain(int argc, TCHAR *argv[])
{
	// 解决中文字符显示
	setlocale(LC_ALL, "chs");

	// 创建进程时相关的结构体
	STARTUPINFO si;
	PROCESS_INFORMATION pi;
	ZeroMemory(&si, sizeof(si));
	ZeroMemory(&pi, sizeof(pi));

	// 参数错误时报错
	if (argc != 2)
	{
		printf("Usage: %ws [cmdline]\n", argv[0]);
		return 0;
	}

	// 开始计时
	SYSTEMTIME startTime, endTime;
	GetSystemTime(&startTime);

	// 创建子进程
	if (!CreateProcess(NULL,	// 不指定模块名称，使用命令行
		argv[1],		// 命令行
		NULL,           // 不继承进程句柄
		NULL,           // 不继承线程句柄
		FALSE,          // 不继承当前进程句柄
		0,              // 无进程创建标志
		NULL,           // 使用父进程环境
		NULL,           // 使用父进程目录
		&si,            // STARTUPINFO指针
		&pi)			// PROCESS_INFORMATION指针
		)
	{
		// 创建失败
		printf("CreateProcess failed (%d).\n", GetLastError());
		return 0;
	}

	// 等待子进程结束
	WaitForSingleObject(pi.hProcess, INFINITE);

	// 结束计时
	GetSystemTime(&endTime);

	// 关闭进程和线程句柄
	CloseHandle(pi.hProcess);
	CloseHandle(pi.hThread);

	// 计算时间差得到结果
	int result = endTime.wMilliseconds - startTime.wMilliseconds + (endTime.wSecond - startTime.wSecond) * 1000 + (endTime.wMinute - startTime.wMinute) * 60000 + (endTime.wHour - startTime.wHour) * 3600000;
	printf("\nRun time: %dms\n", result);
	return 0;
}
```

## Linux 版
```cpp
#include <stdio.h>
#include <unistd.h>
#include <sys/time.h>
#include <wait.h>

int main(int argc,char *argv[])
{
    // 存储时间的结构体
    struct timeval startTime, endTime;

    // 参数错误时报错
    if (argc == 1)
    {
        printf("Usage: %s [cmdline]\n", argv[0]);
        return 0;
    }

    // 创建子进程
    pid_t pid = fork();

    // 开始计时
    gettimeofday(&startTime, NULL);

    if(pid < 0)
    {
        // 创建失败
        printf("fork() failed.\n");
        return 0;
    }
    else if(pid == 0)
    {
        // 执行命令行
        execvp(argv[1], &argv[1]);
    }
    else
    {
        // 等待子进程结束
        wait(NULL);

        // 结束计时
        gettimeofday(&endTime, NULL);
    }

    // 计算时间差得到结果
    long result = endTime.tv_usec - startTime.tv_usec +  (endTime.tv_sec - startTime.tv_sec) * 1000000;
    printf("\nRun time: %ldμs\n", result);
    return 0;
} 
```
