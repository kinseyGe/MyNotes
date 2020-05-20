# Phoenix数据类型

数据类型 | Java Mapped | 占用大小(byte)|范围
---|---|---|---
INTEGER| java.lang.Integer|4|-2147483648 to 2147483647
UNSIGNED_INT| java.lang.Integer|4| 0 to 2147483647
BIGINT|java.lang.Long|8|-9223372036854775808 to 9223372036854775807
UNSIGNED_LONG |java.lang.Long|8|0 to 9223372036854775807
TINYINT |java.lang.Byte||-128 to 127
UNSIGNED_TINYINT | java.lang.Byte||0 to 127
SMALLINT |java.lang.Short|2|-32768 to 32767
UNSIGNED_SMALLINT | java.lang.Short|2|0 to 32767
FLOAT |java.lang.Float|4|-3.402823466 E + 38 to 3.402823466 E + 38
UNSIGNED_FLOAT |java.lang.Float|4|0 to 3.402823466 E + 38
DOUBLE |java.lang.Double|8|-1.7976931348623158 E + 308 to 1.7976931348623158 E + 308
UNSIGNED_DOUBLE |java.lang.Double|8|0 to  1.7976931348623158 E + 308
DECIMAL |java.math.BigDecimal|The maximum precision is 38 digits|DECIMAL(precision,scale)
BOOLEAN |java.lang.Boolean|1|TRUE AND FALSE
TIME |java.sql.Time|8|yyyy-MM-dd hh:mm:ss
DATE |java.sql.Date|8|yyyy-MM-dd hh:mm:ss
TIMESTAMP |java.sql.Timestamp|12|yyyy-MM-dd hh:mm:ss[.nnnnnnnnn]
UNSIGNED_TIME |java.sql.Time|8|yyyy-MM-dd hh:mm:ss
UNSIGNED_DATE |java.sql.Date|8|yyyy-MM-dd hh:mm:ss
UNSIGNED_TIMESTAMP |java.sql.Timestamp|12|yyyy-MM-dd hh:mm:ss[.nnnnnnnnn]
VARCHAR |java.lang.String||
CHAR |java.lang.String||
BINARY |byte[]||
VARBINARY |byte[]||
ARRAY|java.sql.Array||
