---
layout: article
title: "操作系统实践 - 生产者消费者问题"
tags: 操作系统实践 作业
---
## 实验要求
通过编写**多进程程序**实现典型的生产者和消费者问题。  
完成 Windows 版本和 Linux 版本。
<!--more-->

一个大小为 3 的缓冲区，初始为空。  
2 个生产者：  
- 随机等待一段时间，往缓冲区添加数据
- 若缓冲区已满，等待消费者取走数据后再添加
- 重复 6 次

3 个消费者：  
- 随机等待一段时间，从缓冲区读取数据
- 若缓冲区为空，等待生产者添加数据后再读取
- 重复 4 次

显示每次添加和读取数据的时间及缓冲区的状态。

### 提示
缓冲区可以采用共享主存实现，进程之间的同步和互斥通过互斥体和信号量实现。

## 程序设计与实现
- 共享内存中使用数组表示堆栈，相当于大小有限的缓冲区
- 2 个信号量，Full、Empty；1 个互斥体，Mutex
  - Linux 版本使用信号量实现互斥
- 进程 PID 作为随机数生成的种子

### 程序逻辑
#### 主进程
1. 创建共享内存
2. 映射共享内存，初始化堆栈内容
3. 创建信号量、互斥体
4. 创建 2 个生产者进程，随机等待
5. 创建 3 个消费者进程，随机等待
6. 等待所有进程结束
7. 关闭信号量、互斥体、共享内存

#### 生产者进程
1. 映射共享内存
2. 打开信号量、互斥体
3. P(empty)，随机等待
4. P(mutex)
5. 生产产品并输出缓冲区状态
6. V(full)
7. V(mutex)
8. 3 到 7 执行 6 次
9. 关闭信号量、互斥体、共享内存

#### 消费者进程
1. 映射共享内存
2. 打开信号量、互斥体
3. P(full)，随机等待
4. P(mutex)
5. 取走产品并输出缓冲区状态
6. V(empty)
7. V(mutex)
8. 3 到 7 执行 4 次
9. 关闭信号量、互斥体、共享内存

