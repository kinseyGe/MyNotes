```xml
/**
pom依赖
<dependency>
   	<groupId>org.elasticsearch</groupId>
 	<artifactId>elasticsearch-hadoop</artifactId>
    <version>2.2.0-m1</version>
</dependency>
**/
```
```java
import data.spark.batch.cardbin.util.CardBinFields;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.sql.SQLContext;
//import ....

public class SparkConnectionEs{
    //spark直连es并通过CardBinFields实体转为sparkRdd从而注册成table
    private static String sourceIP = "192.168.23.23";
    private static String esPath = "ybs_cardbin_info_bak/cardbin";//es_index/es_type
    public static void main(String[] args) throws Exception {
    JavaRDD<CardBinFields> esdataRdd = JavaEsSpark.esRDD(sparkContext, esPath).map(new Function<Tuple2<String, Map<String, Object>>, CardBinFields>() {
			private static final long serialVersionUID = 1L;

			public CardBinFields call(Tuple2<String, Map<String, Object>> v1) throws Exception {
				CardBinFields cardbin = new CardBinFields();
				cardbin.setId(v1._1);
				cardbin.setBank_no(v1._2.get("bank_no").toString());
				return cardbin;
			}
		});
        DataFrame tfcardnoDF = sqlContext.createDataFrame(esdataRdd, CardBinFields.class).select("id", "bank_no");
		tfcardnoDF.registerTempTable("ES_FIELDS");
    }
}

```
