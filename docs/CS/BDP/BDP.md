## 1. 并行计算与大数据处理技术概述
### 为什么需要并行计算？
#### 提高计算机性能
1. 提高处理器字长
2. 提高集成度
3. 流水线等微体系结构
4. 提高处理器频率
但单核处理器性能提升接近极限：集成度不可能无限提高、指令集并行度接近极限、处理器速度与存储器速度差异越来越大、功耗散热大幅增加
#### 迫切需要发展并行计算技术的主要原因
1. 单处理器性能提升达到极限
2. 应用领域计算规模和复杂度大幅提高

### 并行计算技术的分类
- 数据和指令处理结构：弗林(Flynn)分类, SISD, SIMD, MISD, MIMD
- 按并行类型: 位级并行、指令级并行、线程级并行、数据级、任务级
- 按存储访问构架：共享内存(UMA(Uniform Memory Access)结构)、分布式共享存储(NUMA)、分布式内存(NUMA)
- 按系统类型：多核/众核并行计算系统MC（Multicore/Manycore)、对称多处理系统SMP（Symmetric Multi-Processing）、大规模并行处理MPP（Massive Parallel Processing）、集群（Cluster）、网格（Grid）![](../../img/Pasted%20image%2020240620141625.png)![](../../img/Pasted%20image%2020240620141735.png)
- 按计算特征：数据密集型(大规模Web信息检索)、计算密集型(建模与渲染)、数据密集与计算密集混合型(3D电影渲染)
- 按并行程序设计模型/方法：共享内存变量 (Shared Memory Variables)、消息传递方式(Message Passing)、MapReduce方式
### 并行计算的主要技术问题
多核/多处理器网络互连结构技术
存储访问体系结构
分布式数据与文件管理
并行计算任务分解与算法设计
并行程序设计模型和方法
数据同步访问和通信控制
可靠性设计与容错技术
并行计算软件框架平台
系统性能评价和程序并行度评估

#### 系统性能评估和程序并行度评估
Amdahl定律：
![](../../img/Pasted%20image%2020240620142804.png)
一个并行程序可加速程度是有限制的，并非可无限加速，并非处理器越多越好

### MPI并行程序设计
MPI (Message Passing Interface)，基于消息传递的高性能并行计算编程接口
在处理器间以消息传递方式进行数据通信和同步，以库函数形式为程序员提
供了一组易于使用的编程接口。
特点：提供可靠的、面向消息的通信；在高性能科学计算领域广泛使用，适
合于处理计算密集型的科学计算；独立于语言的编程规范，可移植性好
#### MPI主要功能
1. 用常规语言编程方式，所有节点运行同一个程序，但处理不同的数据
2. 提供点对点通信(Point-point communication)
3. 提供节点集合通信(Collective communication)
4. 提供用户自定义的复合数据类型传输
#### MPI基本程序结构
![](../../img/Pasted%20image%2020240620143207.png)
#### MPI并行程序设计接口
![](../../img/Pasted%20image%2020240620143337.png)
![](../../img/Pasted%20image%2020240620143355.png)
##### 通信组（Communicator）
为了在指定的范围内进行通信，可以将系统中的处理器划分为不同的通信组；一个处理器可以同时参加多个通信组；MPI定义了一个最大的缺省通信组: MPI_COMM_WORLD，指明系统中所有的进程都参与通信。一个通信组中的总进程数可以由MPI_Comm_Size调用来确定。
##### 进程标识
为了在通信时能准确指定一个特定的进程，需要为每个进程分配一个进程标识，一个通信组中每个进程标识号由系统自动编号（从0开始）；进程标识号可以由MPI_Comm_Rank调用来确定

##### 点对点通信
- 同步通信：阻塞式通信，等待通信操作完成后才返回(MPI_Send, MPI_Recv)
- 同步通信时一定要等到通信操作完成，这会造成处理器空闲，因而可能导致系统效率下降，为此MPI提供异步通信功能
- 异步通信：非阻塞式通信，不等待通信操作完成即返回![](../../img/Pasted%20image%2020240620143618.png)
### 为什么需要大规模数据并行处理？
- 数据处理能力大幅落后于数据增长速度
- 大数据隐含着更准确的事实

