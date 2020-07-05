### client 和 nginx 简易交互过程
- step1： `client` 发起 `Http` 请求
- step2：`DNS` 服务器解析域名拿到主机 `IP`
- step3：默认端口 `80`，通过 `IP`、`Port` 建立 `TCP/IP` 连接
- step4：`TCP/IP` 三次握手，建立连接成功发送数据包
- step5：`nginx` 匹配请求
    - `case.html`：静态内容，分发静态内容响应
    - `case.php`：`php` 脚本，转发请求内容到 `php-fpm` 进程，分发 `php-fpm` 返回的内容响应

- step6：`TCP/IP` 四次挥手，断开连接


### nginx 和 php 简易交互过程

- 背景：`web server(nginx)` 和服务端语言交互依赖的是 `CGI(Common Gateway Interface)` 协议，由于 `cgi` 执行效率不高（每次执行都需要起一个 `php-cgi` 解析器进程，这期间会进行加载 `php.ini` 配置等一系列的操作），所以后期产生了 `fast-cgi` 协议（一种常驻型的协议），`php-cgi` 实现了 `fast-cgi`，但变更 `php.ini` 配置后需重启 `php-cgi` 才能让新的 `php-ini` 生效，不可以平滑重启。相比之下，`php-fpm` 提供了更好的进程管理方式，可以有效的控制内存和进程并可以平滑重启 `php` 配置。

- 流程：
  - step1：`nginx` 接收到一条 `http` 请求，通过 `location` 指令会把请求参数转变成 `php` 能懂的 `php` 变量
  ```
  // nginx 配置资料
  location ~ \.php$ {
    include snippets/fastcgi-php.conf; //step1
    fastcgi_pass unix:/run/php/php7.0-fpm.sock;
  }
  ```
  - step2：`nginx` 匹配到 `.php` 结尾的访问，通过 `fastcgi_pass` 命令传递给 `php-fpm.sock` 文件，其实这里的 `nginx` 发挥的反向代理的角色，把 `http` 协议请求转到 `fastcgi` 协议请求
  ```
  // nginx 配置资料
  location ~ \.php$ {
    include snippets/fastcgi-php.conf;
    fastcgi_pass unix:/run/php/php7.0-fpm.sock; //step2
  }
  ```
  - step3：`php-fpm.sock` 文件会被 `php-fpm` 的 `master` 进程所引用，这里的 `nginx` 和 `php-fpm` 使用的是 `linux` 进程间通信方式 `unix domain socks`，是一种基于文件而不是网络底层协议的通信方式
  - step4：`php-fpm` 的 `master` 进程接收到请求后，会把请求分发到 `php-fpm` 的子进程，每个 `php-fpm` 子进程都包含一个 `php` 解析器
  - step5：`php-fpm` 进程处理完进程请求后返回给 `nginx`

### php-fpm 进程管理的三种方式

- `static`：静态方式，`php-fpm` 启动时及启动最大子进程数，优点是不需要额外的 `fork` 子进程过程，适合专门的服务器
  - `pm.max_children`：最大子进程数
- `dynamic`：动态方式，配置最大数和启动数，空闲数，实际使用过程是去 `fork` 进程，优点灵活节省内存，缺点 `fork` 过程有性能消耗
  - `pm.max_children`：最大子进程数
  - `pm.start_servers`： 启动数，等于 `min_spare_servers + (max_spare_servers - min_spare_servers)/2`
  - `pm.min_spare_servers`：最小空闲进程数，如果空闲进程(`idle`)数小于该值，启动一个子进程
  - `pm.max_spare_servers`：最大空闲进程数，如果空闲进程(`idle`)数大于该值，`kill`一个子进程
- `ondemand`：按需方式, 不启动子进程，按需 `fork`，优点节省资源
  - `pm.max_children`：最大子进程数
  - `pm.process_idle_timeout`：子进程空闲多少秒后被 `kill`
