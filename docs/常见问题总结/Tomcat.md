- 问题描述: tomcat启动正常，加载项目也很快但是访问项目超级慢

**解决方案**
> 打开$JAVA_PATH/jre/lib/security/java.security这个文件，找到下面的内容

```
  securerandom.source=file:/dev/random
  替换成  
  securerandom.source=file:/dev/./urandom
```
---
- 问题描述: web项目启动后，有的浏览器可以访问到，有的不能访问，说明是浏览器可能做了什么限制，下面说一种对端口做了限制的情况

**解决方案**
> 为了尽可能适配所有用户的使用情况，还是乖乖的修改服务器端的端口吧，比如Google对6666端口就做了限制，所以还是乖乖的改成8081等其他端口
---
