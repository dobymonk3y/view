# 基础巩固
[toc]

## Redis 和 Memcache 的区别

```
共同点
```

- 都是内存数据库
- 都可以做一主多从的分布式集群

```
不同点
```

- Redis 支持`hash` `list` `set` `sorted set` `string`等多种类型数据,Memcache仅支持键值数据
- Redis只适用单核,Memcache可适用多核多线程.所以100k一下数据Redis性能好,100K以上数据Memcache性能好
- Redis支持持久化,Memcache不支持数据持久化
- Redis单个key存放的数据有1GB的先知,Memcache单个key存放的数据有1M的先知
- Redis利用单线程模型提供了事务的功能,Memcache提供了cas命令保证数据一致性
- Redis
- 支持主从复制模式做分布式,Memcach可以使用magent的一致性hash做分布式

> CAS（Check and Set）是一个确保并发一致性的机制，属于“乐观锁”范畴； CAS原理很简单：拿版本号，操作，对比版本号，如果一致就操作，不一致就放弃任何操作

## PHP有哪些超全局变量

`$_GET` `$_POST` `$_SESSION` `$_COOKIE` `$_ENV` `$_SERVER` `$_FILES` `$_REQUEST` `$_GLOBALS`

## PHP有哪些魔术常量

- `__DIR__`：文件所在的绝对路径，等价于 dirname(`__FILE__`)。
- `__FILE__`：文件的完整路径和文件名。
- `__NAMESPACE__`：当前命名空间的名称。
- `__CLASS__`：类的名称。如过需要获取后期绑定的类名，使用 `get_call_class()`。
- `__TRAIT__`：Trait 的名字。
- `__METHOD__`：类的方法名。
- `__FUNCTION__`：函数名称。
- `__LINE__`：文件中的当前行号。
- 
## Apache 和 Nginx 有什么区别?

- Apache支持模块动态和静态编译,Nginx只支持静态编译
- Nginx对FastCGI的支持要比Apache要好
- Nginx使用epoll连接方式,异步非阻塞,Apache使用select阻塞方式
- Nginx安装体积小,Apache安装体积大
- Nginx以线程方式处理请求,Apache以进程处理,Nginx对资源要求低
- Apache比Nginx稳定
- Apache模块多


## 事务的四大特性

1. 持久性
2. 原子性
3. 一致性
4. 隔离性

> 十元一个（持原一隔）

## InnoDB和MyISAM存储引擎有什么区别？
1. 存储结构
    MyISAM：每个MyISAM在磁盘上存储成三个文件。第一个文件的名字以表的名字开始，扩展名指出文件类型。.frm文件存储表定义。数据文件的扩展名为.MYD (MYData)。索引文件的扩展名是.MYI (MYIndex)。
    InnoDB：所有的表都保存在同一个数据文件中（也可能是多个文件，或者是独立的表空间文件），InnoDB表的大小只受限于操作系统文件的大小，一般为2GB。
2. 存储空间
    MyISAM：可被压缩，存储空间较小。支持三种不同的存储格式：静态表(默认，但是注意数据末尾不能有空格，会被去掉)、动态表、压缩表。
    InnoDB：需要更多的内存和存储，它会在主内存中建立其专用的缓冲池用于高速缓冲数据和索引。
3. 可移植性、备份及恢复
    MyISAM：数据是以文件的形式存储，所以在跨平台的数据转移中会很方便。在备份和恢复时可单独针对某个表进行操作。
    InnoDB：免费的方案可以是拷贝数据文件、备份 binlog，或者用 mysqldump，在数据量达到几十G的时候就相对痛苦了。
4. 事务支持
    MyISAM：强调的是性能，每次查询具有原子性,其执行数度比InnoDB类型更快，但是不提供事务支持。
    InnoDB：提供事务支持事务，外部键等高级数据库功能。 具有事务(commit)、回滚(rollback)和崩溃修复能力(crash recovery capabilities)的事务安全(transaction-safe (ACID compliant))型表。
5. AUTO_INCREMENT
    MyISAM：可以和其他字段一起建立联合索引。引擎的自动增长列必须是索引，如果是组合索引，自动增长可以不是第一列，他可以根据前面几列进行排序后递增。
    InnoDB：InnoDB中必须包含只有该字段的索引。引擎的自动增长列必须是索引，如果是组合索引也必须是组合索引的第一列。
