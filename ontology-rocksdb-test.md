# RocksDB、LevelDB 测试报告
测试环境：
RocksDB：version 5.10  
操作系统：Ubuntu 16.04.4 LTS   
CPU：16 * Intel(R) Xeon(R) CPU E5-2698B v3 @ 2.00GHz  
MEM：32G  
DISK：SSD 最高 20000 iops  

## 1、RocksDB 性能测试

### 1.1、C++版RocksDB

Key：10Byte  
Value：100Byte  
数据集：50000000  

随机写：427350 TPS  
单线程随机读：107296 TPS  
8线程随机读：609756 TPS  

### 1.2、Go版RocksDB

Key：10Byte  
Value：100Byte  
Entries：50000000  

随机写：338600 TPS  
单线程随机读：245766 TPS   
8线程随机读：942625 TPS  

go版性能大致是C++版的70%。这里的测试结果现实go版的新能要比C++版高不少，是因为C++的版没有设置布隆过滤器。go版去掉布隆过滤器后的测试结果，8线程的TPS为373050。

### 1.3、LevelDB

Key：10Byte  
Value：100Byte  
Entries：50000000  

随机写：67057 TPS  
随机读：51968 TPS  

## 2、Ontology存储模块集成测试

构造包含5W个native的ont转账交易的区块，一个测试200个区块，共1000W交易。一个转账交易大概200bytes，需要4次写，2次读。执行结束后，整个数据规模为2.6G。

### 2.1 LevelDB 

平均耗时：5.048s  
平均TPS：9904.6  
平均DB耗时比率：49.11%  

平均DB耗时比率:是指DB读写耗费的时间占整个存储模块耗费时间的一个比率。

### 2.2 RocksDB 

平均耗时：3.629s  
平均TPS：13777.54  
平均DB耗时比率：30.54%  

![pic](https://github.com/gasby88/docs/blob/master/img/%E5%AD%98%E5%82%A8%E6%A8%A1%E5%9D%97%E4%BA%A4%E6%98%93%E8%90%BD%E5%B8%90tps.jpeg)
