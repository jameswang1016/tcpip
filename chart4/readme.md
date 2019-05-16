4.2：实现基于TCP的服务端/客户端

tcp服务器端的默认函数调用顺序：
socket创建套接字-->bind分配套接字地址-->listen等待连接请求状态-->accept允许连接-->read/write数据交换-->close断开连接

hello_server.c 回顾hello world服务器端

tcp客户端的默认函数调用顺序：

socket创建套接字-->connect请求连接-->read/write交换数据-->close断开连接


问题： serv_addr.sin_port=htons(atoi(argv[1]));//??argv[1]是啥子意思？？
       回答：读取命令行输入的参数
       server里面 socket产生的套接字与accept的套接字到底啥子区别
        回答：https://blog.csdn.net/sinat_24368229/article/details/90264941

4.3实现迭代服务器端/客户端

echo_server.c：回声服务器端
echo_client.c:回声客户端

