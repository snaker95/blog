# HTTP Keep-Alive 作用

## 作用:

Keep-Alive: 使客户端到服务器的连接持续有效, 当出现对服务器的后继请求时, Keep-Alive功能避免了建立或者重新连接. Web服务器, 基本上都支持HTTP Keep-Alive.



## 缺点:

对于提供静态内容的网站来说, 这个功能通常很有用. 但是, 对于负载较重的网站来说, 虽然为客户保留打开的连接有一定的好处, 但同样影响了性能, 因为在处理暂停期间, 本来可以释放的资源仍旧被占用. 当Web服务器和应用服务器在同一台机器上运行时, Keep-Alive功能对资源利用的影响尤其突出.



## 解决:

Keep-Alive: timeout=5, max=100

* timeout: 过期时间5秒(对应httpd.conf里的参数是: KeepAliveTimeout);
* max是最多100次请求, 强制断掉连接;