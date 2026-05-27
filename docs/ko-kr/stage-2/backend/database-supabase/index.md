# 从数据库到 Supabase

在上节课中，我们学会了 UI 设计程序 Mastergo 和 Figma 的基本用法，能够使用 github 进行代码的获取与版本管理，并通过 Zeabur 部署网站将自己的应用 / 网站传达给更多人使用。

为了帮助大家更好地衔接知识，在开始本节课关于设计工具与部署的新内容前，让我们一起通过几道简单的题目快速回顾一下上节课的核心知识点：

1. 什么是前端设计工具、Figma、MasterGo 的定义和使用方式。
2. 将设计稿转换为代码的基础方法。
3. 什么是 Github，如何配置 SSH，如何构建自己的第一个仓库。
4. 部署是什么意思，如何使用 Zeabur，如何将 Github 或本地代码部署至公共网络给大家访问。

如果对以上任何一个问题还有印象模糊的地方，建议先回顾一下上节课的文档和讲义。欢迎随时在微信学习群中提出疑问。

在本节课中，我们将学习如何让一个 APP / 网站从能跑起来变为更接近真实线上产品：除了用数据库管理程序运行中的各种数据变化外，还要具备完善的用户体系（注册、登录、权限等）以及其他关键后端能力。我们会以 Supabase 这一后端服务平台为主线，先用它实现“数据库 + 用户系统”这两项基础功能，再以 Supabase 提供的组件为参照，进一步理解现代云服务后端服务通常包含的核心模块，以及各模块的具体职能与作用逻辑。

# 你将学到

1. 什么是数据、什么是数据库，常见数据库与使用方法
2. 什么是 supabase，如何使用 supabase 进行基础的数据库操作
3. 如何使用 supabase 为应用添加基础用户管理功能
4. 学会 Supabse 进阶功能：realtime、storage、edge function
5. 学会为Supabase增加 google 与 github 登录支持

- 一款支持用户注册 / 登录，并能将数据存入在线数据库的基础应用
- 一套可复用的 Supabase 后端代码模板（数据库 + 用户管理等），供后续项目直接套用

# 1. 什么是数据库

## 1.1 什么是数据

在数字世界里，数据（Data）无处不在。简单来说，数据是信息的载体。你朋友的联系方式、一篇微信文章、一条短视频、游戏里的角色等级，这些都是数据。在我们的应用中，数据就是需要被记录和管理的一切信息，比如用户的个人资料、订单历史、程序设置等。

一般而言，数据在程序中有不同的表现形式，最简单的就是变量，我们可以用不同变量记录简单的数字：

```python
# Python variable definition examples

# Integer variable: stores age information
age = 30

# Boolean variable: stores status (whether active)
is_active = True  # True means active, False means inactive

# List variable: stores a set of score data
scores = [85, 92, 78, 90]  # Contains 4 integer elements representing different scores

# Dictionary variable: stores multiple related information of a user
user_info = {
    "age": 30,           # Key "age" corresponds to the value of age
    "height": 1.80,      # Key "height" corresponds to the value of height (unit: meter)
    "login_count": 156   # Key "login_count" corresponds to the value of login times
}
```

而对于上述所说的个人资料、订单历史这类复杂的数据而言，我们可以用更复杂的表格进行数据的表示：

| user_id | name  | email             |
| ------- | ----- | ----------------- |
| 1001    | Alice | alice@example.com |
| 1002    | Bob   | bob@example.com   |

| order_id | user_id | amount | status    |
| -------- | ------- | ------ | --------- |
| 901      | 1001    | 29.99  | completed |
| 902      | 1002    | 15.50  | pending   |

但对于结构复杂、具有层级关系或字段不固定的数据，我们可以用 JSON 格式进行描述 —— 它是互联网通用的数据中间格式，几乎所有程序都能读取解析，跨系统传数据很方便。例如，一个订单可能包含多个商品，每个商品又有自己的名称、数量和价格。用传统的表格来表示会很笨拙：要么得拆成 “订单表”“商品表” 多张表，靠关联字段才能体现 “订单包含商品” 的关系；要么在一张表用 “商品 1 名称、商品 1 价格、商品 2 名称……” 这类冗余字段，遇到商品数量不固定时根本没法适配；而 JSON 能直接用嵌套结构把 “订单 - 商品 - 商品属性” 的层级说清，既直观又灵活。

```json
{
  "order_id": 901,
  "user_id": 1001,
  "amount": 29.99,
  "status": "completed",
  "items": [
    { "sku": "BG-001", "name": "牛肉汉堡", "quantity": 1, "price": 18.00 },
    { "sku": "SD-003", "name": "炸薯条", "quantity": 1, "price": 6.99 },
    { "sku": "DK-002", "name": "可乐", "quantity": 1, "price": 5.00 }
  ],
  "shipping_address": {
    "street": "科技园路123号",
    "city": "深圳",
    "zip_code": "518057"
  }
}
```

更进一步的，如果我们考虑一个被编码成向量（Vector）的数据，向量数据通常是文本、图片或音频等非结构化数据经过 AI 模型（如 Embedding 模型）处理后得到的数值表示。它的表示形式可能是：

`[0.123, -0.456, 0.789, ..., -0.234]` (一个由几百甚至上千个浮点数组成的数组)

总的来说，在现实世界中有太多不同形态、用途的数据值得我们详细分析，每种数据可能都需要专门的数据库用于存储，具体可参考下图——是不是感觉非常多？

