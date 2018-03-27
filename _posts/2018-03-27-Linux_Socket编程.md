### [简介](https://www.cnblogs.com/skynet/archive/2010/12/12/1903949.html)
#### 本地进程之间如何通信
* 消息传递(管道、FIFO、消息队列)
* 同步(互斥量、条件变量、读写锁、文件和写记录锁、信号量)
* 共享内存(匿名的和具名的)
* 远程过程调用


#### 网络中的进程之间如何通信

* 在网络通信中如何唯一的标识一个进程？([端口](https://www.zhihu.com/question/22577025))

		在本地可以通过进程PID来唯一标识一个进程，但是在网络中不行，我们可以用TCP/IP协议来解决这个问题。  
		网络层的“IP地址”可以唯一标识网络中的主机。
		传输层的“协议+端口”可以唯一标识主机中的应用程序(进程)
		因此可以利用(IP地址，协议，端口)就可以标识网络中的进程。网络中的进程通信就可以利用这个标志与其他进程交互。

#### 什么是Socket

socket起源于Unix，而Unix/Linux基本哲学之一就是“一切皆文件”，都可以用**打开open -> 读写write/read -> 关闭close**模式操作，而Socket就是这个模式的一个实现。Socket即是一种特殊的文件，一些socket函数就是对其进行的操作(读/写IO、打开、关闭)

#### Socket基本操作

	int socket(int domain, int type, int protocol);

socket函数对应于普通文件的打开操作，普通文件的打开操作返回一个文件描述字，而socket()用于创建一个socket描述符， 它**唯一标识一个socket**，这个socket描述字和文件描述字一样，后面的操作会用到它，把它作为参数，通过它来进行一些读写操作。  

* domain： 协议族(family)。有AF\_INET、AF\_INET6、AF\_LOCAL等。协议族决定了socket的地址类型，在通信中必须采用对应的地址。如AF\_INET决定了要用ipv4地址(32位)和端口号(16位)的组合。

当我们调用socket创建一个socket时，返回的socket描述字存在于协议族空间中，没有一个具体的地址。如果想要给它赋值一个地址，就必须要调用bind()函数，否则就当调用connect()、listen()时系统会自动随机分配一个端口。

	int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen)

bind函数时把一个地址族中的特定地址赋给socket,例如AF\_INET、AF\_INET6就是把一个ipv4或ipv6地址和端口号组合赋给socket。   
参数：

* sockfd:socket描述字，是通过socket()函数创建的，唯一标识了一个socket，bind函数就是将这个描述字绑定一个名字。  
* addr： 一个const struct sockaddr*指针，指向要绑定给sockfd的协议地址。   

通常服务器在启动的时候都会绑定一个众所周知的地址（如ip地址+端口号），用于提供服务，客户就可以通过它来接连服务器；而客户端就不用指定，有系统自动分配一个端口号和自身的ip地址组合。这就是为什么通常服务器端在listen之前会调用bind()，而客户端就不会调用，而是在connect()时由系统随机生成一个。  

	int listen(int sockfd, int backlog);

作为一个服务器，在调用socket()、bind()之后就会调用listen()来监听这个socket，如果客户端这时调用connect()发出连接请求，服务器端就会接收到这个请求。  
参数：  

* sockfd： 要监听的socket描述字
* backlog： 相应的socket可以排队的最大连接的个数。

socket()函数创建的socket默认是一个主动类型的，listen函数将socket变为被动类型的，等待客户的连接请求。

	int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
参数：  

* sockfd: 客户端的socket描述字
* addr： 服务器的socket地址
* addrlen： socket地址的长度。

客户端通过调用connect函数来建立于TCP服务器的连接。  

	int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);

1、TCP服务器依次调用socket()、bind()、listen()后，就会监听指定的socket地址。  
2、TCP客户端依次调用socket()、connect()后就向TCP服务器发送了一个连接请求。  
3、TCP服务器监听到这个请求后，会调用accept()函数接受请求，这样连接就建立了。之后就可以开始网络I/O操作，类似于普通文件的读写I/O操作。

参数：   

*  sockfd: 服务器的socket描述字
*  addr： 用于返回客户端的协议地址。
*  addrlen： 协议地址的长度。  

如果accept成功，则返回值是由**内核自动生成的一个全新的描述字**代表与返回客户的TCP连接。  

	注意：accept的第一个参数为服务器的socket描述字，是服务器开始调用socket()函数生成的，称之为 监听socket描述字。
	accept返回的是 已连接的socket描述字。

**一个服务器通常仅仅只创建一个监听socket描述字，它在服务器的生命周期一直存在，而内核为每个由服务器进程接受的客户连接创建了一个已连接socket描述字，当服务器完成了对某个客户的服务，相应的已连接socket描述字就被关闭**  

----------

	ssize_t  sendmsg(int sockfd, const struct msghdr *msg, int flags)
	ssize_t  recvmsg(int sockfd, struct msghdr *msg,  int flags);

这两个是读取和发送的函数，和普通文件操作一样。
