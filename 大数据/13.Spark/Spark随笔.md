1、Spark 的谓词下推









2、ds1.join(ds2,ds1("name") === ds2("sname"),"left_outer").show，如果是多个不同字段的链接应该怎么写





3、为什么val wordCounts2: DStream[(String, Int)] = pairs.reduceByKeyAndWindow(

_ + _,
 _ - _, Seconds(20), Seconds(10), 2)

wordCounts2.print

需要checkpoint的支持？



4、为什么reduceByKeyAndWindow 不可以用 `_+_` 而是 (a: Int, b: Int) => a + b





5、Shuffle 的时候，进入环形缓冲区就排序，还是达到阈值，触发溢写的时候排序？









Spark：
并行度和CPU的core数，共同决定的是有多少个Task在同时运行，如果并行度 > CPU的core，那么任务需要等待，
直到其他任务释放CPU的core，如果CPU的Core > 并行度，那么任务会并行计算，具体有多少任务Task和并行度没有关系。

job -> Stage -> Task
Job的数量由Action决定，遇到一个Action，就会生成一个Job。
Stage划分（Taskset）：发生宽依赖（Shuffle）就会划分Stage
Task的数量：取决于最后一个Stage的分区数，一个分区就是一个Task

分区数：

默认分区数是Max(executors的core，2），

生产上一般是读取HDFS文件：
如果HDFS是128M为一个块，现有一个文件是是5个G，那么相当于该文件有40个block，那么最终生成的分区数就是40（假设无Shuffle，未改变分区数的情况）

现在假设整个集群有20个Core，我们设置并行度一般是core * （2~3），假设设置并行度为60
那么当任务运行的时候，由于分区数是40，我们启动Task的数量也是40，虽然并行度设置的是60，但是由于只有20个Core，
所以会有20Task先执行，剩余的20个Task等待执行中的Task释放Core。

