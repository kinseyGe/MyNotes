### nginx.conf配置文件

```
# 定义Nginx运行的用户和用户组
user root;

# nginx进程数，建议设置为等于CPU总核心数。
# 在配置文件的顶级main部分，worker角色的工作进程的个数，master进程是接收并分配请求给worker处理。
# 这个数值简单一点可以设置为cpu的核数grep ^processor /proc/cpuinfo | wc -l，也是 auto 值，
# 如果开启了ssl和gzip更应该设置成与逻辑CPU数量一样甚至为2倍，可以减少I/O操作。如果nginx服务器还有其它服务，可以考虑适当减少。

worker_processes auto;

# 错误日志: 存放路径
# 制定错误日志路径，级别。这个设置可以放入全局块，http块，server块，级别依次为：debug|info|notice|warn|error|crit|alert|emerg
error_log /var/log/nginx/error.log;

# pid(进程标识符): 存放路径   --- nginx:master 进程
pid /run/nginx.pid;

# 指定进程可以打开的最大描述符：数目
# 这个指令是指当一个nginx进程打开的最多文件描述符数目，理论值应该是最多打开文件数（ulimit -n）与nginx进程数相除，
# 但是nginx分配请求并不是那么均匀，所以最好与ulimit -n 的值保持一致。
# 这是因为nginx调度时分配请求到进程并不是那么的均衡，所以假如填写10240，总并发量达到3-4万时就有进程可能超过10240了，这时会返回502错误。
worker_rlimit_nofile 65535;

# Load dynamic modules. See /usr/share/nginx/README.dynamic.
# 加载动态模块 : 存放路径
include /usr/share/nginx/modules/*.conf;

# events模块中包含nginx中所有处理连接的设置.
# 事件模块可以使用指令来配置网络机制，某些参数可能会直接应用程序性能,下面是事件模块可配置的指令
events {
	
	# 选择一个可用的事件的模型(可以在编译时指定)，Nginx会自动选择事件的模型
	# 支持下面的事件模型:
		# select：只能在Windows下使用，这个事件模型不建议在高负载的系统使用
		
		# poll:Nginx默认首选，但不是在所有系统下都可用
		
		# kqueue:这种方式在FreeBSD 4.1+, OpenBSD2.9+, NetBSD 2.0, 和 MacOS X系统中是最高效的

		# epoll: 这种方式是在Linux 2.6+内核中最高效的方式

		# rtsig:实时信号，可用在Linux 2.2.19的内核中，但不适用在高流量的系统中

		# /dev/poll: Solaris 7 11/99+,HP/UX 11.22+, IRIX 6.5.15+, and Tru64 UNIX 5.1A+操作系统最高效的方式

		# eventport: Solaris 10最高效的方式
	use epoll;
	
	# 每个工作进程的最大连接数量。根据硬件调整，和前面工作进程配合起来用，尽量大，但是别把cpu跑到100%就行。
	# 每个进程允许的最多连接数，理论上每台nginx服务器的最大连接数为。worker_processes*worker_connections
	worker_connections 1024;
	
	# keepalive超时时间。
	keepalive_timeout 60;
	
	# 客户端请求头部的缓冲区大小。这个可以根据你的系统分页大小来设置，一般一个请求头的大小不会超过1k，不过由于一般系统分页都要大于1k，所以这里设置为分页大小。
	# 分页大小可以用命令getconf PAGESIZE 取得。   --- 4096
	client_header_buffer_size 4k;
	
	# 这个将为打开文件指定缓存，默认是没有启用的，max指定缓存数量，建议和打开文件数一致，inactive是指经过多长时间文件没被请求后删除缓存。
	open_file_cache max=65535 inactive=60s;
	
	# 这个是指多长时间检查一次缓存的有效信息。
	open_file_cache_valid 80s;
	
	# open_file_cache指令中的inactive参数时间内文件的最少使用次数，如果超过这个数字，文件描述符一直是在缓存中打开的，如上例，如果有一个文件在inactive时间内一次没被使用，它将被移除。
	open_file_cache_min_uses 1;
	
	# 这个指令指定是否在搜索一个文件时记录cache错误. 默认 off
	open_file_cache_errors on;
	
	# 设置网路连接序列化，防止惊群现象发生，默认为on
	# 当一个新连接到达时，如果激活了accept_mutex，那么多个Worker将以串行方式来处理，其中有一个Worker会被唤醒，其他的Worker继续保持休眠状态；
	# 如果没有激活accept_mutex，那么所有的Worker都会被唤醒，不过只有一个Worker能获取新连接，其它的Worker会重新进入休眠状态，这就是「惊群现象」。
	# 惊群现象解释: https://blog.huoding.com/2013/08/24/281
	accept_mutex on; 
	
	# 设置一个进程是否同时接受多个网络连接,默认为off
	multi_accept on;
}
    
http {
        
	# 自定义日志格式
	log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
					  'fwf[$http_x_forwarded_for] tip[$http_true_client_ip] '
					  '$status $body_bytes_sent "$http_referer" '
					  '"$http_user_agent" "$http_x_forwarded_for"';
	
	# 访问日志路径
	access_log  /var/log/nginx/access.log  main;

	# 允许sendfile方式传输文件，默认为off，
	# 可以在http块，server块，location块。（sendfile系统调用不需要将数据拷贝或者映射到应用程序地址空间中去）
	sendfile            on;
	
	# 每个进程每次调用传输数量不能大于设定的值，默认为0，即不设上限。
	sendfile_max_chunk 100k;
	
	# 终端应用程序每产生一次操作就会发送一个包，而典型情况下一个包会拥有一个字节的数据以及40个字节长的包头，于是产生4000%的过载，很轻易地就能令网络发生拥塞。
	# 为了避免这种情况，TCP堆栈实现了等待数据 0.2秒钟，因此操作后它不会发送一个数据包，而是将这段时间内的数据打成一个大的包。这一机制是由Nagle算法保证。
	
	# tcp_nopush 是 FreeBSD 的一个 socket 选项，对应 Linux 的 TCP_CORK，
	# Nginx 里统一用 tcp_nopush 来控制它，并且只有在启用了 sendfile 之后才生效。
	# 启用它之后，数据包会累计到一定大小之后才会发送，减小了额外开销，提高网络效率。
	tcp_nopush          on;
	
	# tcp_nodelay 也是一个 socket 选项，启用后会禁用 Nagle 算法，尽快发送数据，某些情况下可以节约 200ms（Nagle
	# 算法原理是：在发出去的数据还未被确认之前，新生成的小数据先存起来，凑满一个 MSS 或者等到收到确认后再发送）。
	# Nginx 只会针对处于 keep-alive 状态的 TCP 连接才会启用 tcp_nodelay。
	
	# 可以看到 tcp_nopush 是要等数据包累积到一定大小才发送，tcp_nodelay 是要尽快发送，二者相互矛盾。
	# 实际上，它们确实可以一起用，最终的效果是先填满包，再尽快发送。
	tcp_nodelay         on;
	
	# 指定服务端为每个 TCP 连接最多可以保持多长时间。Nginx 的默认值是 75 秒，有些浏览器最多只保持 60 秒,值为0会禁用keep-alive客户端连接；
	# 默认情况下，nginx已经自动开启了对client连接的keep alive支持（同时client发送的HTTP请求要求keep alive）。一般场景可以直接使用
	keepalive_timeout   65;
	
	# 用于设置一个keep-alive连接上可以服务的请求的最大数量，当最大请求数量达到时，连接被关闭。默认是100。
	# 真实含义，是指一个keep alive建立之后，nginx就会为这个连接设置一个计数器，记录这个keep alive的长连接上已经接收并处理的客户端请求的数量。
	# 如果达到这个参数设置的最大值时，则nginx会强行关闭这个长连接，逼迫客户端不得不重新建立新的长连接。
	# 简单计算一下，QPS=10000时，客户端每秒发送10000个请求(通常建立有多个长连接)，每个连接只能最多跑100次请求，意味着平均每秒钟就会有100个长连接因此被nginx关闭。
	# 同样意味着为了保持QPS，客户端不得不每秒中重新新建100个连接。因此，就会发现有大量的TIME_WAIT的socket连接(即使此时keep alive已经在client和nginx之间生效)。
	# 因此对于QPS较高的场景，非常有必要加大这个参数，以避免出现大量连接被生成再抛弃的情况，减少TIME_WAIT。
	keepalive_requests 10000;
	
	# 影响散列表的冲突率。
	# types_hash_max_size越大，就会消耗更多的内存，但散列key的冲突率会降低，检索速度就更快。
	# types_hash_max_size越小，消耗的内存就越小，但散列key的冲突率可能上升。
	types_hash_max_size 2048;

	# 保存服务器名字的hash表是由指令 server_names_hash_max_size 和 server_names_hash_bucket_size所控制的。
	# 参数hash bucket size总是等于hash表的大小，并且是一路处理器缓存大小的倍数。在减少了在内存中的存取次数后，使在处理器中加速查找hash表键值成为可能。
	# 如果 hash bucket size等于一路处理器缓存的大小，那么在查找键的时候，最坏的情况下在内存中查找的次数为2。第一次是确定存储单元的地址，第二次是在存储单元中查找键值。
	# 因此，如果Nginx给出需要增大 hash max size 或 hash bucket size的提示，那么首要的是增大前一个参数的大小.
	server_names_hash_bucket_size 64;
	
	
	# 开启gzip
	# gzip虽然好用，但是以下类型的资源不建议启用:
		# 1.图片类型
			# 原因：图片如jpg、png本身就会有压缩，所以就算开启gzip后，压缩前和压缩后大小没有多大区别，所以开启了反而会白白的浪费资源。
		# 2.大文件
			# 会消耗大量的cpu资源，且不一定有明显的效果。
	gzip on;
	
	# 启用gzip压缩的最小文件，小于设置值的文件将不会压缩
	gzip_min_length 1k;
	
	# 进行压缩的文件类型。javascript有多种形式。其中的值可以在 mime.types 文件中找到。
	gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;

	# gzip压缩比,取值为1~9
	# 1 压缩比最小处理速度最快，9 压缩比最大但处理最慢（传输快但比较消耗cpu）。
	gzip_comp_level 6;
	
	# 是否在http header中添加Vary: Accept-Encoding，建议开启
	gzip_vary on;

	# Nginx作为反向代理的时候启用，开启或者关闭后端服务器返回的结果，匹配的前提是后端服务器必须要返回包含"Via"的 header头。
		# off - 关闭所有的代理结果数据的压缩
		# expired - 启用压缩，如果header头中包含 "Expires" 头信息
		# no-cache - 启用压缩，如果header头中包含 "Cache-Control:no-cache" 头信息
		# no-store - 启用压缩，如果header头中包含 "Cache-Control:no-store" 头信息
		# private - 启用压缩，如果header头中包含 "Cache-Control:private" 头信息
		# no_last_modified - 启用压缩,如果header头中不包含 "Last-Modified" 头信息
		# no_etag - 启用压缩 ,如果header头中不包含 "ETag" 头信息
		# auth - 启用压缩 , 如果header头中包含 "Authorization" 头信息
		# any - 无条件启用压缩
	gzip_proxied any
	
	# 设置系统获取几个单位的缓存用于存储gzip的压缩结果数据流. 例如:
	# 16 8k 代表以8k为单位，按照原始数据大小以8k为单位的16倍申请内存。
	# 4 8k 代表以8k为单位，按照原始数据大小以8k为单位的4倍申请内存。
	gzip_buffers 16 8k;
	
	# 禁用IE 6 gzip
	gzip_disable "MSIE [1-6]\.";
	
	# 文件扩展名与文件类型映射表
	# HTTP request里面有一个头叫 Accept，列出浏览器可以接受的mime type，HTTP response 的Content-Type 的值 在Accept 里面。
	# 我的理解是Nginx 会根据请求的文件的扩展名来决定返回什么 Content-Type，除非后端Web程序手动设置了Content-Type，如果Web程序没设置，Nginx也没找到对应文件的扩展名的话，就使用默认的Type，
	# 这个在Nginx 里用 default_type定义，比如 default_type  application/octet-stream; 。
	include             /etc/nginx/mime.types
	
	# 默认文件类型
	default_type        application/octet-stream;
	
	# 默认编码
	charset utf-8;

	# Load modular configuration files from the /etc/nginx/conf.d directory.
	# See http://nginx.org/en/docs/ngx_core_module.html#include
	# for more information.
	include /etc/nginx/conf.d/*.conf;
	include /home/flex/server/nginx/*.conf;

	# upstream 支持5种. 
	# nginx原生支持的分配方式有: 轮询,权重,ip_hash
	# 第三方支持的分配方式有: fair和url_hash.
	
	# down 表示当前的server暂时不参与负载
	# max_fails 表示允许请求失败的次数默认为1,当超过最大次数时.返回proxy_next_upstream 模块定义的错误
	# fail_timeout 表示max_fails次失败后，暂停的时间
	# backup 表示其它所有的非backup机器down或者忙的时候，请求backup机器。所以这台机器压力会最轻。
	
	# 1.轮询: 
		# 轮询是upstream的默认分配方式，即每个请求按照时间顺序轮流分配到不同的后端服务器，如果某个后端服务器 down 掉后，能自动剔除。
	upstream yeguxin.top1 {
		server x.x.x.x:8888;
		server x.x.x.x:8888;
		server x.x.x.x:8888;
	}
	
	# 2.weight(权重)
		# 指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。 
	upstream yeguxin.top2 {
		server x.x.x.x:8888 weight=5;
		server x.x.x.x:8888 weight=10;
		server x.x.x.x:8888 weight=10;
	}
	
	# 3.ip_hash
		# 每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题.
	upstream yeguxin.top3 {
		ip_hash;
		server x.x.x.x:8888;
		server x.x.x.x:8888;
		server x.x.x.x:8888;
	}

	# 4.fair
		# 按后端服务器的响应时间来分配请求，响应时间短的优先分配。与weight分配策略类似。
	upstream yeguxin.top4 {
		server x.x.x.x:8888;
		server x.x.x.x:8888;
		server x.x.x.x:8888;
		fair;
	}

	# 5.url_hash
		# 按访问url的hash结果来分配请求，使每一个url定向到同一个后端服务器。后端服务器为缓存时比較有效。
		# 注意：在upstream中加入hash语句。server语句中不能写入weight等其他的參数，hash_method是使用的hash算法。
	upstream yeguxin.top5 {
		server x.x.x.x:8888;
		server x.x.x.x:8888;
		server x.x.x.x:8888;
		hash $request_uri;
		hash_method crc32;
	}
	
	server {
		
		# 监听的端口
		listen      80;
		
		# 用来指定IP地址或者域名，多个域名之间用空格分开。
		server_name chat.paas.scorpio.uat.newtank.cn;
		
		#rewrite 指令是使用指定的正则表达式regex来匹配请求的urI，如果匹配成功，则使用replacement更改URI。
		# rewrite指令按照它们在配置文件中出现的顺序执行。可以使用flag标志来终止指令的进一步处理。
		# 如果替换字符串replacement以http：//，https：//或$ scheme开头，则停止处理后续内容，并直接重定向返回给客户端。
		rewrite ^(.*)$	https://$host$1	permanent;
	
		# Location 配置
		# 处理请求.主要是用于针对某些特定的 URL 进行配置，可以由前缀字符串定义，也可以由正则表达式定义。
			# =  : 如果使用=作为修饰符，当请求的URI与给定的location_match的值完全匹配时，则该location模块将被视为匹配。
			# ^~ : 如果使用^~修饰符，若该location块被选为最佳非正则表达式匹配，则不会发生正则表达式匹配。即若该规则匹配上时，其他规则自然被忽略。
			# ~  : 如果使用~作为修饰符，location将被解析为区分大小写的正则表达式匹配。
			# ~* : 如果使用~*修饰符，location将被解析为大小写不敏感的正则表达式匹配。
			# /  :  通用匹配（因为所有的地址都以 / 开头，所以这条规则将匹配到所有请求），如果没有其它匹配，任何请求都会匹配到。
			# 默认情况， Nginx 
				# 1.先进行前缀字符串匹配.
				# 2.然后进行正则表达式匹配：如果前缀字符串匹配到了，并且前缀字符串有^~ ，就不继续往下匹配正则表达式；
				# 3.如果没有这个^~ ，即使前缀匹配到了，也要进行正则表则式匹配，如果正则表达式匹配到了，就是用正则表达式的，没有就是用前缀字符串匹配到的路径。
			# 总结如下：
				# 匹配优先级：精确匹配 >（^~) > 正则匹配 > 字符串（长 > 短）
		
		
		# 通用匹配，但是正则表达式和最长字符串会优先被匹配
		location / {
			
			# root 指令指定静态文件在文件系统中的路径。
			# 请求的 URI 中，和 location 相关的部分会和 root 指定的路径组合成完整的目录名和文件名，从而获取到静态文件。
			root   /home/scorpio/paas-a1/frontend-chat;
			try_files $uri $uri/ /index.html;
		}

		# 前缀字符串匹配,匹配任何以 /documents/ 开头的请求
		# 只有后面的正则表达式没有匹配到时，该配置才会被采用
		location /documents/ {
		   
			[ configuration C ] 
		}
		
		 # 前缀字符串匹配, 匹配任何以 /images/ 开头的请求，匹配成功以后，会停止搜索后面的正则表达式匹配
		location ^~ /images/ {
			root /usr/share/nginx/html/laravel/public
			
			# 用来设置 HTTP 应答中的 Expires 和 Cache-Control 的头标时间，来告诉浏览器访问这个静态文件时，不用再去请求服务器，直接从本地缓存读取就可以了
			expires 7d; 
		}

		# 正则表达式匹配，匹配所有以 gif，jpg，jpeg 结尾的请求
		# 然而，所有请求 /images/ 下的图片会被 configuration D 处理，因为 ^~ 指令，匹配不到这一条规则
		location ~* \.(gif|jpg|jpeg)$ {
			[ configuration E ] 
		}

		# 有时，需要将原来的 URL 请求跳转到新的 URL 链接，但又不想使原来的 URL 失效，比如访问 http://y.top/test 时，需要跳转到 http://yeguxin.top/ ，此时可以配置一个跳转：
		location = /test/ {
			return 302 http://yeguxin.top/;
		}
		
		location / {
			# 即允许重新定义或添加字段传递给代理服务器的请求头。该值可以包含文本、变量和它们的组合。在没有定义proxy_set_header时会继承之前定义的值。
			# 当字段不在请求头中就无法传递了，在这种情况下，可通过设置Host变量，将需传递值赋给Host变量
			# 服务器名称通过代理服务器传递
			proxy_set_header Host $host;    
			proxy_set_header X-Real-IP $remote_addr;
			proxy_set_header X_Forwarded-For $proxy_add_x_forwarded_for;
			
			# 请求转向yeguxin.top2定义的服务器列表，即反向代理，对应upstream负载均衡器，还可以是 http://ip:port
			# proxy_pass 指令将请求转发给和 URL 相关联的被代理的服务器，然后把被代理的服务器的响应转发给客户端。
			proxy_pass http://yeguxin.top2;
		   
			proxy_set_header Upgrade $http_upgrade;
			proxy_set_header Connection  "upgrade";
		}
		gzip on;
		gzip_vary on;
		gzip_comp_level 6;
		gzip_proxied any;
		gzip_types text/plain text/css application/javascript application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;
		gzip_buffers 16 8k;
    }
}
```