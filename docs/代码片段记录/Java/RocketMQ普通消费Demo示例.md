 ```xml
    // pom文件配置
    <dependency>
        <groupId>org.apache.rocketmq</groupId>
        <artifactId>rocketry-client</artifactId>
        <version>4.3.1</version>
    </dependency>
```

```java
    // 生产者demo代码
    import org.apache.rocketmq.client.producer.DefaultMQProducer;
    import org.apache.rocketmq.client.producer.SendResult;
    import org.apache.rocketmq.common.message.Message;
    import org.apache.rocketmq.remoting.common.RemotingHelper;

    public class RocketMQProducer {
        public static void main(String[] args) throws Exception {
            // 实例化 producer，指定一个唯一的组名
            DefaultMQProducer producer = new DefaultMQProducer("risen_rocketmq_group");
            producer.setVipChannelEnabled(false);
            // 指定 name server 地址
            producer.setNamesrvAddr("192.168.5.57:9876;192.168.5.58:9876");
            // 启动 producer
            producer.start();
            //测试发送一百条数据
            /*for (int i = 0; i < 100; i++) {
                // 创建消息实例, 指定 topic, tag 和 message
                Message msg = new Message("rocketmq_test", "TagA", ("Hello RocketMQ " + i).getBytes(RemotingHelper.DEFAULT_CHARSET));
                // 发送消息，同步方式，等待返回结果
                SendResult sendResult = producer.send(msg);
                System.out.println(sendResult);
            }*/
            //测试发送一条数据
            Message msg = new Message("rocketmq_test", "TagA", ("我是一条消息").getBytes(RemotingHelper.DEFAULT_CHARSET));
            //发送消息，同步方式，等待返回结果
            SendResult sendResult = producer.send(msg);
            System.out.println(sendResult);
            // 关闭
            producer.shutdown();
        }
    }

    // 消费者demo代码
    import org.apache.rocketmq.client.consumer.DefaultMQPushConsumer;
    import org.apache.rocketmq.client.consumer.listener.ConsumeConcurrentlyContext;
    import org.apache.rocketmq.client.consumer.listener.ConsumeConcurrentlyStatus;
    import org.apache.rocketmq.client.consumer.listener.MessageListenerConcurrently;
    import org.apache.rocketmq.client.exception.MQClientException;
    import org.apache.rocketmq.common.consumer.ConsumeFromWhere;
    import org.apache.rocketmq.common.message.MessageExt;

    import java.util.List;

    public class RocketMQConsumer {
        public static void main(String[] args) throws MQClientException {
            //需要一个risen_rocketmq_group名字作为构造方法的参数，这里为risen_rocketmq_group
            DefaultMQPushConsumer consumer = new DefaultMQPushConsumer("risen_rocketmq_group");
            consumer.setVipChannelEnabled(false);
            //同样也要设置NameServer地址
            consumer.setNamesrvAddr("192.168.5.57:9876;192.168.5.58:9876");

            //这里设置的是一个consumer的消费策略
            //CONSUME_FROM_LAST_OFFSET 默认策略，从该队列最尾开始消费，即跳过历史消息
            //CONSUME_FROM_FIRST_OFFSET 从队列最开始开始消费，即历史消息（还储存在broker的）全部消费一遍
            //CONSUME_FROM_TIMESTAMP 从某个时间点开始消费，和setConsumeTimestamp()配合使用，默认是半个小时以前
            consumer.setConsumeFromWhere(ConsumeFromWhere.CONSUME_FROM_FIRST_OFFSET);
            //设置consumer所订阅的Topic和Tag，*代表全部的Tag
            consumer.subscribe("rocketmq_test", "*");

            //设置一个Listener，主要进行消息的逻辑处理
            consumer.registerMessageListener(new MessageListenerConcurrently() {
                public ConsumeConcurrentlyStatus consumeMessage(List<MessageExt> msgs,ConsumeConcurrentlyContext context) {
                    for(MessageExt msg : msgs){
                        System.out.println(msg + ",消费的数据内容为===>" + new String(msg.getBody()));
                    }
                    System.out.println("----------------------------------------------");
                    //返回消费状态
                    //CONSUME_SUCCESS 消费成功
                    //RECONSUME_LATER 消费失败，需要稍后重新消费
                    return ConsumeConcurrentlyStatus.CONSUME_SUCCESS;
                }
            });
            //调用start()方法启动consumer
            consumer.start();
            System.out.println("Consumer Started.");
        }
    }
```
![Demo结果](http://tva1.sinaimg.cn/large/007X8olVly1g6wmvc6brdj316p08n75d.jpg)
