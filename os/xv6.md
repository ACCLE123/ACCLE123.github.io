## xv6


### 参考网站

- [schedule](https://pdos.csail.mit.edu/6.828/2021/schedule.html)
- [offical](https://pdos.csail.mit.edu/6.828/2021/xv6/book-riscv-rev2.pdf)
- [中文文档](https://th0ar.gitbooks.io/xv6-chinese/content/content/chapter0.html)
- [doc](https://pdos.csail.mit.edu/6.828/2012/xv6/book-rev7.pdf)
- [tzyt](https://ttzytt.com/2022/07/xv6_lab1_record/)
- [csdn deadpool](https://blog.csdn.net/weixin_44465434/article/details/111524650)
- [whileskies](https://github.com/whileskies/xv6-labs-2020/tree/main)



### basic





#### arch

- multiplexing
- isolation
- interaction

three cpu mode
- machine mode
- supervisor mode(内核态)
- user mode(用户态)


explain: 

privilege code is programmed by assmble. it can control the hardware.

machine mode has full privilege. cpu start in machine mode. xv6 executes a few lines in machine mode and then changes to supervisor mode.

in supervisor mode, cpu can execute privileged instructions. if an application in user mode attempts to execute a privileged instruction, then the cpu doesn't execute the instruction, but switches to supervisor mode so that supervisor-code can terminate the application.


- monolithic (单体内核) (unix)
- microkernel (微内核) (mach)

[mach](https://developer.apple.com/library/archive/documentation/Darwin/Conceptual/KernelProgramming/Mach/Mach.html)

[apple archive](https://developer.apple.com/library/archive/navigation/)


monolithic: entire operating system resides in the kernel.
microkernel: operating system code execute the bulk of operating system in user mode

- process

the unit of isolation in os is a process.





##### sv39



**3 level page table**

`satp` register:

the root register of the page table



**process and page table**

every process has its own page table in the user mode

all process share the only one page table in supervie mode



**virtural memory address**

39bits = 9 + 9 + 9 + 12(offsite)

 

**page table entry**

PTE

54bits = 44 + 10

44 PPN (physical page number)



**physical memory address**  

56bits = 44 + 12

44 PPN



**elegant design**

a address has 64bit in 64 bit architecture

`64bit = 8B = 2^3B`

computer storage is byte-addressable, so we can use 3 bit to express a address

pagesize is `2^12 KB(using 12 bits can express it)`

so 12bits = 9(index) + 3(offset)

 this is why we use 9 + 9 + 9 + 12 = 39 to express the virtual address





**cache**

TLB is a cache stored the PTE in order to map the PNN fast

`sfence.vma` is a function, which can flush the TLB in risc-v





#### trap

there are three kinds of event which cause CPU set aside ordinary execution of instructions and force a transfer of control to special code that handle the event.

1. syscall: user program executes the `ecall` to ask the kernel to do something 
2. execption: some illegal instruction want be executed in the user mode or supervise mode
3. interrupt: it made by divice signal (when interrupt is occuring, the os will executes a divice driver code)

Xv6 handles all traps in the kernel



**register**

* `stver`: the kernel wirtes the address of its trap handler here
* `sepc`:  the `pc` is stored here before the `pc` is overwritten. the `sret` instruction copies `sepc` to the `pc`.
* `scause`: puts a number here that describes the reason for the trap
* `sscratch`: kernel place a value at the start of a trap
* `sstatus`: the sie bit in `sstatus` controls whether device interrupt are enabled. the `spp` bit indicates whether a trap come from user mode or supervisor mode



**the following other than timer interrupts**

1. if the trap is a interrupt, and the `sstatus` `sie` bit is clear, don't do any of following.
2. clear the `sie` in `sstatus`
3. copy the `pc` to `sepc`
4. save  the current mode in the `spp` in `sstatus`
5. set `scause`
6. set mode to supervisor
7. copy `stvec` to `pc`
8. start excuting at the new `pc`

note: cpu doesn't switch to  the kernel page table, doesn't switch to a stack in the kernel, and doesn't save any registers other than the `pc`, since it can provide flexibility to software.













### lab1 

[lab1](https://pdos.csail.mit.edu/6.828/2021/labs/util.html)

#### concept


#### pipe

```c++

#include "kernel/fd_types.h"
#include "kernel/types.h"
#include "user/user.h"
enum PIPE_END { REC = 0, SND = 1 };

int main(int argc, char* argv[]) {
  if (argc != 1) {
    fprintf(STDERR, "usage pingpong(no parameter)\n");
    exit(1);
  }
  
  int parent_fd[2];
  int child_fd[2];
  char buf[20];

  pipe(child_fd);
  pipe(parent_fd);

  int pid;
  if ((pid = fork()) == 0) {
    close(parent_fd[SND]);
    close(child_fd[REC]);

    read(parent_fd[REC], buf, 4);
    printf("%d: received %s\n", getpid(), buf);
    write(child_fd[SND], "pong", 4);

    exit(0);
  
  } else {
    close(parent_fd[REC]);
    close(child_fd[SND]);

    write(parent_fd[SND], "ping", 4);
    read(child_fd[0], buf, 4);
    printf("%d: received %s\n", getpid(), buf);

    exit(0);
  }
}

```
实现pingpong的代码，目前对这段代码困惑的点：
1. 每个只关闭了一端 对这个close不是特别理解

#### primes

#### find

#### xargs

#### thinking

1. 比较重要的函数 read write fork exec pipe
2. 需要再次完成一遍 因为不是独立完成的







### lab2

[tzyt lab2](https://ttzytt.com/2022/07/xv6_lab2_record/?highlight=xv6)

the user mode registration of system calls is `user/user.h`.

```c++
// system calls
int fork(void);
int exit(int) __attribute__((noreturn));
...
```

these system calls is impelemented in `user/usys.S`, which generated by `usys.pl`

```asm
...
# generated by usys.pl - do not edit
#include "kernel/syscall.h"
.global fork
fork:
 li a7, SYS_fork
 ecall
 ret
.global exit
exit:
 li a7, SYS_exit
 ecall
 ret
.global wait
...
```

function pointer is restored in the `a7`

explain:

> A process can make a system call by executing the RISC-V `ecall` instruction. This instruction raises the hardware privilege level and changes the program counter to a kernel-defined entry point. The code at the entry point switches to a kernel stack and executes the kernel instructions that implement the system call. When the system call completes, the kernel switches back to the user stack and returns to user space by calling the `sret` instruction, which lowers the hardware privilege level and resumes executing user instructions just after the system call instruction. A process’s thread can “block” in the kernel to wait for I/O, and resume where it left off when the I/O has finished

`#include "kernel/syscall.h"`

SYS_fork is a macro defined in `kernel/syscall.h`

`kernel/syscall.c`  

```c++
void
syscall(void)
{
  int num;
  struct proc *p = myproc();

  num = p->trapframe->a7;
  if(num > 0 && num < NELEM(syscalls) && syscalls[num]) {
    p->trapframe->a0 = syscalls[num]();
    // call the system function through the function id
  } else {
    printf("%d %s: unknown sys call %d\n",
            p->pid, p->name, num);
    p->trapframe->a0 = -1;
  }
}
```



>gcc 对于 risc-v 使用的部分函数调用约定有下面几点[link1](https://ttzytt.com/2022/07/xv6_lab2_record/?highlight=xv6)：
>
>- 返回值（32 位 int）放在 a0 寄存器中
>- 参数（32 位 int）从左到右放在 a0、a1……a7 中。如果还有，则从右往左压栈，第 9 个参数在栈顶。

`kernel/proc.c` has implemented a lot of system calls about process

1. `allocproc` create a new process
2. `freeproc` release the process





#### lab3







