##### Kafka重复消费
1. 原因：
   - 消费者宕机，导致消息已经消费但是offset未提交
   - 消费者使用自动提交，但是还未提交时，有新加入的消费者或移除的消费者，发生再平衡。再次消费，消费者会根据提交的偏移量来，于是重复消费了数据
   - 消息处理耗时太久，或者一次拉取的数据太多了，导致超过了max.poll.interval.ms的时间，导致认为当前消费者已死，从而发生在平衡。
2. 处理思路：
   - 消息表，记录已消费的数据
   - 数据库唯一索引
   - 自动提交改为手动提交
   - 缓存已消费过的ID
