### xv6


#### 参考网站

- [offical theory](https://pdos.csail.mit.edu/6.828/2021/schedule.html)
- [offical](https://pdos.csail.mit.edu/6.828/2021/xv6/book-riscv-rev2.pdf)
- [中文文档](https://th0ar.gitbooks.io/xv6-chinese/content/content/chapter0.html)
- [doc](https://pdos.csail.mit.edu/6.828/2012/xv6/book-rev7.pdf)
- [tzyt](https://ttzytt.com/2022/07/xv6_lab1_record/)
- [csdn deadpool](https://blog.csdn.net/weixin_44465434/article/details/111524650)
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