## Windows 版
```cpp
#include <Windows.h>
#include <Shlwapi.h>
#include <stdio.h>
#include <stdlib.h>
#include <process.h>
#include <tchar.h>
#include <locale.h>
#pragma comment(lib, "shlwapi.lib ")

struct buffer
{
	// 共享内存，使用堆栈存储
	int stack[3];
	int topPos;
};

HANDLE MakeSharedFile()
// 创建共享内存
{
	// 创建临时文件映射对象
	HANDLE hMapping = CreateFileMapping(
		INVALID_HANDLE_VALUE,	// 需指定
		NULL,					// 安全属性
		PAGE_READWRITE,			// 页保护
		0,						// 高位
		sizeof(buffer),			// 低位
		L"Buffer"				// 名称
	);
	// 在文件映射上创建视图
	LPVOID pData = MapViewOfFile(hMapping, FILE_MAP_ALL_ACCESS, 0, 0, 0);
	ZeroMemory(pData, sizeof(buffer));
	UnmapViewOfFile(pData);
	// 返回起始虚地址
	return(hMapping);
}

HANDLE cloneProcess(const TCHAR *type)
// 创建子进程
{
	// 创建进程时相关的结构体
	STARTUPINFO si;
	PROCESS_INFORMATION pi;
	ZeroMemory(&si, sizeof(si));
	ZeroMemory(&pi, sizeof(pi));

	// 应用名与命令行
	TCHAR exeName[MAX_PATH];
	TCHAR cmdLine[MAX_PATH];
	// 获取程序名
	GetModuleFileName(NULL, exeName, MAX_PATH);
	PathStripPath(exeName);
	// Unicode 下使用 wsprint 拼接
	wsprintf(cmdLine, L"%ws %ws", exeName, type);

	// 创建子进程
	CreateProcess(
		NULL,		// 自己
		cmdLine,	// 命令行参数
		NULL, NULL, FALSE, NULL, NULL, NULL,
		&si, &pi);
	// 返回句柄
	return(pi.hProcess);
}

int _tmain(int argc, TCHAR *argv[])
{
	// 解决中文字符显示
	setlocale(LC_ALL, "chs");
	// 生成随机数种子
	srand((unsigned)_getpid());
	// 时间
	SYSTEMTIME time;

	if (argc == 1)
	// 主进程
	{
		// 创建并映射共享内存
		HANDLE hMapping = MakeSharedFile();
		LPVOID pFile = MapViewOfFile(hMapping, FILE_MAP_ALL_ACCESS, 0, 0, 0);
		// 初始化缓冲区内容
		struct buffer *shrMem = (struct buffer*)(pFile);
		shrMem->topPos = -1;
		for (int i = 0; i < 3; i++)
		{
			shrMem->stack[i] = 0;
		}
		// 创建信号量
		HANDLE semEmpty = CreateSemaphore(NULL, 3, 3, L"Empty");
		HANDLE semFull = CreateSemaphore(NULL, 0, 3, L"Full");
		// 创建互斥体，初值为空
		HANDLE hMutex = CreateMutex(NULL, false, L"Mutex");

		// 子进程句柄数组
		HANDLE childHandle[5];
		int i = 0;
		// 创建2个生产者
		for (i; i < 2; i++)
		{
			childHandle[i] = cloneProcess(L"Producer");
			Sleep(rand() % 1001 + 1000);
		}
		// 创建3个消费者
		for (i; i < 5; i++)
		{
			childHandle[i] = cloneProcess(L"Comsumer");
			Sleep(rand() % 1001 + 1000);
		}
		
		// 等待所有子进程结束
		WaitForMultipleObjects(5, childHandle, TRUE, INFINITE);
		// 关闭所有句柄
		for (i = 0; i < 5; i++)
		{
			CloseHandle(childHandle[i]);
		}
		// 关闭映射
		UnmapViewOfFile(pFile);
		CloseHandle(hMapping);
		// 关闭互斥体
		CloseHandle(hMutex);
		// 关闭信号量
		CloseHandle(semEmpty);
		CloseHandle(semFull);
		printf("按回车键结束");
		getchar();
	}
	else if (_tcscmp(argv[1], L"Producer") == 0)
	// 生产者
	{
		// 映射共享内存
		HANDLE hMapChild = OpenFileMapping(FILE_MAP_ALL_ACCESS, FALSE, L"Buffer");
		LPVOID pFile = MapViewOfFile(hMapChild, FILE_MAP_ALL_ACCESS, 0, 0, 0);
		struct buffer *shrMem = (struct buffer*)(pFile);
		// 打开信号量
		HANDLE semEmpty = OpenSemaphore(SEMAPHORE_ALL_ACCESS, FALSE, L"Empty");
		HANDLE semFull = OpenSemaphore(SEMAPHORE_ALL_ACCESS, FALSE, L"Full");
		// 打开互斥体
		HANDLE hMutex = OpenMutex(MUTEX_ALL_ACCESS, false, L"Mutex");
		// 运行6次
		for (int i = 0; i < 6; i++)
		{
			// P(empty)
			WaitForSingleObject(semEmpty, INFINITE);
			// 随机等待
			Sleep(rand() % 1001 + 1000);
			// P(mutex)
			WaitForSingleObject(hMutex, INFINITE);
			// 生产产品
			shrMem->stack[++(shrMem->topPos)] = 1;
			// 控制台输出
			GetSystemTime(&time);
			printf("生产者于%02d:%02d:%02d:%03d添加产品\n", time.wHour, time.wMinute, time.wSecond, time.wMilliseconds);
			printf("当前堆栈内容：%d %d %d，共%d个产品\n\n", shrMem->stack[0], shrMem->stack[1], shrMem->stack[2], shrMem->topPos + 1);
			// V(full)
			ReleaseSemaphore(semFull, 1, NULL);
			// V(mutex)
			ReleaseMutex(hMutex);
		}
		// 关闭一堆东西
		CloseHandle(semEmpty);
		CloseHandle(semFull);
		CloseHandle(hMutex);
		UnmapViewOfFile(pFile);
		CloseHandle(hMapChild);
	}
	else
	// 消费者
	{
		// 映射共享内存
		HANDLE hMapChild = OpenFileMapping(FILE_MAP_ALL_ACCESS, FALSE, L"Buffer");
		LPVOID pFile = MapViewOfFile(hMapChild, FILE_MAP_ALL_ACCESS, 0, 0, 0);
		struct buffer *shrMem = (struct buffer*)(pFile);
		// 打开信号量
		HANDLE semEmpty = OpenSemaphore(SEMAPHORE_ALL_ACCESS, FALSE, L"Empty");
		HANDLE semFull = OpenSemaphore(SEMAPHORE_ALL_ACCESS, FALSE, L"Full");
		// 打开互斥体
		HANDLE hMutex = OpenMutex(MUTEX_ALL_ACCESS, false, L"Mutex");
	
		// 运行4次
		for (int i = 0; i < 4; i++)
		{		
			// P(full)
			WaitForSingleObject(semFull, INFINITE);
			// 随机等待
			Sleep(rand() % 1001 + 1000);
			// P(mutex)
			WaitForSingleObject(hMutex, INFINITE);
			// 取走产品
			shrMem->stack[(shrMem->topPos)--] = 0;
			// 控制台输出
			GetSystemTime(&time);
			printf("消费者于%02d:%02d:%02d:%03d取走产品\n", time.wHour, time.wMinute, time.wSecond, time.wMilliseconds);
			printf("当前堆栈内容：%d %d %d，共%d个产品\n\n", shrMem->stack[0], shrMem->stack[1], shrMem->stack[2], shrMem->topPos + 1);
			// V(empty)
			ReleaseSemaphore(semEmpty, 1, NULL);
			// V(mutex)
			ReleaseMutex(hMutex);
		}
		// 关闭一堆东西
		CloseHandle(semEmpty);
		CloseHandle(semFull);
		CloseHandle(hMutex);
		UnmapViewOfFile(pFile);
		CloseHandle(hMapChild);
	}
	return 0;
}
```