6. 表锁差异
    MyISAM：只支持表级锁，用户在操作myisam表时，select，update，delete，insert语句都会给表自动加锁，如果加锁以后的表满足insert并发的情况下，可以在表的尾部插入新的数据。
    InnoDB：支持事务和行级锁，是innodb的最大特色。行锁大幅度提高了多用户并发操作的新能。但是InnoDB的行锁，只是在WHERE的主键是有效的，非主键的WHERE都会锁全表的。
7. 全文索引
    MyISAM：支持 FULLTEXT类型的全文索引
    InnoDB：5.7之后支持FULLTEXT类型的全文索引
8. 表主键
    MyISAM：允许没有任何索引和主键的表存在，索引都是保存行的地址。
    InnoDB：如果没有设定主键或者非空唯一索引，就会自动生成一个6字节的主键(用户不可见)，数据是主索引的一部分，附加索引保存的是主索引的值。
9. 表的具体行数
    MyISAM：保存有表的总行数，如果select count(*) from table;会直接取出出该值。
    InnoDB：没有保存表的总行数，如果使用select count(*) from table；就会遍历整个表，消耗相当大，但是在加了wehre条件后，myisam和innodb处理的方式都一样。
10. CURD操作
    MyISAM：如果执行大量的SELECT，MyISAM是更好的选择。
    InnoDB：如果你的数据执行大量的INSERT或UPDATE，出于性能方面的考虑，应该使用InnoDB表。DELETE 从性能上InnoDB更优，但DELETE FROM table时，InnoDB不会重新建立表，而是一行一行的删除，在innodb上如果要清空保存有大量数据的表，最好使用truncate table这个命令。
11. 外键
    MyISAM：不支持
    InnoDB：支持
12.崩溃自动恢复
    MyISAM：不支持
    InnoDB：支持


## 悲观锁和乐观锁是什么？

```
悲观锁
```

> 使用

1. 关闭 autocommit=0;
2. 在事务中使用 select ... from ... where ... for update 给行数据加排它锁
3. select 命中的行必须有索引,否则会锁表

> 优点

保守策略, 数据安全性高

> 缺点

1. 有加锁等额外开销,效率低
2. 可能引起死锁
3. 降低并行行,数据被锁住后其他事务必须等待


```
乐观锁
```

> 使用

1. 表中增加版本号或时间戳数据列
2. 读取数据时同时读取版本号
3. 更新数据时添加版本号为条件,同时版本号+1
4. 如果更新失败, 提示用户

> 优点

1. 没有锁, 效率高
2. 不会引起死锁 

> 缺点

列表文本遇到两个事务同一时间读取一行数据时, 会引起问题

## PHP json_decode()函数如何返回关联数组？

```
echo json_decode($str, true);
```

## POST 与 GET 有什么区别

- 本质上来说GET做查询, POST用于修改
- GET通过URL传递数据,POST把数据放在BODY体内
- GET在URL中传递参数有长度限制在1024字节.POST默认上限为2M
- POST比GET更安全
- GET在浏览器回退时是无害的,不会丢失传递的数据,而POST会再次提交请求
- GET产生的URL地址可以存为书签,POST不可以
- GET只能进行url编码,而POST支持多种编码方式
- GET请求会被浏览器主动cache,POST不会,除非手动设置
- GET请求参数会被保留在浏览器历史记录里,POST不会被保留

## 如何修改POST数据传输上限值

修改 `php.ini` 文件中的 `post_max_size`参数

## 如何查看Nginx使用哪个端口？

使用下面命令可以查出Nginx `master` 进程的PID
```
$ ps -aux | grep nginx
1234 root      0:00 nginx: master process nginx -g daemon off;
5678 nginx     0:00 nginx: worker process
```

然后根据PID查看Nginx使用的端口：

```
$ netstat -anp | grep 1234
tcp        0      0 127.0.0.11:43489        0.0.0.0:*               LISTEN      -
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      1234/nginx: master pro
```

可以看到PID为1234的Nginx使用本地的80端口

> 扩展

Linux输出结果很多是没有列头的，我们可以用head打印出来，如上两个命令可以这样：

```
$ ps -aux | head -1; ps -aux | grep nginx
$ netstat -anp | head -2; netstat -anp | grep nginx
```

