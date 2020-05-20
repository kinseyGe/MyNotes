```scala
val connection =() =>{
      Class.forName("com.oscar.Driver").newInstance()
      DriverManager.getConnection(URL,用户名,密码)
    }

new JdbcRDD(sparkSession.sparkContext,
  connection,"SELECT * FROM TEST WHERE 1 = ? AND 1 = ?" ,1, 1, 5,
  res => {
    var dataMap:Map[String,Any] = Map()
    val rsmd = res.getMetaData
    val count = rsmd.getColumnCount
    for (i <- 1 to count) {
      val key = rsmd.getColumnLabel(i)
      val value = res.getString(i)
      dataMap += (key -> value)
    }
    dataMap
  }
)
```
