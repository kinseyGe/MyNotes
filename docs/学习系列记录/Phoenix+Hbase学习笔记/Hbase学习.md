# HBase的逻辑模型

  1. com.cnn.ww对应的是rowkey，相当于mysql的主键的概念
  2. contents,anchor：这两个对应的是列族的概念，在物理的存储上，同一个列族的数据存储在相同文件
  3. cnnsi.com,mylook.ca：对应的是列族下面的列，在hbase中列是可以动态增加的
  4. 对应的方格数据表示的是单元数据,即对应rowkey,cf:column下面的具体的值,其中tn:表示的是时间戳，单元数据的不同版本