## 如何查看linux服务器的负载

查看Linux服务器的负载主要是观察CPU的使用,内存的使用,IO小号这3部分

常见的命令有 

`top`  展示cpu和内存的使用情况

`iostat` 查看IO开销

`uptime` 查看过去1 5 15分钟的负载平均值

`free` 内存剩余量

`df` 磁盘使用情况

`du` 文件占用信息 `du -h --max-depth=1` 查看当前目录大小 

## 进程间通信方式有哪些?

主要有8种通信方式

1. 匿名管道
2. 高级管道
3. 有名管道
4. 消息队列
5. 信号量
6. 信号
7. 共享内存
8. 套接字

> 每个进程各自有不同的用户地址空间,任何一个进程的全局变量在另一个进程中都看不到,所以进程之间要交换数据必须通过内核,在内核中开辟一块缓冲区,进程A把数据从用户空间拷贝到内核缓冲区,进程B再从内核缓冲区把数据读取出来,内核提供的这种机制称为进程间通信

### 匿名管道通信

匿名管道( `pipe` ): 管道是一种半双工的通信方式,数据只能单向流动,而且只能在具有亲缘关系的进程间使用. 进程的亲缘关系通常指父子进程关系

```
// 需要的头文件
#include <unistd.h>

// 通过pipe()函数来创建匿名管道
// 返回值：成功返回0，失败返回-1
// fd参数返回两个文件描述符
// fd[0]指向管道的读端，fd[1]指向管道的写端
// fd[1]的输出是fd[0]的输入。
int pipe (int fd[2]);
```

通过匿名管道实现进程间通信的步骤如下：
1. 父进程创建管道,得到两个文件描述符指向管道的两端
2. 父进程fork出子进程,子进程也有两个文件描述符指向同一个管道
3. 父进程关闭fd[0],子进程关闭fd[1],即父进程关闭管道读端,子进程关闭管道写端( 因为管道只支持单向通信 ). 父进程可以往管道里写, 子进程可以从管道里读, 管道是用环形队列实现的, 数据从写端流入,从读端流出,这样就实现了进程间通信


### 高级管道通信

高级管道( `popen` ): 将另一个程序当做一个新的进程在当前程序进程中启动, 则它算是前程序的子进程, 这种方式我们称之为高级管道方式

### 有名管道通信

有名管道 ( `named pipe` ): 有名管道也是半双工的通信方式, 但是它允许无亲缘关系进程间的通信

### 消息队列通信

消息队列 ( `message queue` ): 消息队列是由消息的链表, 存放在内核中并由消息队列标识符标识. 消息队列克服了信号传递信息少、管道只能承载无格式字节流一级缓冲区大小受限等缺点.

### 信号量通信 

信号量( `semophore` ) ： 信号量是一个计数器，可以用来控制多个进程对共享资源的访问。它常作为一种锁机制，防止某进程正在访问共享资源时，其他进程也访问该资源。因此，主要作为进程间以及同一进程内不同线程之间的同步手段。

### 信号

信号 ( `sinal` ) ： 信号是一种比较复杂的通信方式，用于通知接收进程某个事件已经发生。

### 共享内存通信 

共享内存( `shared memory` ) ：共享内存就是映射一段能被其他进程所访问的内存，这段共享内存由一个进程创建，但多个进程都可以访问。共享内存是最快的 IPC 方式，它是针对其他进程间通信方式运行效率低而专门设计的。它往往与其他通信机制，如信号两，配合使用，来实现进程间的同步和通信。

### 套接字通信

套接字( `socket` ) ： 套接口也是一种进程间通信机制，与其他通信机制不同的是，它可用于不同机器间的进程通信。

通信过程如下：

#### 1. 命名socket

`SOCK_STREAM` 式本地套接字的通信双方均需要具有本地地址，其中服务器端的本地地址需要明确指定，指定方法是使用 `struct sockaddr_un` 类型的变量。

#### 2. 绑定

`SOCK_STREAM` 式本地套接字的通信双方均需要具有本地地址，其中服务器端的本地地址需要明确指定，指定方法是使用 `struct sockaddr_un` 类型的变量，将相应字段赋值，再将其绑定在创建的服务器套接字上，绑定要使用 bind 系统调用，其原形如下：

