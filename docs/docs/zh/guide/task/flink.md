# Flink节点

## 综述

Flink 任务类型，用于执行 Flink 程序。对于 Flink 节点：

1. 当程序类型为 Java、Scala 或 Python 时，worker 使用 Flink 命令提交任务 `flink run`。更多详情查看 [flink cli](https://nightlies.apache.org/flink/flink-docs-release-1.14/docs/deployment/cli/) 。

2. 当程序类型为 SQL 时，worker 使用`sql-client.sh` 提交任务。更多详情查看 [flink sql client](https://nightlies.apache.org/flink/flink-docs-master/docs/dev/table/sqlclient/) 。

## 创建任务

- 点击项目管理-项目名称-工作流定义，点击“创建工作流”按钮，进入 DAG 编辑页面；
- 拖动工具栏的 <img src="../../../../img/tasks/icons/flink.png" width="15"/> 任务节点到画板中。

## 任务参数

- 节点名称：设置任务的名称。一个工作流定义中的节点名称是唯一的。
- 运行标志：标识这个节点是否能正常调度,如果不需要执行，可以打开禁止执行开关。
- 描述：描述该节点的功能。
- 任务优先级：worker 线程数不足时，根据优先级从高到低依次执行，优先级一样时根据先进先出原则执行。
- Worker 分组：任务分配给 worker 组的机器执行，选择 Default，会随机选择一台 worker 机执行。
- 环境名称：配置运行脚本的环境。
- 失败重试次数：任务失败重新提交的次数。
- 失败重试间隔：任务失败重新提交任务的时间间隔，以分钟为单位。
- 延迟执行时间：任务延迟执行的时间，以分钟为单位。
- 超时告警：勾选超时告警、超时失败，当任务超过"超时时长"后，会发送告警邮件并且任务执行失败。
- 程序类型：支持 Java、Scala、 Python 和 SQL 四种语言。
- 主函数的 Class：Flink 程序的入口 Main Class 的**全路径**。
- 主程序包：执行 Flink 程序的 jar 包（通过资源中心上传）。
- 部署方式：支持 cluster、 local 和 application （Flink 1.11和之后的版本支持，参见 [Run an application in Application Mode](https://nightlies.apache.org/flink/flink-docs-release-1.11/ops/deployment/yarn_setup.html#run-an-application-in-application-mode)） 三种模式的部署。
- 初始化脚本：用于初始化会话上下文的脚本文件。
- 脚本：用户开发的应该执行的 SQL 脚本文件。
- Flink 版本：根据所需环境选择对应的版本即可。
- 任务名称（选填）：Flink 程序的名称。
- jobManager 内存数：用于设置 jobManager 内存数，可根据实际生产环境设置对应的内存数。
- Slot 数量：用于设置 Slot 的数量，可根据实际生产环境设置对应的数量。
- taskManager 内存数：用于设置 taskManager 内存数，可根据实际生产环境设置对应的内存数。
- taskManager 数量：用于设置 taskManager 的数量，可根据实际生产环境设置对应的数量。
- 并行度：用于设置执行 Flink 任务的并行度。
- 主程序参数：设置 Flink 程序的输入参数，支持自定义参数变量的替换。
- 选项参数：支持 `--jar`、`--files`、`--archives`、`--conf` 格式。
- 资源：如果其他参数中引用了资源文件，需要在资源中选择指定。
- 自定义参数：是 Flink 局部的用户自定义参数，会替换脚本中以 ${变量} 的内容
- 前置任务：选择当前任务的前置任务，会将被选择的前置任务设置为当前任务的上游。

## 任务样例

### 执行 WordCount 程序

本案例为大数据生态中常见的入门案例，常应用于 MapReduce、Flink、Spark 等计算框架。主要为统计输入的文本中，相同的单词的数量有多少。（Flink 的 Releases 附带了此示例作业）

#### 在 DolphinScheduler 中配置 flink 环境

若生产环境中要是使用到 flink 任务类型，则需要先配置好所需的环境。配置文件如下：`bin/env/dolphinscheduler_env.sh`。

![flink-configure](../../../../img/tasks/demo/flink_task01.png)

####  上传主程序包

在使用 Flink 任务节点时，需要利用资源中心上传执行程序的 jar 包，可参考[资源中心](../resource/configuration.md)。

当配置完成资源中心之后，直接使用拖拽的方式，即可上传所需目标文件。

![resource_upload](../../../../img/tasks/demo/upload_jar.png)

#### 配置 Flink 节点

根据上述参数说明，配置所需的内容即可。

![demo-flink-simple](../../../../img/tasks/demo/flink_task02.png)

### 执行 FlinkSQL 程序

根据上述参数说明，配置所需的内容即可。

![demo-flink-sql-simple](../../../../img/tasks/demo/flink_sql_test.png)

## 注意事项：

- Java 和 Scala 只是用来标识，没有区别，如果是 Python 开发的 Flink 则没有主函数的 class，其余的都一样。

- 使用 SQL 执行 Flink SQL 任务，目前只支持 Flink 1.13及以上版本。
