---
title: Introduction on eBPF
mathjax: true
date: 2022-06-18 18:47:36

---

# Introduction

[eBPF](https://ebpf.io/) (**e**xternal **B**erkerly **P**acket **F**ilter) is a technology originated in the Linux kernel that can run sandboxed programs in the OS kernel. It provides users with the ability to ingest user-defined program into the OS kernel without modifying the kernel code.

In this post, I will share some basics about eBPF and write a simple eBPF program.

<!-- more -->

# Why we need eBPF?

eBPF is originated in the Linux kernel, which is used to **provide the flexibility to dispatch network packets to their corresponding process**. Initiallly, the Linux uses virtualization to run multiple user processes in the same time, howerver there is only on network interface to send and receive network packets. When receiving the network packet, the kernel needs to know which process should be sent to. But polling every process to check the information is inefficient. Also, adding user logic into the kernel may increase its complexity and makes it harder to maintain. Consequently, eBPF is introduced and used to provide such a flexibility and programmability to run user-defined program in the kernel, without harming the OS.

eBPF provides both **high performance** and **extensibility**:

+ JIT compiler is able to provide high execution efficency as running code in the kernel
+ It could add protocol and routing policy easily without modifying the kernel.

---

# How eBPF works?

eBPF programs are **event-driven** and should be first loaded into the kernel before being ran. The events like some system calls will trigger the corresponding eBPF program to run. The following graph is the architecture for loading and verifying eBPF program in the kernel.

![eBPF loader & Verification](https://ebpf.io/static/go-1a1bb6f1e64b1ad5597f57dc17cf1350.png)

1. Write an eBPF program in C;

2. Compile the source code into bytecode;

3. Load the program into kernel using an API, which will initiate a system call to load the program.

4. In the system call, the kernal first verifyt the program is safe enough to run in the kernel. This part may including:

   a.  **make sure the addresses reference in the program will not access the addresses out of the user process**;

   b. **make sure the program doesn't contain control loop, which may trap the kernel into infinite loop**;

   c. limit the number of instructions (e.g, 4096);

5. After verifying, the kernel will save the program in eBPF map;

6. When network packets come in, the kernel will check if there is any regitsered eBPF program to run.

---

# A Simple eBPF Program

---

Fist we need to write a eBPF program to define which event we want to listen and what we want to do when the event happens. Then we need to load this program into the kernel.

# eBPF Program

`ebpf_prog.c`

```c
#include <linux/bpf.h>
#define SEC(NAME) __attribute__((section(NAME), used))

static int (*bpf_trace_printk)(const char *fmt, int fmt_size,
                               ...) = (void *)BPF_FUNC_trace_printk;

// specify where we want to register
// here we listen to the event of execve is called
SEC("tracepoint/syscalls/sys_enter_execve")
  
// actual program, when the event is triggerred, this program will be executed.
int bpf_prog(void *ctx) {
  // print a string
  char msg[] = "Hello World!";
  // print the log into pipe
  bpf_trace_printk(msg, sizeof(msg));
  return 0;
}

// license of this program, have to keep it identical to the kernel license
char _license[] SEC("license") = "GPL";
```

Then we compile this file into bytecode:

```bash
clang -O2 -target bpf -c ebpf_prog.c \
-I/kernel-src/tools/testing/selftests/bpf \ 
-o ebpf_prog.o
```

After that, we load our program into the kernel:

`monitor-exec.c`

```c
#include "bpf_load.h"
#include <stdio.h>

int main(int argc, char **argv) {
  // load the ebpf program
  if (load_bpf_file("bpf_program.o") != 0) {
    printf("The kernel didn't load the BPF program\n");
    return -1;
  }
	// read log from pipe to show in the command line
  read_trace_pipe();

  return 0;
}
```

Then we compile this file and run:

```bash
clang -DHAVE_ATTR_TEST=0 -o monitor-exec -lelf \
-I/kernel-src/samples/bpf \
-I/kernel-src/tools/lib \
-I/kernel-src/tools/perf \
-I/kernel-src/tools/include \
-L/usr/local/lib64 -lbpf \
/kernel-src/samples/bpf/bpf_load.c monitor-exec.c
```

Then you can see the result from the command line.

---

# Application

eBPF is not only used in network packet filtering right now. As developing and populating rapidly these years, eBPF is now a widely-used technology covering a wide range of applications.

## Troubleshooting

By using [kprobe](https://lwn.net/Articles/132196/#:~:text=KProbes%20is%20a%20debugging%20mechanism,specific%20events%2C%20trace%20problems%20etc.) and [tracepoints](https://docs.kernel.org/trace/tracepoints.html), eBPF could provide end-to-end tracing ability as well as provide profiling statistics continuously.

## Security Control

eBPF can hook on every system calls. Also it can filter on all network packets and operations on sockets, we can apply security control by loading some inspection or filter program with eBPF into the kernel. Such a method will provide a better protection than using a user-level process to check.

## Performance Monitoring

Comparing to traditional metrics middleware, eBPF could provide dynamic computational ability to provide user-defined metrics efficiently.

---

# Summary

From my perspective, eBPF is a Linux kernel technology in the beginning but now it's more like a programing idea right now. It provides us an insight that by using the sandbox model, we could provide such a flexibility and extensibility to allow externel clients to ingest logic into the internal system, which could not only protect the system but also improve the efficiency. I will keep my eye on the development of eBPF and see how far it will go.

If you have any ideas, please feel free to share with me. Thanks!

---

# Reference

1. [eBPF](https://ebpf.io/)
2. [kprobe](https://lwn.net/Articles/132196/#:~:text=KProbes%20is%20a%20debugging%20mechanism,specific%20events%2C%20trace%20problems%20etc.)
3. [tracepoints](https://docs.kernel.org/trace/tracepoints.html)