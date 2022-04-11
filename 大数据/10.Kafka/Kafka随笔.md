kafka索引、日志、页缓存事务



解决脑裂和惊群问题引入的Group Coordinator







是不是只有基于时间戳删除日志的方式会存在删除活跃的日志分段的数据的情况？





在日志压缩的时候，提到遍历Topic，第一次遍历的时候，记录每个key的hash最后一次出现的偏移量，那这个key的hash值不会冲突嘛？不同的key，hash值相同。





消费者每次poll数据之前，都会提交偏移量（在__consumer_offsets那节课提到的），是提交上次poll数据的偏移量吗？如果是，那么偏移量自动提交的时间的设置是不是就没有意义？





生产者ack 设置为-1，min.insync.replicas = 2，此时ISR中有三个副本，那么是同步了两个副本就给生产者响应还是同步了三个副本响应？





当 unclean.leader.election.enable = true 时，Leader挂了， 那么是会先从ISR里面获取副本作为Leader，ISR没有的时候，再从OSR中选择副本作为Leader。还是说，当Leader挂了，不管ISR中有没有同步副本，都有可能从OSR中选择副本作为Leader