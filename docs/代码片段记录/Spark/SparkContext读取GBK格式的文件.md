```scala
sparkContext.hadoopFile(args(0),classOf[TextInputFormat], classOf[LongWritable], classOf[Text], 1)
      .map(p => new String(p._2.getBytes, 0, p._2.getLength, "GBK"))
      .map(_.split("#"))
```