### 什么是MapReduce
- 基于集群的高性能并行计算平台(Cluster Infrastructure)
- 并行程序开发与运行框架(Software Framework)
- 并行程序设计模型与方法(Programming Model & Methodology)

### 为什么MapReduce重要
- 提供高效的大规模数据处理方法
- 改变了大规模尺度上组织计算的方式
- 第一个不同于冯诺依曼结构的、基于集群而非单机的计算方式的重大突破
- 目前为止最为成功的基于大规模计算资源的并行计算抽象方法


## 2.MapReduce简介
### MapReduce的基本模型和处理思想
- 如何对付大数据处理：分而治之
- ![](../../img/Pasted%20image%2020240620150937.png)
### Map Reduce
特点
- Map和Reduce为程序员提供了一个清晰的操作接口抽象描述
- 描述了对一组数据处理的两个阶段的抽象操作
- 仅仅描述了需要做什么，不需要关注怎么做

### 基于Map和Reduce的并行计算模型
![](../../img/Pasted%20image%2020240620151323.png)
- 各个map函数对所划分的数据并行处理，从不同的输入数据产生不同的中间结果输出
- 进行reduce处理之前，必须等到所有的map函数做完，因此，在进入reduce前需要有一个同步障(barrier)；这个阶段也负责对map的中间结果数据进行收集整理(aggregation & shuffle)处理，以便reduce更有效地计算最终结果
- 最终汇总所有reduce的输出结果即可获得最终结果
### 如何提供统一的计算框架
主要需求和目标：
- 实现自动并行化计算
- 为程序员隐藏系统层细节
MapReduce提供一个统一的计算框架，可完成：
- 任务调度
- 数据/代码互定位
- 出错处理
- 分布式数据存储与文件管理
- Combiner和Partitioner（设计目的和作用）
	- 为了减少数据通信开销，中间结果数据进入reduce节点前需要进行合并(combine)处理，把具有同样主键的数据合并到一起避免重复传送；一个reduce节点所处理的数据可能会来自多个map节点，因此，map节点输出的中间结果需使用一定的策略进行适当的划分(partitioner)处理，保证相关数据发送到同一个reduce节点。

### MapReduce的主要设计思想
- 向“外”横向扩展，而非向“上”纵向扩展
- 失效被认为是常态
- 把处理向数据迁移
- 顺序处理数据、避免随机访问数据
- 为应用开发者隐藏系统层细节
- 平滑无缝的可扩展性

## 03-1 Google MapReduce基本架构
Google的三驾马车
- GFS
- MapReduce
- Bigtable
#### Google MapReduce并行处理的基本过程
1. 有一个待处理的大数据，被划分为大小相同的数据块(如64MB)，及与此相应的用户作业程序
2. 系统中有一个负责调度的主节点(Master)，以及数据Map和Reduce工作节点(Worker)
3. 用户作业程序提交给主节点
4. 主节点为作业程序寻找和配备可用的Map节点，并将程序传送给Map节点
5. 主节点也为作业程序寻找和配备可用的Reduce节点，并将程序传送给Reduce节点
6. 主节点启动每个Map节点执行程序，每个Map节点尽可能读取本地或本机架的数据进行计算
7. 每个Map节点处理读取的数据块,并做一些数据整理工作(combining,sorting等)并将中间结果存放在本地；同时通知主节点计算任务完成并告知中间结果数据存储位置
8. 主节点等所有Map节点计算完成后，开始启动Reduce节点运行；Reduce节点从主节点所掌握的中间结果数据位置信息，远程读取这些数据
9. Reduce节点计算结果汇总输出到一个结果文件即获得整个处理结果

#### 带宽优化
- Q:大量的键值对数据在传送给Reduce节点时会引起较大的通信带宽开销
- A:每个Map节点处理完成的中间键值对将由combiner做一个合并压缩，即把那
些键名相同的键值对归并为一个键名下的一组数值。

