#10.1进程概念及应用

##通过调用fork函数创建进程：fork.c

 - 父进程:fork函数返回子进程ID。
 - 子进程:fork函数返回0;
 
## 10.2进程和僵尸进程

- 产生僵尸进程的原因

  调用fork函数产生子进程的终止方式：1.传递参数并调用exit函数
                                    2.main函数中执行return语句并返回值
  向exit函数传递的参数值和main函数的return语句返回的值都会传递给操作系统。而操作系统不会销毁子进程，××直到把这些值传递给产生该子进程的父进程×。处于这个状态下的进程就是僵尸进程。将子进程变成僵尸进程的正是操作系统。

- 如何销毁：
   应该向创建子进程的父进程传递子进程的exit参数值或return语句的返回值。
   操作系统不会主动把这些参数传递给父进程。只有父进程主动发起请求（函数调用）时，操作系统才会传递该值。即如果父进程未主动要求获得子进程的结束状态值，操作系统将一直保存，并让子进程长时间处于僵尸进程状态。

zombie.c是僵尸进程的原因：运行之后的结果如下所示：
ubuntu@ubuntu:~/code/tcpip/chart10$ ./zombie 
Child Process ID: 77260 
Hi, I am a child process
End child process

程序开始运行后，将在如上所示的状态暂停，跳出这种状态前（30秒内），应验证子进程是否为僵尸进程，如下所示：
ubuntu@ubuntu:~$ ps au
USER        PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root       3791  0.0  0.0  23008   252 tty1     Ss+  5月29   0:00 /sbin/age
root       3799  0.0  3.3 498680 66984 tty7     Ssl+ 5月29   0:54 /usr/lib/
ubuntu    75574  0.0  0.1  29832  3744 pts/4    Ss+  10:29   0:00 /bin/bash
ubuntu    75584  0.0  0.1  29796  3820 pts/18   Ss   10:29   0:00 /bin/bash
ubuntu    75594  0.0  0.1  29768  2576 pts/19   Ss   10:29   0:00 /bin/bash
ubuntu    75604  0.0  0.1  29768  2668 pts/20   Ss+  10:29   0:00 /bin/bash
ubuntu    77259  0.0  0.0   4356   724 pts/18   S+   14:34   0:00 ./zombie
ubuntu    77260  0.0  0.0      0     0 pts/18   Z+   14:34   0:00 [zombie] <
ubuntu    77261  0.0  0.1  44432  3052 pts/19   R+   14:34   0:00 ps au

可以看出PID为77260的进程为僵尸进程（Z+）。另外，经过30秒的等待后，PID为10976的父进程和之前的僵尸子进程同时销毁。

