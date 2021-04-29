* 大数据技术解决了什么问题？

  大数据技术解决的主要是海量数据的存储和计算。

* Hadoop的广义和狭义之分

  狭义的Hadoop：指的是一个框架，Hadoop是由三个部分组成，HDFS：分布式文件系统存储；MapReduce：分布式离线计算框架；Yarn：资源调度框架

  广义的Hadoop：广义的Hadoop不仅仅包含Hadoop，除了Hadoop框架之外的还有一个辅助框架，Flume（日志数据采集），Sqooq：关系型数据库数据的采集

  Hive：深度依赖Hadoop框架完成计算（通过写SQL语句），Hbase：大数据领域的数据库

  Sqooq：数据的导出

  广义Hadoop指的是一个生态圈。

* **客户端在HDFS中下载文件，需要先请求NameNode获取块列表和块所在的DataNode节点信息，然后再去DataNode中下载，如果此时其中的一个DataNode节点挂了，怎么办？**

* **HDFS是如何分割数据的，根据什么分割的？**

* **一个DataNode中可以有多少个Block，是根据什么来决定的，服务器本身的磁盘的大小？如果有两个都是64M的文件，会存放在同一个Block中吗？**

*  **[关于 HDFS 的 file size 和 block size](https://blog.csdn.net/samhacker/article/details/23089157)**

* **NameNode如何管理存储元数据？**

  计算机存储数据两种选择，内存、磁盘

  如果元数据存储磁盘，无法面对客户端对元数据信息的任意低延迟的响应，但是安全性高。

  如果元数据存储内存，元数据可以高效查询和快速响应，但是无法持久化。

  所以HDFS的解决方案：内存+磁盘

  NameNode内存+FsImage文件（磁盘）

* **新问题：磁盘和内存中的元数据如何划分？两个元数据一模一样，还是两个数据合并到一起才是完整的数据？**

  如果是一模一样：client对元数据进行增、删、改操作，需要保证两个数据的一致性。FsImage操作效率也不高。

  两个合并=完整：NameNode采取的就是这种模式，但是也有FsImage操作效率低的问题，所以引入了edits文件（日志文件，只能追加写），edits文件记录的是client的增删改操作。

  不在选择让NameNode把数据dump除了形成FsImage文件（dump操作比较消耗资源）
  
* **NameNode如何确定加载那些Edits文件？**

  需要借助fsimage文件的最后数字编码，来确定哪些edits之前是没有合并到fsimage中，启动时只需要加载那些未合并的edits文件即可

* **129M的文件在HDFS存储的时候会不会被切片成两块？**