#### 计算优化
- Reduce节点必须要等到所有Map节点计算结束才能开始执行，因此，如果有一个计算量大、或者由于某个问题导致很慢结束的Map节点，则会成为严重的“拖后腿者”。
- 把一个Map计算任务让多个Map节点同时做，取最快完成者的计算结果。

#### 失效处理
- 主节点失效
	- 主节点中会周期性地设置检查点(checkpoint)，检查整个计算作业的执行情况，一旦某个任务失效，可以从最近有效的检查点开始重新执行，避免从头开始计算的时间浪费
	- 如果只有一个Master，它不太可能失败；因此，如果Master失败，将中止MapReduce计算。
- 工作节点失效
	- 工作节点失效是很普遍发生的，主节点会周期性地给工作节点发送检测命令，如果工作节点没有回应，这认为该工作节点失效，主节点将终止该工作节点的任务并把失效的任务重新调度到其它工作节点上重新执行。

#### 用数据分区解决数据相关性问题
- 一个Reduce节点上的计算数据可能会来自多个Map节点，因此，为了在进入Reduce节点计算之前，需要把属于一个Reduce节点的数据归并到一起。
- 在Map阶段进行了Combining以后，可以根据一定的策略对Map输出的中间结果进行分区(partitioning)，这样即可解决以上数据相关性问题避免Reduce计算过程中的数据通信。


### 分布式文件系统GFS的基本工作原理
#### Google GFS的基本设计原则
- 廉价本地磁盘分布存储
- 多数据自动备份解决可靠性
- 为上层的MapReduce计算框架提供支撑
#### Google GFS的基本构架和工作原理
##### GFS Master
Master上保存了GFS文件系统的三种元数据：
1. 命名空间(Name Space)即整个分布式文件系统的目录结构
2. Chunk与文件名的映射表
3. Chunk副本的位置信息，每一个Chunk默认有3个副本
1.2.通过操作日志提供容错处理能力，3.保存在ChunkServer上，Master启动/ChunkServe注册时完成在ChunkServer上的生成。

##### GFS ChunkServer
- GFS中每个数据块划分缺省为64MB
- 每个数据块会分别在3个(缺省情况下)不同的地方复制副本；
- 对每一个数据块，仅当3个副本都更新成功时，才认为数据保存成功。
- 当某个副本失效时，Master会自动将正确的副本数据进行复制以保证足够的副本数；

#### 数据访问工作过程
![](../../img/Pasted%20image%2020240620155504.png)
#### GFS的系统管理技术
1. 大规模集群安装技术
2. 故障检测技术
3. 节点动态加入技术
4. 节能技术


### 分布式结构化数据表BigTable
#### BigTable的基本作用和设计思想
- GFS是一个文件系统，难以提供对结构化数据的存储和访问管理。为此，Google在GFS之上又设计了一个结构化数据存储和访问管理系统—BigTable，为应用程序提供比单纯的文件系统更方便、更高层的数据操作能力。
#### BigTable设计动机和目标
- 需要存储管理海量的结构化半结构化数据
- 海量的服务请求
- 商用数据库无法适用
#### BigTable数据模型—多维表
BigTable主要是一个分布式多维表，表中的数据通过：
- 一个行关键字（row key）
- 一个列关键字（column key）
- 一个时间戳（timestamp）
进行索引和查询定位的。

![](../../img/Pasted%20image%2020240620160354.png)


#### BigTable基本架构
##### 主服务器
- 新子表分配
- 子表服务器监控
- 负载均衡
##### 子表服务器
- BigTable中的数据都以子表形式保存在子表服务器上，客户端程序也直接和子表服务器通信。
- 当一个新子表产生，子表服务器的主要问题包括子表的定位、分配、及子表数据的最终存储。
##### 子表的基本存储结构SSTable
- SSTable是BigTable内部的基本存储结构，以GFS文件形式存储在GFS文件系统中。一个SSTable实际上对应于GFS中的一个64MB的数据块(Chunk)
- SSTable中的数据进一步划分为64KB的子块，因此一个SSTable可以有多达1千个这样的子块。为了维护这些子块的位置信息，需要使用一个Index索引

