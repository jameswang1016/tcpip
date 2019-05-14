字节序转换：
endian_conv.c展示的是主机字节序与网络字节序之间的转换：
ubuntu@ubuntu:~/jameswang/code/tcpip/chart3$ gcc endian_conv.c -o conv
ubuntu@ubuntu:~/jameswang/code/tcpip/chart3$ ./conv 
Host ordered port: 0x1234 
Network ordered port: 0x3412 
Host ordered address: 0x12345678 
Network ordered address: 0x78563412 



3.4网络地址的初始化与分配

inet_addr.c:点分十进制格式的字符串转换为32为整形数据并返回：
运行结果：
ubuntu@ubuntu:~/jameswang/code/tcpip/chart3$ ./addr 
Network ordered interger addr: 0x4030201 
Error occured!
从运行结果看出：inet_addr函数不仅可以把ip地址转化为32位整数型，而且可以检测无效的ip地址

inet_aton.c跟上面的功能相同，只不过该函数利用了in_addr结构体，且其使用频率更高！
运行结果：
ubuntu@ubuntu:~/jameswang/code/tcpip/chart3$ ./aton 
Network ordered interger addr: 0x4f7ce87f 

inet_ntoa.c：网络字节序整数型IP地址转化为我们熟悉的点分十进制字符串形式。
运行结果：
ubuntu@ubuntu:~/jameswang/code/tcpip/chart3$ ./ntoa 
Dotted-Decimal notation1: 1.2.3.4 
Dotted-Decimal notation2: 1.1.1.1 
Dotted-Decimal notation3: 1.2.3.4 