```
int bind(int socket, const struct sockaddr *address, size_t address_len);
```

其中 `socket` 表示服务器端的套接字描述符，`address` 表示需要绑定的本地地址，是一个 `struct sockaddr_un` 类型的变量，`address_len` 表示该本地地址的字节长度。

#### 3. 监听

服务器端套接字创建完毕并赋予本地地址值（名称，本例中为`Server Socket`）后，需要进行监听，等待客户端连接并处理请求，监听使用 `listen` 系统调用，接受客户端连接使用`accept`系统调用，它们的原形如下：

```
int listen(int socket, int backlog);
int accept(int socket, struct sockaddr *address, size_t *address_len);
```

其中 `socket` 表示服务器端的套接字描述符；`backlog` 表示排队连接队列的长度（若有多个客户端同时连接，则需要进行排队）；`address` 表示当前连接客户端的本地地址，该参数为输出参数，是客户端传递过来的关于自身的信息；`address_len` 表示当前连接客户端本地地址的字节长度，这个参数既是输入参数，又是输出参数。

#### 4. 连接服务器

客户端套接字创建完毕并赋予本地地址值后，需要连接到服务器端进行通信，让服务器端为其提供处理服务。

对于SOCK_STREAM类型的流式套接字，需要客户端与服务器之间进行连接方可使用。连接要使用 connect 系统调用，其原形为

```
int connect(int socket, const struct sockaddr *address, size_t address_len);
```

其中`socket`为客户端的套接字描述符，`address` 表示当前客户端的本地地址，是一个` struct sockaddr_un` 类型的变量，`address_len` 表示本地地址的字节长度。实现连接的代码如下：

```
connect(client_sockfd, (struct sockaddr*)&client_address, sizeof(client_address));
```

#### 5. 相互发送接收数据 

无论客户端还是服务器，都要和对方进行数据上的交互，这种交互也正是我们进程通信的主题。一个进程扮演客户端的角色，另外一个进程扮演服务器的角色，两个进程之间相互发送接收数据，这就是基于本地套接字的进程通信。发送和接收数据要使用 write 和 read 系统调用，它们的原形为：

```
int read(int socket, char *buffer, size_t len);
int write(int socket, char *buffer, size_t len);
```

其中 `socket` 为套接字描述符；`len` 为需要发送或需要接收的数据长度；

对于 `read` 系统调用，`buffer` 是用来存放接收数据的缓冲区，即接收来的数据存入其中，是一个输出参数；

对于 `write` 系统调用，`buffer` 用来存放需要发送出去的数据，即 `buffer` 内的数据被发送出去，是一个输入参数；返回值为已经发送或接收的数据长度。

#### 6.  断开连接

交互完成后，需要将连接断开以节省资源，使用close系统调用，其原形为：

```
int close(int socket);
```

## 什么是线程?

线程是操作系统能够进行运算调度的最小单位, 它被包含在进程之中, 是进程中的实际运作单位

程序员可以通过它进行多处理器编程, 你可以使用多线程对运算密集型任务提速

比如, 如果一个线程完成一个任务要100Ms, 那么用10个线程完成该任务只需要10ms.

## PHP函数内部 static 和 global 关键字有什么作用？

`static` 是静态变量,在局部函数中存在且只能初始化一次,使用过后再次使用会使用上次执行的结果; 作为计数器, 程序内部缓存, 单例模式中都有用到

`global` 关键字, 引用全局变量

`static` 静态方法,是类的成员方法,但不需要实例化类可直接使用

`$GLOBALS` 在函数内使用具有全局作用域的变量,如$GLOBALS['a']


## Redis 支持哪几种持久化方式？

Redis 持久化分为 RDB 持久化与 AOF 持久化.

RDB 持久化可以在指定间隔时间内生成数据集的时间点快照.

而AOF持久化记录服务器执行的所有写操作命令,并在服务器启动时, 通过执行这些命令来还原数据集. AOF 文件中的命令全部以 Redis 协议的格式来保存, 新命令会被追加到未见的末尾. Redis 还可以在后台对 AOF 文件进行重写 ( rewrite ), 使得 AOF 文件的体积不会超出保存数据集状态所需的实际大小.

