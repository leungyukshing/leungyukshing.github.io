---
title: 操作系统实验2--进程通信与shell实现
tags:
  - Operating System
abbrlink: 2583
date: 2018-08-12 13:14:24
---
## 介绍
&emsp;&emsp;本实验主要涉及到两个内容：进程通信和shell实现。进程通信的核心是使用一个**共享内存区域**，shell的实现是为了更好地理解**命令解释器**。
<!-- more -->

## 进程通信
&emsp;&emsp;进程间的数据是不共享的，如果我们需要若干个进程共享数据，就必须人为地创建一个数据共享区域。在这个实验中，我们使用子进程来生成一个Fibonacci数列，然后使用父进程来打印这个数列。

### 共享内存区域
&emsp;&emsp;共享内存区域用于存储一个Fibonacci数列，定义如下：
```C
struct {
  long fib_sequence[MAX_SEQUENCES];
  int sequence_size;
} share_data;
```

### 共享内存函数调用
&emsp;&emsp;以下是几个共享内存函数的调用：
  + 创建或打开共享存储区(shmget)：依据用户给出的整数值key，创建新区或打开现有区，返回一个共享存储区ID。
  + 连接共享存储区(shmat)：连接共享存储区到本进程的地址空间，返回共享存储区首地址。父进程已连接的共享存储区可被fork创建的子进程继承。
  + 拆除共享存储区连接(shmdt)：拆除共享存储区与本进程地址空间的连接。
  + 共享存储区控制(shmctl)：对共享存储区进行控制。如：共享存储区的删除需要显式调用shmctl(shmid, IPC_RMID, 0)；

&emsp;&emsp;简要的用法是：
```C
#include <stdio.h>
#include <sys/shm.h>
#include <sys/stat.h>
int main()
{
	/* the identifier for the shared memory segment */
	int segment_id;
	/* a pointer to the shared memory segment */
	char* shared_memory;
	/* the size (in bytes) of the shared memory segment */
	const int segment_size = 4096;
	/** allocate  a shared memory segment */
	segment_id = shmget(IPC_PRIVATE, segment_size, S_IRUSR | S_IWUSR);
	/** attach the shared memory segment */
	shared_memory = (char *) shmat(segment_id, NULL, 0);
	printf("shared memory segment %d attached at address %p\n", segment_id, shared_memory);
	/** write a message to the shared memory segment   */
	sprintf(shared_memory, "Hi there!");
	/** now print out the string from shared memory */
	printf("*%s*\n", shared_memory);
	/** now detach the shared memory segment */
	if ( shmdt(shared_memory) == -1) {
		fprintf(stderr, "Unable to detach\n");
	}
	/** now remove the shared memory segment */
	shmctl(segment_id, IPC_RMID, NULL);
	return 0;
}
```

