[Licensed under the CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.zh)

在此感谢 [@hbase](https://hbase.apache.org/)

文档不定时更新，如有疑问请及时联系本文作者，当前文档版本1.0.0

[hbase-1.6.0-bin.tar.gz](https://www.apache.org/dyn/closer.lua/hbase/1.6.0/hbase-1.6.0-bin.tar.gz)

~~~
./hdfs dfsadmin -safemode get            // 获取模式状态
./hdfs dfsadmin -safemode enter          // 进入安全模式
./hdfs dfsadmin -safemode leave          // 离开安全模式   (./hadoop dfsadmin -safemode leave)

// 导出表(备份表)
./hbase org.apache.hadoop.hbase.mapreduce.Export table1 /opt/table1.tar.gz 

// 导入表(还原表)
./hbase org.apache.hadoop.hbase.mapreduce.Import table1 /opt/table1.tar.gz

// 拷贝表
./hbase org.apache.hadoop.hbase.mapreduce.CopyTable --new.name=testCopy test

// 创建快照
./hbase snapshot create -n test_snapshot -t test

// 查看快照
>list_snapshots
SNAPSHOT                TABLE + CREATION TIME
 test_snapshot          test (Sun Sep 04 20:31:00 +0800 2016)
1 row(s) in 0.2080 seconds

// 导出快照
./hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot -snapshot test_snapshot -copy-to /home/hbase/snapshot/test

// 导入快照
./hbase org.apache.hadoop.hbase.snapshot.ExportSnapshot -snapshot test_snapshot -copy-from /home/hbase/snapshot/test -copy-to /hbase/

// 恢复快照
>restore _snapshot 'test_snapshot'

// 重命名快照里面的表名
>clone_snapshot 'test_snapshot','test_2'

// 统计行数
// 1.数据量较少时
count 'table1'   
// 2.使用mapreduce统计
$HBASE_HOME/bin/hbase org.apache.hadoop.hbase.mapreduce.RowCounter 'table1'   
 
// 3.1创建hive与hbase的关联表
CREATE TABLE hive_hbase_1(key INT,value STRING) STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler' WITH SERDEPROPERTIES ("hbase.columns.mapping"=":key,cf:val") TBLPROPERTIES("hbase.table.name"="t_hive","hbase.table.default.storage.type"="binary");

// 3.2hive关联已经存在的hbase
CREATE EXTERNAL TABLE hive_hbase_1(key INT,value STRING) STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler' WITH SERDEPROPERTIES ("hbase.columns.mapping"=":key,cf:val") TBLPROPERTIES("hbase.table.name"="t_hive","hbase.table.default.storage.type"="binary");
 
// 进入shell模式
./hbase shell

// 查看所有表
hbase(main):037:0> list

// 查看表是否存在
hbase(main):037:0> exists 'table2'

// 查看表信息
hbase(main):037:0> describe 'table1'

// 查询表限制10条输出
hbase(main):037:0> scan 'table2',{LIMIT=>10}

// 查询表限制10条输出列族列名的值
hbase(main):037:0> scan 'table2',{COLUMN=>'data:repo_id',LIMIT=>10}

// 根据列值查询
hbase(main):037:0> scan 'table1',FILTER=>"ValueFilter(=,'substring:TASKGJJ3100001472632284636880')"

// 根据roukey查询
hbase(main):037:0> scan 'table2',FILTER=>"PrefixFilter('0000014964524df2ff396bd5bf6fdd4d')"

// 根据rowkey查询
hbase(main):037:0> get 'table2','0000014964524df2ff396bd5bf6fdd4d'

// 根据rowkey和列族和列名
hbase(main):037:0> get "table2",'0000025c0cd7865c7a1aa3f06e393fa9','data:repo_id'

// 根据列名查询
hbase(main):037:0> scan 'table2',FILTER=>"ColumnPrefixFilter('repo_id')"

// 根据列名和列值查询(and 和 or)
hbase(main):037:0> scan 'table2',FILTER=>"ColumnPrefixFilter('repo_id')AND ValueFilter(=,'substring:15962')OR ValueFilter(=,'substring:186')"

// 指定检索值列举出检索值对应的所有列及value数据
hbase(main):037:0> scan "table2",{FILTER=>"SingleColumnValueFilter('data','repo_id',=,'substring:30')"}

// 指定检索值列举出检索值对应的所有列及value数据(正则表达式)
hbase(main):037:0> scan "table2",{FILTER=>"SingleColumnValueFilter('data','repo_id',=,'regexstring:liu')"}
~~~