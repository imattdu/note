# API





## Environment



### getExecutionEnvironment

**这个比较常用**

​		创建一个执行环境，表示当前执行程序的上下文。 如果程序是独立调用的，则 此方法返回本地执行环境；如果从命令行客户端调用程序以提交到集群，则此方法 返回此集群的执行环境，也就是说，getExecutionEnvironment 会根据查询运行的方 式决定返回什么样的运行环境，是最常用的一种创建执行环境的方式。



```java
StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
```

### createLocalEnvironment

返回本地执行环境，需要在调用时指定默认的并行度。

```java
LocalStreamEnvironment env = StreamExecutionEnvironment.createLocalEnvironment(1);
```





### createRemoteEnvironment

返回集群执行环境，将 Jar 提交到远程服务器。需要在调用时指定 JobManager 的 IP 和端口号，并指定要在集群中运行的 Jar 包。



```java
StreamExecutionEnvironment env =
StreamExecutionEnvironment.createRemoteEnvironment("jobmanage-hostname", 6123, "YOURPATH//WordCount.jar");
```







## source



### 从集合读取数据



com.matt.apitest.source

```java
package com.matt.apitest.source;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.datastream.DataStreamSource;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;

import java.util.Arrays;

/**
 * @author matt
 * @create 2022-01-16 14:36
 */
public class SourceTest1_Collection {


    public static void main(String[] args) throws Exception {

        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        env.setParallelism(1);
        DataStream<SensorReading> dataStream = env.fromCollection(Arrays.asList(
                new SensorReading("sensor_1", 1547718199L, 35.8),
                new SensorReading("sensor_6", 1547718201L, 15.4),
                new SensorReading("sensor_7", 1547718202L, 6.7),
                new SensorReading("sensor_10", 1547718205L, 38.1)
        ));

        dataStream.print("collection");

        // job name
        env.execute("my");


    }
}

```



### 从文件读取数据



```java
package com.matt.apitest.source;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;

import java.util.Arrays;

/**
 * @author matt
 * @create 2022-01-16 14:54
 */
public class SourceTest2_File {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        //env.setParallelism(1);
        // /Users/matt/workspace/java/bigdata
        DataStream<String> dataStream = env.readTextFile("/Users/matt/workspace/java/bigdata/study-flink/src/main/resources/sensor.txt");

        dataStream.print("file");

        // job name
        env.execute("my");

    }

}

```

### 以 kafka 消息队列的数据作为来源





```xml
<dependency>
   <groupId>org.apache.flink</groupId>
   <artifactId>flink-connector-kafka-0.11_2.12</artifactId>
   <version>1.10.1</version>
</dependency>
```





```java
package com.matt.apitest.source;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.api.common.serialization.SimpleStringSchema;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.connectors.kafka.FlinkKafkaConsumer011;

import java.util.Arrays;
import java.util.Properties;

/**
 * @author matt
 * @create 2022-01-16 15:05
 */
public class SourceTest3_Kafka {


    public static void main(String[] args) throws Exception {

        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        env.setParallelism(1);

        //
        Properties properties = new Properties();
        properties.setProperty("bootstrap.servers", "matt05:9092");
        properties.setProperty("group.id", "consumer-group");
        properties.setProperty("key.deserializer",
                "org.apache.kafka.common.serialization.StringDeserializer");
        properties.setProperty("value.deserializer",
                "org.apache.kafka.common.serialization.StringDeserializer");
        properties.setProperty("auto.offset.reset", "latest");

        DataStream<String> dataStream = env.addSource( new
                FlinkKafkaConsumer011<String>("sensor", new SimpleStringSchema(), properties));

        dataStream.print("kafka");

        // job name
        env.execute("kafak_job");


    }
}

```









### 自定义 Source



实现SourceFunction接口