##### 子表寻址
![](../../img/Pasted%20image%2020240620161327.png)


## 03-2 Hadoop MapReduce基本架构
### Hadoop 分布式文件系统HDFS
#### HDFS的基本特征
- 模仿Google GFS设计实现
- 存储极大数目的信息，支持很大的单个文件。
- 提供数据的高可靠性和容错能力
- 提供对数据的快速访问；并提供良好的可扩展性
- 与GFS类似，HDFS是MapReduce的底层数据存储支撑
- HDFS对顺序读进行了优化
- 数据支持一次写入，多次读取；不支持已写入数据的更新操作。
- 数据不进行本地缓存
- 基于块的文件存储，默认的块的大小是64MB
- 多副本数据块形式存储
#### HDFS基本架构
![](../../img/Pasted%20image%2020240620162442.png)
![](../../img/Pasted%20image%2020240620162453.png)


##### NameNode
- 负责管理分布式文件系统的命名空间（Namespace）
	- FsImage用于维护文件系统树以及文件树中所有的文件和文件夹的元数据
	- 操作日志文件EditLog中记录了所有针对文件的创建、删除、重命名等操作
- 名称节点记录了每个文件中各个块所在的数据节点的位置信息

![](../../img/Pasted%20image%2020240620162842.png)
名称节点运行期间EditLog不断变大的问题

##### SecondaryNameNode
![](../../img/Pasted%20image%2020240620163007.png)
##### DataNode
分布式文件系统HDFS的工作节点，负责数据的存储和读取，会根据客户端或者是名称节点的调度来进行数据的存储和检索，并且向名称节点定期发送自己所存储的块的列表。

#### HDFS可靠性与出错恢复
- DataNode节点的检测
	- NameNode不断检测DataNode是否有效
	- 若失效，则寻找新的节点替代，将失效节点数据重新分布
- 集群负载均衡
- 数据一致性：校验和(checksum)
- 主节点元数据失效
##### NameNode出错
HDFS设置了备份机制，把这些核心文件同步复制到备份服务器SecondaryNameNode上。当名称节点出错时，就可以根据备份服务器SecondaryNameNode中的FsImage和Editlog数据进行恢复。

##### DataNode出错
- 每个数据节点会定期向名称节点发送“心跳”信息，向名称节点报告自己的状态。
- 名称节点会定期检查副本数量，一旦发现某个数据块的副本数量小于冗余因子，就会启动数据冗余复制，为它生成新的副本。
- **HDFS和其它分布式文件系统的最大区别就是可以调整冗余数据的位置。**
##### 数据出错
- 客户端在读取到数据后，会采用md5和sha1对数据块进行校验，以确定读取到正确的数据。
- 在文件被创建时，客户端就会对每一个文件块进行信息摘录，并把这些信息写入到同一个路径的隐藏文件里面。
- 当客户端读取文件的时候，会先读取该信息文件，然后，利用该信息文件对每个读取的数据块进行校验，如果校验出错，客户端就会请求到另外一个数据节点读取该文件块，并且向名称节点报告这个文件块有错误，名称节点会定期检查并且重新复制这个块。
### Hadoop MapReduce的基本工作原理
#### Hadoop MapReduce基本构架与工作过程
![](../../img/Pasted%20image%2020240620164155.png)

#### Hadoop MapReduce主要组件
##### 文件输入格式InputFormat
- 定义了数据文件如何分割和读取
- 定义InputSplits，将一个文件分开成为任务
- 为RecordReader提供一个工厂，用来读取这个文件
当启动一个Hadoop任务的时候，一个输入文件所在的目录被输入到FileInputFormat对象中。
FileInputFormat从这个目录中读取所有文件。然后FileInputFormat将这些文件分割为一个或者多
个InputSplits。
通过在JobConf对象上设置JobConf.setInputFormat设置文件输入的格式。