### 代码实现
&emsp;&emsp;我们需要先连接共享内存区域，然后再使用`fork()`来创建子进程，生成数列，最后使用父进程打印数列。完整代码如下：
```C
#include <stdio.h>
#include <sys/shm.h>
#include <sys/stat.h>

#define MAX_SEQUENCES 10

#ifndef SHARE_H
#define SHARE_H
struct {
  long fib_sequence[MAX_SEQUENCES];
  int sequence_size;
} share_data;
#endif
int main(int argc, char const *argv[])
{
  int i, sequenceSize;
  /* handle argv */
  if (argc != 2) {
    fprintf(stderr, "Usage: ./fib <size>\n");
    return -1;
  }
  sequenceSize = atoi(argv[1]);

  if (sequenceSize > MAX_SEQUENCES) {
    fprintf(stderr, "sequence size should be less than < %d\n", MAX_SEQUENCES);
    return -1;
  }

  /* the identifier for the shared memory segment */
  int segment_id;
  /* a pointer to the shared memory segment */
  struct share_data* shared_memory;

  /* allocate a shared memory segment */
  segment_id = shmget(IPC_PRIVATE, sizeof(share_data), S_IRUSR | S_IWUSR);
  if (segment_id == -1) {
    fprintf(stderr, "Allocate Shared Memory Fail!\n");
    return 1;
  }
  printf("Create Shared Memory Successfully, segment_id = %d\n", segment_id);

  /* attach the shared memory segment */
  shared_memory = (share_data *) shmat(segment_id, NULL, 0);
  if (shared_memory == (share_data *)-1) {
    fprintf(stderr, "Attach Shared Memory Fail!\n");
    return 0;
  }

  shared_memory->sequence_size = sequenceSize;
  pid_t pid = fork();
  if (pid < 0) {
    return 1;
  }
  /* child */
  else if (pid == 0) {
    shared_memory->fib_sequence[0] = 0;
    shared_memory->fib_sequence[1] = 1;
    for (int i = 2; i < shared_memory->sequence_size; i++) {
      shared_memory->fib_sequence[i] = shared_memory->fib_sequence[i - 1] + shared_memory->fib_sequence[i - 2];
    }
    /* now detach the shared memory segment */
    shmdt(shared_memory);
  }
  /* parent */
  else {
    printf("In Parent Process:\n");
    for (int i = 0; i < shared_memory->sequence_size; i++) {
      printf("shared_memory->fib_sequence[%d] = %d\n", i, shared_memory->fib_sequence[i]);
    }
    /* now detach the shared memory segment */
    shmdt(shared_memory);
    /* now remove the shared memory segment */
    shmctl(segment_id, IPC_RMID, NULL);
  }
  return 0;
}
```
运行结果如下：
![进程通信](/images/fib.png)

## shell的简单实现
&emsp;&emsp;shell是一个简单的命令解释器，这里我们需要实现一个能够处理简单的命令的shell。关键是使用`execvp()`来执行系统调用，从而执行我们输入的命令。同时，我们还需要这个shell具有记录历史输入的功能，用户输入`Ctrl+c`后，命令行显示历史记录并退出。

### 思路
&emsp;&emsp;我们需要创建一个共享内存区域存储执行过的命令，在每一次执行命令前都先将命令存储，在执行后**通过检查`execvp()`的返回值来判断是否成功执行**。若执行失败，则在共享内存中将该记录删除。为了实现后台，还需要使用`background`变量来判断该命令是在前台执行还是在后台执行。**在前台执行的命令需要等待子进程完成**。

