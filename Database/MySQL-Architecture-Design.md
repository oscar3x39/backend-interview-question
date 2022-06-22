# High availability

- DNS & nginx 負載均衡
- Reverse proxy 反向代理
- Weight 代表權重
- 負載均衡算法 hash key [consistent]
- 失敗重試
```
upstream backend{
    server 192.168.1.101:8080 max_fails=2 fail_timeout=10s weight=1;
    server 192.168.1.102:8080 max_fails=2 fail_timeout=10s weight=2;
}
location / {
	proxy_pass http://backend;
}
```

Automation check upstream health
https://github.com/yaoweibin/nginx_upstream_check_module

> TCP心跳檢查
```
check interval=3000 rise=1 fall=3 timeout=2000 type=tcp;

interval： 检测间隔时间，此处配置了每隔3s检测一次。
fall： 检测失败多少次后，上游服务器被标识为不存活。
rise： 检测成功多少次后，上游服务器被标识为存活，并可以处理请求。
```

> HTTP心跳檢查
```
check_http_send ”HEAD /status HTTP/1.0\r\n\r\n“
check_http_expect_alive http_2xx http_3xx;

check_http_send： 即检查时发的HTTP请求内容。
check_http_expect_alive： 当上游服务器返回匹配的响应状态码时，则认为上游服务器存活。
```

> 備份伺服器
```
upstream backend{
	server 192.168.1.101:8080 weight=1;
	server 192.168.1.102:8080 weight=2 backup;
}
```

> 故障伺服器
```
upstream backend{
	server 192.168.1.101:8080 weight=1;
	server 192.168.1.102:8080 weight=2 down;
}
```

# 限流

### redis限流
### nginx限流

> 连接数限流模块 ngx_http_limit_conn_module

> 漏桶算法实现的请求限流模块 ngx_http_limit_req_module
```
http{
	limit_conn_zone $binary_remote_addr zone=addr:10m;
	limit_conn_log_level error;
	limit_conn_status 503;
    ...
    server{
		location /limit{
			limit_conn addr 1;
		}
    }
    ...
}

limit_conn： 要配置存放key和计数器的共享内存区域和指定key的最大连接数。此处指定的最大连接数是1，表示Nginx最多同时并发处理1个连接。
limit_conn_zone： 用来配置限流key及存放key对应信息的共享内存区域大小。此处的key是binaryremoteaddr”，表示IP地址，也可以使用binary_remote_addr”，表示IP地址，也可以使用binaryr​emotea​ddr”，表示IP地址，也可以使用server_name作为key来限制域名级别的最大连接数。
limit_conn_status： 配置被限流后返回的状态码，默认返回503。
limit_conn_log_level： 配置记录被限流后的日志级别，默认error级别。
```

### ngx_http_limit_req_module

> 漏桶算法实现，用于对指定key对应的请求进行限流，比如，按照IP维度限制请求速率。配置示例如下
```
limit_conn_log_level error;
limit_conn_status 503;
...
server{
	location /limit{
		limit_req zone=one burst=5 nodelay;
    }
}
limit_req： 配置限流区域、桶容量（突发容量，默认为0）、是否延迟模式（默认延迟）。
limit_req_zone： 配置限流key、存放key对应信息的共享内存区域大小、固定请求速率。此处指定的key是“$binary_remote_addr”，表示IP地址。固定请求速率使用rate参数配置，支持10r/s和60r/m，即每秒10个请求和每分钟60个请求。不过，最终都会转换为每秒的固定请求速率（10r/s为每100毫秒处理一个请求，60r/m为每1000毫秒处理一个请求）。
limit_conn_status： 配置被限流后返回的状态码，默认返回503。
limit_conn_log_level： 配置记录被限流后的日志级别，默认级别为error。
```

# 降級
```
在降级前需要对系统进行梳理，判断系统是否可以丢丢卒保帅，从而整理出那些可以降级，那些不能降级。
```
### 服务功能降级
- 比如，渲染商品详情页时，需要调用一些不太重要的服务（相关分类、热销榜等），而这些服务在异常情况下直接不获取，即降级即可。
### 读降级
- 比如，多级缓存模式，如果后端服务有问题，则可以降级为只读缓存，这种方式适用于对读一致性要求不高的场景。
### 写降级
- 比如，秒杀抢购，我们可以只进行Cache的更新，然后异步扣减库存到DB，保证最终一致性即可，此时可以将DB降级为Cache。

# 超時與重試
```
在访问服务之后，由于网络或其他原因迟迟没有响应而超时，此时为了用户体验度，可以默认发起第二次请求，进行尝试。

代理层超时与重试： nginx
web容器超时与重试
中间件和服务之间超时与重试
数据库连接超时与重试
nosql超时与重试
业务超时与重试
前端浏览器ajax请求超时与重试
```

# 排队
```
最为常见的削峰方案是使用消息队列，通过把同步的直接调用转换成异步的间接推送缓冲瞬时流量
```
- 线程池加锁等待
- 本地内存蓄洪等待
- 本地文件序列化写，再顺序读

### 缺點
```
请求积压。流量高峰如果长时间持续，达到了队列的水位上限，队列同样会被压垮，这样虽然保护了下游系统，但是和请求直接丢弃也没多大区别
用户体验。异步推送的实时性和有序性自然是比不上同步调用的，由此可能出现请求先发后至的情况，影响部分敏感用户的购物体验
```

### 過濾

> 过滤的核心目的是通过减少无效请求的数据IO保障有效请求的IO性能。

```
读限流：对读请求做限流保护，将超出系统承载能力的请求过滤掉
读缓存：对读请求做数据缓存，将重复的请求过滤掉
写限流：对写请求做限流保护，将超出系统承载能力的请求过滤掉
写校验：对写请求做一致性校验，只保留最终的有效数据
```

# Concurrency

# Architecture
* Load balancing
 * HAProxy
 * Nginx
 * ProxySQL

### Read/Write Splitting 讀寫分離
### Master-Slave Replication 主從式架構

### Scale up
  * application
  * connection
  * database
  * file system
  * os kernel
  * hardware
### Scale out
  * Replication
  * Clustering
  * Sharding
  * Disaster Recovery
  * Multi Regional Resiliency