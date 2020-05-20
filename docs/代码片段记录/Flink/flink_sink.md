
```scala
package utils

import java.net.{InetAddress, InetSocketAddress}
import java.sql.{Connection, DriverManager, PreparedStatement}
import java.util
import java.util.Properties

import org.apache.flink.api.common.functions.RuntimeContext
import org.apache.flink.api.common.serialization.SimpleStringSchema
import org.apache.flink.configuration.Configuration
import org.apache.flink.streaming.api.functions.sink.{RichSinkFunction, SinkFunction}
import org.apache.flink.streaming.connectors.elasticsearch.{ElasticsearchSinkFunction, RequestIndexer}
import org.apache.flink.streaming.connectors.elasticsearch5.ElasticsearchSink
import org.apache.flink.streaming.connectors.kafka.FlinkKafkaProducer010
import org.apache.flink.streaming.connectors.kafka.internals.KeyedSerializationSchemaWrapper
import org.apache.flink.streaming.connectors.redis.RedisSink
import org.apache.flink.streaming.connectors.redis.common.config.FlinkJedisPoolConfig
import org.apache.flink.streaming.connectors.redis.common.mapper.{RedisCommand, RedisCommandDescription, RedisMapper}
import org.elasticsearch.action.index.IndexRequest
import org.elasticsearch.client.Requests

/**
  * Author: risen
  * Date: 2019/7/25 9:45
  * Desc: flink 流式处理的sink工具类(es,db,redis,kafka)
  */
object FlinkStreamSinkUtils {

  //flink 流式处理到es中,注意其中端口号为9300
  def streamSinkEs(clusterName:String,hosts:String,esPort:String,esIndex:String): ElasticsearchSink[util.HashMap[String,String]] ={
    val esConfig = new util.HashMap[String,String]
    esConfig.put("cluster.name", clusterName)
    esConfig.put("bulk.flush.max.actions", "1")
    //1、用来表示是否开启重试机制
    esConfig.put("bulk.flush.backoff.enable", "true")
    //2、重试策略，又可以分为以下两种类型
    //a、指数型，表示多次重试之间的时间间隔按照指数方式进行增长。eg:2 -> 4 -> 8 ...
    //esConfig.put("bulk.flush.backoff.type", "EXPONENTIAL");
    //b、常数型，表示多次重试之间的时间间隔为固定常数。eg:2 -> 2 -> 2 ...
    esConfig.put("bulk.flush.backoff.type", "CONSTANT")
    //3、进行重试的时间间隔。对于指数型则表示起始的基数
    esConfig.put("bulk.flush.backoff.delay", "2")
    //4、失败重试的次数
    esConfig.put("bulk.flush.backoff.retries", "3")
    //如果是集群可以用逗号分隔来实现连接
    val transportAddresses = new util.ArrayList[InetSocketAddress]
    for(host <- hosts.split(",")){
      transportAddresses.add(new InetSocketAddress(InetAddress.getByName(host), esPort.toInt))
    }
    new ElasticsearchSink(esConfig, transportAddresses, new ElasticsearchSinkFunction[util.HashMap[String,String]] {

      //实现es逻辑操作增删改查
      def createIndexRequest(dataMap: util.HashMap[String,String]): IndexRequest = {
        //接受到的信息作为一个字符串处理（这里的不一定用string传入，传入类型取决于DataStream的类型）
        val esType = dataMap.get("es_type")
        val esId = dataMap.get("es_id")
        dataMap.remove("es_type")
        dataMap.remove("es_id")
        Requests.indexRequest().index(esIndex).`type`(esType).source(dataMap).id(esId)
      }
      override def process(t: util.HashMap[String,String], runtimeContext: RuntimeContext, requestIndexer: RequestIndexer): Unit = {
        requestIndexer.add(createIndexRequest(t))
      }
    })
  }

  //flink 流式处理到redis中
  def streamSinkRedis(redisHost:String,redisPort:String): RedisSink[(String,String)] ={
    val jedisPoolConfig = new FlinkJedisPoolConfig.Builder().setHost(redisHost).setPort(redisPort.toInt).build()
    new RedisSink[(String, String)](jedisPoolConfig,new MyRedisMapper())
  }

  //flink 流式处理到kafka中
  def streamSinkKafka(topicName:String,brokerList:String,groupId:String): FlinkKafkaProducer010[String] ={
    val properties = new Properties()
    properties.setProperty("bootstrap.servers",brokerList)
    properties.setProperty("group.id",groupId)
    new FlinkKafkaProducer010[String](topicName,new KeyedSerializationSchemaWrapper[String](new SimpleStringSchema()),properties)
  }

  //flink 流式处理到数据库中
  def streamSinkDB(classDriverName:String,jdbcUrl:String,username:String,password:String,sql:String):MyDbMapper ={
    new MyDbMapper(classDriverName,jdbcUrl,username,password,sql)
  }
}

//实现redisMapper
class MyRedisMapper extends RedisMapper[Tuple2[String,String]]{
  override def getCommandDescription: RedisCommandDescription = {
    new RedisCommandDescription(RedisCommand.LPUSH)
  }
  override def getKeyFromData(data: (String, String)): String = data._1

  override def getValueFromData(data: (String, String)): String = data._2
}

//实现RichSinkFunction
class MyDbMapper(classDriverName:String,jdbcUrl:String,username:String,password:String,sql:String) extends RichSinkFunction[util.HashMap[Integer,Object]]{
  var conn: Connection = _
  var pres:PreparedStatement = _
  override def open(parameters: Configuration): Unit = {
    Class.forName(classDriverName)
    conn = DriverManager.getConnection(jdbcUrl, username, password)
    pres = conn.prepareStatement(sql)
    super.close()
  }

  override def invoke(dataMap: util.HashMap[Integer,Object], context: SinkFunction.Context[_]): Unit = {
    import scala.collection.JavaConversions._
    for (index <- dataMap.keySet()){
      val value = dataMap.get(index)
      pres.setObject(index,value)
    }
    println(dataMap.values())
    pres.executeUpdate()
  }
  override def close(): Unit = {
    conn.close()
    pres.close()
  }
}

```
