#  TCP/IP网络编程

## 套接字

### #include<sys/socket.h>

### 创建套接字

- int socket(int domain,int type,int protocol)

	- 成功：1
	- 失败：0

### 分配地址信息（IP&port）

- int bind(int sockfd, struct sockaddr *myaddr, sockelen_t addrlen)

	- 成功：1
	- 失败：0

### TCP套接字

服务器端（监听）套接字

- 转为可接收请求状态

	- int listen(int sockfd, int backlog)

		- 成功：1
		- 失败：0

- 受理连接请求

	- int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen )

		- 成功：自动创建的socket的描述符
		- 失败：-1

- 客户端套接字

	- 请求连接

		- int connect(int sockfd, struct sockaddr *serv_addr, sockelen_t addrlen)

			- 成功：1
			- 失败：0

		- 此函数完成IP和port的分配

### UDP套接字

- ssize_t sendto(int sock , void *buff, size_t nbytes, int flags, struct sockaddr *to, socklen_t addrlen);

	- 成功：传输字节数
	- 失败：-1
	- sock: UDP 套接字描述符
	- buff：待传输数据的缓冲区地址值
	- nbytes: 待传输数据长度，以字节为单位
	- flags: 可选参数，若省略默认为0
	- to : 存有目标地址信息的sockaddr结构体变量的地址值
	- addrlen: 传递给参数to的地址值结构体变量长度
	- 首次调用sendto时分配IP和端口

- ssize_t recvfrom(int sock , void *buff, size_t nbytes, int flags, struct sockaddr *from, socklen_t addrlen);

	- 成功：接收的字节数
	- 失败：-1
	- sock: UDP 套接字描述符
	- buff：接收数据的缓冲区地址值
	- nbytes: 可接收的最大字节数据长度，必须小于buff指定的缓冲区大小
	- flags: 可选参数，若省略默认为0
	- to : 存有发送端地址信息的sockaddr结构体变量的地址值
	- addrlen: 保存参数from的结构体变量长度的变量地址

## 地址族
与数据序列

### 网络地址

- ipv4

	- A 类

		- 一字节网络地址，首字节0~127，首位0

	- B类

		- 两字节网络地址，首字128~191，首位10

	- C类

		- 三字节网络地址，首字节192~223，首位110

	- D类

		- 四字节网络地址——广播地址

- ipv4地址结构体

	- struct sockaddr_in

		- sa_family_t  sin_family  //地址族

			- AF_INET  : ipv4地址族
			- AF_INET6  : ipv6地址族
			- AF_LCOAL ：本地通信中采用的UNIX协议的地址族

		- uint16_t  sin_port  //16位TCP/UDP端口号

			- 以网络字节序保存

		- struct  int_addr  sin_addr  //32位IP地址

			- in_addr_t s_addr //32位IP地址
			- 以网络字节序保存

		- char sin_zero[8]  //不使用

			- 为使sock_addr_in与sockaddr保持大小一致而插入

	- struct sockaddr

		- char sa_data[14]
		- sa_family_t  sin_family  //地址族

- 网络字节序

	- 大端序

		- 高位字节存储低位地址
		- 转换函数

			- unsigned short htons(unsigned short)
			- unsigned short ntohs(unsigned short)
			- unsigned long htonl(unsigned long)
			- unsigned long ntohl(unsigned long)

- （服务器端）
初始化与分配

	- 点分十进制的转化

		- #include<arpa/inte.h>
		- in_addr_t inet_addr(const char *string)

			- 成功：32位大端IP地址
			- 失败：INADDR_NONE

		- int inet_aton(const char *string, struct in_addr * addr)

		  string 含有需转换的IP地址信息的字符串地址值
		  addr 将保存转换结果的in_addr结构体变量的地址值

			- 成功：1
			- 失败：0

	- 整型IP地址到点分十进制

		- char * inet_ntoa(struct in_addr adr)

			- 成功：点分十进制IP字符串
			- 失败：-1

		- #include<arpa/inte.h>

	- 初始化

		- struct sockaddr_in addr;
		- char * serv_ip= "211.217.168.13";
