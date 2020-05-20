```scala
  def getValue(baseConfigKey:String,baseConfigPath:String):String={
    val baseConfigData = Source.fromFile(baseConfigPath,"UTF-8").mkString
    JSON.parseFull(baseConfigData).asInstanceOf[Some[Map[String, Any]]].get(baseConfigKey).toString
  }
```