![](/zh-cn/stage-2/backend/database-supabase/images/image1.png)

## 1.2 为什么我们需要数据库

我们已经了解到真实世界中的数据往往结构复杂，**为了高效存储与使用这些数据，我们需要一个专门的程序或容器来管理它们** —— 这便是数据库（Database）的诞生初衷。数据库本质上是一款特殊程序，核心作用就是对数据进行规范化组织、安全存储、系统化管理，并支持高效查询调用。

想象一下，若没有数据库，应用数据会陷入怎样的困境？当用户关闭浏览器或退出应用时，所有临时加载的信息都会直接丢失；我们既无法永久保存用户的使用状态（比如登录信息、个性化设置），也没法在不同用户之间共享关键数据（比如商品库存、订单记录）。我们需要有一个装置帮我们存储所有的数据！

更灵活的是，数据库的部署方式可按需选择：既可以部署在本地服务器，满足数据本地化管理的需求；也能部署到云端，云端数据库支持弹性扩容（Scale），可随数据量与访问量增长扩展能力、承载海量数据与高并发，即便用户量大幅提升，也能保障用户的正常使用体验。

归纳而言，数据库凭借高效的持久化存储、精细化管理与快速查询能力，主要解决了以下核心问题：

- **数据的持久化存储** ： 如果没有数据库，数据将仅存在于应用的内存中，一旦应用关闭，数据就会丢失。数据库解决了这个问题，它将数据持久地存储在硬盘等存储介质上，确保了数据的长期保存，降低了丢失风险。
- **便捷的数据查询与分析** ： 数据库提供了强大的查询语言（如 SQL），让用户可以轻松、高效地对海量数据进行复杂的查询、筛选和分析，从而帮助企业做出更明智的决策。 如果没有数据库，从大量无序文件中查找特定信息将是一项极其耗时且困难的任务。
- **支持高性能与高并发访问** ： 数据库通过索引优化、查询缓存、连接池以及分布式架构等技术，能够在毫秒级时间内响应查询请求，并支撑成千上万用户的并发访问。这对于现代互联网应用（如电商平台秒杀活动、社交网络实时动态）至关重要，确保了系统的响应速度和用户体验。如果没有数据库的高性能支撑，面对海量用户请求时系统将会出现严重延迟甚至崩溃。
- **保证数据的完整性和一致性** ： 数据库通过一系列机制（如约束、触发器）来确保数据的准确性和一致性。 这意味着数据库中的数据必须符合预设的规则，例如，用户的年龄必须是数字，订单号必须是唯一的，从而有效防止了非法或无效数据的产生。
- **确保数据的安全性** ： 数据库提供了强大的安全机制，包括用户身份验证、访问控制和数据加密等，以保护数据免受未经授权的访问、修改或破坏。为了应对硬件故障、人为失误或恶意攻击等意外情况，数据库还提供了数据备份和恢复功能。 通过定期备份，可以在数据丢失或损坏时及时恢复，保障了业务的连续性。

## 1.3 关系型数据库与非关系型数据库

前面我们已经了解了数据库的核心价值、部署方式与弹性优势，而在实际选择时，首先要面对的就是数据库的两大核心类别：关系型数据库与非关系型数据库（NOSQL），我们可以用简单的两段话简单理解他们的区别：

关系型数据库就像结构严谨的Excel表格，所有数据必须预先定义好格式（定义好 Schema 的内容, 比如要有姓名和年龄，且姓名必须是文字，年龄必须是数字），并通过关联字段（用来连接不同表格的标识，如身份证号）将不同表格连接起来。它的好处是数据精确可靠，特别适合银行转账、库存管理等不能出错的场景，但缺点是调整结构比较麻烦，海量数据下性能会受限。

非关系型数据库则像灵活的文件夹，可以存放格式各异的文档、图片或键值对（类似字典的"词-解释"结构），不需要提前规定好每份数据的结构。它更容易应对快速变化的需求和超大规模数据（比如社交媒体的海量帖子），扩展（增加服务器提升性能）起来也更方便，但牺牲了部分关联查询能力（跨不同数据表整理信息的能力）和一致性保障（确保数据时刻准确不矛盾），适合对容错性要求较高的互联网应用。

那么，实际应用中该如何选择数据库？从场景划分总结来看，关系型数据库常见于金融交易、库存管理、订单处理、账务系统等需要强一致性、复杂事务处理以及频繁读写均衡访问的场景；而非关系型数据库更适配社交媒体内容存储、实时日志分析、物联网海量数据写入、推荐系统特征读多写多等高并发、读写模式不均衡且结构灵活的需求。

但对于企业而言，在初级阶段并不需要花大量时间思考什么需要使用什么数据库。当前的数据库已是非常成熟的产品服务，最直接的方式是咨询不同云服务厂商（指提供服务器、存储、数据库、软件、算力等 IT 资源与技术服务的服务商）。我们可直接对接云服务官方销售，根据自身产品业务需求匹配适配的数据库方案；而构建企业级应用的便捷路径，便是优先与专业厂商合作。（需注意：企业级服务价格通常较高，建议先多方调研对比，也可选择购买服务器自行部署开源数据库程序作为替代方案。）