```java
package com.matt.apitest.source;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.api.functions.source.SourceFunction;

import java.util.HashMap;
import java.util.Properties;
import java.util.Random;

/**
 * @author matt
 * @create 2022-01-16 15:50
 */
public class SourceTest4_UDF {


    public static void main(String[] args) throws Exception {

        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        env.setParallelism(1);

        DataStream<SensorReading> dataStream = env.addSource(new MySensorSouce());
        dataStream.print("kafka");

        // job name
        env.execute("kafak_job");


    }

    public static class MySensorSouce implements SourceFunction<SensorReading> {
        // 标志位 控制数据的产生
        private boolean running = true;

        /**
         * 功能：
         *
         * @param ctx
         * @return void
         * @author matt
         * @date 2022/1/16
         */
        @Override
        public void run(SourceContext<SensorReading> ctx) throws Exception {
            // 随机数发生器
            Random random = new Random();

            HashMap<String, Double> sensorTempMap = new HashMap();
            for (int i = 0; i < 10; i++) {
                sensorTempMap.put("sensor_" + (i + 1), 60 + random.nextGaussian() * 20);
            }

            while (running) {
                for (String sensorId : sensorTempMap.keySet()) {
                    // 当前温度随机波动
                    Double newtemp = sensorTempMap.get(sensorId) + random.nextDouble();
                    sensorTempMap.put(sensorId, newtemp);
                    ctx.collect(new SensorReading(sensorId, System.currentTimeMillis(), newtemp));
                }

                // 控制输出评率
                Thread.sleep(5000L);
            }
        }

        @Override
        public void cancel() {
            running = false;
        }
    }
}

```







## Transform





### map flatmap filter



```java
package com.matt.apitest.transform;

import org.apache.flink.api.common.functions.FilterFunction;
import org.apache.flink.api.common.functions.FlatMapFunction;
import org.apache.flink.api.common.functions.MapFunction;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.util.Collector;

import java.io.File;

/**
 * @author matt
 * @create 2022-01-16 17:16
 */
public class TransFormTest1_Base {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        env.setParallelism(16);
        DataStream<String> inputStream = env.readTextFile("/Users/matt/workspace/java/bigdata/study-flink/src/main/resources/sensor.txt");

        // 1 map string -> len(string)
        // 方-》园
        DataStream<Integer> mapStream = inputStream.map(new MapFunction<String, Integer>() {
            @Override
            public Integer map(String s) throws Exception {
                return s.length();
            }
        });

        // mapStream.print("map");

        // flatMap 按逗号切分字端

        DataStream<String> flatMapStream = inputStream.flatMap(new FlatMapFunction<String, String>() {
            @Override
            public void flatMap(String s, Collector<String> collector) throws Exception {
                String[] fields = s.split(",");
                for (String field : fields) {
                    collector.collect(field);
                }
            }
        });

        // 3.filter 过滤 筛选某个数据
        DataStream<String> filterStream = inputStream.filter(new FilterFunction<String>() {
            @Override
            public boolean filter(String s) throws Exception {
                // true 要 false 不要这个数据
                return s.startsWith("sensor_1");
            }
        });

        mapStream.print("map");
        flatMapStream.print("flatMap");
        filterStream.print("filter");

        // job name
        env.execute("trans-form");

    }


}

```





### keyedBy