## Linux 版
```cpp
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <time.h>
#include <wait.h>
#include <sys/shm.h>
#include <sys/sem.h>

#define EMPTY 0
#define FULL 1
#define MUTEX 2

struct buffer
{
    // 共享内存，使用堆栈存储
    int stack[3];
    int topPos;
};

void P(int sem_id, int sem_num)
{
    // P操作
    struct sembuf p;
    p.sem_flg = 0;
    p.sem_num = sem_num;
    p.sem_op = -1;
    semop(sem_id, &p, 1);
}

void V(int sem_id, int sem_num)
{
    // V操作
    struct sembuf v;
    v.sem_flg = 0;
    v.sem_num = sem_num;
    v.sem_op = 1;
    semop(sem_id, &v, 1);
}

int main(int argc, char *argv[])
{
    // 初始化一堆东西
    int sem_ID, shm_ID;
    struct buffer *shrMem;
    pid_t comsumer, producer;
    time_t timep;

    // 创建信号量
    sem_ID = semget((key_t)1875, 3, IPC_CREAT | 0666);
    semctl(sem_ID, EMPTY, SETVAL, 3);
    semctl(sem_ID, FULL, SETVAL, 0);
    semctl(sem_ID, MUTEX, SETVAL, 1);
    // 创建并映射共享内存
    shm_ID = shmget(IPC_PRIVATE, sizeof(buffer), IPC_CREAT | 0644);
    shrMem = (struct buffer *)(shmat(shm_ID, 0, 0));
    // 初始化缓冲区内容
    for (int i = 0; i < 3; i++)
    {
        shrMem->stack[i] = 0;
    }
    shrMem->topPos = -1;

    // 创建2个生产者
    for (int i = 0; i < 2; i++)
    {
        producer = fork();
        sleep(rand() % 1 + 1);
        if (producer == 0)
        {
            srand((unsigned)getpid());
            // 映射共享内存
            shrMem = (struct buffer *)(shmat(shm_ID, 0, 0));
            for (int j = 0; j < 6; j++)
            {
                // P(empty)
                P(sem_ID, EMPTY);
                // 随机等待
                sleep(rand() % 1 + 1);
                // P(mutex)
                P(sem_ID, MUTEX);
                // 生产产品
                shrMem->stack[++(shrMem->topPos)] = 1;
                // 终端输出
                timep = time(NULL);
                printf("生产者于%s添加产品\n", ctime(&timep));
                printf("当前堆栈内容：%d %d %d，共%d个产品\n\n", shrMem->stack[0], shrMem->stack[1], shrMem->stack[2], shrMem->topPos + 1);
                fflush(stdout);
                // V(full)
                V(sem_ID, FULL);
                // V(mutex)
                V(sem_ID, MUTEX);
            }
            // 解除共享内存映射
            shmdt(shrMem);
            exit(0);
        }
    }

    // 创建3个消费者
    for (int i = 0; i < 3; i++)
    {
        comsumer = fork();
        sleep(rand() % 1 + 1);
        if (comsumer == 0)
        {
            srand((unsigned)getpid());
            // 映射共享内存
            shrMem = (struct buffer *)(shmat(shm_ID, 0, 0));
            for (int j = 0; j < 4; j++)
            {
                // P(full)
                P(sem_ID, FULL);
                // 随机等待
                sleep(rand() % 1 + 1);
                // P(mutex)
                P(sem_ID, MUTEX);
                // 取走产品
                shrMem->stack[(shrMem->topPos)--] = 0;
                // 终端输出
                timep = time(NULL);
                printf("消费者于%s取走产品\n", ctime(&timep));
                printf("当前堆栈内容：%d %d %d，共%d个产品\n\n", shrMem->stack[0], shrMem->stack[1], shrMem->stack[2], shrMem->topPos + 1);
                fflush(stdout);
                // V(empty)
                V(sem_ID, EMPTY);
                // V(mutex)
                V(sem_ID, MUTEX);
            }
            // 解除共享内存映射
            shmdt(shrMem);
            exit(0);
        }
    }
    // 等待子进程结束
    while (wait(0) != -1);
    // 解除共享内存映射
    shmdt(shrMem);
    // 删除共享内存和信号量
    shmctl(shm_ID, IPC_RMID, 0);
    shmctl(sem_ID, IPC_RMID, 0);
    printf("按回车键结束");
    getchar();
    return 0;
}
```
