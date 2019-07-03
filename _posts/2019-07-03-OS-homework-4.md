---
layout: post
title: "实验五 - 复制文件"
subtitle: "操作系统实践编程作业"
date: 2019-07-03
author: "Duck"
tags:
    - 操作系统实践
    - 编程作业
---
## 实验要求
完成一个目录复制命令 mycp ，包括目录下的文件和子目录。  
需要实现 Windows 版本和 Linux 版本。  
目录拷贝时需要支持多级目录（子目录）的拷贝，需要支持 Linux 里的 soft link 和 Windows 中的快捷方式拷贝。

### 提示
#### Windows
CreateFile(), ReadFile(), WriteFile(), CloseHandle() 等函数

#### Linux
creat，read，write 等系统调用

## 程序设计与实现
- `Main` 函数检查程序参数个数、源目录与目标目录是否存在
  - 无错误则进入 `CopyDirectoryDIY` 函数
- `CopyDirectoryDIY` 函数进入源目录，开始遍历文件夹/文件/（Linux 版专属）符号链接
  - 文件，调用 `CopyFileDIY` 函数复制到目标路径
  - 文件夹，递归调用自身，参数为新的目标与源的路径
  - （Linux 版专属）符号链接，直接在目标路径创建新链接
- `CopyFileDIY` 函数打开源文件，创建目标文件，通过缓冲区读写完成复制工作


### 关于快捷方式/符号链接
Windows 编程中直接视为普通文件，无需额外操作，按普通文件处理即可。  
Linux 编程中被判定为符号链接后，在新的位置创建指向相同位置的新符号链接，而非复制。

### 示例用法
将 test 文件夹复制到 test2 文件夹  
`mycp test test2`

## Windows 版
```c
#include <Windows.h>
#include <stdio.h>
#include <strsafe.h>
#include <tchar.h>
#include <locale.h>

void CopyFileDIY(TCHAR *source, TCHAR *target)
{
	// ReadFile 用句柄
	HANDLE hsrc = CreateFile(
		source,
		GENERIC_READ,			// 读权限
		FILE_SHARE_READ,		// 读共享
		NULL,					// 默认安全属性
		OPEN_EXISTING,			// 打开已存在文件
		FILE_ATTRIBUTE_NORMAL,	// 默认文件属性
		NULL					// 忽略
	);
	// WriteFile 用句柄
	HANDLE htgt = CreateFile(
		target,
		GENERIC_WRITE,			// 写权限
		FILE_SHARE_WRITE,		// 写共享
		NULL,
		CREATE_ALWAYS,			// 始终创建新文件
		FILE_ATTRIBUTE_NORMAL,
		NULL
	);
	// 建立并初始化缓冲区
	BYTE* buffer = NULL;
	DWORD fileSize = GetFileSize(hsrc, NULL);
	DWORD readSize = 0, readTotal = 0;
	buffer = new BYTE[fileSize];
	ZeroMemory(buffer, fileSize);
	// 从文件读到缓冲区
	while (readTotal < fileSize)
	{
		ReadFile(
			hsrc,				// 读文件句柄
			buffer + readTotal,	// 指向缓冲区位置
			fileSize - readSize,// 要读字节数
			&readSize,			// 已读字节数
			NULL
		);
		readTotal += readSize;
	}
	// 从缓冲区写到文件
	DWORD writeSize = 0, writeTotal = 0;
	while (writeTotal < fileSize)
	{
		WriteFile(
			htgt,				// 写文件句柄
			buffer + writeTotal,
			fileSize,			// 要写字节数
			&writeSize,			// 已写字节数
			NULL
		);
		writeTotal += writeSize;
	}
	// 释放缓冲区
	delete[] buffer;
	buffer = NULL;
	// 关闭句柄
	CloseHandle(hsrc);
	CloseHandle(htgt);
}

void CopyDirectoryDIY(TCHAR *source, TCHAR *target) 
{
	WIN32_FIND_DATA FindData;
	TCHAR src[MAX_PATH];	// 源路径
	TCHAR tgt[MAX_PATH];	// 目标路径
	// 初始化源路径
	ZeroMemory(&src, sizeof(src));
	StringCchCopy(src, ARRAYSIZE(src), source);
	StringCchCat(src, ARRAYSIZE(src), L"\\*.*");
	// 找到目录
	HANDLE hFFF = FindFirstFile(src, &FindData);
	while (FindNextFile(hFFF, &FindData) != 0)
	{
		// 重新初始化两个路径，因为被修改过
		ZeroMemory(&src, sizeof(src));
		ZeroMemory(&tgt, sizeof(tgt));
		StringCchCopy(src, ARRAYSIZE(src), source);
		StringCchCat(src, ARRAYSIZE(src), L"\\");
		StringCchCopy(tgt, ARRAYSIZE(tgt), target);
		StringCchCat(tgt, ARRAYSIZE(tgt), L"\\");
		// 目录 or 文件
		if (FindData.dwFileAttributes == FILE_ATTRIBUTE_DIRECTORY)
		{
			// 目录，排除 . 和 ..
			if (_tcscmp(FindData.cFileName, L".") != 0 && _tcscmp(FindData.cFileName, L"..") != 0)
			{
				// 路径 + 名称
				StringCchCat(src, ARRAYSIZE(src), FindData.cFileName);
				StringCchCat(tgt, ARRAYSIZE(tgt), FindData.cFileName);
				// 创建新目录
				CreateDirectory(tgt, NULL);
				// 复制目录中文件，递归调用自己
				CopyDirectoryDIY(src, tgt);
			}
		}
		else
		{
			// 文件，路径 + 名称
			StringCchCat(src, ARRAYSIZE(src), FindData.cFileName);
			StringCchCat(tgt, ARRAYSIZE(tgt), FindData.cFileName);
			// 直接复制
			CopyFileDIY(src, tgt);
		}
	}
}

int _tmain(int argc, TCHAR *argv[])
{
	WIN32_FIND_DATA FindData;
	if (argc != 3)
	{
		printf("参数错误\n");
		exit(1);
	}
	else if (FindFirstFile(argv[1], &FindData) == INVALID_HANDLE_VALUE)
	{
		printf("源目录不存在\n");
		exit(1);
	}
	else if (FindFirstFile(argv[2], &FindData) == INVALID_HANDLE_VALUE)
	{
		// 创建目标目录
		CreateDirectory(argv[2], NULL);
	}
	else
	{
		printf("目标目录已存在\n");
		exit(1);
	}
	CopyDirectoryDIY(argv[1], argv[2]);
	printf("复制完成，按回车键结束");
	getchar();
	return 0;
}
```