我们也可参考某家云厂商的[数据库选型推荐](https://help.aliyun.com/zh/govcloud/getting-started/select-database-services)，根据场景可进行不同数据库类型的选择，你可以对比不同云厂商的数据库规格选出最合适的进行使用。

| 数据库类型   | 数据库名称       | 价格 | 适用场景                                                                                                                                                                                                                                                                                                                         |
| ------------ | ---------------- | ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 关系型数据库 | RDS MySQL版      | 低   | 基础版：学习以及小型网站高可用版：一定业务压力的中型数据库场景集群版：业务不允许中断，访问压力较大                                                                                                                                                                                                                               |
|              | RDS SQL server版 | 高   | 基础版：测试以及小型商业化网站高可用版：企业级商业化网站集群版：企业业务不允许中断，访问压力较大                                                                                                                                                                                                                                 |
|              | RDS PostgreSQL版 | 最低 | 基础版：学习以及小型网站高可用版：一定业务压力的中型数据库场景集群版：业务不允许中断，访问压力较大的场景，其性能较一般MySQL高                                                                                                                                                                                                    |
|              | RDS PPAS版       | 高   | 通用型：兼容Oracle业务，但业务压力Udacity，虚拟化可以满足其需求独享型：面对需要独享物理机的业务，一般为高并发Oracle类业务                                                                                                                                                                                                        |
|              | DRDS             | 中   | 入门版本：4 Core 8 G，价格亲民，适合中小型在线业务企业版：16 Core 32 G，复杂 SQL 响应好，适合超高并发在线业务至尊版：32 Core 64 G，复杂 SQL 执行响应最好，提供超大规格选择                                                                                                                                                       |
| NoSQL数据库  | Redis            | 中   | 双机热备Redis：一般作为持久化数据库提高业务可用性集群版本的Redis：一般作为缓存层，加速应用访问，解决一般数据库无法负载的读取压力                                                                                                                                                                                                 |
|              | MongoDB版        | 中   | 单节点实例单节点：适用于开发、测试及其他非企业核心数据存储的场景副本集实例：适用于某些业务场景下对数据库有更高读取性能需求，如阅读类网站、订单查询系统等读多写少场景或有临时活动等突发业务需求分片集群实例：基于多个副本集（每个副本集沿用三副本模式）组成的分片集群实例，提供更高的读取性能需求，为实时在线业务提供高速读取性能 |

光说不易理解，我们通过一个具体的“博客文章”场景，来看看同样的数据在关系数据库 (SQL) 和不同类型的非关系数据库 (NoSQL) 中是如何存储的。

假设我们有一个博客平台，需要存储以下信息：

- 用户（Users）：用户 ID、用户名、邮箱
- 文章（Posts）：文章 ID、标题、内容、作者 ID
- 评论（Comments）：评论 ID、评论内容、评论者 ID、所属文章 ID
- 标签（Tags）：标签 ID、标签名
- 文章与标签的关系：单篇文章关联的多个标签、单个标签对应的多篇文章

### 关系数据库 (SQL) 示例

在SQL数据库中，我们会将不同类型的数据分别存储在不同的表中，并通过“外键”将它们关联起来。这种结构清晰、规范，减少了数据冗余。

以 “内容平台的文章管理” 为例，我们不会把 “用户、文章、评论、标签” 混存，而是拆成 5 张功能单一的表，每张表都有明确的 “职责边界” 和严格的结构定义（Schema）：

- `users` 表 (存储用户信息)

| user_id (主键) | username | email             |
| -------------- | -------- | ----------------- |
| 101            | Alice    | alice@example.com |
| 102            | Bob      | bob@example.com   |

- `posts` 表 (存储文章信息)

| post_id (主键) | title     | content                        | author_id (外键) |
| -------------- | --------- | ------------------------------ | ---------------- |
| 1              | 初识SQL   | 这是关于SQL数据库的一篇文章... | 101              |
| 2              | NoSQL入门 | NoSQL提供了灵活的数据模型...   | 102              |

- `comments` 表 (存储评论信息)

| comment_id (主键) | body             | commenter_id (外键) | post_id (外键) |
| ----------------- | ---------------- | ------------------- | -------------- |
| 1001              | 写得很棒！       | 102                 | 1              |
| 1002              | 学习了。         | 101                 | 2              |
| 1003              | 有没有更多例子？ | 101                 | 1              |

- `tags` 表 (存储标签)

| tag_id (主键) | tag_name |
| ------------- | -------- |
| 51            | 数据库   |
| 52            | 技术     |
| 53            | 入门     |

- `post_tags` 表 (存储文章与标签的多对多关系，体现联表特点)

| post_id (外键) | tag_id (外键) |
| -------------- | ------------- |
| 1              | 51            |
| 1              | 52            |
| 2              | 51            |
| 2              | 52            |
| 2              | 53            |

若需查询 “Alice 发表的《初识 SQL》（post_id=1）的完整信息（含文章内容、作者、评论、标签）”，需执行多表连接（JOIN）查询，通过外键关联 5 张表并聚合数据，SQL 语句如下：

```sql
SELECT
    p.title,
    p.content,
    u.username AS author,
    c.body AS comment,
    t.tag_name AS tag
FROM
    posts p
JOIN
    users u ON p.author_id = u.user_id
LEFT JOIN
    comments c ON p.post_id = c.post_id
LEFT JOIN
    post_tags pt ON p.post_id = pt.post_id
LEFT JOIN
    tags t ON pt.tag_id = t.tag_id
WHERE
    p.post_id = 1;
```

这个查询会跨越5个表，将所有相关数据聚合在一起返回。这是关系数据库的核心优势：通过规范化和连接操作，可以灵活地进行各种复杂的查询，同时保证了数据的一致性和最小冗余。

### 非关系数据库 (NoSQL) 示例

NoSQL 数据库（如 MongoDB、Redis）的设计思路与 SQL 相反，它不强调数据的拆分与规范，通常会将业务上相关联的所有数据打包聚合在一起，以减少查询时的连接操作，从而提高读取性能。

在 NoSQL 数据库中，文档数据库（Document Database） 是最常用的类型之一，MongoDB 就是典型代表。它以 “文档” 作为基本存储单元，这里的 “文档” 并非我们日常理解的 “文章”，而是一种类似 JSON 的数据结构（MongoDB 中实际使用 BSON 格式，支持更多数据类型）：无需预先定义统一的 Schema（数据结构），每个文档的字段可以灵活增减，字段类型也能自由调整，完美适配数据格式多变的场景。

在文档数据库中，通常会将一篇文章及其所有相关信息（如评论、标签）存储在一个文档中（文档格式类似 JSON，可灵活定义字段，无需预先制定 Schema），核心逻辑是 “将‘一个业务场景下的完整信息’存放在一个文档中”，避免查询时的多数据源拼接。

`posts` 集合中的一个文档示例：

```json
{
  "_id": 1,
  "title": "初识SQL",
  "content": "这是关于SQL数据库的一篇文章...",
  "author": {
    "user_id": 101,
    "username": "Alice",
    "email": "alice@example.com"
  },
  "tags": [
    "数据库",
    "技术"
  ],
  "comments": [
    {
      "comment_id": 1001,
      "body": "写得很棒！",
      "commenter": {
        "user_id": 102,
        "username": "Bob"
      }
    },
    {
      "comment_id": 1003,
      "body": "有没有更多例子？",
      "commenter": {
        "user_id": 101,
        "username": "Alice"
      }
    }
  ]
}
```

这种设计的优势非常直观：当你需要获取 “第一篇文章的完整信息（含作者、评论、标签）” 时，只需通过 `_id:1` 查询这一个文档，数据库一次读取就能返回所有数据，无需像 SQL 那样执行 3-4 次表连接操作，读取效率大幅提升。

但它也存在明显的 trade-off（取舍）：由于数据是 “聚合存储”，会不可避免地产生数据冗余—— 比如作者 “Alice” 的 `username` 被嵌入到她写的每一篇文章文档中，如果某天 “Alice” 将用户名改为 “Alice_New”，理论上需要遍历所有包含她信息的文章文档，逐一更新 `author.username` 字段，不仅操作繁琐，还可能因网络或服务器问题导致部分文档更新失败，出现 “同一用户在不同文章中用户名不一致” 的情况。

不过在实际业务中，这种冗余往往是 “可接受的”：对于博客、资讯、电商商品详情等 “ **读多写少** ” 的场景（用户查看内容的次数远多于作者修改用户名的次数），用少量的冗余换取 “极致的读取性能” 是更优的选择；而如果是 “写多读少”（如频繁修改用户信息）的场景，则需要结合业务需求权衡是否使用文档数据库。

以上是对不同数据库的简单介绍，如果你对更多具体的数据库类型感兴趣，你可以参考如下资料尝试不同类型的数据库。

Examples of SQL databases：
[Db2](https://www.ibm.com/products/db2-database)、[MySQL](https://cloud.ibm.com/catalog#highlights)、[PostgreSQL](https://www.ibm.com/think/topics/postgresql)、[YugabyteDB](https://www.yugabyte.com/)、[CockroachDB](https://www.cockroachlabs.com/)、[Oracle Database](https://www.ibm.com/products/postgres-enterprise)、[Azure SQL Database](https://www.ibm.com/consulting/microsoft)

Examples of NoSQL databases：
[Redis](https://www.ibm.com/think/topics/redis)、[CouchDB](https://www.ibm.com/think/topics/couchdb)、[MongoDB](https://www.ibm.com/think/topics/mongodb)、[Cassandra](https://cloud.ibm.com/catalog#highlights)、[Elasticsearch](https://www.ibm.com/think/topics/elasticsearch)、[BigTable](https://www.techtarget.com/searchdatamanagement/news/252512583/Google-scales-up-Cloud-Bigtable-NoSQL-database)、[Neo4j](https://neo4j.com/users/ibm/)、[HBase](https://www.ibm.com/think/topics/hbase)

# 2. Supabase

在前面我们已经介绍了几类常见的数据库，以及它们各自适合的使用场景。不过在真实项目里，数据库通常只是后端体系中的一个基础模块：除了存储和查询数据，你还需要解决**用户注册登录、权限校验、文件上传与存储、对外 \*\***API\***\* 接口、甚至定时任务、实时通知**等一整套问题。仅仅选好数据库，并不能让你的应用“立刻就能上线运行”，中间还隔着一大圈繁琐的后端工程工作。

所以，我们需要考虑一个更大的背景： **后端服务** 。一个完整的应用，通常都由“前端 + 后端”组成：前端负责页面展示和用户交互，后端则负责数据存储、用户登录、业务逻辑处理等。过去，开发者往往需要自己搭建服务器、配置数据库、设计并实现 API，还要手动处理权限管理、安全策略、扩展性和监控运维等事务，整个过程既重复又耗时。为了解决这些重复劳动，业界出现了 **BaaS（Backend as a Service，后端即服务）** ：把数据库、用户认证、文件存储、实时能力等常见后端功能打包成一个云端平台，开发者通过 SDK / API 就能直接调用这些能力，而无需从零搭建和运维基础设施。

在这个背景下，[Supabase](https://supabase.com/) 就可以看作是新一代的 BaaS 代表：它以 PostgreSQL 作为核心数据库，在其之上集成了 Auth、Storage、Realtime、Edge Functions、Vector 等一整套后端能力，为开发者提供一个“以 Postgres 为中心的一站式后端平台”。接下来，我们就从这个角度出发，从“只选数据库”升级到“选择完整的后端开发平台”，具体看看 Supabase 能帮我们省掉哪些工作，又是如何让一个项目从原型到可用产品的距离大幅缩短的。

## 2.1 分步指南

在清晰把握 Supabase 的整体定位后，接下来我们将沿着 Supabase 控制台的操作路径，逐项拆解它具体提供哪些核心能力，以及每项能力对应的核心职责。我们会详细介绍 supabase 涉及的每个选项，帮助你快速入门 supabase 的基本操作。

![](/zh-cn/stage-2/backend/database-supabase/images/image2.png)

访问 Supabase 官网并登录后，在控制台首页点击 New project 进入创建流程；

输入需要配置的主要内容 Project Name、数据库密码，地址只需要选择为与程序目标用户最接近的区域即可。

![](/zh-cn/stage-2/backend/database-supabase/images/image3.png)

创建成功后，控制台左侧侧边栏将显示所有核心功能模块（Table Editor、SQL Editor、Database、Authentication 等），后续操作将围绕这些模块展开。

![](/zh-cn/stage-2/backend/database-supabase/images/image4.png)

### 表编辑器

Table Editor 可以当成是 Supabase 的可视化数据表编辑器，它能让你像操作 Excel 一样直接查看和修改数据库里的数据，无需编写 SQL 语句，只需要用鼠标交互即可修改数据内容。

![](/zh-cn/stage-2/backend/database-supabase/images/image5.png)

其中值得关注的是 Schema，Schema 可理解为数据库内的 “资源容器”，用于对表、视图、函数、索引等资源进行分组管理，主要作用有二：一是避免命名冲突（不同 Schema 下可存在同名table），二是实现权限隔离（如仅允许特定用户访问某 Schema 下的表）；

点击编辑器顶部的 Schema 下拉框可切换不同容器，日常开发中一般只需关注两类：

- `public`：默认的公共资源容器，开发者新建的业务表（如 “文章表”“评论表”）均存储于此；
- `auth`：用户认证专属容器，其中的 `users` 表自动存储所有注册用户信息（如用户 ID、邮箱、登录时间），不建议手动修改此 Schema 下的默认表，避免影响认证功能；

![](/zh-cn/stage-2/backend/database-supabase/images/image6.png)![](/zh-cn/stage-2/backend/database-supabase/images/image7.png)

### SQL 编辑器

SQL Editor 作为 Supabase 的 SQL 语句执行器，可让你用代码的方式直接操作数据库。你可以让大模型直接生成 SQL 语句，在右侧输入后点击 RUN 即可用语句创建或修改 table，也可以直接在 Results 中直接看到筛选出的 table 数据。

![](/zh-cn/stage-2/backend/database-supabase/images/image8.png)

你可以在运行 RUN 之后，在 Table Editor 的 public schema 里找到新建后的数据表；并且运行后的语句会保存在左侧的 PRIVATE 栏中，甚至可以点击下方的爱心标志对这一条查询或创建语句进行收藏。

### 数据库管理中心

Database 是 Supabase 的数据库管理中心，支持可视化地查看和管理所有数据表，并通过表的相互连线理解不同表间的关联关系（即外键约束，表示数据间的引用关系）。

![](/zh-cn/stage-2/backend/database-supabase/images/image9.png)

如果你想要手动新建 table，可以在 tables 中直接新建表格，我们会在之后的教程中详细讲解。

![](/zh-cn/stage-2/backend/database-supabase/images/image10.png)

### 身份认证

Authentication 负责管理用户的注册、登录和权限。默认的用户管理系统数据都在此处存储，它提供了开箱即用的用户注册、登录、密码重置、邮箱验证等功能，并支持第三方 OAuth 登录（如微信、GitHub、Google 等）。所有用户数据会自动同步到数据库的 `auth.users` 表中。

![](/zh-cn/stage-2/backend/database-supabase/images/image11.png)

你可以在 Provider 选项中找到不同 supabase 支持的用户信息登录入口，默认使用 Email；如果你想使用 Github 或者 Google 账户进行登录，还需要更多属性配置，我们会在下面的课程中进行详细讲解。

![](/zh-cn/stage-2/backend/database-supabase/images/image12.png)

在 Sign In / Providers 里还包含了对注册邮箱行为的控制，如果你不想每次邮箱注册都必须让用户接受邀请后才能成为用户，你可以取消 Confirm email 的强制要求。

![](/zh-cn/stage-2/backend/database-supabase/images/image13.png)

如果你想切换非 Supabase 的其他 auth 系统服务商，你可以点击 Third Party Auth，比如此处就使用 Clerk 作为第三方的系统服务商。

![](/zh-cn/stage-2/backend/database-supabase/images/image14.png)

如果你担心注册用户在短期内访问量过大，你可以在 Rate Limits 中启用对应的流量限制策略：

![](/zh-cn/stage-2/backend/database-supabase/images/image15.png)

### 存储

Storage 是 Supabase 的存储系统，兼容 amazon cloud 的 s3 概念，可用于存储任意类型的文件（如图片、视频、文档、音频等），并提供访问权限管理（公开或私有）和下载链接获取（永久链接或临时链接），你能够很方便在应用中对用户涉及到的文件内容进行上传与下载管理，并与 Supabase 的认证系统无缝集成，实现精细化的访问控制。

![](/zh-cn/stage-2/backend/database-supabase/images/image16.png)

我们将会在本节课的进阶 project 中讲解 storage 的具体用法。

![](/zh-cn/stage-2/backend/database-supabase/images/image17.png)

如果你想使用 S3 的相关协议进行操作，可以直接使用对应的配置：

![](/zh-cn/stage-2/backend/database-supabase/images/image18.png)

> Amazon Cloud（亚马逊云服务，简称 AWS）是亚马逊提供的云计算平台（就像一个大型的网络机房，你可以按需租用计算和存储资源）。S3（Simple Storage Service）是 AWS 里专门用来存储文件的服务（类似一个无限大的网盘，可以存图片、视频、备份等各种文件），它是目前最流行的对象存储服务，已经成为了事实上的行业标准。
>
> **为什么要做成 S3 兼容 \*\***API\*\* ** ?** ：S3 已经存在近 20 年，市面上有大量现成的工具、SDK 和文档，兼容 S3 意味着你可以直接用这些资源，不用从头开始制作各类相关工具，能够快速满足业务上线的需求。

### 边缘函数

如果你不想部署后端，但是想使用数据库和函数操作，你可以使用 Edge Functions 构建无需自建服务器的后端核心能力，它是 Supabase 提供的全球分布式服务端函数。简单来说，它让你无需购买和管理自己的后端服务器，就能直接编写并部署在云端的后端代码。这些函数部署在全球网络的边缘节点上，会自动在离你的用户最近的位置运行，从而大幅降低网络延迟，提供极致的响应速度。你可以在 Supabase 的仪表盘中直接创建、编辑和部署，整个开发流程非常便捷。

![](/zh-cn/stage-2/backend/database-supabase/images/image19.png)

Edge Functions 的一个核心用途是充当安全的中间层，保护你的敏感信息和鉴权密钥。在前端代码中直接调用第三方服务（如 OpenAI、Stripe）会暴露你的 API Key，带来极大的安全风险。通过 Edge Functions，你的前端应用只与你的 supabase 函数通信，所有秘密只在 supabase 中保管。

![](/zh-cn/stage-2/backend/database-supabase/images/image20.png)

Edge Functions 的函数使用 secrets 中暴露的密钥作为环境变量，通过 `Deno.env.get` 加载，从而实现第三方服务的调用。这样一来，敏感密钥就永远不会暴露在客户端（你的浏览器），彻底杜绝了被盗用的风险。

![](/zh-cn/stage-2/backend/database-supabase/images/image21.png)

请求 Supabase Edge Function 时，需在请求头携带对应的 Supabase 密钥，下面是一个极简示例：

```javascript
// 核心配置（替换为你的实际信息）
const projectId = "你的 Supabase 项目ID";
const functionName = "目标 Edge Function 名称";
const supabaseKey = "Supabase anon_key";

// 调用函数
async function callEdgeFunction() {
  const url = `https://${projectId}.supabase.co/functions/v1/${functionName}`;

  try {
    const response = await fetch(url, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        "Authorization": `Bearer ${supabaseKey}` // 关键：携带密钥完成认证
      },
      body: JSON.stringify({ order_id: "123", action: "refund" }) // 自定义请求数据
    });

    const result = await response.json();
    console.log("调用成功：", result);
  } catch (error) {
    console.error("调用失败：", error.message);
  }
}

// 执行调用
callEdgeFunction();
```

此外，Edge Functions 与 Supabase 的用户认证系统无缝集成。当已登录的用户调用一个函数时，其身份信息会传递给函数。这使得你可以在函数内部轻松识别当前用户，并根据其身份执行权限控制。更重要的是，函数在操作数据库时会自动遵循你设置好的行级安全策略（Row Level Security），确保用户只能访问和修改他们有权操作的数据，让构建安全的多用户应用变得简单。

Edge Functions 的应用场景非常广泛，能够处理各种后端任务。它们非常适合用来监听来自第三方服务的 Webhook 事件（例如支付成功、代码提交等），并自动执行相应的数据处理逻辑。你也可以用它来发送邮件通知、生成 PDF 报告、创建自定义的 API 接口来封装复杂的业务逻辑，或者执行任何你希望在服务端完成的计算任务，极大地扩展了你应用的能力。

具体到一个常见的例子：身份认证工具 Clerk 。Clerk 仅用于处理用户登录、注册、信息更新等认证相关操作，并不直接管理你的业务数据库。如果你想要将这些认证动态同步到业务数据库中，则需要通过触发 Webhook 事件请求 Edge Functions 实现。Edge Functions 能够监听 Clerk 发出的 Webhook 信号，自动执行数据同步逻辑，让 Supabase 数据库中的用户信息与 Clerk 登录状态实时对齐，全程无需你部署独立后端。

### 实时数据同步引擎

Realtime 是 Supabase 的实时数据同步引擎，它允许你的应用即时接收数据库的变化通知，而无需反复轮询 API。当数据库中的数据发生 `INSERT`、`UPDATE` 或 `DELETE` 操作时，Realtime 会通过 WebSocket 将这些变化实时推送给所有已连接的客户端。这对于构建需要实时交互的应用至关重要。

Realtime 主要包含三大核心功能，覆盖了绝大多数实时场景：

1. **Postgres Changes：** 直接监听数据库表的变化。你可以精确地订阅特定表、特定事件（增、删、改），甚至可以根据筛选条件来接收通知，并与行级安全策略（Row Level Security）完美集成，确保用户只能收到他们有权限查看的数据变更。
2. **Broadcast：** 允许客户端之间通过频道（Channel）发送低延迟的临时消息。这非常适合实现聊天室、实时光标追踪、在线游戏状态同步等功能。
3. **Presence：** 用于追踪和同步在线用户状态。你可以用它来轻松实现“谁在线上”、“当前有X人正在查看”等功能，非常适合协作类应用。

我们会在后续的项目制学习中详细介绍该部分的内容。

### 项目设置

Project Settings 是 Supabase 项目的高级配置部分，你可在此实现计算资源的深度调度，以及各类功能底层参数的精细化配置。

![](/zh-cn/stage-2/backend/database-supabase/images/image22.png)

在入门阶段，我们只需聚焦以下两个核心板块，一个是 Data API，我们在此可获取关键的 “Supabase URL”， 它是形如 `https://xxx.supabase.co` 的 RESTful 端点，是所有数据查询、新增、修改、删除操作的 “入口地址”。前端或服务端需通过该 URL 初始化 Supabase 客户端，建立与数据库的连接。

![](/zh-cn/stage-2/backend/database-supabase/images/image23.png)

另一个重点是 API Keys，选择 “Legacy anon, service_role API keys” 标签页，其中的 anon public 密钥 是前端场景的重要身份凭证，它的权限被 RLS 严格限制，仅能访问用户被授权的数据。而 service_role 密钥属于 “服务端高权限密钥”，具备绕过行级安全的能力，可执行批量数据操作、系统级配置等敏感操作。绝对禁止公开分享，若泄露需立即生成新密钥并更新服务端配置。

![](/zh-cn/stage-2/backend/database-supabase/images/image24.png)

其余配置项在当前阶段无需深究，待后续有进阶使用需求时再逐一探索即可。

## 2.1 创建你的第一个 SQL 数据表

以上是 Supabase 的界面介绍，接下来我们将深入 Supabase 的核心数据库的操作环节。

在 Supabase 中创建数据表，主要有以下两种常用方式，你可以根据需求选择：

1. （推荐）借助大语言模型生成适配 Supabase 的 SQL 语句，直接在 **SQL Editor（** 前文介绍的 SQL 语句执行器）中粘贴执行，高效快捷，我们会在下个部分环节重点说明这个操作过程。
2. 通过可视化操作创建：在左侧侧边栏找到 Database 模块，点击进入后选中侧边栏的 Tables，在右侧点击 New table 按钮，即可通过图形化界面创建数据表。

![](/zh-cn/stage-2/backend/database-supabase/images/image25.png)

值得注意的是，对应数据表的名称以及存储的数据类型可在下方的 Columns 中指定。

![](/zh-cn/stage-2/backend/database-supabase/images/image26.png)

对于关系数据库，其中很重要的特点是表与表之间的关联，你可以在下方找到 `Foreign keys` ，点击创建相应的关联关系：

![](/zh-cn/stage-2/backend/database-supabase/images/image27.png)

其中 `Foreign keys` 表达了表与表之间的关联关系：一个或一组字段，它在当前表（子表）中的值，会引用另一张表（父表）中主键的值。

例如，在创建 `学生表`的时候，我们可以这样定义外键：（`所属班级编号` 这一列是一个外键。这个外键引用了 `班级表` 里的 `班级编号` 这一列。）

```sql
CREATE TABLE 学生表 (
    学生学号 INT PRIMARY KEY,
    学生姓名 VARCHAR(50),
    所属班级编号 INT,
    FOREIGN KEY (所属班级编号) REFERENCES 班级表(班级编号)
);
```

更具体举例而言，我们可以可视化观察对应的表的结构：

班级表：
这张表里记录了所有班级的信息，每个班级都有一个独一无二的班级编号。班级编号就是这张表的主键 (Primary Key)，是每个班级的唯一身份证。

| 班级编号 | 班级名称   |
| -------- | ---------- |
| 101      | 一年级一班 |
| 102      | 一年级二班 |

学生表：
这张表记录了所有学生的信息。每个学生都属于一个特定的班级，对吗？那么我们怎么知道哪个学生在哪个班级呢？

我们可以在学生表里增加一列，叫做 `所属班级编号`。

| 学生学号 | 学生姓名 | 所属班级编号 |
| -------- | -------- | ------------ |
| 2024001  | 张三     | 101          |
| 2024002  | 李四     | 102          |
| 2024003  | 王五     | 101          |

在该例子中，学生表中的 `所属班级编号` 列就是外键 (Foreign Key)。

在 Supabse 中，点击添加 Foreign Key 后，你可直接选择进行关联表对应列的选取

![](/zh-cn/stage-2/backend/database-supabase/images/image28.png)

## 2.3 SQL Editor 简介与数据库基本操作

接下来我们将分步执行一系列 SQL 脚本，熟悉常见的 SQL 中的增删查改操作。你可以将每个步骤的代码复制到 SQL Editor 中，执行并观察结果。

你可以在该目录下获得所有的测试 SQL 文件：

https://github.com/THU-SIGS-AIID/Project5-Supabase-Demos/tree/main/apps/sql-examples

### **2.3.1 **`CREATE`** - 创建表结构**

`CREATE TABLE` 语句用于为新表定义模式（Schema），包括其列（Columns）、对应的数据类型（Data Types）以及任何约束（Constraints），简单理解是创建了一个数据表。

```sql
-- Step 1: Create the 'orders' table
-- This file is fully independent and creates a sample table for later steps.
CREATE TABLE IF NOT EXISTS orders (
  id serial PRIMARY KEY,
  user_id int NOT NULL,            -- User ID
  status text NOT NULL,            -- Order status (e.g. paid, pending)
  amount numeric(10, 2) NOT NULL,  -- Order total amount
  details jsonb,                   -- Item and extra details as JSON
  placed_at timestamptz DEFAULT now(), -- Order creation time
  is_paid boolean DEFAULT false    -- Paid flag
);

-- Expected Output:
-- Orders table created if it did not exist.
-- No data inserted. (Querying returns zero rows for now.)
-- If table already exists, no error occurs.
```

成功执行后，系统将提示脚本已完成。你可以在 Table Editor 中看到对应的表被创建完成：

![](/zh-cn/stage-2/backend/database-supabase/images/image29.png)

### **2.3.2 **`INSERT`** - 填充初始数据**

表结构创建完毕后，下一步是使用 `INSERT INTO` 语句向表中添加数据行。

```sql
-- Step 2: Insert initial rows into the orders table
-- Provides realistic, varied data for demo/testing. All values are self-contained.
INSERT INTO orders (user_id, status, amount, details, placed_at, is_paid) VALUES
  (2001, 'pending', 23.50, '{"items":[{"sku":"BGR001","name":"Beef Burger","qty":1,"price":12.00}]}', now() - interval '2 days', false),
  (2002, 'paid', 50.00, '{"items":[{"sku":"BGR002","name":"Chicken Burger","qty":2,"price":10.00},{"sku":"DRK001","name":"Lemonade","qty":2,"price":5.00}]}', now() - interval '1 day', true),
  (2003, 'cancelled', 15.00, '{"items":[{"sku":"FRY001","name":"French Fries","qty":3,"price":5.00}], "reason":"Not available"}', now() - interval '45 days', false),
  (2004, 'paid', 22.98, '{"items":[{"sku":"BGR003","name":"Veggie Burger","qty":2,"price":9.99}], "promo":"SUMMER22"}', now() - interval '10 days', true),
  (2005, 'pending', 18.75, '{"items":[{"sku":"SAL001","name":"Salad","qty":1,"price":6.75},{"sku":"BGR001","name":"Beef Burger","qty":1,"price":12.00}]}', now() - interval '7 hours', false),
  (2006, 'paid', 8.00, '{"items":[{"sku":"DRK002","name":"Cola","qty":2,"price":4.00}]}', now() - interval '3 hours', true),
  (2007, 'refunded', 14.50, '{"items":[{"sku":"BGR003","name":"Veggie Burger","qty":1,"price":9.99},{"sku":"FRY001","name":"French Fries","qty":1,"price":4.51}], "refund_reason":"Late delivery"}', now() - interval '15 days', false),
  (2008, 'paid', 26.99, '{"items":[{"sku":"BGR002","name":"Chicken Burger","qty":2,"price":10.00},{"sku":"DRK001","name":"Lemonade","qty":1,"price":6.99}]}', now() - interval '12 days', true),
  (2009, 'pending', 9.99, '{"items":[{"sku":"BGR003","name":"Veggie Burger","qty":1,"price":9.99}]}', now() - interval '30 minutes', false),
  (2010, 'paid', 19.89, '{"items":[{"sku":"BGR001","name":"Beef Burger","qty":1,"price":12.00},{"sku":"DRK002","name":"Cola","qty":2,"price":3.95}]}', now() - interval '5 days', true),
  (2011, 'cancelled', 0.00, '{"items":[], "reason":"User cancelled"}', now() - interval '2 days', false);

-- Expected Output:
-- After running this script, SELECT * FROM orders will show about 11 rows with varied user_id, status, amount, details (JSON), placed_at, and is_paid fields.
-- For example:
-- | id | user_id | status    | amount | is_paid | placed_at           |
-- |----|---------|-----------|--------|---------|---------------------|
-- | 1  | 2001    | pending   | 23.50  | false   | 2025-10-28 13:40:00Z|
-- | 2  | 2002    | paid      | 50.00  | true    | ...                 |
-- |... | ...     | ...       | ...    | ...     | ...                 |
```

执行成功后，此时表中已经插入了原始数据，你可以进入到 Table Editor 界面刷新后看到结果，也可以直接在 SQL Editor 界面中新建窗口，执行查询语句 `SELECT * FROM orders;`查看结果：

![](/zh-cn/stage-2/backend/database-supabase/images/image30.png)
