# WebServer
高并发服务器
利用muduo+Reactor模型（多Reactor多线程模型），线程池和epoll非阻塞IO多路复用实现高并发网络服务器。  

非常有意思的实现框架，可以使刚接触后台服务器开发的人来讲在整体上对服务器实现处理请求和返回相应有一个清晰的认识。  

梳理一下  

1.EventLoop  
每个muduo网络库有一个事件驱动循环线程池EventLoopThreadPool  
每个线程池中有多个事件驱动线程EventLoopThread  
每个线程运行一个EventLoop事件循环  
每个EventLoop事件循环包含一个io复用Poller，一个计时器队列TimerQueue  
每个Poller监听多个Channel，TimerQueue其实也是一个Channel  
每个Channel对应一个fd，在Channel被激活后调用回调函数  
每个回调函数是在EventLoop所在线程执行  
所有激活的Channel回调结束后EventLoop继续让Poller监听

