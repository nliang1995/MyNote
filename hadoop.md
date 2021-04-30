[toc]

<div style="page-break-after:always"></div>



### Hadoop1.x、2.x和3.x的区别

- Hadoop1.x：由**MapReduce（计算+资源调度）**、HDFS（数据存储）和Common（辅助工具）组成。

- Hadoop2.x：由**MapReduce（计算）**、**Yarn（资源调度）**、HDFS（数据存储）和Common（辅助工具）组成。

- Hadoop3.x：与Hadoop2.x的组成基本相同。

### HDFS（Hadoop Distributed File Syste）

HDFS是一个文件系统，用于存储文件，通过目录树来定文件；其次，它是分布式的，由多台服务器联合起来实现其功能，集群中的服务器有各自的角色。适合一次写入，多次读出的场景。

HDFS遵循主/从架构，由单个NameNode（NN）和多个DataNode（DN）组成：

- NameNode（NN）：存储文件的元数据，如文件名，文件目录结构、文件属性（生成时间、副本数、文件权限），以及每个文件的块列表和块所在的DataNode等。

* DataNode（DN）：在本地文件系统存储文件块数据以及块数据的校验和。
* Secondary NameNode（2NN）：每隔一段时间对NameNode元数据备份。

### Yarn（Yet Another Resource Negotiator）

Apache Yarn是Hadoop2.x引入的集群资源管理系统。用户可以将各种服务框架部署在Yarn中，由Yarn进行统一地管来和资源分配。

ResourceManager（RM）：ResourceManager 通常在独立的机器上以后台进程的形式运行，它是整个集群资源的主要协调者和管理者。 ResourceManager 负责给用户提交的所有应用程序分配资源，它根据应用程序优先级、队列容量、ACLs、数据位置等信息，做出决策，然后以共享的、安全的、多租户 的方式制定分配策略，调度集群资源。  

NodeManager（NM）：NodeManager 是 YARN 集群中的每个具体节点的管理者。主要负责该节点内所有容器的生命周期的管理，监视资源和跟踪节点健康。具体如下：  

- 启动时向 ResourceManager 注册并定时发送心跳消息，等待 ResourceManager 的指令。
- 维护 Container 的生命周期，监控 Container 的资源使用情况 。
- 管理任务运行时的相关依赖，根据 ApplicationMaster 的需要，在启动 Container 之前将需要的程序及其依赖拷贝到本地  。

ApplicationMaster（AM）：在用户提交一个应用程序时，YARN 会启动一个轻量级的进程AM，负责协调来自 ResourceManager 的资源，并通过 NodeManager 监视容器内资源的使用情况，同时还负责任务的监控与容错。具体如下 ：

- 根据应用的运行状态来决定动态计算资源需求。
- 向RM申请资源，监控申请的资源的使用情况。
- 跟踪任务状态和进度，报告资源的使用情况和应用的进度信息。
- 负责任务的容错。

Container：Container是Yarn中的资源抽象，它封装了某个节点上的多维度资源，如内存、CPU、磁盘、网络等。当AM向RM申请资源时，RM为AM返回的资源是用Container表示的。Yarn会为每个任务分配一个Container，该任务只能使用该Container中描述的资源。AM可在Container内运行任何类型的任务。例如：MapReduce ApplicationMaster请求一个容器来启动map或reduce任务，而Giraph ApplicationMaster请求一个容器来运行Giraph任务。