##### 输入数据分块InputSplits
-  InputSplit定义了输入到单个Map任务的输入数据
- 一个MapReduce程序被统称为一个Job
- InputSplit将文件分为64MB的大小
	- 配置文件hadoop-site.xml中的mapred.min.split.size参数控制这个大小
-  mapred.tasktracker.map.task.maximum用来控制某一个节点上所有map任务的最大数目

##### 数据记录读入RecordReader
- InputSplit定义了一项工作的大小，但是没有定义如何读取数据
- RecordReader实际上定义了如何从数据上转化为一个(key,value)对的详细方法，并将数据输出到Mapper类中
- TextInputFormat提供了LineRecordReader

##### Mapper
- 每一个Mapper类的实例生成了一个Java进程（在某一个InputSplit上执行）
- 有两个额外的参数OutputCollector以及Reporter，前者用来收集中间结果，后者用来获得环境参数以及设置当前执行的状态。

##### Combiner
-  合并相同key的键值对，减少partitioner时候的数据通信开销；
- 是在本地执行的一个Reducer，满足一定的条件才能够执行。

##### Partitioner & Shuffle
在Map工作完成之后，每一个 Map函数会将结果传到对应的Reducer所在的节点，此时，用户可以提供一个Partitioner类，用来决定一个给定的(key,value)对传输的具体位置。

##### Sort
传输到每一个节点上的所有的Reduce函数接收到的(key,value)都会被Hadoop自动排序（即Map生成的结果传送到某一个节点的时候，会被自动排序）

##### Reducer
- 执行用户定义的Reduce操作
- 接收到一个OutputCollector的类作为输出

##### 文件输出格式OutputFormat
- 写入到HDFS的所有OutputFormat都继承自FileOutputFormat
- 每一个Reducer都写一个文件到一个共同的输出目录，文件名是part-nnnnn，其中nnnnn是与每一个reducer相关的一个号（partition id）
- FileOutputFormat.setOutputPath()
- JobConf.setOutputFormat()

## 04 Hadoop安装运行与程序开发
### Hadoop安装方式
单机模式、单机伪分布模式、集群模式
?????????????????????

### HDFS编程
FileSystem基类
- FileSystem是一个通用文件系统的抽象基类，可以被分布式文件系统继承，所有可能使用Hadoop文件系统的代码，都要使用这个类
- FileSystem的open()方法返回的是一个输入流FSDataInputStream对象，在HDFS文件系统中，具体的输入流就是DFSInputStream；FileSystem中的create()方法返回的是一个输出流FSDataOutputStream对象，在HDFS文件系统中，具体的输出流就是DFSOutputStream。
创建文件
`public FSDataOutputStream create(Path f);`
打开文件
`public abstract FSDataInputStream open(Path f,int bufferSize) throws IOException`
获取文件信息
`public abstract FileStatus getFileStatus(Path f) throws IOException;`
获取目录信息
`public FileStatus[] listStatus(Path f) throws IOException;`
文件读取
`public final int read(byte[] b) throws IOException`
文件写入
`public void write(byte[] b,int off,int len) throws IOException`
关闭
`public void close()throws IOException`
删除
`public abstract boolean delete(Path f, boolean recursive) throws IOException`


## 06-1 HBase基本原理与程序设计

### HBase基本原理
#### 设计目标与功能特点
目标
1. **应对HDFS局限**，提供一个分布式数据管理系统
2.  提供基于列存储模式的大数据表管理能力
3. HBase试图提供随机和实时的数据读写访问能力
4. 具有高可扩展性、高可用性、容错处理能力、负载平衡能力、实时数据查询能力
特点
1. 列式存储
2. 表数据是稀疏的多维映射表
3. 读写的严格一致性（区别于Cassandra的最终一致性）
4. 提供很高的数据读写速度，为写数据进行了特别优化
5. 良好的线形可扩展性
6. 提供海量数据存储能力
7. 数据会自动分片
8. 具有自动的失效检测和恢复能力，保证数据不丢失
9. 提供了方便的与HDFS和MapReduce集成的能力
10. 提供Java API作为编程接口

