# epoll

## 程序接口
```c
int epoll_create(int size);
```
在内核中创建epoll实例并返回一个epoll文件描述符。 在最初的实现中，调用者通过 size 参数告知内核需要监听的文件描述符数量。
如果监听的文件描述符数量超过 size, 则内核会自动扩容。而现在 size 已经没有这种语义了，但是调用者调用时 size 依然必须大于 0，以保证后向兼容性。

```c
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
```
向 epfd 对应的内核epoll 实例添加、修改或删除对 fd 上事件 event 的监听。op 可以为 EPOLL_CTL_ADD, EPOLL_CTL_MOD, EPOLL_CTL_DEL 分别对应的是添加新的事件，
修改文件描述符上监听的事件类型，从实例上删除一个事件。如果 event 的 events 属性设置了 EPOLLET flag，那么监听该事件的方式是边缘触发。

```c
int epoll_wait(int epfd, struct epoll_event *events, int maxevents, int timeout);
```
当 timeout 为 0 时，epoll_wait 永远会立即返回。而 timeout 为 -1 时，epoll_wait 会一直阻塞直到任一已注册的事件变为就绪。
当 timeout 为一正整数时，epoll 会阻塞直到计时 timeout 毫秒终了或已注册的事件变为就绪。因为内核调度延迟，阻塞的时间可能会略微超过 timeout 毫秒。

## 底层
1. 进程调用epoll_create方法，Linux内核会创建一个eventpoll结构体
```c
struct eventpoll{
    ....
    /*红黑树的根节点，这颗树中存储着所有添加到epoll中的需要监控的事件*/
    struct rb_root  rbr;
    /*双链表中则存放着将要通过epoll_wait返回给用户的满足条件的事件*/
    struct list_head rdlist;
    ....
};
```
+ 一个红黑树，存着监控的事件
+ 一个双向链表，存着要返回给用户满足条件的事件

2. 进程调用epoll_ctl方法，所有添加到epoll中的事件都会创建一个epitem结构体, 并与设备(网卡)驱动程序建立回调ep_poll_callback，它会将发生的事件添加到rdlist双链表中
```c
struct epitem{
    struct rb_node  rbn;//红黑树节点
    struct list_head    rdllink;//双向链表节点
    struct epoll_filefd  ffd;  //事件句柄信息
    struct eventpoll *ep;    //指向其所属的eventpoll对象
    struct epoll_event event; //期待发生的事件类型
}
```

3. 进程调用epoll_wait方法，检查eventpoll对象中的rdlist双链表中是否有epitem元素，如果rdlist不为空，则把发生的事件复制到用户态（文件描述符），清空rdlist，同时将事件数量返回给用户。

![epoll](picture/epoll.jpg)

## LT ET


[wiki](https://zh.wikipedia.org/wiki/Epoll)  
[参考1](https://zhuanlan.zhihu.com/p/92737875)  
[参考2](https://segmentfault.com/a/1190000018517562)