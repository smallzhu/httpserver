# myHttpServer

# 简介
这是一个由C++语言编写的Web服务器，目前支持GET方法请求静态资源。支持解析POST方法。支持HTTP长连接。实现了简易CGI功能。

# 开发环境

- 操作系统: Ubuntu 20.04
- 编译器: g++ 9.3.0
- 版本控制: git
- 自动化构建: cmake

# 核心技术

- 基于epoll的IO复用机制实现Reactor模式，采用边缘触发（ET）模式，和非阻塞模式
- 针对长连接，epoll使用EPOLLONESHOT保证一个长连接在任意时刻都只被一个线程处理
- 线程模型将划分为主线程和工作线程，主线程接收客户端连接（accept），然后将连接放入任务队列后用条件变量通知休眠的工作线程。工作线程负责和客户端进行数据的交互
- 利用线程池管理所有的工作线程，避免其不必要构造和析构
- 基于链表+哈希表实现定时器功能，定时剔除不活跃连接。定时器的插入、更新和删除时间复杂度都为O(1)
- RAII技术管理程序中的互斥锁、条件变量等资源
- 统一信号源。利用匿名管道接受所有的进程信号，然后由主线程中的epoll统一处理
- 异步日志功能。通过在内存中构建循环缓冲区，利用专用后端线程执行文件写入从而将文件IO从前端线程中分离。

# [开发过程中遇到的问题](开发过程中遇到的问题.md)

# [TODO LIST](TODOList.md)

# [BenchMark](benchMark.md)