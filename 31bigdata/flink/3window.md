

# window

## window

### 概述

window 是一种切割无限数据 为有限块进行处理的手段。

Window 是无限数据流处理的核心，Window 将一个无限的 stream 拆分成有限大小的”buckets”桶，我们可以在这些桶上做计算操作。





### 类型



CountWindow：按照指定的数据条数生成一个 Window，与时间无关。

TimeWindow：按照时间生成 Window。



对于 TimeWindow，可以根据窗口实现原理的不同分成三类：滚动窗口（Tumbling Window）、滑动窗口（Sliding Window）和会话窗口（Session Window）。







#### 1.滚动窗口（Tumbling Windows）：

 将数据依据固定的窗口长度对数据进行切片。

 **特点：时间对齐，窗口长度固定，没有重叠。** 

滚动窗口分配器将每个元素分配到一个指定窗口大小的窗口中，滚动窗口有一 个固定的大小，并且不会出现重叠。例如：如果你指定了一个 5 分钟大小的滚动窗 口，窗口的创建如下图所示：





![](https://raw.githubusercontent.com/imattdu/img/main/img/202203150009317.png)





适用场景：适合做 BI 统计等（做每个时间段的聚合计算）。



*BI(BusinessIntelligence)即[商务智能](https://www.zhihu.com/search?q=商务智能&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A150453753})，它是一套完整的解决方案，用来将企业中现有的数据进行有效的整合，快速准确的提供报表并提出决策依据，帮助企业做出明智的业务经营决策。*



#### 2.滑动窗口（Sliding Windows）



滑动窗口是固定窗口的更广义的一种形式，滑动窗口由固定的窗口长度和滑动 间隔组成。

**特点：时间对齐，窗口长度固定，可以有重叠。**



滑动窗口分配器将元素分配到固定长度的窗口中，与滚动窗口类似，窗口的大 小由窗口大小参数来配置，另一个窗口滑动参数控制滑动窗口开始的频率。因此， 滑动窗口如果滑动参数小于窗口大小的话，窗口是可以重叠的，在这种情况下元素 会被分配到多个窗口中。



例如，你有 10 分钟的窗口和 5 分钟的滑动，那么每个窗口中 5 分钟的窗口里包 含着上个 10 分钟产生的数据，如下图所示：





![](https://raw.githubusercontent.com/imattdu/img/main/img/202203150012483.png)









适用场景：对最近一个时间段内的统计（求某接口最近 5min 的失败率来决定是 否要报警）。



#### 3.会话窗口



由一系列事件组合一个指定时间长度的 timeout 间隙组成，类似于 web 应用的 session，也就是一段时间没有接收到新数据就会生成新的窗口。



**特点：时间无对齐。**



session 窗口分配器通过 session 活动来对元素进行分组，session 窗口跟滚动窗口和滑动窗口相比，不会有重叠和固定的开始时间和结束时间的情况，相反，当它 在一个固定的时间周期内不再收到元素，即非活动间隔产生，那个这个窗口就会关 闭。一个 session 窗口通过一个 session 间隔来配置，这个 session 间隔定义了非活跃 周期的长度，当这个非活跃周期产生，那么当前的 session 将关闭并且后续的元素将 被分配到新的 session 窗口中去。







![](https://raw.githubusercontent.com/imattdu/img/main/img/202203150014097.png)







## window API





### TimeWindow



TimeWindow 是将指定时间范围内的所有数据组成一个 window，一次对一个 window 里面的所有数据进行计算。



#### 1.滚动窗口 

Flink 默认的时间窗口根据 Processing Time 进行窗口的划分，将 Flink 获取到的 数据根据进入 Flink 的时间划分到不同的窗口中。



```java
package com.matt.apitest.window;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.streaming.api.windowing.time.Time;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;

public class Test1 {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        //env.setParallelism(1);



        DataStream<String> inputStream = env.socketTextStream("localhost", 777);
        // 转换成SensorReading类型
        DataStream<SensorReading> dataStream = inputStream.map(line -> {
            String[] fields = line.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        });
        // 开窗测试
        // windowAll 都放在一个窗口里面
        DataStream<SensorReading> t1 = dataStream.keyBy("id")
                .timeWindow(Time.seconds(10)).minBy("temperatrue");

        t1.print();

        env.execute("my");

    }
}


```

minBy():可以是元组的顺序也可以对象的属性



**时间间隔可以通过 Time.milliseconds(x)，Time.seconds(x)，Time.minutes(x)等其 中的一个来指定。**



#### 2.滑动窗口（SlidingEventTimeWindows）

滑动窗口和滚动窗口的函数名是完全一致的，只是在传参数时需要传入两个参 数，一个是 window_size，一个是 sliding_size。



下面代码中的 sliding_size 设置为了 5s，也就是说，每 5s 就计算输出结果一次， 每一次计算的 window 范围是 15s 内的所有元素。





```java
package com.matt.apitest.window;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.streaming.api.windowing.time.Time;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;

public class Test1 {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        //env.setParallelism(1);



        DataStream<String> inputStream = env.socketTextStream("localhost", 777);
        // 转换成SensorReading类型
        DataStream<SensorReading> dataStream = inputStream.map(line -> {
            String[] fields = line.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        });
        // 开窗测试
        // windowAll 都放在一个窗口里面
        DataStream<SensorReading> t1 = dataStream.keyBy("id")
                .timeWindow(Time.seconds(15), Time.seconds(5)).minBy("temperatrue");

        t1.print();

        env.execute("my");

    }
}

```





#### 3.



#### CountWindow



CountWindow 根据窗口中相同 key 元素的数量来触发执行，执行时只计算元素 数量达到窗口大小的 key 对应的结果。 



**注意：CountWindow 的 window_size 指的是相同 Key 的元素的个数，不是输入 的所有元素的总数。**





##### 1滚动窗口

默认的 CountWindow 是一个滚动窗口，只需要指定窗口大小即可，当元素数量 达到窗口大小时，就会触发窗口的执行。





```java
package com.matt.apitest.window;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.streaming.api.windowing.time.Time;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;

public class Test1 {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        //env.setParallelism(1);



        DataStream<String> inputStream = env.socketTextStream("localhost", 777);
        // 转换成SensorReading类型
        DataStream<SensorReading> dataStream = inputStream.map(line -> {
            String[] fields = line.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        });
        // 开窗测试
        // windowAll 都放在一个窗口里面
        DataStream<SensorReading> t1 = dataStream.keyBy("id")
                .countWindow(2).minBy("temperatrue");

        t1.print();

        env.execute("my");

    }
}

```





##### 2.滑动窗口

滑动窗口和滚动窗口的函数名是完全一致的，只是在传参数时需要传入两个参 数，一个是 window_size，一个是 sliding_size。 





下面代码中的 sliding_size 设置为了 2，也就是说，每收到两个相同 key 的数据 就计算一次，每一次计算的 window 范围是 10 个元素。





```java
package com.matt.apitest.window;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.streaming.api.windowing.time.Time;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;

public class Test1 {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        //env.setParallelism(1);



        DataStream<String> inputStream = env.socketTextStream("localhost", 777);
        // 转换成SensorReading类型
        DataStream<SensorReading> dataStream = inputStream.map(line -> {
            String[] fields = line.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        });
        // 开窗测试
        // windowAll 都放在一个窗口里面
        DataStream<SensorReading> t1 = dataStream.keyBy("id")
                .countWindow(10,5).minBy("temperatrue");

        t1.print();

        env.execute("my");

    }
}

```







### window function



window function 定义了要对窗口中收集的数据做的计算操作，主要可以分为两 类：





- 增量聚合函数（incremental aggregation functions） 每条数据到来就进行计算，保持一个简单的状态。典型的增量聚合函数有 ReduceFunction, AggregateFunction。
- 全窗口函数（full window functions） 先把窗口所有数据收集起来，等到计算的时候会遍历所有数据。 ProcessWindowFunction 就是一个全窗口函数。







```java
package com.matt.apitest.window;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.api.common.functions.AggregateFunction;
import org.apache.flink.streaming.api.windowing.time.Time;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;

public class Test1 {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        //env.setParallelism(1);



        DataStream<String> inputStream = env.socketTextStream("localhost", 777);
        // 转换成SensorReading类型
        DataStream<SensorReading> dataStream = inputStream.map(line -> {
            String[] fields = line.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        });
        // 开窗测试
        // windowAll 都放在一个窗口里面
        DataStream<Long> t1 = dataStream.keyBy("id")
                .timeWindow(Time.seconds(15))
                .aggregate(new AggregateFunction<SensorReading, Long, Long>() {


                    @Override
                    public Long createAccumulator() {
                        return 0l;
                    }

                    @Override
                    public Long add(SensorReading sensorReading, Long l1) {
                        return sensorReading.getTimestamp() + l1;
                    }

                    @Override
                    public Long getResult(Long l1) {
                        return l1;
                    }

                    @Override
                    public Long merge(Long aLong, Long acc1) {
                        return null;
                    }
                });

        t1.print();

        env.execute("my");

    }
}

```









```java
package com.matt.apitest.window;

import com.matt.apitest.beans.SensorReading;
import org.apache.commons.collections.IteratorUtils;
import org.apache.flink.api.common.functions.AggregateFunction;
import org.apache.flink.api.java.tuple.Tuple;
import org.apache.flink.api.java.tuple.Tuple3;
import org.apache.flink.streaming.api.functions.windowing.WindowFunction;
import org.apache.flink.streaming.api.windowing.time.Time;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.api.windowing.windows.TimeWindow;
import org.apache.flink.util.Collector;

public class Test1 {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        //env.setParallelism(1);



        DataStream<String> inputStream = env.socketTextStream("localhost", 777);
        // 转换成SensorReading类型
        DataStream<SensorReading> dataStream = inputStream.map(line -> {
            String[] fields = line.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        });
        // 开窗测试
        // windowAll 都放在一个窗口里面
        DataStream<Tuple3<String, Long, Integer>> t2 = dataStream.keyBy("id")
                .timeWindow(Time.seconds(10))
                .apply(new WindowFunction<SensorReading, Tuple3<String, Long, Integer>, Tuple, TimeWindow>() {
                    @Override
                    public void apply(Tuple tuple, TimeWindow timeWindow, Iterable<SensorReading> input, Collector<Tuple3<String, Long, Integer>> out) throws Exception {
                        String id = tuple.getField(0);
                        long windowEnd = timeWindow.getEnd();
                        int count = IteratorUtils.toArray(input.iterator()).length;
                        Tuple3<String, Long, Integer> res = new Tuple3<>(id, windowEnd, count);
                        out.collect(res);
                    }
                });

        t2.print();
        env.execute("my");

    }
}

```







### 其他api-后面在看





.trigger() —— 触发器 定义 window 什么时候关闭，触发计算并输出结果 

.evitor() —— 移除器 定义移除某些数据的逻辑 

.allowedLateness() —— 允许处理迟到的数据 

.sideOutputLateData() —— 将迟到的数据放入侧输出流 

.getSideOutput() —— 获取侧输出流















![](https://raw.githubusercontent.com/imattdu/img/main/img/202203150124760.png)





```sh
Exception in thread "main" org.apache.flink.api.common.functions.InvalidTypesException: Could not determine TypeInformation for the OutputTag type. The most common reason is forgetting to make the OutputTag an anonymous inner class. It is also not possible to use generic type variables with OutputTags, such as 'Tuple2<A, B>'.
	at org.apache.flink.util.OutputTag.<init>(OutputTag.java:65)
	at com.matt.apitest.window.WindowTest1_TimeWindow.main(WindowTest1_TimeWindow.java:92)
Caused by: org.apache.flink.api.common.functions.InvalidTypesException: The types of the interface org.apache.flink.util.OutputTag could not be inferred. Support for synthetic interfaces, lambdas, and generic or raw types is limited at this point
	at org.apache.flink.api.java.typeutils.TypeExtractor.getParameterType(TypeExtractor.java:1244)
	at org.apache.flink.api.java.typeutils.TypeExtractor.privateCreateTypeInfo(TypeExtractor.java:789)
	at org.apache.flink.api.java.typeutils.TypeExtractor.createTypeInfo(TypeExtractor.java:769)
	at org.apache.flink.api.java.typeutils.TypeExtractor.createTypeInfo(TypeExtractor.java:762)
	at org.apache.flink.util.OutputTag.<init>(OutputTag.java:62)
	... 1 more

Process finished with exit code 1

```





https://blog.csdn.net/qq_39598180/article/details/114491282





```java
OutputTag<String> pageTag = new OutputTag<>("page");
```

关于为什么会出现这个问题，我们知道在Java中泛型的实现方式是基于Code sharing机制的，也就是对同一个原始类型下的泛型类型只生成同一份目标代码，即字节码，基于此机制JVM会将泛型的类型进行擦除，这与cpp中的泛型实现有本质上的不同，因此也被称为假泛型

解决方案
通过{}重写该类，显式指定该对象的泛型类型即可

OutputTag<String> pageTag = new OutputTag<String>("page"){};











```java
package com.matt.apitest.window;

import com.matt.apitest.beans.SensorReading;
import org.apache.commons.collections.IteratorUtils;
import org.apache.flink.api.common.functions.AggregateFunction;
import org.apache.flink.api.java.tuple.Tuple;
import org.apache.flink.api.java.tuple.Tuple3;
import org.apache.flink.streaming.api.datastream.SingleOutputStreamOperator;
import org.apache.flink.streaming.api.functions.windowing.WindowFunction;
import org.apache.flink.streaming.api.windowing.time.Time;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.api.windowing.windows.TimeWindow;
import org.apache.flink.util.Collector;
import org.apache.flink.util.OutputTag;

public class Test1 {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        //env.setParallelism(1);



        DataStream<String> inputStream = env.socketTextStream("localhost", 777);
        // 转换成SensorReading类型
        DataStream<SensorReading> dataStream = inputStream.map(line -> {
            String[] fields = line.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        });
        // 开窗测试
        // windowAll 都放在一个窗口里面
        // 其他api
        OutputTag<SensorReading> outputTag = new OutputTag<SensorReading>("rate"){};

        SingleOutputStreamOperator<Tuple3<String, Long, Integer>> t3 = dataStream.keyBy("id")
                .timeWindow(Time.seconds(15))
                .allowedLateness(Time.minutes(1))
                .sideOutputLateData(outputTag)
                .apply(new WindowFunction<SensorReading, Tuple3<String, Long, Integer>, Tuple, TimeWindow>() {
                    @Override
                    public void apply(Tuple tuple, TimeWindow timeWindow, Iterable<SensorReading> input, Collector<Tuple3<String, Long, Integer>> out) throws Exception {
                        String id = tuple.getField(0);
                        long windowEnd = timeWindow.getEnd();
                        int count = IteratorUtils.toArray(input.iterator()).length;
                        Tuple3<String, Long, Integer> res = new Tuple3<>(id, windowEnd, count);
                        out.collect(res);
                    }


                });


       // t3.print();
        t3.getSideOutput(outputTag).print("rate");

        env.execute("my");

    }
}

```





默认情况下，如果不指定allowedLateness，其值是0，即对于watermark超过end-of-window之后，还有此window的数据到达时，这些数据被删除掉了。





# 时间语义与wartermark



##  Flink 中的时间语义





![](https://raw.githubusercontent.com/imattdu/img/main/img/202203152334431.png)





Event Time：是事件创建的时间。它通常由事件中的时间戳描述，例如采集的 日志数据中，每一条日志都会记录自己的生成时间，Flink 通过时间戳分配器访问事 件时间戳。



Ingestion Time：是数据进入 Flink 的时间。



Processing Time：是每一个执行基于时间操作的算子的本地系统时间，与机器 相关，默认的时间属性就是 Processing Time。





## EventTime的引入



在 Flink 的流式处理中，绝大部分的业务都会使用 eventTime，一般只在 eventTime 无法使用时，才会被迫使用 ProcessingTime 或者 IngestionTime。





```java
StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
//env.setParallelism(1);
// 默认使用processTime
// 从调用时刻开始给 env 创建的每一个 stream 追加时间特征
env.setStreamTimeCharacteristic(TimeCharacteristic.EventTime);
```



## Watermark



### 概述



![](https://raw.githubusercontent.com/imattdu/img/main/img/202203152338804.png)



事件时间早的数据并不一定先进入flink(数据是乱序的)



那么此时出现一个问题，一旦出现乱序，如果只根据 eventTime 决定 window 的 运行，我们不能明确数据是否全部到位，但又不能无限期的等下去，此时必须要有 个机制来保证一个特定的时间后，必须触发 window 去进行计算了，这个特别的机 制，就是 Watermark。





- Watermark 是一种衡量 Event Time 进展的机制。
- Watermark 是用于处理乱序事件的，而正确的处理乱序事件，通常用 Watermark 机制结合 window 来实现。
- 数据流中的 Watermark 用于表示 timestamp 小于 Watermark 的数据，都已经 到达了，因此，window 的执行也是由 Watermark 触发的。
- Watermark 可以理解成一个延迟触发机制，我们可以设置 Watermark 的延时 时长 t，每次系统会校验已经到达的数据中最大的 maxEventTime，然后认定 eventTime 小于 maxEventTime - t 的所有数据都已经到达，如果有窗口的停止时间等于 maxEventTime – t，那么这个窗口被触发执行。



![](https://raw.githubusercontent.com/imattdu/img/main/img/202203152344699.png)



窗口长度5， 延迟时间2

**7只是会触发窗口1关闭，但是7不会进入窗口1，而是进入窗口2**







### 引入





```java
package com.matt.apitest.window;

import com.matt.apitest.beans.SensorReading;
import org.apache.commons.collections.IteratorUtils;
import org.apache.flink.api.common.functions.AggregateFunction;
import org.apache.flink.api.java.tuple.Tuple;
import org.apache.flink.api.java.tuple.Tuple3;
import org.apache.flink.streaming.api.TimeCharacteristic;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.datastream.SingleOutputStreamOperator;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.api.functions.timestamps.AscendingTimestampExtractor;
import org.apache.flink.streaming.api.functions.timestamps.BoundedOutOfOrdernessTimestampExtractor;
import org.apache.flink.streaming.api.functions.windowing.WindowFunction;
import org.apache.flink.streaming.api.windowing.time.Time;
import org.apache.flink.streaming.api.windowing.windows.TimeWindow;
import org.apache.flink.util.Collector;
import org.apache.flink.util.OutputTag;

/**
 * @author matt
 * @create 2022-02-08 23:49
 */
public class WindowTest3_EventTimeWindow {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        //env.setParallelism(1);
        // 默认使用processTime
        // 从调用时刻开始给 env 创建的每一个 stream 追加时间特征
        env.setStreamTimeCharacteristic(TimeCharacteristic.EventTime);
        env.getConfig().setAutoWatermarkInterval(100);

        DataStream<String> inputStream = env.socketTextStream("localhost", 778);
        // 转换成SensorReading类型
        DataStream<SensorReading> dataStream = inputStream.map(line -> {
            String[] fields = line.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        })
        //乱序
        // 最大延迟时间2s
          
          
          
        .assignTimestampsAndWatermarks(new BoundedOutOfOrdernessTimestampExtractor<SensorReading>(Time.seconds(2)) {
            @Override
            public long extractTimestamp(SensorReading sensorReading) {
                // 必须为ms毫秒
                return sensorReading.getTimestamp() * 1000L;
            }
        });
        // 非乱序
        //.assignTimestampsAndWatermarks(new AscendingTimestampExtractor<SensorReading>() {
        //    @Override
        //    public long extractAscendingTimestamp(SensorReading sensorReading) {
        //        return sensorReading.getTimestamp() * 1000L;
        //    }
        //});

        



        
        env.execute("my");

    }

}

```









### Assigner with periodic watermarks

**周期性的生成 watermark**

周期性的生成 watermark：系统会周期性的将 watermark 插入到流中(水位线也 是一种特殊的事件!)。默认周期是 200 毫秒。可以使用 ExecutionConfig.setAutoWatermarkInterval()方法进行设置。





```java
// 每100ms引入一个woatermark
env.getConfig().setAutoWatermarkInterval(100);
```





产生 watermark 的逻辑：每隔 0.1 秒钟，Flink 会调用 AssignerWithPeriodicWatermarks 的 getCurrentWatermark()方法。如果方法返回一个时间戳大于之前水位的时间戳，新的 watermark 会被插入到流中。这个检查保证了 水位线是单调递增的。如果方法返回的时间戳小于等于之前水位的时间戳，则不会 产生新的 watermark。



```java
// 自定义周期性时间戳分配器
public static class MyPeriodicAssigner implements
AssignerWithPeriodicWatermarks<SensorReading>{

private Long bound = 60 * 1000L; // 延迟一分钟
 private Long maxTs = Long.MIN_VALUE; // 当前最大时间戳
 @Nullable
 @Override
 public Watermark getCurrentWatermark() {
 return new Watermark(maxTs - bound);
 }
 @Override
 public long extractTimestamp(SensorReading element, long previousElementTimestamp)
{
 maxTs = Math.max(maxTs, element.getTimestamp());
 return element.getTimestamp();
 }
}
```







### Assigner with punctuated watermarks 

间断式产生woatermark

间断式地生成 watermark。和周期性生成的方式不同，这种方式不是固定时间的， 而是可以根据需要对每条数据进行筛选和处理。直接上代码来举个例子，我们只给 sensor_1 的传感器的数据流插入 watermark：



```java
public static class MyPunctuatedAssigner implements
AssignerWithPunctuatedWatermarks<SensorReading>{
 private Long bound = 60 * 1000L; // 延迟一分钟
 @Nullable
 @Override
 public Watermark checkAndGetNextWatermark(SensorReading lastElement, long
extractedTimestamp) {
 if(lastElement.getId().equals("sensor_1"))
 return new Watermark(extractedTimestamp - bound);
 else
 return null;
 }
 @Override
 public long extractTimestamp(SensorReading element, long previousElementTimestamp)
{
 return element.getTimestamp();
 }
}
```







![](https://raw.githubusercontent.com/imattdu/img/main/img/202203160024213.png)







# processFunction





我们之前学习的转换算子是无法访问事件的时间戳信息和水位线信息的。而这 在一些应用场景下，极为重要。例如 MapFunction 这样的 map 转换算子就无法访问 时间戳或者当前事件的事件时间。





Flink 提供了 8 个 Process Function： 

• ProcessFunction 

• KeyedProcessFunction 

• CoProcessFunction 

• ProcessJoinFunction 

• BroadcastProcessFunction 

• KeyedBroadcastProcessFunction 

• ProcessWindowFunction 

• ProcessAllWindowFunction





## keyedProcessFunction





KeyedProcessFunction 用来操作 KeyedStream。KeyedProcessFunction 会处理流 的每一个元素，输出为 0 个、1 个或者多个元素。所有的 Process Function 都继承自 RichFunction 接口，所以都有 open()、close()和 getRuntimeContext()等方法。而 KeyedProcessFunction还额外提供了两个方法





processElement(I value, Context ctx, Collector out), 流中的每一个元素都 会调用这个方法，调用结果将会放在 Collector 数据类型中输出。Context 可以访问元素的时间戳，元素的 key，以及 TimerService 时间服务。Context 还 可以将结果输出到别的流(side outputs)。







onTimer(long timestamp, OnTimerContext ctx, Collector out) 是一个回调 函数。当之前注册的定时器触发时调用。参数 timestamp 为定时器所设定的 触发的时间戳。Collector 为输出结果的集合。OnTimerContext 和 processElement 的 Context 参数一样，提供了上下文的一些信息，例如定时器 触发的时间信息(事件时间或者处理时间)。







## TimerService 和 定时器（Timers）

Context 和 OnT™imerContext 所持有的 TimerService 对象拥有以下方法: 



• long currentProcessingTime() 返回当前处理时间 

• long currentWatermark() 返回当前 watermark 的时间戳 

• void registerProcessingTimeTimer(long timestamp) 会注册当前 key 的 processing time 的定时器。当 processing time 到达定时时间时，触发 timer。 

• void registerEventTimeTimer(long timestamp) 会注册当前 key 的 event time 定 时器。当水位线大于等于定时器注册的时间时，触发定时器执行回调函数。

• void deleteProcessingTimeTimer(long timestamp) 删除之前注册处理时间定时 器。如果没有这个时间戳的定时器，则不执行。 

• void deleteEventTimeTimer(long timestamp) 删除之前注册的事件时间定时 器，如果没有此时间戳的定时器，则不执行。 当定时器 timer 触发时，会执行回调函数 onTimer()。注意定时器 timer 只能在 keyed streams 上面使用。







### 



```java
package com.matt.apitest.processfunction;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.api.common.state.ValueState;
import org.apache.flink.api.common.state.ValueStateDescriptor;
import org.apache.flink.api.java.tuple.Tuple;
import org.apache.flink.configuration.Configuration;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.api.functions.KeyedProcessFunction;
import org.apache.flink.util.Collector;

public class Test1_ProcessKeyedProcessFunction {


    public static void main(String[] args) throws Exception {


        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        env.setParallelism(1);
        DataStream<String> inputStream = env.socketTextStream("localhost", 777);
        // 转换成SensorReading类型
        DataStream<SensorReading> dataStream = inputStream.map(line -> {
            String[] fields = line.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        });


        // 测试 keyedProcessFunction

        dataStream.keyBy("id")
                        .process(new MyProcess()).print();



        env.execute("my");

    }


    public static class MyProcess extends KeyedProcessFunction<Tuple, SensorReading, Integer> {

        ValueState<Long> tsTimerState;

        @Override
        public void open(Configuration parameters) throws Exception {
            tsTimerState = getRuntimeContext().getState(new ValueStateDescriptor<Long>("ts-timer", Long.class));

        }

        @Override
        public void processElement(SensorReading sensorReading, KeyedProcessFunction<Tuple, SensorReading, Integer>.Context context, Collector<Integer> collector) throws Exception {
            collector.collect(sensorReading.getId().length());

            Long timestamp = context.timestamp();
            Tuple currentKey = context.getCurrentKey();
            // context.output();
            context.timerService().currentProcessingTime();
            context.timerService().currentWatermark();

            context.timerService().registerProcessingTimeTimer(context.timerService().currentProcessingTime() + 1000);

            tsTimerState.update(context.timerService().currentProcessingTime() + 1000);

            // ms
            context.timerService().registerEventTimeTimer((sensorReading.getTimestamp() + 10) * 1000);

            //context.timerService().deleteEventTimeTimer(tsTimerState.value());

        }


        @Override
        public void onTimer(long timestamp, KeyedProcessFunction<Tuple, SensorReading, Integer>.OnTimerContext ctx, Collector<Integer> out) throws Exception {
            System.out.println(timestamp + "定时器触发");

        }
    }



}

```







## 案例：温度上升触发报警





```java
package com.matt.apitest.processfunction;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.api.common.state.ValueState;
import org.apache.flink.api.common.state.ValueStateDescriptor;
import org.apache.flink.api.java.tuple.Tuple;
import org.apache.flink.configuration.Configuration;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.api.functions.KeyedProcessFunction;
import org.apache.flink.util.Collector;

public class Test2_ApplicationTest {

    public static void main(String[] args) throws Exception {


        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        env.setParallelism(1);
        DataStream<String> inputStream = env.socketTextStream("localhost", 777);
        // 转换成SensorReading类型
        DataStream<SensorReading> dataStream = inputStream.map(line -> {
            String[] fields = line.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        });


        // 测试 keyedProcessFunction

        dataStream.keyBy("id")
                .process(new MyProcess1()).print();



        env.execute("my");

    }

    // 检测一段时间温度上升
    public static class MyProcess1 extends KeyedProcessFunction<Tuple, SensorReading, String> {

        // 时间间隔
        private Integer interval = 10;
        // 上次温度
        ValueState<Double> lastTempState;
      	// 上次时间
        ValueState<Long> timerState;


        @Override
        public void open(Configuration parameters) throws Exception {
            lastTempState = getRuntimeContext().getState(new ValueStateDescriptor<Double>("lastTempState", Double.class, Double.MIN_VALUE));
            timerState = getRuntimeContext().getState(new ValueStateDescriptor<Long>("timerState", Long.class));


        }


        @Override
        public void processElement(SensorReading sensorReading, KeyedProcessFunction<Tuple, SensorReading, String>.Context context, Collector<String> collector) throws Exception {
            Double lastTemp = lastTempState.value();
            Long lastTimer = timerState.value();

            // 状态是否存在
           	// 温度上升 且上个温度是null 注册定时器 立即触发
            if (sensorReading.getTemperatrue() > lastTemp && lastTimer == null) {
                long ts = context.timerService().currentProcessingTime() + interval * 1000l;
                // 注册一个时间
                context.timerService().registerProcessingTimeTimer(ts);
                timerState.update(ts);
             // 温度下降 且上次温度不为null 去除定时器
            } else if (sensorReading.getTemperatrue() < lastTemp && lastTimer != null) {
                context.timerService().deleteEventTimeTimer(timerState.value());
                timerState.clear();
            }
            // 更新温度
            lastTempState.update(sensorReading.getTemperatrue());
        }


        @Override
        public void onTimer(long timestamp, KeyedProcessFunction<Tuple, SensorReading, String>.OnTimerContext ctx, Collector<String> out) throws Exception {
            System.out.println("持续上升" + ctx.getCurrentKey().getField(0));
            lastTempState.clear();
        }
    }


}

```







### 侧输出流（SideOutput）





大部分的 DataStream API 的算子的输出是单一输出，也就是某种数据类型的流。 除了 split 算子，可以将一条流分成多条流，这些流的数据类型也都相同。process function 的 side outputs 功能可以产生多条流，并且这些流的数据类型可以不一样。 一个 side output 可以定义为 OutputTag[X]对象，X 是输出流的数据类型。process function 可以通过 Context 对象发射一个事件到一个或者多个 side outputs。



温度小于30度输出到测输出流

```java
package com.matt.apitest.processfunction;

import com.matt.apitest.beans.SensorReading;
import org.apache.flink.api.common.state.ValueState;
import org.apache.flink.api.common.state.ValueStateDescriptor;
import org.apache.flink.api.java.tuple.Tuple;
import org.apache.flink.configuration.Configuration;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.datastream.SingleOutputStreamOperator;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.api.functions.KeyedProcessFunction;
import org.apache.flink.streaming.api.functions.ProcessFunction;
import org.apache.flink.util.Collector;
import org.apache.flink.util.OutputTag;

public class Test3_Sideoutputcase {

    public static void main(String[] args) throws Exception {


        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        env.setParallelism(1);
        DataStream<String> inputStream = env.socketTextStream("localhost", 777);
        // 转换成SensorReading类型
        DataStream<SensorReading> dataStream = inputStream.map(line -> {
            String[] fields = line.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        });


        // 测试 测输出流 实现分流操作

        // 匿名
        OutputTag<SensorReading> outputTag = new OutputTag<SensorReading>("lowTemp"){};

        SingleOutputStreamOperator<SensorReading> highTempStream = dataStream.process(new ProcessFunction<SensorReading, SensorReading>() {
            @Override
            public void processElement(SensorReading sensorReading, ProcessFunction<SensorReading, SensorReading>.Context context, Collector<SensorReading> collector) throws Exception {
                //

                if (sensorReading.getTemperatrue() > 30) {
                    collector.collect(sensorReading);
                } else {
                    // 输出到侧边流
                    context.output(outputTag, sensorReading);
                }
            }
        });

        highTempStream.print("high");

        highTempStream.getSideOutput(outputTag).print("lowTem");

        env.execute("my");

    }

    // 检测一段时间温度上升
    public static class MyProcess1 extends KeyedProcessFunction<Tuple, SensorReading, String> {

        // 时间间隔
        private Integer interval = 10;
        ValueState<Double> lastTempState;
        ValueState<Long> timerState;


        @Override
        public void open(Configuration parameters) throws Exception {
            lastTempState = getRuntimeContext().getState(new ValueStateDescriptor<Double>("lastTempState", Double.class, Double.MIN_VALUE));
            timerState = getRuntimeContext().getState(new ValueStateDescriptor<Long>("timerState", Long.class));


        }


        @Override
        public void processElement(SensorReading sensorReading, KeyedProcessFunction<Tuple, SensorReading, String>.Context context, Collector<String> collector) throws Exception {
            Double lastTemp = lastTempState.value();
            Long lastTimer = timerState.value();

            // 状态是否存在
            if (sensorReading.getTemperatrue() > lastTemp && lastTimer == null) {
                long ts = context.timerService().currentProcessingTime() + interval * 1000l;
                // 注册一个时间
                context.timerService().registerProcessingTimeTimer(ts);
                timerState.update(ts);
            } else if (sensorReading.getTemperatrue() < lastTemp && lastTimer != null) {
                context.timerService().deleteEventTimeTimer(timerState.value());
                timerState.clear();
            }
            lastTempState.update(sensorReading.getTemperatrue());
        }


        @Override
        public void onTimer(long timestamp, KeyedProcessFunction<Tuple, SensorReading, String>.OnTimerContext ctx, Collector<String> out) throws Exception {
            System.out.println("持续上升" + ctx.getCurrentKey().getField(0));
            lastTempState.clear();
        }
    }


}

```











迟到数据输到侧输出流

```java
package com.matt.apitest.window;

import com.matt.apitest.beans.SensorReading;
import org.apache.commons.collections.IteratorUtils;
import org.apache.flink.api.common.functions.AggregateFunction;
import org.apache.flink.api.java.tuple.Tuple;
import org.apache.flink.api.java.tuple.Tuple3;
import org.apache.flink.streaming.api.TimeCharacteristic;
import org.apache.flink.streaming.api.datastream.DataStream;
import org.apache.flink.streaming.api.datastream.SingleOutputStreamOperator;
import org.apache.flink.streaming.api.environment.StreamExecutionEnvironment;
import org.apache.flink.streaming.api.functions.timestamps.AscendingTimestampExtractor;
import org.apache.flink.streaming.api.functions.timestamps.BoundedOutOfOrdernessTimestampExtractor;
import org.apache.flink.streaming.api.functions.windowing.WindowFunction;
import org.apache.flink.streaming.api.windowing.time.Time;
import org.apache.flink.streaming.api.windowing.windows.TimeWindow;
import org.apache.flink.util.Collector;
import org.apache.flink.util.OutputTag;

/**
 * @author matt
 * @create 2022-02-08 23:49
 */
public class WindowTest3_EventTimeWindow {

    public static void main(String[] args) throws Exception {
        StreamExecutionEnvironment env = StreamExecutionEnvironment.getExecutionEnvironment();
        //env.setParallelism(1);
        // 默认使用processTime
        // 从调用时刻开始给 env 创建的每一个 stream 追加时间特征
        env.setStreamTimeCharacteristic(TimeCharacteristic.EventTime);
        env.getConfig().setAutoWatermarkInterval(100);

        DataStream<String> inputStream = env.socketTextStream("localhost", 778);
        // 转换成SensorReading类型
        DataStream<SensorReading> dataStream = inputStream.map(line -> {
            String[] fields = line.split(",");
            return new SensorReading(fields[0], new Long(fields[1]), new Double(fields[2]));
        })
        //乱序
        .assignTimestampsAndWatermarks(new BoundedOutOfOrdernessTimestampExtractor<SensorReading>(Time.seconds(2)) {
            @Override
            public long extractTimestamp(SensorReading sensorReading) {
                // 必须为ms毫秒
                return sensorReading.getTimestamp() * 1000L;
            }
        });
        // 非乱序
        //.assignTimestampsAndWatermarks(new AscendingTimestampExtractor<SensorReading>() {
        //    @Override
        //    public long extractAscendingTimestamp(SensorReading sensorReading) {
        //        return sensorReading.getTimestamp() * 1000L;
        //    }
        //});

        OutputTag<SensorReading> outputTag = new OutputTag<SensorReading>("late"){};
        // 开窗测试
        // windowAll 都放在一个窗口里面
        SingleOutputStreamOperator<SensorReading> minTempStream = dataStream.keyBy("id")
                .timeWindow(Time.seconds(15))
               .allowedLateness(Time.minutes(1))
                // 1分钟后测输入流
               .sideOutputLateData(outputTag)
                .minBy("temperatrue");
        //temperatrue
        minTempStream.print("minTemp");

        minTempStream.getSideOutput(outputTag).print("late");



        //OutputTag<SensorReading> outputTag = new OutputTag<SensorReading>("late") {
        //};
        //
        //// 基于事件时间的开窗聚合，统计15秒内温度的最小值
        //SingleOutputStreamOperator<SensorReading> minTempStream = dataStream.keyBy("id")
        //        .timeWindow(Time.seconds(15))
        //        .allowedLateness(Time.minutes(1))
        //        .sideOutputLateData(outputTag)
        //        .minBy("temperatrue");
        //
        //minTempStream.print("minTemp");
        //minTempStream.getSideOutput(outputTag).print("late");
        env.execute("my");

    }

}

```