### HBase数据模型
- HBase的数据模型是一个**稀疏、多维度、排序**的映射表，这张表的索引是**行键、列族、列限定符和时间戳**。
- 所有数据都以**字符串**进行存储，用户需要自行进行数据类型的转换
- **每一行**都有一个可排序的**行键**和**任意多的列**
- 表由一个或多个**列族**组成，一个列族可以包含很多个**列**。列族支持动态扩展，不需要预定义数量及类型。
- 由行键、列族、列限定符可以确定一个单元格，单元格内有数据的不同版本（HBase执行更新操作时不删除，只生成新的），再使用**时间戳**进行索引。

- 行主键：用来检索记录的主键
	- 通过单个row key访问/通过row key的范围range来访问/全表扫描。
	- 存储时，数据按照Row key的字典序排序存储，因此应该将经常一起读取的行存储放到一起。
	- 行的一次读写是原子操作。
- 列族：
	- HBase表中的每个列，都归属于某个列族，**列名都以列族作为前缀**。
	- **列族**是表的schema的一部分，**必须在使用表之前定义。**
	- **访问控制**（读/写）、磁盘和内存的使用统计都是在列族层面进行的。
- 时间戳：
	- 每个cell都保存着同一份数据的多个版本。
	- HBase 提供了两种数据版本回收方式。一是保存数据的最后 n 个版本，二是保存最近一段时间内的版本（比如最近七天）。可以针对每个列族进行设置。

- HBase通过行关键字、列（列族：列名）和时间戳确定一个存储单元，即：
	- {row key, column family, column name, timestamp} -> value
- HBase可以支持的查询方式：
	- 通过单个row key访问
	- 通过row key的范围来访问
	- 全表扫描

### HBase的基本构架
由一个MasterServer和由一组子表数据区服务器RegionServer构成，分别存储逻辑大表中的部分数据。大表中的底层数据存于HDFS中。
- **Master：** 主服务器 Master 主要负责表和 Region 的管理工作：
    - 管理用户对表的增加、删除、修改、查询等操作
    - 实现不同Region服务器之间的**负载均衡**
    - 在Region分裂或合并后，负责重新调整Region的分布
    - 对发生故障失效的Region服务器上的Region进行迁移
- **RegionServer**：服务器是HBase中最核心的模块，负责维护分配给自己的Region，并响应用户的读写请求

### HBase数据存储管理方法
- HBase主服务器
	- 与BigTable类似，HBase使用主服务器HServer来管理所有子表服务器。主服务器维护所有子表服务器在任何时刻的状态
	- 当一个新的子表服务器注册时，主服务器让新的子表服务器装载子表
	- 若主服务器与子表服务器连接超时，那么子表服务器将自动停止，并重新启动；而主服务器则假定该子表服务器已死机，将其上的数据转移至其它子表服务器，将其上的子表标注为空闲，并在重新启动后另行分配使用。
- HBase子表数据存储与子表服务器
	- 与BigTable类似，大表被分为很多个子表（Region），每个子表存储在一个子表服务器RegionServer上
		- 每个Region由以下信息标识：
		- <表名，该Region的起始row key，创建时间>每个Region的最佳大小取决于单台服务器的有效处理能力
		- 同一个Region不会被分拆到多个Region服务器
		- 每个Region服务器存储10-1000个Region
	- 每个子表中的数据区Region由很多个数据存储块Store构成，而每个Store数据块又由存放在内存中的memStore和存放在文件中的StoreFile构成![](../../img/Pasted%20image%2020240620193252.png)
- HBase数据的访问
	- 当客户端需要进行数据更新时，先查到子表服务器，然后向子表提交数据更新请求。提交的数据并不直接存储到磁盘上的数据文件中，而是添加到一个基于内存的子表数据对象memStore中，当memStore中的数据达到一定大小时，系统将自动将数据写入到文件数据块StoreFile中。
	- 每个文件数据块StoreFile最后都写入到底层基于HDFS的文件中。
	- 需要查询数据时，子表先查memStore。如果没有，则再查磁盘上的StoreFile。每个StoreFile都有类似B树的结构，允许进行快速的数据查询。StoreFile将定时压缩，多个压缩为一个。
	- 两个小的子表可以进行合并；子表大到超过某个指定值时，子表服务器就需要调用HRegion.closeAndSplit()，把它分割为两个新的子表。