## Linux 版
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <dirent.h>
#include <sys/stat.h>
#include <string.h>
#include <fcntl.h>
#define MAX_PATH 260

void CopyFileDIY(char *source, char *target)
{
    int in, out, flag;
    // 获取文件信息
    struct stat statbuf;
    stat(source, &statbuf);
    // 建立并初始化缓冲区
    char *buffer = NULL;
    buffer = new char[statbuf.st_size];
    memset(buffer, '0', statbuf.st_size);   
    in = open(source, O_RDONLY, S_IRUSR); // 打开文件
    out = creat(target, statbuf.st_mode); // 创建文件
    // 读->缓冲区->写
    while ((flag = read(in, buffer, statbuf.st_size)) > 0)
    {
        write(out, buffer, flag);
    }
    // 释放缓冲区
    delete[] buffer;
    buffer = NULL;
    // 关闭文件
    close(in);
    close(out);
}

void CopyDirectoryDIY(char *source, char *target)
{
    struct stat statbuf;
    struct dirent *direntbuf;
    DIR *dir;
    char src[MAX_PATH]; // 源路径
    char tgt[MAX_PATH]; // 目标路径
    // 打开目录
    dir = opendir(source);
    while ((direntbuf = readdir(dir)) != NULL)
    {
        // 重新初始化两个路径，因为被修改过
        memset(src, '0', sizeof(src));
        memset(tgt, '0', sizeof(tgt));
        strcpy(src, source);
        strcat(src, "/");
        strcpy(tgt, target);
        strcat(tgt, "/");
        // 目录 or 文件 or 符号链接
        if (direntbuf->d_type == DT_DIR)
        {
            // 目录，排除 . 和 ..
            if (strcmp(direntbuf->d_name, ".") != 0 && strcmp(direntbuf->d_name, "..") != 0)
            {
                // 路径 + 名称
                strcat(src, direntbuf->d_name);
                strcat(tgt, direntbuf->d_name);
                // 获取目录信息
                stat(src, &statbuf);
                // 创建新目录
                mkdir(tgt, statbuf.st_mode);
                // 复制目录中文件，递归调用自己
                CopyDirectoryDIY(src, tgt);
            }
        }
        else if (direntbuf->d_type == DT_REG)
        {
            // 文件，路径 + 名称
            strcat(src, direntbuf->d_name);
            strcat(tgt, direntbuf->d_name);
            // 直接复制
            CopyFileDIY(src, tgt);
        }
        else if (direntbuf->d_type == DT_LNK)
        {
            // 符号链接，路径 + 名称
            strcat(src, direntbuf->d_name);
            strcat(tgt, direntbuf->d_name);
            char buffer[MAX_PATH];
            memset(buffer, '\0', sizeof(buffer));
            // 读取源符号链接
            readlink(src, buffer, sizeof(buffer) - 1);
            // 创建新符号链接
            symlink(buffer, tgt);
        }
        else
        {
            // 其他就跳过
            continue;
        }
    }
}

int main(int argc, char *argv[])
{
    struct stat statbuf;
    DIR *dir;
    if (argc != 3)
    {
        printf("参数错误\n");
        exit(1);
    }
    else if ((dir = opendir(argv[1])) == NULL)
    {
        printf("源目录不存在\n");
        exit(1);
    }
    else if ((dir = opendir(argv[2])) == NULL)
    {
        // 获取源目录信息
        stat(argv[1], &statbuf);
        // 创建目标目录
        mkdir(argv[2], statbuf.st_mode);
    }
    else
    {
        printf("目标目录已存在\n");
        exit(1);
    }
    CopyDirectoryDIY(argv[1], argv[2]);
    printf("复制完成，按回车键结束");
    getchar();
    return 0;
}
```