Redis 还可以同时使用 AOF 持久化和 RDB 持久化. 在这种情况下, 当 Redis 重启时, 它会优先使用 AOF 文件来还原数据集, 因为 AOF 文件保存的数据集通常比 RDB 文件所保存的数据集更完整. 

## PHP 下载图片有哪些方法. 

1. `file_get_contents`
2. `readfile` 读取内容
3. `fopen` 系列函数
4. `curl`

## CGI  FastCGI 和 php-fpm 都是什么? 它们和 Nginx 之间是什么关系?

`CGI` 通用网关接口, 用于 WEB 服务器和应用程序间的交互, 定义输出输入规范

用户的请求通过WEB服务器( 例如 `Nginx` ) 转发给 `FastCGI` 进程, `FastCGI` 进程再调用应用程序进行处理( 如 `php解析器` ), 应用程序的处理结果返回给 `FastCGI`, `FastCGI` 返回给 `Nginx` 进行输出. 

假设当前WEB服务器使用的是 `Nginx`, 应用程序是 `PHP` , 而 `php-fpm` 是管理 `FastCGI` 的,这就是它们其中的关系. 

`FastCGI` 用来提高 `CGI` 程序性能, 启动一个 `master` , 再启动多个 `worker` , 不需要每次解析 `php.ini` , 而 `php-fpm` 实现了 `FastCGI` 协议, 是 `FastCGI` 的进程管理器, 支持平滑重启, 可以启动的时候预先生成过个进程.

## 什么是 CSRF 攻击? 如何防范?

```
概念
```

`CSRF` 跨站请求伪造, 攻击方伪装用户身份发送请求从而窃取信息或者破坏系统

```
原理
```

用户访问网站A登录并生成了`cookie`, 再访问网站B, 如果A网站存在CSRF漏洞, 此时B网站给A网站的请求( 此事相当于是用户访问 ), A网站会认为是用户发送的请求, 从而B网站就成功伪装了用户身份, 因此叫 `跨站脚本攻击`

```
CSRF防范
```

1. 合理规范api 请求方式, GET POST
2. 对`POST`请求加token令牌验证, 生成一个随机码并存入`Session`, 表单中带上这个随机码, 提交的时候服务端进行验证随机码是否相同


## 什么是 XSS 攻击，如何防范？

```
概念
```

XSS，跨站脚本攻击。

简单来说, xss 就是正常页面执行了用户或者黑客提交的前段代码, 比如你用了

> eval('这里执行了用户提交的代码')

或者你的页面正常解析了用户提交的html代码,如用户提交的个人信息是:

> <script>window.location.href="恶意网站连接";</script>


而你不加过滤转义就入库,然后页面正常解析html代码,最终用户访问这个页面就会跳转到恶意网站 ,这就是XSS

```
防范
```

不相信任何输入，对输入内容进行过滤 以及 转义(如PHP的htmlentities、htmlspecialchars)。

## jQuery中$("#content .abc") 和 $("#content").find(".abc")哪个效率更高？

后者，后者使用原生的 `document.getElementByName`，`ID` > `Tag` > `Class`。

`$("#content").find(".abc").find()` 方法会调用浏览器的原生方法（`getElementById`，`getElementByName`，`getElementByTagName` 等等），所以速度较快。比 `$("#content .abc")` 效率快很多。

第一种慢的原因：在于 jQuery 内部使用各种选择器链条的选择顺序是从右到左，所以这条语句是先选.abc，然后再一个个过滤出父元素 `#content`，这导致它慢很多。

## ajax 中如何执行跨域访问？ 

> 跨域的存在是因为浏览器的同源策略，一个源表示协议、端口、域名都相同，否则就形成了跨域。

通过JavaScript的 `callback` 方式调用，jQuery封装了jsonp方式的请求。

```
callback({"result": 0, "msg": "ok", "data": {xxx}})
```

服务器响应头

```
header("Access-Control-Allow-Origin: *");                   /*星号表示所有的域都可以接受*/
header("Access-Control-Allow-Methods: GET, POST");
```

jsonp的例子
JAVASCRIPT代码：

```
$.getJSON('http://www.example.com/jsonp.php?callback=?','firstname=Jeff',function(res){
    alert('Your name is '+res.fullname);
});
```

服务器代码：

```
<?php
$fname = $_GET['firstname'];
if($fname=='Jeff'){
    //header("Content-Type: application/json");
    echo $_GET['callback'] . '(' . "{'fullname' : 'Jeff Hansen'}" . ')';
}
```