- HBase数据记录的查询定位
	- **描述所有子表和子表中数据块的元数据都存放在专门的元数据表中**，并存储在特殊的子表中。子表元数据会不断增长，因此会使用多个子表来保存。而**所有元数据子表的元数据都保存在根子表中**。**主服务器会扫描根子表，从而得到所有的元数据子表位置，再进一步扫描这些元数据子表即可获得所寻找子表的位置。**
- HBase使用三层类似B+树的结构来保存Region位置
	- 通过Zookeeper里的文件得到**-ROOT-表**的位置，-ROOT-表永远不会被分割为多个Region
	- 通过-ROOT-表**查找.META.表**中**相应Region**的位置，为了加快访问，.META.表的全部Region的数据都会全部保存在**内存**中
	- 通过.META.表找到所要的用户表相应Region的位置![](../../img/Pasted%20image%2020240620194104.png)
- ![](../../img/Pasted%20image%2020240620194342.png)
- ![](../../img/Pasted%20image%2020240620194403.png)
- 客户端访问数据时的“三级寻址”
	- 为了加速寻址，客户端会缓存位置信息，同时，需要解决缓存失效问题
	- 寻址过程客户端只需要询问Zookeeper服务器，不需要连接Master服务器

### HBase基本操作
#### HBase shell 操作
##### 表的管理
- **查看有哪些表**：`list`
- **创建表**：`create <table>, {NAME => <family>[, VERSIONS => <VERSIONS>]}`
- **删除表**：`disable <table>, drop <table>
- **查看表结构**：`describe <table>`
- **修改表结构**：`alter 't1', {NAME => 'f1'}, {NAME => 'f2', METHOD => 'delete'}`
	- `{NAME => 'f1'}`：第一个修改操作。它没有指定 `METHOD` 属性，所以**默认**是**添加**或者修改名为 `f1` 的**列族**。如果列族 `f1` 已经存在，它将保持不变，除非指定了额外的属性来修改它。
##### 表数据的增删查改
- **添加数据**：`put <table>,<rowkey>,<family:column>,<value>,<timestamp>`
- **查询某行记录**（获取行）：`get <table>,<rowkey>[,<family:column>,<timestamp>]`
- **扫描表**(获取列)：`scan <table>, {COLUMNS => [ <family:column>,.... ], LIMIT => num}`
- **查询表中的数据行数**：`count <table>, {INTERVAL => intervalNum, CACHE => cacheNum}`
	- `INTERVAL`: 每处理`intervalNum`**行数据**后，会向客户端报告进度。
	- `CACHE`: 用于指定在单个RPC请求中预取的行数，以提高扫描操作的性能。
- **删除行中的某个列值**（必须指定列名）：`delete <table>, <rowkey>, <family:column> , <timestamp>`
- **删除行**：`deleteall '<table>', '<rowkey>'`
- **删除表中的所有数据**：`truncate <table>`

##### HBase中的disable和enable
- disable和enable都是HBase中比较常见的操作，很多对table的修改都需要表在disable的状态下才能进行
- disable ′students′将表students的状态更改为disable的时候，HBase会在zookeeper中的table结点下做记录
- 在zookeeper记录下该表的同时，还会将表的region全部下线，region为offline状态
- enable的过程和disable相反，会把表的所有region上线，并删除zookeeper下的标志。如果在enable前，META中有region的server信息，那么此时会在该server上将该region 上线；如果没有server的信息，那么此时还要随机选择一台机器作为该region的server
### HBase编程方法示例
![](../../img/Pasted%20image%2020240620200110.png)
![](../../img/Pasted%20image%2020240620200206.png)
![](../../img/Pasted%20image%2020240620200225.png)
![](../../img/Pasted%20image%2020240620200311.png)
![](../../img/Pasted%20image%2020240620200323.png)
![](../../img/Pasted%20image%2020240620200416.png)

