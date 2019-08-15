## 
# 20.2内核模式的同步方式

## 基于互斥量对象的同步

- 互斥量被某一线程获取时为non-signaled,释放的时候进入signaled状态。可以使用WaitSingleObject函数验证互斥量是否已分配。该函数调用结果有两种：

    - 调用后进入阻塞状态：互斥量对象已被其他线程获取，现在处于non-signaled
    - 调用后直接返回：其他线程未占用互斥量对象，现处于signaled状态。  **注意互斥量在WaitSingleObject函数返回后进入non-signaled**

- HANDLE Createmutex(LPSECURITY_ATTRIBUTES  lpMutexAttributes,BOOL bInitialOwner,LPCTSTR lpName);
  - 成功时返回创建的互斥量对象句柄，失败时返回NULL。
  -lpMutexAttributes:传递安全相关的配置信息
  - bInitialOwner：如果为TRUE，则创建出的互斥量对象属于调用该函数的线程，同事进入non-signaled状态；如果为FALSE,那么此时状态为signaled
  - lpName: 
  
- CloseHandle:销毁内核对象

- BOOL ReleaseMutex(HANDLE hMutex):释放互斥量的函数


  
  -基于互斥量的临界区保护代码
  
  WaitSingleObject（hMutex,INFINITE）;
  //临界区的开始
  //.....
  //临界区的结束
  ReleaseMutex(hMutex);
  
  WaitSingleObject函数使互斥量进入non-signaled状态，限制访问临界区，所以相当于临界区的门禁系统。相反，ReleaseMutex函数使互斥量重新进入signaled状态，所以相当于临界区的出口
  
  
  
  
  ## 基于事件的同步
- 事件同步与互斥量同步相比有很大不同，区别在于，该方式下创建的对象，可以在auto-reset模式以及manual-reset模式中任选其一
- CreateEvent(LPSECURITY_ATTRIBUTES lpMutexAttributes,BOOL bManualReset,BOOL bInitialState,LPCTSTR lpName);

    - bManualReset:传入TRUE时创建manual-reset模式的事件对象，传入FALSE时创建auto-reset模式的事件对象
    - bInitialState：传入TRUE时创建signaled状态的事件对象，传入FALSE时创建non-signaled状态的事件对象
    
- manual-reset模式下常用的两个函数：(这两个函数也是配合WaitSingleObject函数一起实现同步的)
    - ResetEvent//to the non-signaled
    - SetEvent  //to the signaled
