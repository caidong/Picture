数据建模过程中，需要从业务系统ODS同步业务和日志数据到数仓。
![数据流](https://gitee.com/vole_store/pic-bed/raw/master/BlogImage/%E6%8D%95%E8%8E%B7.PNG)
- 同步方式：
1. 时效性
    - 离线: 数据实时性要求低，定时从业务系统同步数据，对业务系统影响小 T+1
    - 在线: 数据实时性要求高，实时从业务系统同步数据 T+0
2. 数据量
	- 增量: 每次同步数据更新部分
	- 全量: 每次同步所有数据
- 开源同步工具
1. Sqoop、Camus
2. Canal、Flink-cdc 
    由于在某些业务场景下对数据实时要求越来越高，决定学习使用Flink-CDC (Change Data Capture)
    [项目git地址](https://github.com/ververica/flink-cdc-connectors)
    Flink-cdc基于binlog日志文件，
- 优点：
1. 实时消费日志,流处理
2. 保障数据一致性，文件里包含所有的历史变更明细
3. 保障实时性，可以较好做到增量同步


## canal 基于 MySQL 数据库binlog增量日志解析，提供增量数据订阅和消费
1. canal 工作原理
	- canal 模拟 MySQL slave 的交互协议，伪装自己为 MySQL slave ，向 MySQL master 发送dump 协议
	- MySQL master 收到 dump 请求，开始推送 binary log 给 slave (即 canal )
	- canal 解析 binary log 对象(原始为 byte 流)

2. 使用流程
