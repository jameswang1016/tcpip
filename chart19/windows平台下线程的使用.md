# 19.1 内核对象

# 19.2基于windows的线程创建

- CreateThread函数
- _beginthreadex函数（线程安全的）

- uintptr_t _beginthreadex (void * security, unsigned stack_size,unsigned(* start_address)(void *),void * arglist,unsigned initflag,unsigned * thrdaddr);

  - 重点关注一下3，4，6形参
  - start_address：传递线程的main函数信息

  - arglist：调用main函数时传递的参数信息

  - thrdaddr：用于保存线程ID的变量地址值


# 内核对象的2种状态
## 内核对象的状态以及状态查看

- 线程内核对象中需要重点关注线程是否终止：
   - 终止：signaled状态
   - 未终止：non_signaled状态

 - 进程或线程终止时，操作系统会把相应的内核对象改为signaled状态
   
## WaitForSingleObject & WaitForMultipleObject

- DWORD WaitForSingleObject(HANDLE hHandle,DWORD dwMilliseconds);

    - hHandle:查看状态的内核对象句柄
    - dwMilliseconds: 以1/1000秒为单位指定超时，传递INFINITE时函数不会返回，直到内核对象变成signaled状态
    - 返回值：进入signaled状态返回 WAIT_OBJECT_0,超时返回WAIT——TIMEOUT
    
内核对象的两种模式    
- auto_reset模式：该函数由于发生时间(变为signaled状态)返回时，有时会把相应内核对象再次改为non-signaled状态。这种可以再次进入non-signaled状态的内核对象称为auto_reset模式。

- manual-reset模式：该函数由于发生时间(变为signaled状态)返回时，不会自动将内核对象改为non-signaled状态。这种内核对象称为auto_reset模式。



