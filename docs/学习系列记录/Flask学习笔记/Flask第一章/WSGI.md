- web服务网关接口(web server gateway interface)
- 本身它是一个协议，实现该协议的模块为：wsgiref、werkzeug，实现协议的模块本质上就是一个socket服务端主要用于接受用户的请求，并处理。一般web框架基于wsgi实现，这样实现关注点分离。
- 示例

```
#!/usr/bin/env python
# -*- coding:utf-8 -*-
from werkzeug.wrappers import Request, Response
@Request.application
def hello(request):
    return Response('Hello World!')

if __name__ == '__main__':
    from werkzeug.serving import run_simple
    run_simple('localhost', 4000, hello)
```

实质上flask源码就是采用的上述函数进行的二次封装，如下

```
from werkzeug.wrappers import Response
from werkzeug.serving import run_simple
class Flask(object):
    #初始化函数入口
    def __call__(self, env,start_response):
        resonse = Response('hello')
        return resonse(env,start_response)

    def run(self,host,port):
        return run_simple(self,host,port)
app = Flask()
if __name__ == '__main__':
    app.run('host','port')
```

> 中间件：通过重写__call__()函数可以实现比before_request更靠前的操作