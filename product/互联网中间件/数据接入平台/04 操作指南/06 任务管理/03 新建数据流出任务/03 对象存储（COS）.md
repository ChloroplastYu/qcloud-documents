## 操作场景

数据接入平台提供数据流出能力，您可以将 CKafka 数据分发至 COS 以便于对数据进行分析与下载等操作。

## 前提条件

- 该功能目前依赖对象存储（COS）产品，使用时需开通相关产品功能。
- 已创建好数据流出目标存储桶。

## 操作步骤

### 创建任务

1. 登录 [DIP 控制台](https://console.cloud.tencent.com/ckafka/datahub-overview)。
2. 在左侧导航栏单击**任务管理** > **任务列表**，选择好地域后，单击**新建任务**。
3. 填写任务名称，任务类型选择**数据流出**，数据目标类型选择 **日志服务（CLS）**，单击**下一步**。
4. 配置数据源信息。
   ![](https://qcloudimg.tencent-cloud.cn/raw/f8b47026ccb8b0982605b59d7b926f5b.png)
   - 源 Topic 类型：选择数据源 Topic
     - DIP Topic：选择在数据接入平台提前创建好的 Topic，详情参见 [Topic 管理](https://cloud.tencent.com/document/product/1591/77020)。
     - CKafka Topic：选择在 CKafka 创建好的实例和 Topic，一条数据流出任务最多支持选择 5 个源 Topic，选中的 Topic 内的数据格式需要保持一致方可转储成功。详情参见 [Topic 管理](https://cloud.tencent.com/document/product/597/73566)。
   - 起始位置：选择转储时历史消息的处理方式，topic offset 设置。
5. 设置上述信息后，单击**下一步**，单击**预览 Topic 数据**，将会选取**源 Topic** 中的第一条消息进行解析。
>? 目前解析消息需要满足以下条件：
>
>- 消息为 JSON 字符串结构。
>- 源数据必须为单层 JSON 格式，嵌套 JSON 格式可使用使用 [数据处理](https://cloud.tencent.com/document/product/1591/77082#3) 进行简单的消息格式转换。 
6. （可选）开启数据处理规则，具体配置方法请参见 [简单数据处理](https://cloud.tencent.com/document/product/1591/74495)。
7. 单击**下一步**，配置数据目标信息。
   ![](https://qcloudimg.tencent-cloud.cn/raw/645967f035a525a06b138fa2c973d9ed.png)
   - 目标存储桶：对不同的 Topic，选取相应的 COS 中 Bucket，则请求消息会自动在 Bucket 下创建 `instance-id/topic-id/date/timestamp` 为名称的文件路径进行存储。
   - 聚合方式：请至少填写一种聚合方式，文件将根据指定方式聚合进入 COS 存储桶。如果指定了两种聚合方式，则会同时生效。例：指定每1h或1GB聚合一次，若在1h之前达到1GB，则文件会聚合，同时在1h时也会聚合一次。
> ! 当单条消息大小大于设置的聚合文件大小时，消息可能会被截断。
>
   - 存储格式：支持 csv 和 json 格式。
   - 角色授权：使用对象存储（COS）产品功能，您需要授予一个第三方角色代替您执行访问相关产品权限。
8. 单击**提交**，可以在任务列表看到刚刚创建的任务，在状态栏可以查看任务创建进度。