Ajax发jsonp请求：

```
$.ajax({
    url: "http://api.flickr.com/services/rest/?method=flickr.interestingness.getList ",
    dataType: "jsonp",
    jsonp: 'jsoncallback',
    success: function(data) {
        alert(data);
    }
});
```

注意：对于上面第二个ajax示例，url中不必带有 `callback` 参数，jquery会自动添加。

Jsonp参数是 `callback` 名称，指的就是服务端`$_GET[‘callback’]`里的 `callback` 的名称。

实际发的请求就是：`http://api.flickr.com/services/rest/?method=flickr.interestingness.getList?jsoncallback=jquery332232323&_ 1471419449018`

`dataType: ‘jsonp’`，用于表示这是一个 JSONP 请求。 `jsonp: ‘callback’`，用于告知服务器根据这个参数获取回调函数的名称，通常约定就叫 callback。 `jsonpCallback: ‘dosomething’`，回调函数的名称，也是前面callback参数的值，可省略，jquery会自动生成。

> JSONP 的原理

AJAX 无法跨域是受到“同源政策”的限制，但是带有 `src` 属性的标签（例如`<script>`、`<img>`、`<iframe>`）是不受该政策限制的，因此我们可以通过向页面中动态添加`<script>`标签来完成对跨域资源的访问，这也是 JSONP 方案最核心的原理。

缺点：防止xss注入

## $(document).ready() 函数作用域是什么？

`ready()`函数用于在当前文档结构载入完毕后立即执行指定的函数。

该函数的作用相当于`window.onload`事件。

你可以多次调用该函数，从而绑定多个函数，jQuery将在DOM文档结构加载完毕后按照绑定顺序立即执行这些函数。

> 请注意：请不要在一个页面同时使用`ready()`函数和`<body>`元素的`onload`事件绑定函数，因为它们之间并不完全兼容。如果你必须使用`load`，那么请不要使用`jQuery`的`ready()`和`load()`来为`window`或更多指定项(例如图片)添加`load`事件处理器。

该函数属于jQuery对象(实例)。

## $(this) 和 this 关键字在 jQuery 中有何不同？

一个是 `JQuery` 对象，一个是 `Javascript` 的属性

## PHP判断远程文件是否存在有哪几种方法？

1. `file_get_contents` 方法
2. Curl 组件
3. `fwrite` `fgets` `fclose` 方法


> 方法1

需要开启`allow_url_fopen`：

```
<?php
$url = "http://cn.wordpress.org/wordpress-3.3.1-zh_CN.zip";
$fileExists = @file_get_contents($url, null, null, -1, 1) ? true : false;
echo $fileExists; //返回1，就说明文件存在。

```

> 方法2 

该方法需要服务器支持Curl组件：

```
<?php
function check_remote_file_exists($url) {
    $curl = curl_init($url); // 不取回数据
    curl_setopt($curl, CURLOPT_NOBODY, true);
    curl_setopt($curl, CURLOPT_CUSTOMREQUEST, 'GET'); // 发送请求
    $result = curl_exec($curl);
    $found = false; // 如果请求没有发送失败
    if ($result !== false) {

        /** 再检查http响应码是否为200 */
        $statusCode = curl_getinfo($curl, CURLINFO_HTTP_CODE);
        if ($statusCode == 200) {
            $found = true;
        }
    }
    curl_close($curl);

    return $found;
}

$url = "http://cn.wordpress.org/wordpress-3.3.1-zh_CN.zip";
echo check_remote_file_exists($url); // 返回1，说明存在。
```

> 方法3

```
<?php
/*
 函数：remote_file_exists
 功能：判断远程文件是否存在
 参数： $url_file -远程文件URL
 返回：存在返回true，不存在或者其他原因返回false
*/
function remote_file_exists($url_file){
    //检测输入
    $url_file = trim($url_file);
    if (empty($url_file)) { return false; }
    $url_arr = parse_url($url_file);
    if (!is_array($url_arr) || empty($url_arr)){return false; }

    //获取请求数据
    $host = $url_arr['host'];
    $path = $url_arr['path'] ."?".$url_arr['query'];
    $port = isset($url_arr['port']) ?$url_arr['port'] : "80";

    //连接服务器
    $fp = fsockopen($host, $port, $err_no, $err_str,30);
    if (!$fp){ return false; }

    //构造请求协议
    $request_str = "GET ".$path."HTTP/1.1";
    $request_str .= "Host:".$host."";
    $request_str .= "Connection:Close";

    //发送请求
    fwrite($fp,$request_str);
    $first_header = fgets($fp, 1024);
    fclose($fp);

    //判断文件是否存在
    if (trim($first_header) == ""){ return false;}
    if (!preg_match("/200/", $first_header)){
        return false;
    }

    return true;
}
```