![](https://raw.githubusercontent.com/imattdu/img/main/img/202203121630615.png)





DataStream → KeyedStream：逻辑地将一个流拆分成不相交的分区，每个分 区包含具有相同 key 的元素，在内部以 hash 的形式实现的。







```java
package com.matt.apitest.transform;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.api.common.functions.FilterFunction;
import org.apache.flink.api.common.functions.FlatMapFunction;
import org.apache.flink.api.common.functions.MapFunction;
import org.apache.flink.api.java.tuple.Tuple;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.datastream.KeyedStream;
import org.apache.flink.streaming.api.datastream.SingleOutputStreamOperator;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.util.Collector;

/**
 * @author matt
 * @create 2022-01-17 22:14
 */
public class TransFormTest1_RollingAggregation {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        env.setParallelism(16);
        DataStream<String> inputStream = env.readTextFile("/Users/matt/workspace/java/bigdata/study-flink/src/main/resources/sensor.txt");
        
        // SensorReading
        //DataStream<SensorReading> dataStream = inputStream.map(new MapFunction<String, SensorReading>() {
        //
        //    @Override
        //    public SensorReading map(String value) throws Exception {
        //        String[] fields = value.split(",");
        //        return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        //    }
        //});

        DataStream<SensorReading> dataStream = inputStream.map( s -> {
            String[] fields = s.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        });

         //分组
        KeyedStream<SensorReading, Tuple> keyedStream = dataStream.keyBy("id");
        keyedStream.print("keyed");
        
      
      // job name
        env.execute("trans-form");

    }

}

```





### 滚动聚合算子（Rolling Aggregation）



**这些算子可以针对 KeyedStream 的每一个支流做聚合。**



- sum()
- min()
- max()
- minBy()
- maxBy()



max只更新统计的字段

maxBy:非max字段使用最大的值字段那条记录的字段





```java
package com.matt.apitest.transform;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.api.common.functions.FilterFunction;
import org.apache.flink.api.common.functions.FlatMapFunction;
import org.apache.flink.api.common.functions.MapFunction;
import org.apache.flink.api.java.tuple.Tuple;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.datastream.KeyedStream;
import org.apache.flink.streaming.api.datastream.SingleOutputStreamOperator;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.util.Collector;

/**
 * @author matt
 * @create 2022-01-17 22:14
 */
public class TransFormTest1_RollingAggregation {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        env.setParallelism(16);
        DataStream<String> inputStream = env.readTextFile("/Users/matt/workspace/java/bigdata/study-flink/src/main/resources/sensor.txt");
        
      

        DataStream<SensorReading> dataStream = inputStream.map( s -> {
            String[] fields = s.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        });

         //分组
        KeyedStream<SensorReading, Tuple> keyedStream = dataStream.keyBy("id");

        // KeyedStream<SensorReading, String> keyedStream1 = dataStream.keyBy(d -> d.getId());
        // 滚动聚合
        // max 当前的字段 maxBy

        // maxBy 最大的那条记录 没有她大 非max字段也要改
        // max 最大值那条字段
        // DataStream<SensorReading> resultStream = keyedStream.max("temperatrue");
        //resultStream.print();
        keyedStream.print("keyed");
        // job name
        env.execute("trans-form");

    }

}

```



### reduce



KeyedStream → DataStream：一个分组数据流的聚合操作，合并当前的元素 和上次聚合的结果，产生一个新的值，返回的流中包含每一次聚合的结果，而不是 只返回最后一次聚合的最终结果







```java
package com.matt.apitest.transform;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.api.common.functions.ReduceFunction;
import org.apache.flink.api.java.tuple.Tuple;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.datastream.KeyedStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;

/**
 * @author matt
 * @create 2022-01-17 22:48
 */
public class TransFormTest3 {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        env.setParallelism(16);
        DataStream<String> inputStream = env.readTextFile("/Users/matt/workspace/java/bigdata/study-flink/src/main/resources/sensor.txt");

        DataStream<SensorReading> dataStream = inputStream.map(s -> {
            String[] fields = s.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        });

        //分组
        KeyedStream<SensorReading, Tuple> keyedStream = dataStream.keyBy("id");

        DataStream<SensorReading> resultStream = keyedStream.reduce(new ReduceFunction<SensorReading>() {
            @Override
            public SensorReading reduce(SensorReading v1, SensorReading v2) throws Exception {
                return new SensorReading(v1.getId(), v2.getTimestamp(),
                        Math.max(v1.getTemperatrue(), v2.getTemperatrue()));
            }
        });
        resultStream.print();
        // job name
        env.execute("trans-form");

    }

}


```





### split select



![](https://raw.githubusercontent.com/imattdu/img/main/img/202203121704070.png)



DataStream → SplitStream：根据某些特征把一个 DataStream 拆分成两个或者 多个 DataStream。





![](https://raw.githubusercontent.com/imattdu/img/main/img/202203121704955.png)



SplitStream→DataStream：从一个 SplitStream 中获取一个或者多个 DataStream。





**需求：传感器数据按照温度高低（以 30 度为界），拆分成两个流。**





拆分 选择





```java
package com.matt.apitest.transform;

import com.matt.apitest.beans.SensorReading;
import org.apache.commons.math3.geometry.partitioning.SubHyperplane;
import org.apache.flink.api.common.functions.MapFunction;
import org.apache.flink.api.common.functions.ReduceFunction;
import org.apache.flink.api.java.tuple.Tuple;
import org.apache.flink.api.java.tuple.Tuple2;
import org.apache.flink.api.java.tuple.Tuple3;
import org.apache.flink.streaming.api.collector.selector.OutputSelector;
import org.apache.flink.streaming.api.datastream.*;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.api.functions.co.CoMapFunction;

import java.util.Collections;

/**
 * @author matt
 * @create 2022-01-17 23:30
 */
public class TransFromTest4_MultiplStreams {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        env.setParallelism(16);
        DataStream<String> inputStream = env.readTextFile("D:\\matt\\workspace\\idea\\hadoop\\study-flink\\src\\main\\resources\\sensor.txt");

        DataStream<SensorReading> dataStream = inputStream.map(s -> {
            String[] fields = s.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        });


        //1. 分流
        SplitStream<SensorReading> splitStream = dataStream.split(new OutputSelector<SensorReading>() {
            @Override
            public Iterable<String> select(SensorReading sensorReading) {
                return (sensorReading.getTemperatrue() > 30) ? Collections.singletonList("high") :
                        Collections.singletonList("low") ;
            }
        });

        DataStream<SensorReading> highTempStream = splitStream.select("high");
        DataStream<SensorReading> lowTempStream = splitStream.select("low");




        highTempStream.print();
        System.out.println("-------");
        lowTempStream.print();

        // job name
        env.execute("trans-form");

    }

}

```









### Connect 和 CoMap







![](https://raw.githubusercontent.com/imattdu/img/main/img/202203131653269.png)





DataStream,DataStream → ConnectedStreams：连接两个保持他们类型的数 据流，两个数据流被 Connect 之后，只是被放在了一个同一个流中，内部依然保持 各自的数据和形式不发生任何变化，两个流相互独立。







![](https://raw.githubusercontent.com/imattdu/img/main/img/202203131654180.png)



ConnectedStreams → DataStream：作用于 ConnectedStreams 上，功能与 map 和 flatMap 一样，对 ConnectedStreams 中的每一个 Stream 分别进行 map 和 flatMap 处理。





connetc 将俩个流放到一个流中





```java
package com.matt.apitest.transform;

import com.matt.apitest.beans.SensorReading;
import org.apache.commons.math3.geometry.partitioning.SubHyperplane;
import org.apache.flink.api.common.functions.MapFunction;
import org.apache.flink.api.common.functions.ReduceFunction;
import org.apache.flink.api.java.tuple.Tuple;
import org.apache.flink.api.java.tuple.Tuple2;
import org.apache.flink.api.java.tuple.Tuple3;
import org.apache.flink.streaming.api.collector.selector.OutputSelector;
import org.apache.flink.streaming.api.datastream.*;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.api.functions.co.CoMapFunction;

import java.util.Collections;

/**
 * @author matt
 * @create 2022-01-17 23:30
 */
public class TransFromTest4_MultiplStreams {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        env.setParallelism(16);
        DataStream<String> inputStream = env.readTextFile("/Users/matt/workspace/java/bigdata/study-flink/src/main/resources/sensor.txt");

        DataStream<SensorReading> dataStream = inputStream.map(s -> {
            String[] fields = s.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        });


        //1. 分流
        SplitStream<SensorReading> splitStream = dataStream.split(new OutputSelector<SensorReading>() {
            @Override
            public Iterable<String> select(SensorReading sensorReading) {
                return (sensorReading.getTemperatrue() > 30) ? Collections.singletonList("high") :
                        Collections.singletonList("low") ;
            }
        });

        DataStream<SensorReading> highTempStream = splitStream.select("high");
        DataStream<SensorReading> lowTempStream = splitStream.select("low");




        highTempStream.print();
        System.out.println("-------");
        lowTempStream.print();


        System.out.println("河流");
        DataStream<Tuple2<String, Double>> warningStream = highTempStream.map(new MapFunction<SensorReading, Tuple2<String, Double>>() {
            @Override
            public Tuple2<String, Double> map(SensorReading v) throws Exception {
                return new Tuple2<>(v.getId(), v.getTemperatrue());
            }
        });

        ConnectedStreams<Tuple2<String, Double>, SensorReading> connectedStreams = warningStream.connect(lowTempStream);


        SingleOutputStreamOperator<Object> res = connectedStreams.map(new CoMapFunction<Tuple2<String, Double>, SensorReading, Object>() {
            @Override
            public Object map1(Tuple2<String, Double> v) throws Exception {
                return new Tuple3<>(v.f0, v.f1, "high temp warning");
            }

            @Override
            public Object map2(SensorReading v) throws Exception {
                return new Tuple2<>(v.getId(), "normal");
            }
        });

        res.print();

        System.out.println("union");
        DataStream<SensorReading> union = highTempStream.union(lowTempStream);
        union.print();

        // job name
        env.execute("trans-form");

    }

}

```





### Union





![](https://raw.githubusercontent.com/imattdu/img/main/img/202203131656023.png)



DataStream → DataStream：对两个或者两个以上的 DataStream 进行 union 操 作，产生一个包含所有 DataStream 元素的新 DataStream。



#### **Connect 与 Union 区别**

1. Union 之前两个流的类型必须是一样，Connect 可以不一样，在之后的 coMap 中再去调整成为一样的。
2. Connect 只能操作两个流，Union 可以操作多个。



```java
package com.matt.apitest.transform;

import com.matt.apitest.beans.SensorReading;
import org.apache.commons.math3.geometry.partitioning.SubHyperplane;
import org.apache.flink.api.common.functions.MapFunction;
import org.apache.flink.api.common.functions.ReduceFunction;
import org.apache.flink.api.java.tuple.Tuple;
import org.apache.flink.api.java.tuple.Tuple2;
import org.apache.flink.api.java.tuple.Tuple3;
import org.apache.flink.streaming.api.collector.selector.OutputSelector;
import org.apache.flink.streaming.api.datastream.*;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.api.functions.co.CoMapFunction;

import java.util.Collections;

/**
 * @author matt
 * @create 2022-01-17 23:30
 */
public class TransFromTest4_MultiplStreams {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        env.setParallelism(16);
        DataStream<String> inputStream = env.readTextFile("/Users/matt/workspace/java/bigdata/study-flink/src/main/resources/sensor.txt");

        DataStream<SensorReading> dataStream = inputStream.map(s -> {
            String[] fields = s.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        });


        //1. 分流
        SplitStream<SensorReading> splitStream = dataStream.split(new OutputSelector<SensorReading>() {
            @Override
            public Iterable<String> select(SensorReading sensorReading) {
                return (sensorReading.getTemperatrue() > 30) ? Collections.singletonList("high") :
                        Collections.singletonList("low") ;
            }
        });

        DataStream<SensorReading> highTempStream = splitStream.select("high");
        DataStream<SensorReading> lowTempStream = splitStream.select("low");




        highTempStream.print();
        System.out.println("-------");
        lowTempStream.print();


        System.out.println("河流");
        DataStream<Tuple2<String, Double>> warningStream = highTempStream.map(new MapFunction<SensorReading, Tuple2<String, Double>>() {
            @Override
            public Tuple2<String, Double> map(SensorReading v) throws Exception {
                return new Tuple2<>(v.getId(), v.getTemperatrue());
            }
        });

        ConnectedStreams<Tuple2<String, Double>, SensorReading> connectedStreams = warningStream.connect(lowTempStream);


        SingleOutputStreamOperator<Object> res = connectedStreams.map(new CoMapFunction<Tuple2<String, Double>, SensorReading, Object>() {
            @Override
            public Object map1(Tuple2<String, Double> v) throws Exception {
                return new Tuple3<>(v.f0, v.f1, "high temp warning");
            }

            @Override
            public Object map2(SensorReading v) throws Exception {
                return new Tuple2<>(v.getId(), "normal");
            }
        });

        res.print();

        System.out.println("union");
        DataStream<SensorReading> union = highTempStream.union(lowTempStream);
        union.print();

        // job name
        env.execute("trans-form");

    }

}

```











## 数据类型





### Flink 支持所有的 Java 和 Scala 基础数据类型，Int, Double, Long, String, …



```java
DataStream<Integer> numberStream = env.fromElements(1, 2, 3, 4);
numberStream.map(data -> data * 2);
```





### Java 和 Scala 元组（Tuples）



```java
DataStream<Tuple2<String, Integer>> personStream = env.fromElements(
 new Tuple2("Adam", 17),
 new Tuple2("Sarah", 23) );
personStream.filter(p -> p.f1 > 18);
```





### Scala 样例类（case classes）

```scala
case class Person(name: String, age: Int)
val persons: DataStream[Person] = env.fromElements(
Person("Adam", 17),
Person("Sarah", 23) )
persons.filter(p => p.age > 18)
```







###  Java 简单对象（POJOs）





```java
public class Person {
public String name;
public int age;
 public Person() {}
 public Person(String name, int age) {
this.name = name;
this.age = age;
}
}
DataStream<Person> persons = env.fromElements(
new Person("Alex", 42),
new Person("Wendy", 23));
```

### 其它（Arrays, Lists, Maps, Enums, 等等）



Flink 对 Java 和 Scala 中的一些特殊目的的类型也都是支持的，比如 Java 的 ArrayList，HashMap，Enum 等等。









## 实现 UDF 函数





一个实现接口的类或者是匿名内部类 或者是lambda函数











### Rich Functions

“富函数”是 DataStream API 提供的一个函数类的接口，所有 Flink 函数类都 有其 Rich 版本。它与常规函数的不同在于，可以获取运行环境的上下文，并拥有一 些生命周期方法，所以可以实现更复杂的功能。



Rich Function 有一个生命周期的概念。典型的生命周期方法有： 

⚫ open()方法是 rich function 的初始化方法，当一个算子例如 map 或者 filter 被调用之前 open()会被调用。 

⚫ close()方法是生命周期中的最后一个调用的方法，做一些清理工作。 

⚫ getRuntimeContext()方法提供了函数的 RuntimeContext 的一些信息，例如函数执行的并行度，任务的名字，以及 state 状态





```java
package com.matt.apitest.transform;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.api.common.functions.MapFunction;
import org.apache.flink.api.common.functions.RichMapFunction;
import org.apache.flink.api.java.tuple.Tuple2;
import org.apache.flink.api.java.tuple.Tuple3;
import org.apache.flink.configuration.Configuration;
import org.apache.flink.streaming.api.collector.selector.OutputSelector;
import org.apache.flink.streaming.api.datastream.ConnectedStreams;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.datastream.SingleOutputStreamOperator;
import org.apache.flink.streaming.api.datastream.SplitStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.api.functions.co.CoMapFunction;
import scala.Enumeration;

import java.util.Collections;

/**
 * @author matt
 * @create 2022-01-24 23:55
 */
public class TransfromTest5_RichFunction {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        env.setParallelism(4);
        DataStream<String> inputStream = env.readTextFile("/Users/matt/workspace/java/bigdata/study-flink/src/main/resources/sensor.txt");

        DataStream<SensorReading> dataStream = inputStream.map(s -> {
            String[] fields = s.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        });


        
        DataStream<Tuple2<String, Integer>> resStream = dataStream.map(
                new MyMapper()
        );
        resStream.print();


        // job name
        env.execute("trans-form");

    }


    public static class MyMapper extends RichMapFunction<SensorReading, Tuple2<String, Integer>> {


        @Override
        public Tuple2<String, Integer> map(SensorReading v) throws Exception {
            return new Tuple2<>(v.getId(), getRuntimeContext().getIndexOfThisSubtask());
        }

        public MyMapper() {
            super();
        }

        @Override
        public void open(Configuration parameters) throws Exception {
            System.out.println("init....");
        }

        @Override
        public void close() throws Exception {
            System.out.println("clear...");
        }
    }

}

```





## sink







### kafka





```xml
<dependency>
 <groupId>org.apache.flink</groupId>
 <artifactId>flink-connector-kafka-0.11_2.12</artifactId>
 <version>1.10.1</version>
</dependency>
```







```java
package com.matt.apitest.sink;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.api.common.serialization.SimpleStringSchema;
import org.apache.flink.streaming.api.SimpleTimerService;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.connectors.kafka.FlinkKafkaConsumer011;
import org.apache.flink.streaming.connectors.kafka.FlinkKafkaProducer011;

/**
 * @author matt
 * @create 2022-01-25 23:33
 */
public class SinkTest1_kafka {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        //env.setParallelism(1);
        DataStream<String> dataStream = env.readTextFile("D:\\matt\\workspace\\idea\\hadoop\\study-flink\\src\\main\\resources\\sensor.txt");

        dataStream.print("file");

        DataStream<String> resStream = dataStream.map(line -> {
            String[] f = line.split(",");
            return new SensorReading(f[0], new Long(f[1]), new Double(f[2])).toString();
        });

        resStream.print();

        resStream.addSink(new FlinkKafkaProducer011<String>("matt05:9092",
                "test", new SimpleStringSchema()));


        // job name
        env.execute("my");

    }
}

```





### redis





```xml
        <dependency>
            <groupId>org.apache.bahir</groupId>
            <artifactId>flink-connector-redis_2.11</artifactId>
            <version>1.0</version>
        </dependency>

```







```java
package com.matt.apitest.sink;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.api.common.serialization.SimpleStringSchema;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.connectors.kafka.FlinkKafkaProducer011;
import org.apache.flink.streaming.connectors.redis.RedisSink;
import org.apache.flink.streaming.connectors.redis.common.config.FlinkJedisPoolConfig;
import org.apache.flink.streaming.connectors.redis.common.mapper.RedisCommand;
import org.apache.flink.streaming.connectors.redis.common.mapper.RedisCommandDescription;
import org.apache.flink.streaming.connectors.redis.common.mapper.RedisMapper;

/**
 * @author matt
 * @create 2022-01-25 23:57
 */
public class SinkTest2_Redis {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        DataStream<String> dataStream = env.readTextFile("/Users/matt/workspace/java/bigdata/study-flink/src/main/resources/sensor.txt");

        dataStream.print("file");

        DataStream<SensorReading> resStream = dataStream.map(line -> {
            String[] f = line.split(",");
            return new SensorReading(f[0], new Long(f[1]), new Double(f[2]));
        });

        resStream.print();
        FlinkJedisPoolConfig config = new FlinkJedisPoolConfig.Builder()
                .setHost("matt05")
                .setPort(6379)
                .build();
        resStream.addSink(new RedisSink<>(config, new MyRedisMapper()));


        // job name
        env.execute("my");

    }

    public static class MyRedisMapper implements RedisMapper<SensorReading> {
        // 保存到 redis 的命令，存成哈希表
        public RedisCommandDescription getCommandDescription() {
            return new RedisCommandDescription(RedisCommand.HSET, "sensor_tempe");
        }

        // key
        public String getKeyFromData(SensorReading data) {
            return data.getId();
        }

        // v
        public String getValueFromData(SensorReading data) {
            return data.getTemperatrue().toString();
        }
    }

}

```







### es





```xml
<!--ES-->
        <dependency>
            <groupId>org.apache.flink</groupId>
            <artifactId>flink-connector-elasticsearch6_2.12</artifactId>
            <version>1.10.1</version>
        </dependency>
```







```java
package com.matt.apitest.sink;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.api.common.functions.RuntimeContext;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.connectors.elasticsearch.ElasticsearchSinkFunction;
import org.apache.flink.streaming.connectors.elasticsearch.RequestIndexer;
import org.apache.flink.streaming.connectors.elasticsearch6.ElasticsearchSink;
import org.apache.flink.streaming.connectors.redis.RedisSink;
import org.apache.flink.streaming.connectors.redis.common.config.FlinkJedisPoolConfig;
import org.apache.http.HttpHost;
import org.elasticsearch.action.index.IndexRequest;
import org.elasticsearch.client.Requests;

import java.util.ArrayList;
import java.util.HashMap;

/**
 * @author matt
 * @create 2022-01-26 0:21
 */
public class SinkTest3_ES {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        
        DataStream<String> dataStream = env.readTextFile("/Users/matt/workspace/java/bigdata/study-flink/src/main/resources/sensor.txt");

        dataStream.print("file");

        DataStream<SensorReading> resStream = dataStream.map(line -> {
            String[] f = line.split(",");
            return new SensorReading(f[0], new Long(f[1]), new Double(f[2]));
        });

        resStream.print();
        ArrayList<HttpHost> httpHosts = new ArrayList<>();
        httpHosts.add(new HttpHost("localhost", 9200));

        resStream.addSink(new ElasticsearchSink.Builder<SensorReading>(httpHosts, new MyEsSinkFunction()).build());

        // job name
        env.execute("my");

    }

    public static class MyEsSinkFunction implements
            ElasticsearchSinkFunction<SensorReading> {
        @Override
        public void process(SensorReading element, RuntimeContext ctx, RequestIndexer
                indexer) {
            HashMap<String, String> dataSource = new HashMap<>();
            dataSource.put("id", element.getId());
            dataSource.put("ts", String.valueOf(element.getTimestamp()));
            dataSource.put("temp", element.getTemperatrue().toString());
            IndexRequest indexRequest = Requests.indexRequest()
                    .index("sensor")
                    .type("readingData")
                    .source(dataSource);
            indexer.add(indexRequest);
        }
    }

}

```











### jdbc





```java
<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.44</version>
        </dependency>
```









```java
package com.matt.apitest.sink;

import com.matt.apitest.beans.SensorReading;

import org.apache.flink.configuration.Configuration;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.api.functions.sink.RichSinkFunction;

import java.sql.DriverManager;
import java.sql.Connection;
import java.sql.PreparedStatement;

/**
 * @author matt
 * @create 2022-01-26 1:41
 */
public class SinkTest4_MySQL {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        env.setParallelism(16);
        DataStream<String> dataStream = env.readTextFile("/Users/matt/workspace/java/bigdata/study-flink/src/main/resources/sensor.txt");

        

        DataStream<SensorReading> resStream = dataStream.map(line -> {
            String[] f = line.split(",");
            return new SensorReading(f[0], new Long(f[1]), new Double(f[2]));
        });

        resStream.print();
        resStream.addSink(new MyJdbcSink());

        // job name
        env.execute();

    }


    public static class MyJdbcSink extends RichSinkFunction<SensorReading> {
        Connection conn = null;
        PreparedStatement insertStmt = null;
        PreparedStatement updateStmt = null;

        // open 主要是创建连接
        @Override
        public void open(Configuration parameters) throws Exception {
            conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/stu_flink",
                    "root", "root");

            // 创建预编译器，有占位符，可传入参数
            insertStmt = conn.prepareStatement("INSERT INTO sensor_temp (id, temp) VALUES ( ?, ?)");
            updateStmt = conn.prepareStatement("UPDATE sensor_temp SET temp = ? WHERE id = ? ");
        }

        // 调用连接，执行 sql
        @Override
        public void invoke(SensorReading value, Context context) throws Exception {
            // 执行更新语句，注意不要留 super
            updateStmt.setDouble(1, value.getTemperatrue());
            updateStmt.setString(2, value.getId());
            updateStmt.execute();
            // 如果刚才 update 语句没有更新，那么插入
            if (updateStmt.getUpdateCount() == 0) {
                insertStmt.setString(1, value.getId());
                insertStmt.setDouble(2, value.getTemperatrue());
                insertStmt.execute();
            }
        }

        @Override
        public void close() throws Exception {
            insertStmt.close();
            updateStmt.close();
            conn.close();
        }
    }

}

```