### 代码实现
&emsp;&emsp;这里直接给出完整代码，里面含有详细的注释：
```C
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <sys/wait.h>
#include <string.h>
#include <signal.h>
#include <sys/shm.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <wait.h>
#define MAX_LINE 80
/* setup() 用于读入下一行输入的命令，并将它分成没有空格的命令和参数存于数组 args[]中，用NULL作为数组结束的标志 */


typedef struct {
  //char *history[10][MAX_LINE];
  char history[10][MAX_LINE];
  int po;
  int flag; // 0 for false; 1 for true
} shared_data;

/* the identifier for the shared memory segment */
  int segment_id;
  /* a pointer to the shared memory segment */
  shared_data* shared_memory;

void setup(char inputBuffer[], char *args[],int *background) {
  int length, /* 命令的字符数目 */
      i,      /* 循环变量 */
      start,  /* 命令的第一个字符位置 */
      ct;     /* 下一个参数存入args[]的位置 */

  ct = 0;

  /* 读入命令行字符，存入inputBuffer */
  length = read(STDIN_FILENO, inputBuffer, MAX_LINE);

  start = -1;
  if (length == 0) exit(0);            /* 输入ctrl+d，结束shell程序 */
  if (length < 0) {
      perror("error reading the command");
    exit(-1);           /* 出错时用错误码-1结束shell */
  }
  /* 检查inputBuffer中的每一个字符 */
  for (i=0;i<length;i++) {
    switch (inputBuffer[i]) {
      case ' ':
      case '\t' :               /* 字符为分割参数的空格或制表符(tab)'\t'*/
      if (start != -1) {
        args[ct] = &inputBuffer[start];
        ct++;
      }
      inputBuffer[i] = '\0'; /* 设置 C string 的结束符 */
      start = -1;
      break;

      case '\n':                 /* 命令行结束 */
      if (start != -1) {
        args[ct] = &inputBuffer[start];
        ct++;
      }
      inputBuffer[i] = '\0';
      args[ct] = NULL; /* 命令及参数结束 */
      break;

      default :             /* 其他字符 */
      if (start == -1)
        start = i;
      if (inputBuffer[i] == '&') {
        *background  = 1;          /*置命令在后台运行*/
        inputBuffer[i] = '\0';
      }
    }
  }
  args[ct] = NULL; /* 命令字符数 > 80 */
}

void handle_SIGINT() {
  if (shared_memory->flag == 1) {
    exit(0);
  }
  shared_memory->flag = 1;
  printf("\nHistory:\n");
  int i, j;
  for (i = 0; i < shared_memory->po; i++) {
      printf("%s\n", shared_memory->history[i]);
  }
  signal(SIGINT, SIG_IGN);
  exit(0);
}

int main() {
  char inputBuffer[MAX_LINE]; /* 这个缓存用来存放输入的命令 */
  int background; /* ==1时，表示在后台运行命令，即在命令后加上'&' */
  char *args[MAX_LINE/2 + 1]; /* 命令最多40个参数 */
  for (int i = 0; i < MAX_LINE/2 + 1; i++) {
    args[i] = NULL;
  }
  pid_t pid;
  /*set up the signal handler */
  struct sigaction handler;
  handler.sa_handler = handle_SIGINT;
  sigaction(SIGINT, &handler, NULL);

  /* allocate a shared memory segment */
  segment_id = shmget(IPC_PRIVATE, sizeof(shared_data), S_IRUSR | S_IWUSR);
  if (segment_id == -1) {
    fprintf(stderr, "Allocate Shared Memory Fail!\n");
    return 1;
  }
  printf("Create Shared Memory Successfully, segment_id = %d\n", segment_id);
  /* attach the shared memory segment */
  shared_memory = (shared_data *)shmat(segment_id, NULL, 0);
  if (shared_memory == (shared_data *)-1) {
    fprintf(stderr, "Attach Shared Memory Fail!\n");
    return 0;
  }
  shared_memory->flag = 0;
  /* 程序在setup中正常结束 */
  while (1) {
    background = 0;
    printf("COMMAND->");
    fflush(stdout); // 输出缓存内容
    setup(inputBuffer, args, &background); /* 获取下一个输入的命令 */
    pid = fork();
    /* 创建失败 */
    if (pid == -1) {
      printf("Fork Error.\n");
    }
    else if (pid == 0) {
        int j;
        int k = 0;
        for (j = 0; j < sizeof(args); j++) {
          if (args[j] != NULL) {
            for (int c = 0; c < strlen(args[j]); c++,k++) {
              shared_memory->history[shared_memory->po][k] = args[j][c];
            }
            shared_memory->history[shared_memory->po][k++] = ' ';
          }
          else {
            break;
          }
        }
        shared_memory->history[shared_memory->po][k] = '\0';
        shared_memory->po = (shared_memory->po + 1) % 10;

        int error = execvp(args[0], args);
        if (error == -1) {
          shared_memory->po == 0 ? shared_memory->po = 0 : shared_memory->po--;
        }
    }
    if (background == 0) {
      wait(0);
    }
    else {
      setup(inputBuffer, args, &background);
    }
  }
}
```

&emsp;&emsp;运行后结果如下：
![shell命令](/images/shell.png)

  + ① 使用 ls -a命令查看当前目录下的所有文件 （包含隐藏文件） 。
  + ② 使用 `touch test.md`命令创建一个名为`test.md`的文件。
  + ③ 使用 `subl test.md`命令用sublimeText打开这个文件。
  + ④ 使用 `ls`命令查看当前目录下的所有文件（不包含隐藏）。
  + ⑤ `Ctrl+c`命令查看历史输入的命令记录并退出程序，历史记录中只记录正确输入并成功执行的命令。
## 小结
&emsp;&emsp;相信完成这两个实验后，大家对于进程间的通讯有了一定的了解，并且也能够自己实现简单的shell，这个实验的分享就到这里，谢谢！