> test code

```
<?php
$str_url = 'http://cn.wordpress.org/wordpress-3.3.1-zh_CN.zip';
$exits = remote_file_exists($str_url);
echo $exists ? "Exists" : "Not exists";
```

## PHP如何实现对象和数组互相转换？

```
/**
 * 数组 转 对象
 *
 * @param array $arr 数组
 * @return object
 */
function array_to_object($arr) {
    if (gettype($arr) != 'array') {
        return;
    }
    foreach ($arr as $k => $v) {
        if (gettype($v) == 'array' || getType($v) == 'object') {
            $arr[$k] = (object)array_to_object($v);
        }
    }

    return (object)$arr;
}

/**
 * 对象 转 数组
 *
 * @param object $obj 对象
 * @return array
 */
function object_to_array($obj) {
    $obj = (array)$obj;
    foreach ($obj as $k => $v) {
        if (gettype($v) == 'resource') {
            return;
        }
        if (gettype($v) == 'object' || gettype($v) == 'array') {
            $obj[$k] = (array)object_to_array($v);
        }
    }

    return $obj;
}
```


## PHP如何生成指定长度的随机字符串？

```
/**
 * 生成指定长度的随机字符串
 * @param $length int 要生成的字符串长度
 * @return string
 */
function getRandStr($length)
{
    $str = '';
    $strPool = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789abcdefghijklmnopqrstuvwxyz';
    $max = strlen($strPool) - 1;

    for ($i=0; $i<$length; $i++){
        $str .= $strPool[mt_rand(0, $max)];
    }

    return $str;
}
```


## 请说说常见的设计模式和应用场景


### 单例模式

单例模式是一种常用的软件设计模式, 在它的核心结构中只包含一个被称为单例类的特殊类.

通过单例模式可以 ==保证系统中一个类只有一个实例== ,而且该实例易于外借访问, 从而方便对实例个数的控制并节约系统资源.

> 应用场景:
> - 如果希望在系统中某个类的对象只能存在一个,单例模式是最好的解决方案.

### 工厂模式

工厂模式主要是 ==为创建对象提供了接口==

> 应用场景如下:
> - 在编码时 不能预见需要创建哪种类的实例.
> - 系统不应依赖于产品类实例如何被创建 组合和表达的细节

### 策略模式

策略模式: 定义了算法簇,分别封装起来, 让他们之间==可以互相替换==

> 应用场景如下:
> - 一件事情,有很多方案可以实现
> - 我可以在任何时候,决定采用哪一种方法实现
> - 未来可能增加更多的方案
> - 策略模式让方案的变化不会影响到使用方案的客户

举例业务场景如下

系统的操作都要有日志记录，通常会把日志记录在数据库里面，方便后续的管理，但是在记录日志到数据库的时候，可能会发生错误，比如暂时连不上数据库了，那就先记录在文件里面。日志写到数据库与文件中是两种算法，但调用方不关心，只负责写就是。

### 观察者模式

观察者模式又被称作`发布/订阅模式`，定义了对象间一对多依赖，当一个对象改变状态时，它的`所有依赖者都会收到通知并自动更新`。

> 应用场景如下：
> - 对一个对象状态的更新，需要其他对象同步更新，而且其他对象的数量动态可变。
> - 对象仅需要将自己的更新通知给其他对象而不需要知道其他对象的细节。


### 迭代器模式

迭代器模式提供一种方法`顺序访问一个聚合对象中各个元素`,而又不暴露该对象的内部表示

> 应用场景如下:
> - 当你需要访问一个聚集对象,而且不管这些对象是什么都需要遍历的时候, 就应该考虑用迭代器模式.

## 什么是TCP/IP？

PAGE 8