//声明IP地址字符串

			- 可使用INADDR_ANY代替

		- char * serv_port = "9190";
//声明端口字符串
		- memset(&addr, 0, sizeof(addr));
//初始化结构体addr值为全0
		- addr.sin_family=AF_INET;
//指定地址族
		- addr.sin_addr.s_addr = inet_addr(serv_ip);
//基于字符串的IP地址初始化

			- addr.sin_addr.s_addr = inet_addr(INADDR_ANY);

		- addr.sin_port = htons(atoi(serv_port));
//基于字符串的端口号初始化

	- bind()函数负责将初始化信息分配给套接字

### NIC（网络接口卡）

- 端口号（16位，0-65535）
但9-1023一般具有特定用途

### 距离一致性

## TCP-Based 
server/client

### TCP 

- 数据收发无边界
（无大小限制）

	- write(): 将数据移至输出缓冲区
	- read(): 将数据从输入缓冲区读入

- 滑动窗口协议

	- 保证不会发生输入缓冲区溢出的情况

- 套接字以全双工方式工作
- 建立连接

	- Three-way handshaking 
三次握手协议

		- 数据包编号并确认（收到），防止数据包丢失

			- 可靠传输

		- ACK号的增量为传输的数据字节数

			- ACK= SEQ + dataBytes +1 
			- 为了确保所有的字节都正确传输了
			- ACK号码表示该号码之前的数据都以正确收到

		- 数据包内容

			- SEQ
			- ACK

- 数据传输

	- 数据包内容

		- SEQ+数据
		- ACK

- 超时重传
- 断开连接

	- Four-way handshaking
四次握手协议

		- 数据包内容

			- FIN /ACK
			- SEQ
			- ACK

### 面向连接

- 多套接字

## UDP-Based 
server/client

### UDP 

- 数据报
- 未连接套接字

	- 未注册目标地址信息的UDP套接字

- 已连接套接字

	- 已注册目标地址信息的UDP套接字
	- 创建

		- 对UDP套接字使用connect（）函数
		- 并不意味着建立连接，只是向该socket注册地址信息
		- 已连接UDP套接字不但可以使用sendto和recvfrom, 还可以使用write和read

### 无连接

- 单套接字

## 优雅地断开
套接字连接

### 基于TCP的半关闭

- 可接收但无法传输
- 可传输但无法接收
- 连接一旦建立则存在
输入流和输出流两种

	- 断开流

		- close()一次性断开两个流
		- int  shutdown(int sock, int howto)
关闭一个流

			- sock: 需要断开的套接字文件描述符
			- howto: 传递断开方式信息

				- SHUT_RD: 断开输入流
				- SHUT_WR: 断开输出流
				- SHUT_RDWR:同时断开I/O流

## IP地址和域名
之间的转换

### DNS服务器

### #include<netdb.h>

### 利用域名获取IP地址

- struct hostent  *  gethostbyname(const char * hostname);

	- 成功：hostent变量地址
	- 失败：NULL

- struct hostent

	- char * h_name; //official name
	- char  ** h_aliases; //alias list
	- int h_addrtyape; //host address type 
	- int h_length; //address length
	- char   **  h_addr_list;  //address list
实际指向in_addr结构体

### 利用IP地址获取域名

- struct hostent  *  gethostbyaddr(const char * addr, socklen_t len, int family);

	- 成功：hostent变量地址
	- 失败：NULL

- addr : 含有IP地址信息的in_addr结构体指针
- len: 向第一个参数传递地址信息的字节数，ipv4=4, ipv6=16
- family : 传递地址族信息，IPV4=AF_INET,IPV6=AF_INET6

## 套接字的多种可选项

### SOL_SOCKET
//通用选项

- SO_TYPE: 查看套接字类型，只读选项
- SO_RCVBUF: 输入缓冲区大小选项
- SO_SNDBUF: 输出缓冲区大小选项
- 先断开套接字的一段其占用端口会经历Time-wait 状态
- SO_RESUMEADDR

	- 0：time_wait端口不可被分配
	- 1：time_wait端口可被分配

