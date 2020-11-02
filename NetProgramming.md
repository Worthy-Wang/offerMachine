- [阻塞、非阻塞、同步、异步的区别？](#阻塞非阻塞同步异步的区别)
- [IO的五种模型？](#io的五种模型)
- [epoll/poll/select 的区别？](#epollpollselect-的区别)
- [epoll中ET，LT的区别？](#epoll中etlt的区别)
- [Reactor/Proactor模式是什么？](#reactorproactor模式是什么)
- [大规模连接上来后，并发模型怎么设计？](#大规模连接上来后并发模型怎么设计)
- [select返回可读，但是使用read一直只能读到0字节，什么情况？](#select返回可读但是使用read一直只能读到0字节什么情况)
- [connect函数长时间阻塞该怎么办？](#connect函数长时间阻塞该怎么办)
- [socket什么情况下可读？](#socket什么情况下可读)
- [UDP通信中调用connect有什么作用？和TCP连接中的connect有什么区别？](#udp通信中调用connect有什么作用和tcp连接中的connect有什么区别)
- [keepalive是什么？如何使用？](#keepalive是什么如何使用)
- [Socket编程中，如果client断开，服务器如何快速知道？](#socket编程中如果client断开服务器如何快速知道)

<br>

-------------
## 阻塞、非阻塞、同步、异步的区别？
**阻塞/非阻塞** 关注的点是 **进程/线程 是否需要等待数据**；阻塞代表需要等待数据，非阻塞代表不需要等待数据。

**同步/异步** 关注的点是 **主动/被动 读取数据**；同步代表需要主动读取数据，异步代表被动读取数据。

**同步有阻塞和非阻塞之分，异步一定是非阻塞的。**

**举例说明**：
假设老实人小王去书店买书，然而书店没有小王想买的书。。。。。。

* 小王一直在书店等着，直到书店有书，这是 **同步阻塞**。

* 小王隔一段时间就去书店看看有没有书，这是 **同步非阻塞**。

* 老板在书店有书的时候给小王打电话，让小王过来拿书，这是 **信号驱动IO同步非阻塞**

* 老板在书店有书的时候直接把书寄到小王家里，这是 **异步非阻塞**。

此处的小王对应**用户进程**，书对应**数据**，买书对应**系统调用**，书店老板对应**内核**，。

<br>

-------------
## IO的五种模型？
IO的五种模型：
* **阻塞IO**
* **非阻塞IO**
* **IO多路复用**
* **信号驱动IO**
* **异步IO**

讲解五种IO模型的区别，仍然用上一个问题中小王去书店买书的例子说明。

1. 阻塞IO

小王一直在书店等着，直到书店有书，这是 同步阻塞IO。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201102094349264.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1dvcnRoeV9XYW5n,size_16,color_FFFFFF,t_70#pic_center)
2. 非阻塞IO

小王隔一段时间就去书店看看有没有书，这是 同步非阻塞IO。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201102094407675.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1dvcnRoeV9XYW5n,size_16,color_FFFFFF,t_70#pic_center)
3. 多路复用IO

多路复用IO比较特别，在实际进行read/recv时并不会阻塞，但在select/poll/epoll时会阻塞。
小王来书店买书，但有一排书架，并不知道哪个书架上面有书，老板直接告诉小王哪个书架上面有书，这是多路复用IO。多路IO复用也属于**同步非阻塞**的一种。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201102094421630.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1dvcnRoeV9XYW5n,size_16,color_FFFFFF,t_70#pic_center)
4. 信号驱动IO

老板在书店有书的时候给小王打电话，让小王过来拿书，这是 信号驱动IO，也是同步非阻塞的一种。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020110209444168.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1dvcnRoeV9XYW5n,size_16,color_FFFFFF,t_70#pic_center)
5. 异步IO

老板在书店有书的时候直接把书寄到小王家里，这是 异步非阻塞。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201102094455678.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1dvcnRoeV9XYW5n,size_16,color_FFFFFF,t_70#pic_center)

**以上1~4都属于同步，5属于异步。**

<br>

----------
## epoll/poll/select 的区别？

select，poll，epoll都属于 **IO多路复用**，在本质上都属于 **同步非阻塞IO**。
IO多路复用的 **特点在于可以同时监听多个描述符**，一旦有描述符就绪，就可以进行相应的读写操作。

* **select**

select虽然可以同时监听多个描述符，但是select有三个比较大的缺点：

1. 每一次调用select，都需要将fd集合从用户态拷贝到内核态，若fd数量很大，将会造成很大的开销；

2. 每一次调用select，都需要在内核轮询遍历所有传递进来的fd集合，若fd数量很大，将会造成很大的开销；

3. select最多只能同时监听1024个描述符，显然不够用。


* **poll**

poll的内部实现原理和select类似，不过poll没有最大连接数的限制，因为它是基于链表存储的。



* **epoll**

epoll在select和poll上面进行了改进，解决了上面的三个问题。

1. 针对第一个缺点：epoll先通过`epoll_create`创建句柄，再使用`epoll_ctl`将fd集合拷贝进入内核，**保证了fd集合只会拷贝一次**。

2. 针对第二个缺点：epoll在`epoll_ctl`的时候将所有的fd集合拷贝进入内核之后，将其放入等待队列，并为每个fd设置一个回调函数，当有fd描述符就绪时，会调用回调函数，将其加入`就绪队列链表`。`epoll_wait` 的工作实质上就是**主动在就绪链表中查看是否有就绪的fd描述符**。这样直接从轮询遍历fd集合的`O(N)`时间复杂度变为了直接读取就绪队列链表的`O(1)`时间复杂度。

3. 针对第三个缺点：**epoll没有监听描述符数量的上限**，具体数目可以通过 `cat /proc/sys/fs/file-max` 命令查看



**总结**

* select和poll在每一次调用时都需要将所有的fd集合拷贝进入内核态，并以轮询遍历该fd集合，这两点会造成很大的开销；并且能够同时监听的最大数量为1024。

* epoll在`epoll_craete`时创建句柄；并在`epoll_ctl`时将文件描述符拷贝到内核态，再将其加入等待队列，设置相应的回调函数，当fd描述符就绪时，调用回调函数，将该描述符加入就绪链表；`epoll_wait`的作用就是主动在就绪链表中查看是否有就绪的fd描述符；epoll没有最大监听描述符的限制。


<br>

----------

## epoll中ET，LT的区别？
* **LT(Level Triggered)**

LT水平触发是缺省的工作方式，**只要缓冲区有数据就会触发**，支持`block` 和 `non-block socket`。

优点：缓冲区有数据内核就会一直通知，保证了数据的完整性，使得编程不容易出错；

缺点：因为缓冲区有数据时内核就会一直通知，导致内核态频繁切换到用户态，使得内核使用效率低下。

>**select 与 poll都是使用的LT。**


* **ET(Edge Triggered)**

边沿触发的**高速工作模式**，**只有数据到来才触发，不管原缓冲区中是否还有数据**，只支持`non-block socket`，。

优点：内核只会为就绪的描述符发送一次通知，大大的减少了内核的资源浪费。

缺点：buffer有可能无法一次性取出缓冲区中的所有数据，导致缓冲区中有数据剩余。

<br>

------------
## Reactor/Proactor模式是什么？
Reactor模型与Proactor模型都是**并发服务器模型**，Reactor模型属于同步IO，Proactor模型属于异步IO。

* **Reactor**

Reactor模型处理请求的流程：
1.	Reactor首先需要注册相关 事件处理器（Handler）。

2.	事件分离器监听就绪事件。（如epoll）

3.	当就绪事件发生时，调用相应的事件处理器，也就是回调函数。

**总结一下，就是 注册->监听->调用回调函数。**

<br>

* **Proactor**

Proactor模型处理数据的流程：
1. Proactor首先初始化异步程序，并注册相关的事件处理器（Handler）。

2. 事件分离器（如epoll）监听事件，但注意并不是监听 **就绪** 事件，而是监听 **完成** 事件，**这是Proactor不同的一点**。

3. 事件分离器在等待 **完成** 事件的时候，会调用内核将数据放到 **缓冲区** 中，当 **完成事件** 发生时，调用事件处理器 直接从 **内存缓冲区** 读写数据。**这也是Proactor不同的一点，并不会进行实际的IO读写操作，而是直接从内存缓冲区读取数据**。

**总结一下：仍然是 异步+注册->监听完成事件->调用回调函数处理缓冲区。**

<br>

* **总结 Reactor VS Proactor**

1.	Reactor关注的是就绪事件；Proactor关注的是完成事件。

2.	Reactor 需要进行实际的读写操作；Proactor不会进行实际的读写操作，而是直接从内存缓冲区读写。

3.	Reactor编程相对简单；Proactor编程相对复杂，且由于需要稳定的内存缓冲区 造成不稳定性，同时目前Linux下的异步IO仍然是使用epoll实现的。

**所以，目前Linux下实现的高并发网络编程都是以Reactor模型为主。**


>**相关知识补充说明**：
Reactor的常用模型有以下几种：
>1. **单Reactor单线程模型**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201102104014700.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1dvcnRoeV9XYW5n,size_16,color_FFFFFF,t_70#pic_center)
>2. **单Reactor多线程模型**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201102104026760.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1dvcnRoeV9XYW5n,size_16,color_FFFFFF,t_70#pic_center)
>3. **主从Reactor多线程模型**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201102104043603.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1dvcnRoeV9XYW5n,size_16,color_FFFFFF,t_70#pic_center)

>**Reactor模型中有三大角色**：
>* **Reactor**：负责监听事件，当有事件就绪时调用相应的回调函数。（事件就绪分为 连接就绪，读就绪，写就绪）
>* **Acceptor**：负责处理客户端的新连接，并将文件描述符传递给Reactor。
>* **Handler**：事件处理器，是一种通过回调函数（钩子函数）实现的事件处理机制，当handle（文件描述符）上有事件发生时，便会执行该回调函数。


>**关于英文资料中Reactor/Proactor模型专业名词解释**：
>* **Handle**：Handle在Linux中称为文件描述符，在Windows中称句柄，含义相同。Handle是事件的发源地，发生在handle上面的事件可以有connection（连接），read for read（读就绪），read for write（写就绪）。
>* **Synchronous Event Demuliplexer**：同步事件分离器，本质上属于系统调用。如IO多路复用 select/poll/epoll等方法。
>* **Event Handler**：事件处理器，是一种通过回调函数（钩子函数）实现的事件处理机制，当handle上有事件发生时，便会执行该回调函数。
>* **Dispatcher**：初始分发器，也就是reactor，提供了注册，删除，转发Event Handler的方法。当Synchronous Event Demuliplexer 检测到 handle 上面有事件发生时，便会通知dispatcher调用特定的回调函数。

<br>

----------
## 大规模连接上来后，并发模型怎么设计？
采用Reactor + threadpool模型并发网络编程模型。其中epoll可以直接采用**libevent**。

<br>

---------
## select返回可读，但是使用read一直只能读到0字节，什么情况？
说明对端已经断开，read才会一直读到0字节，那么不用再监听该描述符。

<br>

---------
## connect函数长时间阻塞该怎么办？
1. 将socket设置为非阻塞模式，如果服务器发生错误，那么执行connect的时候就可以立刻返回。再通过select来测试socket是否可写，用来判断connect是否成功连接上。
2. 通过信号处理函数设置定时器。

<br>

----------------
## socket什么情况下可读？
1.  当有新的连接时，可以通过accept返回新的套接字；

2. 只要缓冲区中有数据就默认可读，并通过read/recv读取时会返回读取数据的字节数；

3. 对方发送来FIN断开时，会不断返回0；

4. 当socket出现错误时可读，并且会返回-1。



<br>

----------
## UDP通信中调用connect有什么作用？和TCP连接中的connect有什么区别？
1. UDP进行connect后并不会建立三次握手，而是建立一对一的连接，但UDP连接的本质没有变，仍然是不可靠的连接。

2. UDP中调用connect会在内核中将对端的IP,port记录下来。记录之后，也就可以使用read/write，recv/send函数。并且UDP可以多次进行connect，实质上也就是改变对端的IP，port；但是TCP连接只能进行一次connect。

3. UDP中使用connect可以提高效率，没有使用connect的UDP则是在每一次发送报文后需要再断开并重新连接。
	

<br>

------------
## keepalive是什么？如何使用？
**Keepalive是TCP中一个检测死连接的心跳包保活机制，原理就是TCP在空闲了一段时间后会发送数据给对方。**

1. 如果对端会发送ACK，那么就认为对端是存活的；

2. 如果对端就会发送RST包，那么TCP就会撤销该连接；

3. 如果对端不发送ACK，RST，那么TCP在一段超时重传之后自动撤销连接。

Keepalive发送的时间可以通过 cat /proc/sys/net/ipv4/tcp_keepalive_time 查看，通常为7200（2小时）。

<br>


-------------
## Socket编程中，如果client断开，服务器如何快速知道？
1. **正常断开**：通过recv/read等系统调用会不断接收到0字节。

2. **异常断开**：使用心跳包（keepalive机制）；也就是server在一段时间没有接收到client的数据时，会向client发送keepalive packet，根据client的回复做出判断。

>**补充：Keepalive 相关内容见上一个问题。**



