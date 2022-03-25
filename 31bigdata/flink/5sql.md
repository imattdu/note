









更新：

```
toRetractStream
```



```sh
sqlAggr> (true,sensor_1,1)
# 删除 撤回
sqlAggr> (false,sensor_1,1)
# 插入
sqlAggr> (true,sensor_1,2)
sqlAggr> (true,sensor_6,1)
sqlAggr> (false,sensor_6,1)
sqlAggr> (true,sensor_6,2)
sqlAggr> (false,sensor_6,2)
sqlAggr> (true,sensor_6,3)
sqlAggr> (true,sensor_7,1)
sqlAggr> (true,sensor_10,1)
```













table->stream

追加

撤回







s -> t

fromDataStream









```java
package com.matt.apitest.tableapi;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.streaming.api.TimeCharacteristic;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.api.functions.timestamps.BoundedOutOfOrdernessTimestampExtractor;
import org.apache.flink.streaming.api.windowing.time.Time;
import org.apache.flink.table.api.Table;
import org.apache.flink.table.api.java.StreamTableEnvironment;
import org.apache.flink.types.Row;

public class TimeAndWidow5 {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();

        // 指定事件时间
        env.setStreamTimeCharacteristic(TimeCharacteristic.EventTime);

        DataStream<String> inputStream = env.readTextFile("/Users/matt/workspace/java/bigdata/study-flink/src/main/resources/sensor.txt");

        DataStream<SensorReading> dataStream = inputStream.map(line -> {
            String[] split = line.split(",");
            return new SensorReading(split[0], new Long(split[1]), new Double(split[2]));
        })
                // 指定延迟时间
                .assignTimestampsAndWatermarks(new BoundedOutOfOrdernessTimestampExtractor<SensorReading>(Time.seconds(2)) {
                    @Override
                    public long extractTimestamp(SensorReading sensorReading) {
                        // 指定事件时间字段
                        return sensorReading.getTimestamp() * 1000L;
                    }
                });

        // 3.创建表环境
        StreamTableEnvironment tableEnv = StreamTableEnvironment.create(env);


        // 基于流创建一张表
        //Table dataTable = tableEnv.fromDataStream(dataStream, "id, temperatrue, timestamp, pt.proctime");
        // 和类字段保持一致
        Table dataTable = tableEnv.fromDataStream(dataStream, "id, timestamp.rowtime as ts, temperatrue");

        //Table resTable = dataTable.select("id, temperatrue, pt")
        //        .where("id = 'sensor_1'");

        // 执行sql
        tableEnv.toAppendStream(dataTable, Row.class).print("事件时间");
        // tableEnv.toAppendStream(resTable, Row.class).print("res");

        // job name
        env.execute("my");
    }

}

```









![](https://raw.githubusercontent.com/imattdu/img/main/img/202203232336091.png)









可以追加字段 也可以添加 以一个字段









```java
package com.matt.apitest.tableapi;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.streaming.api.TimeCharacteristic;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.api.functions.timestamps.BoundedOutOfOrdernessTimestampExtractor;
import org.apache.flink.streaming.api.windowing.time.Time;
import org.apache.flink.table.api.Table;
import org.apache.flink.table.api.Tumble;
import org.apache.flink.table.api.java.StreamTableEnvironment;
import org.apache.flink.types.Row;

public class TimeAndWidow5 {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();

        // 指定事件时间
        env.setStreamTimeCharacteristic(TimeCharacteristic.EventTime);

        DataStream<String> inputStream = env.readTextFile("/Users/matt/workspace/java/bigdata/study-flink/src/main/resources/sensor.txt");

        DataStream<SensorReading> dataStream = inputStream.map(line -> {
                    String[] split = line.split(",");
                    return new SensorReading(split[0], new Long(split[1]), new Double(split[2]));
                })
                // 指定延迟时间
                .assignTimestampsAndWatermarks(new BoundedOutOfOrdernessTimestampExtractor<SensorReading>(Time.seconds(2)) {
                    @Override
                    public long extractTimestamp(SensorReading sensorReading) {
                        // 指定事件时间字段
                        return sensorReading.getTimestamp() * 1000L;
                    }
                });

        // 3.创建表环境
        StreamTableEnvironment tableEnv = StreamTableEnvironment.create(env);


        // 基于流创建一张表
        //Table dataTable = tableEnv.fromDataStream(dataStream, "id, temperatrue, timestamp, pt.proctime");
        // 和类字段保持一致
        Table dataTable = tableEnv.fromDataStream(dataStream, "id, timestamp.rowtime as ts, temperatrue");

        //Table resTable = dataTable.select("id, temperatrue, pt")
        //        .where("id = 'sensor_1'");


        // 注册table
        tableEnv.createTemporaryView("sensor", dataTable);
        // 5 窗口操作
        // 滚动窗口
        Table resTable = dataTable.window(Tumble.over("10.seconds").on("ts").as("tw"))
               .groupBy("id, tw")
                .select("id, id.count, temperatrue.avg, tw.end");

        //Table resultTable = dataTable.window(Tumble.over("10.seconds").on("ts").as("tw"))
        //        .groupBy("id, tw")
        //        .select("id, id.count, temp.avg, tw.end");



        // tumble_start
        Table resSQLTable = tableEnv.sqlQuery("select id, count(id) as cnt, tumble_end(ts, interval '10' second) from sensor" +
                " group by id, tumble(ts, interval '10' second)");

        // 执行sql
        //tableEnv.toAppendStream(dataTable, Row.class).print("事件时间");
        // tableEnv.toAppendStream(resTable, Row.class).print("res");
        tableEnv.toAppendStream(resTable, Row.class).print("t1");
        tableEnv.toRetractStream(resSQLTable, Row.class).print("t2");

        // job name
        env.execute("my");
    }

}

```







```sh
t2:9> (true,sensor_6,1,2019-01-17 09:43:30.0)
t2:1> (true,sensor_1,1,2019-01-17 09:43:20.0)
t2:5> (true,sensor_10,1,2019-01-17 09:43:30.0)
t1:5> sensor_10,1,38.1,2019-01-17 09:43:30.0
t1:1> sensor_1,1,990.8,2019-01-17 09:43:20.0
t1:9> sensor_6,1,200.4,2019-01-17 09:43:30.0
t1:9> sensor_6,1,999.4,2019-01-17 09:48:30.0
t2:9> (true,sensor_6,1,2019-01-17 09:48:30.0)
t1:1> sensor_7,1,6.7,2019-01-17 09:43:30.0
t2:9> (true,sensor_6,1,2019-01-17 09:55:10.0)
t2:1> (true,sensor_7,1,2019-01-17 09:43:30.0)
t1:9> sensor_6,1,0.4,2019-01-17 09:55:10.0
t2:1> (true,sensor_1,1,2019-01-17 09:46:50.0)
t1:1> sensor_1,1,100.8,2019-01-17 09:46:50.0
```









如果都是空则返回0

FIELD.sum0













topN 表聚合 返回多行多列数据

