### IPPROTO_IP
//IP选项

### IPPROTO_TCP
//TCP选项

### 设置可选项

- #include<sys/socket.h>
- int  getsockopt(int sock, int level, int optname, void * optval, socklen_t * optlen);

	- sock: 套接字描述符
	- level： 可选项的协议层
	- optname: 可选项名
	- optval:get到的值的缓冲地址
	- optlen: opeval的长度

- int setsockopt(int sock, int level, int optname, const void * optval, socklen_t   optlen);

	- sock: 套接字描述符
	- level： 可选项的协议层
	- optname: 可选项名
	- optval:要更改的值的缓冲地址
	- optlen: opeval的长度

### Nagle算法：最大限度缓冲后再发送数据包

- 禁用：将TCP_NODELAY改为1

## 多进程服务器端 

### #include <unistd.h>

### pid_t fork(void)
//创建调用的进程的副本

- 成功：进程ID
- 失败：-1

### 僵尸进程

- 产生

	- 子进程通过exit()推出或者mian()函数中的return返回时子进程不会被销毁直到返回值传递给了父进程

- 销毁僵尸进程
父进程主动获取子进程返回

	- #include<sys/wait.h>
	- pid_t wait(int * statloc)

		- 成功：子进程ID
		- 失败：-1
		- 该函数的返回值存在&statloc

			- WIFEXITED(statloc): 子进程正常结束时为1
			- WEXITEDSTATUS(statloc):子进程返回值

		- 若没有未终结的子进程，程序将被阻塞（blocking）直到有子进程可被终结，因此需谨慎调用该函数

	- pid_t waitpid(pid_t pid,
 int * statloc, int options)

		- 成功：子进程ID
		- 失败：-1
		- pid: 待终结子进程ID，传递-1时代表任意子进程
		- statloc: 同wait()函数
		- options： 指定为WNOHANG时，程序不会被阻塞，无子进程时返回0并退出函数
		- 该函数的返回值存在&statloc

			- WIFEXITED(statloc): 子进程正常结束时为1
			- WEXITEDSTATUS(statloc):子进程返回值

### 信号处理

- #include<signal.h>
- void(*signal(int signo, void(*func)( int)))(int);
//信号注册函数 
//返回值类型为函数指针

	- 三种signo

		- SIGALRM
		- SIGINT
		- SIGCHILD

	- unsigned int alarm(unsigned int seconds)

		- seconds=正整数x: x秒后产生一个SIGALRM信号
		- seconds=0: 之前对SIGALRM信号的预约被取消

- 信号处理函数
即信号处理器
- 发生信号时将唤醒由于sleep()函数而进入阻塞状态的进程
- int sigaction(int signo, const struct sigaction * act , struct sigaction   * oldact);

	- 成功：0
	- 失败：-1
	- struct sigaction 

		- void(*sa_handler)(int)

			- 保存信号处理函数指针

		- sigset_t sa_mask

			- 使用sigemptyset()函数可将sa_mask
的所有位初始化为0

		- int sa_flags

	- signo:信号信息
	- act : 信号处理器
	- oldact: 获取之前注册的信号处理函数指针，不需要时传递0

### 分割TCP的I/O程序

## 多进程客户端实现

### 使用fork()复制进程再将write()和read()放在不同的进程中调用即可

## 第十一章
进程间通信

### 需要有进程间共享内存

### 通过管道（PIPE)实现进程间通信

- 管道并非属于进程的资源，而是属于操作系统
- 创建管道

	- #include<unistd.h>
int pipe(int filedes[2])

		- 成功：0
		- 失败：-1
		- filedes[0]:接受数据文件描述符，即管道出口
		- filedes[1]:发送数据文件描述符，即管道入口
		- 通过fork()将入口/出口的文件描述符传递给子进程

	- 单管道实现双向通信的任务几乎是不可能完成的，
解决方案是采用双通道

### 运用进程间通信

*XMind: ZEN - Trial Version*