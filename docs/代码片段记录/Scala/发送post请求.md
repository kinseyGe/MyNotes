```xml
# pom文件配置
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpcore</artifactId>
    <version>4.4.3</version>
</dependency>
<dependency>
    <groupId>org.apache.httpcomponents</groupId>
    <artifactId>httpclient</artifactId>
    <version>4.5.1</version>
</dependency>
```
```scala
# 代码
def sendPost(url:String,params:String): String ={
    val httpClient = HttpClients.createDefault()
    val post = new HttpPost(url)
    post.setHeader("Content-Type", "application/json")
    post.setEntity(new StringEntity(params, "UTF-8"))
    val response = httpClient.execute(post)
    EntityUtils.toString(response.getEntity,"UTF-8")
  }
```
