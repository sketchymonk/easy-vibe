# 从データベース到 Supabase

前回のレッスンでは，我们学〜する了 UI 設計程序 Mastergo 和 Figma 的基本用法，〜できる够使用 github 进行コード的获取与バージョン管理，并通过 Zeabur デプロイ网站〜する自分で的アプリケーション / 网站传达给更多人使用。

をサポートするため大家更好地衔接知识，在开始本节课关于設計ツール与デプロイ的新内容前，让我们一緒に通过几道シンプルな题目快速回顾一下上节课的コア知识点：

1. 何是フロントエンド設計ツール、Figma、MasterGo 的定义和使用方式。
2. 〜する設計稿转换为コード的基本的な方法。
3. 何是 Github，どのように設定 SSH，どのように構築自分で的第一个リポジトリ。
4. デプロイ是何意思，どのように使用 Zeabur，どのように〜する Github 或本地コードデプロイ至公共网络给大家アクセス。

如果对以上すべての一个問題また印象模糊的地方，提案先回顾一下上节课的ドキュメント和讲义。欢迎随时在微信学习群中提出疑问。

このレッスンでは，します学习どのように让一个 APP / 网站从〜できる跑起来变为更接近真实线上产品：除了用データベース管理程序実行中的各种数据变化外，还〜する具备充実したユーザー体系（新規登録、ログイン、権限等）以及その他重〜するななバックエンド〜できる力。我们〜する以 Supabase 这一バックエンドサービスプラットフォーム为主线，先用它実装“データベース + ユーザーシステム”这两项基本的な機〜できる，再以 Supabase 提供的コンポーネント为参照，进一步理解现代云サービスバックエンドサービス通常含む的コアモジュール，以及各モジュール的具体职〜できる与作用逻辑。

# このレッスンで学ぶこと

1. 何是数据、何是データベース，一般的なデータベース与使用方法
2. 何是 supabase，どのように使用 supabase 进行基本的な的データベース操作
3. どのように使用 supabase 为アプリケーション追加基本的なユーザー管理機〜できる
4. 学〜する Supabse 応用機〜できる：realtime、storage、edge function
5. 学〜する为Supabase増加 google 与 github ログインサポート

- 一款サポートユーザー新規登録 / ログイン，并〜できる〜する数据存入在线データベース的基本的なアプリケーション
- 一套可复用的 Supabase バックエンドコードテンプレート（データベース + ユーザー管理等），供后续プロジェクト直接套用

# 1. 何是データベース

## 1.1 何是数据

在数字世界里，数据（Data）无处不在。簡単に言うと，数据是信息的载体。你朋友的联系方式、一篇微信文章、一条短视频、游戏里的角色等级，これらの都是数据。在我们的アプリケーション中，数据である必〜するがある被レコード和管理的一切信息，例えばユーザー的个人资料、オーダー历史、程序设置等。

一般的に而言，数据在程序中有異なる的表现形式，最シンプルなである変数，できます用異なる変数レコードシンプルな数字：

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

而对于上述所说的个人资料、オーダー历史这类複雑な数据而言，できます用更複雑な表格进行数据的表示：

| user_id | name  | email             |
| ------- | ----- | ----------------- |
| 1001    | Alice | alice@example.com |
| 1002    | Bob   | bob@example.com   |

| order_id | user_id | amount | status    |
| -------- | ------- | ------ | --------- |
| 901      | 1001    | 29.99  | completed |
| 902      | 1002    | 15.50  | pending   |

但对于结构複雑な、具有层级关系或フィールド不固定的数据，できます用 JSON 格式进行説明 —— 它是互联网通用的数据中间格式，几乎すべての程序都〜できる读取解析，跨システム传数据很方便。例えば，一个オーダー可〜できる性がある含む多个商品，各商品又有自分で的名称、数量和価格。用传统的表格来表示〜する很笨拙：〜する么得拆成 “オーダー表”“商品表” 多张表，靠关联フィールド才〜できる体现 “オーダー含む商品” 的关系；〜する么在一张表用 “商品 1 名称、商品 1 価格、商品 2 名称……” 这类冗余フィールド，遇到商品数量不固定时根本没法适配；而 JSON 〜できる直接用嵌套结构把 “オーダー - 商品 - 商品属性” 的层级说清，既直观又灵活。

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

更进一步的，如果我们考虑一个被编码成ベクトル（Vector）的数据，ベクトル数据通常是文本、图片或音频等非结构化数据经过 AI モデル（如 Embedding モデル）処理后得到的数值表示。它的表示形式可〜できる性がある是：

`[0.123, -0.456, 0.789, ..., -0.234]` (一个由几百甚至上千个浮点数组成的数组)

总的来说，在现实世界中有太多異なる形态、用途的数据值得我们详细分析，每种数据可〜できる性がある都必〜するがある专门的データベース用于存储，具体可参考下图——是不是感觉非常に多？

![](/zh-cn/stage-2/backend/database-supabase/images/image1.png)

## 1.2 なぜ必〜するがありますデータベース

すでに了解到真实世界中的数据しばしば结构複雑な，**为了高效存储与使用これらの数据，必〜するがあります一个专门的程序或容器来管理它们** —— 这便是データベース（Database）的诞生初衷。データベース本质上是一款特殊程序，コア作用である对数据进行规范化组织、セキュリティ存储、システム化管理，并サポート高效查询调用。

想象一下，若没有データベース，アプリケーション数据〜する陷入怎样的困境？当ユーザー关闭ブラウザ或退出アプリケーション时，すべての临时加载的信息都〜する直接丢失；我们既无法永久保存ユーザー的使用ステータス（例えばログイン信息、个性化设置），也没法在異なるユーザー間の共享重〜するなな数据（例えば商品库存、オーダーレコード）。必〜するがあります有一个装置帮我们存储すべての的数据！

更柔軟な是，データベース的デプロイ方式可按需選択：既〜できるデプロイ在本地サーバー，满足数据本地化管理的〜する件；也〜できるデプロイ到云端，云端データベースサポート弹性扩容（Scale），可随数据量与アクセス量增长拡張〜できる力、承载海量数据与高并发，即便ユーザー量大幅提升，也〜できる保障ユーザー的正常使用体验。

归纳而言，データベース凭借効率的な持久化存储、精细化管理与快速查询〜できる力，主な解決了以下コア問題：

- **数据的持久化存储** ： 如果没有データベース，数据〜する仅存在于アプリケーション的内存中，一旦アプリケーション关闭，数据就〜する丢失。データベース解決了この問題，它〜する数据持久地存储在硬盘等存储介质上，確保了数据的长期保存，削減了丢失风险。
- **便捷的数据查询与分析** ： データベース提供了強力な查询语言（如 SQL），让ユーザー〜できる轻松、高效地对海量数据进行複雑な查询、筛选和分析，从而帮助企业做出更明智的决策。 如果没有データベース，从大量无序ファイル中查找特定信息〜する是一项極めて耗时且困难的タスク。
- **サポート高性〜できる与高并发アクセス** ： データベース通过索引最適化、查询缓存、接続池以及分布式架构等技术，〜できる够在毫秒级时间内レスポンス查询リクエスト，并支撑成千上万ユーザー的并发アクセス。这对于现代互联网アプリケーション（如电商プラットフォーム秒杀活动、社交网络リアルタイム动态）至关重〜するな，確保了システム的レスポンス速度和ユーザー体验。如果没有データベース的高性〜できる支撑，面对海量ユーザーリクエスト时システム〜する〜する出现严重遅延甚至崩溃。
- **保証数据的完全な性和一致性** ： データベース通过一系列机制（如约束、触发器）来確保数据的准确性和一致性。 这意味着データベース中的数据必須符合预设的ルール，例えば，ユーザー的年龄必須是数字，オーダー号必須是唯一的，从而有效防止了非法或无效数据的产生。
- **確保数据的セキュリティ性** ： データベース提供了強力なセキュリティ机制，包括ユーザー身份検証、アクセス控制和数据加密等，以保护数据免受未经授权的アクセス、変更或破坏。为了应对硬件故障、人为失误或恶意攻击等意外情况，データベース还提供了数据备份和恢复機〜できる。 通过定期的に备份，〜できる在数据丢失或损坏时及时恢复，保障了业务的连续性。

## 1.3 关系型データベース与非关系型データベース

前すでに了解了データベース的コア价值、デプロイ方式与弹性优势，而在实际選択时，まず〜する面对的であるデータベース的两大コア类别：关系型データベース与非关系型データベース（NOSQL），できます用シンプルな两段话シンプルな理解他们的区别：

关系型データベース就像结构严谨的Excel表格，すべての数据必須预先定义好格式（定义好 Schema 的内容, 例えば〜する有姓名和年龄，且姓名必須是文字，年龄必須是数字），并通过关联フィールド（用来接続異なる表格的标识，如身份证号）〜する異なる表格接続起来。它的好处是数据精确可靠，特に适合银行转账、库存管理等不〜できる出错的シナリオ，但缺点是调整结构かなり麻烦，海量数据下性〜できる〜する受限。

非关系型データベース则像柔軟なファイル夹，〜できる存放格式各异的ドキュメント、图片或键值对（類似字典的"词-解释"结构），不必〜するがある提前规定好每份数据的结构。它更容易应对快速变化的〜する件和超大规模数据（例えば社交媒体的海量帖子），拡張（増加サーバー提升性〜できる）起来也更方便，但牺牲了部分关联查询〜できる力（跨異なる数据表整理信息的〜できる力）和一致性保障（確保数据时刻准确不矛盾），适合对容错性〜する求较高的互联网アプリケーション。

那么，实际アプリケーション中该どのように選択データベース？从シナリオ划分总结来看，关系型データベース一般的な于金融交易、库存管理、オーダー処理、账务システム等必〜するがある强一致性、複雑な事务処理以及頻繁に读写均衡アクセス的シナリオ；而非关系型データベース更适配社交媒体内容存储、リアルタイムログ分析、物联网海量数据写入、お勧めシステム特征读多写多等高并发、读写模式不均衡且结构柔軟な〜する件。

但对于企业而言，在初级阶段并不必〜するがある花大量时间思考何必〜するがある使用何データベース。当前的データベース已是非常に成熟的产品サービス，最直接的方式是咨询異なる云サービス厂商（指提供サーバー、存储、データベース、软件、算力等 IT 资源与技术サービス的サービス商）。我们可直接对接云サービス官方销售，根据自身产品业务〜する件匹配适配的データベースソリューション；而構築企业级アプリケーション的便捷パス，便是优先与专业厂商合作。（需注意：企业级サービス価格通常较高，提案先多方调研对比，也可選択购买サーバー自行デプロイ开源データベース程序作为替代ソリューション。）

また可参考某家云厂商的[データベース选型お勧め]( PROT166 )，根据シナリオ可进行異なるデータベース类型的選択，できます对比異なる云厂商的データベース规格选出最合适的进行使用。

| データベース类型   | データベース名称       | 価格 | 适用シナリオ                                                                                                                                                                                                                                                                                                                         |
| ------------ | ---------------- | ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 关系型データベース | RDS MySQL版      | 低   | 基本的な版：学习以及小型网站高可用版：一定业务压力的中型データベースシナリオ集群版：业务不許可中断，アクセス压力较大                                                                                                                                                                                                                               |
|              | RDS SQL server版 | 高   | 基本的な版：テスト以及小型商业化网站高可用版：企业级商业化网站集群版：企业业务不許可中断，アクセス压力较大                                                                                                                                                                                                                                 |
|              | RDS PostgreSQL版 | 最低 | 基本的な版：学习以及小型网站高可用版：一定业务压力的中型データベースシナリオ集群版：业务不許可中断，アクセス压力较大的シナリオ，其性〜できる较一般的にMySQL高                                                                                                                                                                                                    |
|              | RDS PPAS版       | 高   | 通用型：兼容Oracle业务，但业务压力Udacity，虚拟化〜できる满足其〜する件独享型：面对必〜するがある独享物理机的业务，一般的に为高并发Oracle类业务                                                                                                                                                                                                        |
|              | DRDS             | 中   | 入門バージョン：4 Core 8 G，価格亲民，适合中小型在线业务企业版：16 Core 32 G，複雑な SQL レスポンス好，适合超高并发在线业务至尊版：32 Core 64 G，複雑な SQL 実行レスポンス最好，提供超大规格選択                                                                                                                                                       |
| NoSQLデータベース  | Redis            | 中   | 双机热备Redis：一般的に作为持久化データベース向上业务可用性集群バージョン的Redis：一般的に作为缓存层，加速アプリケーションアクセス，解決一般的にデータベース无法负载的读取压力                                                                                                                                                                                                 |
|              | MongoDB版        | 中   | 单节点实例单节点：适用于開発、テスト及その他非企业コア数据存储的シナリオ副本集实例：适用于某些业务シナリオ下对データベース有更高读取性〜できる〜する件，如阅读类网站、オーダー查询システム等读多写少シナリオ或有临时活动等突发业务〜する件分片集群实例：基于多个副本集（各副本集沿用三副本模式）组成的分片集群实例，提供更高的读取性〜できる〜する件，为リアルタイム在线业务提供高速读取性〜できる |

光说不易理解，我们通过一个具体的な“博客文章”シナリオ，来看看同样的数据在关系データベース (SQL) 和異なる类型的非关系データベース (NoSQL) 中是どのように存储的。

假设我们有一个博客プラットフォーム，必〜するがある存储以下信息：

- ユーザー（Users）：ユーザー ID、ユーザー名、邮箱
- 文章（Posts）：文章 ID、标题、内容、作者 ID
- 评论（Comments）：评论 ID、评论内容、评论者 ID、所属文章 ID
- 标签（Tags）：标签 ID、标签名
- 文章与标签的关系：单篇文章关联的多个标签、单个标签对应的多篇文章

### 关系データベース (SQL) 例

在SQLデータベース中，我们〜する〜する異なる类型的数据それぞれ存储在異なる的表中，并通过“外键”〜する它们关联起来。这种结构清晰、规范，減少了数据冗余。

以 “内容プラットフォーム的文章管理” 为例，我们不〜する把 “ユーザー、文章、评论、标签” 混存，而是拆成 5 张機〜できる单一的表，每张表都有明確な “責務边界” 和严格的结构定义（Schema）：

- `users` 表 (存储ユーザー信息)

| user_id (主键) | username | email             |
| -------------- | -------- | ----------------- |
| 101            | Alice    | alice@example.com |
| 102            | Bob      | bob@example.com   |

- `posts` 表 (存储文章信息)

| post_id (主键) | title     | content                        | author_id (外键) |
| -------------- | --------- | ------------------------------ | ---------------- |
| 1              | 初识SQL   | 这是关于SQLデータベース的一篇文章... | 101              |
| 2              | NoSQL入門 | NoSQL提供了柔軟な数据モデル...   | 102              |

- `comments` 表 (存储评论信息)

| comment_id (主键) | body             | commenter_id (外键) | post_id (外键) |
| ----------------- | ---------------- | ------------------- | -------------- |
| 1001              | 写得很棒！       | 102                 | 1              |
| 1002              | 学习了。         | 101                 | 2              |
| 1003              | 有没有更多例子？ | 101                 | 1              |

- `tags` 表 (存储标签)

| tag_id (主键) | tag_name |
| ------------- | -------- |
| 51            | データベース   |
| 52            | 技术     |
| 53            | 入門     |

- `post_tags` 表 (存储文章与标签的多对多关系，体现联表特点)

| post_id (外键) | tag_id (外键) |
| -------------- | ------------- |
| 1              | 51            |
| 1              | 52            |
| 2              | 51            |
| 2              | 52            |
| 2              | 53            |

若需查询 “Alice 发表的《初识 SQL》（post_id=1）的完全な信息（含文章内容、作者、评论、标签）”，需実行多表接続（JOIN）查询，通过外键关联 5 张表并聚合数据，SQL 语句如下：

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

この查询〜する跨越5个表，〜するすべての相关数据聚合在一緒に返す。这是关系データベース的コア优势：通过规范化和接続操作，〜できる灵活地进行各种複雑な查询， 保証了数据的一致性和最小冗余。

### 非关系データベース (NoSQL) 例

NoSQL データベース（如 MongoDB、Redis）的設計思路与 SQL 相反，它不强调数据的拆分与规范，通常〜する〜する业务上相关联的すべての数据打包聚合在一緒に，以減少查询时的接続操作，从而向上读取性〜できる。

在 NoSQL データベース中，ドキュメントデータベース（Document Database） 是最常用的类型之一，MongoDB である典型代表。它以 “ドキュメント” 作为基本存储单元，ここ的 “ドキュメント” 并非我们日常理解的 “文章”，而是1 つの類似 JSON 的数据结构（MongoDB 中实际使用 BSON 格式，サポート更多数据类型）：无需预先定义統一された Schema（数据结构），各ドキュメント的フィールド〜できる灵活增减，フィールド类型也〜できる自由调整，完美适配数据格式多变的シナリオ。

在ドキュメントデータベース中，通常〜する〜する一篇文章及其すべての相关信息（如评论、标签）存储在一个ドキュメント中（ドキュメント格式類似 JSON，可灵活定义フィールド，无需预先制定 Schema），コア逻辑是 “〜する‘一个业务シナリオ下的完全な信息’存放在一个ドキュメント中”，回避查询时的多数据源拼接。

`posts` 集合中的一个ドキュメント例：

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

这种設計的优势非常に直观：当必〜するがあります获取 “第一篇文章的完全な信息（含作者、评论、标签）” 时，只需通过 `_id:1` 查询这一个ドキュメント，データベース一次读取就〜できる返すすべての数据，无需像 SQL 那样実行 3-4 次表接続操作，读取效率大幅提升。

但它也存在明显的 trade-off（取舍）：由于数据是 “聚合存储”，〜する不可回避地产生数据冗余—— 例えば作者 “Alice” 的 `username` 被エンベディング到她写的每一篇文章ドキュメント中，如果某天 “Alice” 〜するユーザー名改为 “Alice_New”，理论上必〜するがある遍历すべての含む她信息的文章ドキュメント，逐一更新 `author.username` フィールド，不仅操作繁琐，还可〜できる性がある因网络或サーバー問題导致部分ドキュメント更新失敗，出现 “同一ユーザー在異なる文章中ユーザー名不一致” 的情况。

ただし実際のビジネスでは，这种冗余しばしば是 “可接受的”：对于博客、资讯、电商商品详情等 “ **读多写少** ” 的シナリオ（ユーザー確認内容的次数远多于作者変更ユーザー名的次数），用少量的冗余换取 “极致的读取性〜できる” 是更优的選択；而如果是 “写多读少”（如頻繁に変更ユーザー信息）的シナリオ，则必〜するがある结合业务〜する件权衡是否使用ドキュメントデータベース。

以上是对異なるデータベース的シンプルな介绍，如果你对更多具体的なデータベース类型感兴趣，できます参考如下资料尝试異なる类型的データベース。

Examples of SQL databases：
[Db2]( PROT167 )、[MySQL]( PROT168 )、[PostgreSQL]( PROT169 )、[YugabyteDB]( PROT170 )、[CockroachDB]( PROT171 )、[Oracle Database]( PROT172 )、[Azure SQL Database]( PROT173 )

Examples of NoSQL databases：
[Redis]( PROT174 )、[CouchDB]( PROT175 )、[MongoDB]( PROT176 )、[Cassandra]( PROT177 )、[Elasticsearch]( PROT178 )、[BigTable]( PROT179 )、[Neo4j]( PROT180 )、[HBase]( PROT181 )

# 2. Supabase

在前すでに介绍了几类よくあるデータベース，以及它们各自适合的使用シナリオ。ただし在真实プロジェクト里，データベース通常只是バックエンド体系中的一个基本的なモジュール：除了存储和查询数据，你还必〜するがある解決**ユーザー新規登録ログイン、権限校验、ファイルアップロード与存储、对外 \*\***API\***\* インターフェース、甚至定时タスク、リアルタイム通知**等一整套問題。仅仅选好データベース，并不〜できる让你的アプリケーション“立刻就〜できる上线実行”，中间还隔着一大圈繁琐的バックエンド工程工作。

そのため，必〜するがあります考虑一个更大的背景： **バックエンドサービス** 。一个完全なアプリケーション，通常都由“フロントエンド + バックエンド”组成：フロントエンド负责ページ表示和ユーザー交互，バックエンド则负责数据存储、ユーザーログイン、业务逻辑処理等。过去，開発者しばしば必〜するがある自分で搭建サーバー、設定データベース、設計并実装 API，还〜する手動で処理権限管理、セキュリティ戦略、拡張性和モニタリング运维等事务，全体の过程既重复又耗时。を解決するためこれらの重复劳动，业界出现了 **BaaS（Backend as a Service，バックエンド即サービス）** ：把データベース、ユーザー认证、ファイル存储、リアルタイム〜できる力等一般的なバックエンド機〜できる打包成一个云端プラットフォーム，開発者通过 SDK / API 就〜できる直接调用これらの〜できる力，而无需从零搭建和运维基本的な设施。

在この背景下，[Supabase]( PROT182 ) 就〜できる看作是新一代的 BaaS 代表：它以 PostgreSQL 作为コアデータベース，在其之上集成了 Auth、Storage、Realtime、Edge Functions、Vector 等一整套バックエンド〜できる力，为開発者提供一个“以 Postgres 为中心的一站式バックエンドプラットフォーム”。次に，我们就从この角度出发，从“只选データベース”アップグレード到“選択完全なバックエンド開発プラットフォーム”，具体看看 Supabase 〜できる帮我们省掉哪些工作，又是どのように让一个プロジェクト从原型到可用产品的距离大幅缩短的。

## 2.1 分步指南

在清晰把握 Supabase 的整体定位后，次にします沿着 Supabase 控制台的操作パス，逐项拆解它具体提供哪些コア〜できる力，以及每项〜できる力对应的コア責務。我们〜する详细介绍 supabase 涉及的各选项，帮助你快速入門 supabase 的基本操作。

![](/zh-cn/stage-2/backend/database-supabase/images/image2.png)

アクセス Supabase 官网并ログイン后，在控制台首页クリック New project 进入作成プロセス；

入力必〜するがある設定的主な内容 Project Name、データベース密码，地址只必〜するがある選択为与程序目標ユーザー最接近的区域即可。

![](/zh-cn/stage-2/backend/database-supabase/images/image3.png)

作成成功后，控制台左侧侧边栏〜する表示すべてのコア機〜できるモジュール（Table Editor、SQL Editor、Database、Authentication 等），后续操作〜する围绕これらのモジュール展开。

![](/zh-cn/stage-2/backend/database-supabase/images/image4.png)

### 表編集器

Table Editor 〜できる当成是 Supabase 的可视化数据表編集器，它〜できる让你像操作 Excel 一样直接確認和変更データベース里的数据，无需编写 SQL 语句，只必〜するがある用鼠标交互即可変更数据内容。

![](/zh-cn/stage-2/backend/database-supabase/images/image5.png)

其中值得关注的是 Schema，Schema 可理解为データベース内的 “资源容器”，用于对表、视图、函数、索引等资源进行分组管理，主な作用有二：一是回避命名冲突（異なる Schema 下可存在同名table），二是実装権限隔离（如仅許可特定ユーザーアクセス某 Schema 下的表）；

クリック編集器顶部的 Schema 下拉框可切り替え異なる容器，日常開発中一般的に只需关注两类：

- `public`：デフォルトの公共资源容器，開発者新建的业务表（如 “文章表”“评论表”）均存储于此；
- `auth`：ユーザー认证专属容器，其中的 `users` 表自動存储すべての新規登録ユーザー信息（如ユーザー ID、邮箱、ログイン时间），不提案手動で変更此 Schema 下的デフォルト表，回避影响认证機〜できる；

![](/zh-cn/stage-2/backend/database-supabase/images/image6.png)![](/zh-cn/stage-2/backend/database-supabase/images/image7.png)

### SQL 編集器

SQL Editor 作为 Supabase 的 SQL 语句実行器，可让你用コード的方式直接操作データベース。できます让大規模モデル直接生成 SQL 语句，在右侧入力后クリック RUN 即可用语句作成或変更 table，也〜できる直接在 Results 中直接看到筛选出的 table 数据。

![](/zh-cn/stage-2/backend/database-supabase/images/image8.png)

できます在実行 RUN 後に，在 Table Editor 的 public schema 里找到新建后的数据表；并且実行后的语句〜する保存在左侧的 PRIVATE 栏中，甚至〜できるクリック下方的爱心标志对这一条查询或作成语句进行收藏。

### データベース管理中心

Database 是 Supabase 的データベース管理中心，サポート可视化地確認和管理すべての数据表，并通过表的相互连线理解異なる表间的关联关系（即外键约束，表示数据间的引用关系）。

![](/zh-cn/stage-2/backend/database-supabase/images/image9.png)

如果你〜したい手動で新建 table，〜できる在 tables 中直接新建表格，我们〜する在後に的チュートリアル中详细讲解。

![](/zh-cn/stage-2/backend/database-supabase/images/image10.png)

### 身份认证

Authentication 负责管理ユーザー的新規登録、ログイン和権限。デフォルトのユーザー管理システム数据都在此处存储，它提供了开箱即用的ユーザー新規登録、ログイン、密码重置、邮箱検証等機〜できる，并サポート第三方 OAuth ログイン（如微信、GitHub、Google 等）。すべてのユーザー数据〜する自動同步到データベース的 `auth.users` 表中。

![](/zh-cn/stage-2/backend/database-supabase/images/image11.png)

できます在 Provider 选项中找到異なる supabase サポート的ユーザー信息ログイン入口，デフォルト使用 Email；如果你想使用 Github 或者 Google 账户进行ログイン，还必〜するがある更多属性設定，我们〜する在下的レッスン中进行详细讲解。

![](/zh-cn/stage-2/backend/database-supabase/images/image12.png)

在 Sign In / Providers 里还含む了对新規登録邮箱行为的控制，如果你不想每次邮箱新規登録都必須让ユーザー接受邀请后才〜できる成为ユーザー，できます取消 Confirm email 的强制〜する求。

![](/zh-cn/stage-2/backend/database-supabase/images/image13.png)

如果你想切り替え非 Supabase 的その他 auth システムサービス商，できますクリック Third Party Auth，例えば此处就使用 Clerk 作为第三方的システムサービス商。

![](/zh-cn/stage-2/backend/database-supabase/images/image14.png)

如果你担心新規登録ユーザー在短期内アクセス量过大，できます在 Rate Limits 中启用对应的流量限制戦略：

![](/zh-cn/stage-2/backend/database-supabase/images/image15.png)

### 存储

Storage 是 Supabase 的存储システム，兼容 amazon cloud 的 s3 概念，可用于存储任意类型的ファイル（如图片、视频、ドキュメント、音频等），并提供アクセス権限管理（公开或私有）和ダウンロードリンク获取（永久リンク或临时リンク），你〜できる够很方便在アプリケーション中对ユーザー涉及到的ファイル内容进行アップロード与ダウンロード管理，并与 Supabase 的认证システム无缝集成，実装精细化的アクセス控制。

![](/zh-cn/stage-2/backend/database-supabase/images/image16.png)

します〜する在本节课的応用 project 中讲解 storage 的具体用法。

![](/zh-cn/stage-2/backend/database-supabase/images/image17.png)

如果你想使用 S3 的相关协议进行操作，〜できる直接使用对应的設定：

![](/zh-cn/stage-2/backend/database-supabase/images/image18.png)

> Amazon Cloud（亚马逊云サービス，简称 AWS）是亚马逊提供的云计算プラットフォーム（就像一个大型的网络机房，できます按需租用计算和存储资源）。S3（Simple Storage Service）是 AWS 里专门用来存储ファイル的サービス（類似一个无限大的网盘，〜できる存图片、视频、备份等各种ファイル），它是目前最流行的对象存储サービス，すでに成为了事实上的行业標準。
>
> **なぜ〜する做成 S3 兼容 \*\***API\*\* ** ?** ：S3 すでに存在近 20 年，市面上有大量现成的ツール、SDK 和ドキュメント，兼容 S3 意味着できます直接用これらの资源，不用从头开始制作各类相关ツール，〜できる够快速满足业务上线的〜する件。

### 边缘函数

如果你不想デプロイバックエンド，しかし想使用データベース和函数操作，できます使用 Edge Functions 構築无需自建サーバー的バックエンドコア〜できる力，它是 Supabase 提供的全球分布式サービス端函数。簡単に言うと，它让你无需购买和管理自分で的バックエンドサーバー，就〜できる直接编写并デプロイ在云端的バックエンドコード。これらの函数デプロイ在全球网络的边缘节点上，〜する自動在离你的ユーザー最近的位置実行，从而大幅削減网络遅延，提供极致的レスポンス速度。できます在 Supabase 的仪表盘中直接作成、編集和デプロイ，全体の開発プロセス非常に便捷。

![](/zh-cn/stage-2/backend/database-supabase/images/image19.png)

Edge Functions 的一个コア用途是充当セキュリティな中间层，保护你的敏感信息和鉴权キー。在フロントエンドコード中直接调用第三方サービス（如 OpenAI、Stripe）〜する暴露你的 API Key，带来极大的セキュリティ风险。通过 Edge Functions，你的フロントエンドアプリケーション只与你的 supabase 函数通信，すべての秘密只在 supabase 中保管。

![](/zh-cn/stage-2/backend/database-supabase/images/image20.png)

Edge Functions 的函数使用 secrets 中暴露的キー作为環境変数，通过 `Deno.env.get` 加载，从而実装第三方サービス的调用。这样一来，敏感キー就永远不〜する暴露在クライアント（你的ブラウザ），彻底杜绝了被盗用的风险。

![](/zh-cn/stage-2/backend/database-supabase/images/image21.png)

リクエスト Supabase Edge Function 时，需在リクエスト头携带对应的 Supabase キー，下是一个极简例：

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

さらに，Edge Functions 与 Supabase 的ユーザー认证システム无缝集成。当已ログイン的ユーザー调用一个函数时，其身份信息〜する传递给函数。这使得できます在函数内部轻松识别当前ユーザー，并根据其身份実行権限控制。更重〜するなな是，函数在操作データベース时〜する自動遵循你设置好的行级セキュリティ戦略（Row Level Security），確保ユーザー只〜できるアクセス和変更他们有权操作的数据，让構築セキュリティな多ユーザーアプリケーション变得シンプルな。

Edge Functions 的アプリケーションシナリオ非常に广泛，〜できる够処理各种バックエンドタスク。它们非常に适合用来监听来自第三方サービス的 Webhook 事件（例えば決済成功、コード提出等），并自動実行相应的数据処理逻辑。你也〜できる用它来送信邮件通知、生成 PDF 报告、作成自定义的 API インターフェース来封装複雑な业务逻辑，或者実行すべての你〜を希望する在サービス端完了的计算タスク，极大地拡張了你アプリケーション的〜できる力。

具体到一个よくある例子：身份认证ツール Clerk 。Clerk 仅用于処理ユーザーログイン、新規登録、信息更新等认证相关操作，并不直接管理你的业务データベース。如果你〜したい〜するこれらの认证动态同步到业务データベース中，则必〜するがある通过触发 Webhook 事件リクエスト Edge Functions 実装。Edge Functions 〜できる够监听 Clerk 发出的 Webhook 信号，自動実行数据同步逻辑，让 Supabase データベース中的ユーザー信息与 Clerk ログインステータスリアルタイム对齐，全程无需你デプロイ独立バックエンド。

### リアルタイム数据同步引擎

Realtime 是 Supabase 的リアルタイム数据同步引擎，它許可你的アプリケーション即时受信データベース的变化通知，而无需反复轮询 API。当データベース中的数据发生 `INSERT`、`UPDATE` 或 `DELETE` 操作时，Realtime 〜する通过 WebSocket 〜するこれらの变化リアルタイム推送给すべての已接続的クライアント。这对于構築必〜するがあるリアルタイム交互的アプリケーション至关重〜するな。

Realtime 主な含む三大コア機〜できる，覆盖了绝大多数リアルタイムシナリオ：

1. **Postgres Changes：** 直接监听データベース表的变化。できます精确地サブスクリプション特定表、特定事件（增、删、改），甚至〜できる根据筛选条件来受信通知，并与行级セキュリティ戦略（Row Level Security）完美集成，確保ユーザー只〜できる收到他们有権限確認的数据变更。
2. **Broadcast：** 許可クライアント間の通过频道（Channel）送信低遅延的临时消息。这非常に适合実装チャット室、リアルタイム光标追踪、在线游戏ステータス同步等機〜できる。
3. **Presence：** 用于追踪和同步在线ユーザーステータス。できます用它来轻松実装“誰在线上”、“当前有X人正在確認”等機〜できる，非常に适合协作类アプリケーション。

我们〜する在后续的プロジェクト制学习中详细介绍该部分的内容。

### プロジェクト设置

Project Settings 是 Supabase プロジェクト的高度な設定部分，你可在此実装计算资源的深度调度，以及各类機〜できる底层パラメータ的精细化設定。

![](/zh-cn/stage-2/backend/database-supabase/images/image22.png)

在入門阶段，必〜するなのは聚焦以下两个コア板块，一个是 Data API，我们在此可获取重〜するなな的 “Supabase URL”， 它是形如 `https://xxx.supabase.co` 的 RESTful 端点，是すべての数据查询、新增、変更、削除操作的 “入口地址”。フロントエンド或サービス端需通过该 URL 初始化 Supabase クライアント，建立与データベース的接続。

![](/zh-cn/stage-2/backend/database-supabase/images/image23.png)

別の重点是 API Keys，選択 “Legacy anon, service_role API keys” 标签页，其中的 anon public キー 是フロントエンドシナリオ的重〜するな身份凭证，它的権限被 RLS 严格限制，仅〜できるアクセスユーザー被授权的数据。而 service_role キー属于 “サービス端高権限キー”，具备绕过行级セキュリティな〜できる力，可実行批量数据操作、システム级設定等敏感操作。绝对禁止公开分享，若泄露需立即生成新キー并更新サービス端設定。

![](/zh-cn/stage-2/backend/database-supabase/images/image24.png)

其余設定项在当前阶段无需深究，待后续有応用使用〜する件时再逐一探索即可。

## 2.1 作成你的第一个 SQL 数据表

以上是 Supabase 的界面介绍，次にします深入 Supabase 的コアデータベース的操作环节。

在 Supabase 中作成数据表，主な有以下两种常用方式，できます根据〜する件選択：

1. （お勧め）借助大規模言語モデル生成适配 Supabase 的 SQL 语句，直接在 **SQL Editor（** 前文介绍的 SQL 语句実行器）中貼り付け実行，高效快捷，我们〜する在下个部分环节重点说明この操作过程。
2. 通过可视化操作作成：在左侧侧边栏找到 Database モジュール，クリック进入后选中侧边栏的 Tables，在右侧クリック New table 按钮，即可通过图形化界面作成数据表。

![](/zh-cn/stage-2/backend/database-supabase/images/image25.png)

注目すべき点として，对应数据表的名称以及存储的数据类型可在下方的 Columns 中指定。

![](/zh-cn/stage-2/backend/database-supabase/images/image26.png)

对于关系データベース，其中很重〜するなな特点是表与表間の的关联，できます在下方找到 `Foreign keys` ，クリック作成相应的关联关系：

![](/zh-cn/stage-2/backend/database-supabase/images/image27.png)

其中 `Foreign keys` 表达了表与表間の的关联关系：一个或一组フィールド，它在当前表（子表）中的值，〜する引用另一张表（父表）中主键的值。

例えば，在作成 `学生表`的时候，できます这样定义外键：（`所属班级编号` 这一列是一个外键。この外键引用了 `班级表` 里的 `班级编号` 这一列。）

```sql
CREATE TABLE 学生表 (
    学生学号 INT PRIMARY KEY,
    学生姓名 VARCHAR(50),
    所属班级编号 INT,
    FOREIGN KEY (所属班级编号) REFERENCES 班级表(班级编号)
);
```

更具体举例而言，できます可视化观察对应的表的结构：

班级表：
这张表里レコード了すべての班级的信息，各班级都有一个独一无二的班级编号。班级编号である这张表的主键 (Primary Key)，是各班级的唯一身份证。

| 班级编号 | 班级名称   |
| -------- | ---------- |
| 101      | 一年级一班 |
| 102      | 一年级二班 |

学生表：
这张表レコード了すべての学生的信息。各学生都属于一个特定的班级，对吗？那么我们どのように知道どの学生在どの班级呢？

できます在学生表里増加一列，叫做 `所属班级编号`。

| 学生学号 | 学生姓名 | 所属班级编号 |
| -------- | -------- | ------------ |
| 2024001  | 张三     | 101          |
| 2024002  | 李四     | 102          |
| 2024003  | 王五     | 101          |

在该例子中，学生表中的 `所属班级编号` 列である外键 (Foreign Key)。

在 Supabse 中，クリック追加 Foreign Key 后，你可直接選択进行关联表对应列的选取

![](/zh-cn/stage-2/backend/database-supabase/images/image28.png)

## 2.3 SQL Editor 简介与データベース基本操作

次にします分步実行一系列 SQL 脚本，熟悉よくある SQL 中的增删查改操作。できます〜する各ステップ的コードコピー到 SQL Editor 中，実行并观察結果。

できます在该ディレクトリ下获得すべての的テスト SQL ファイル：

https://github.com/THU-SIGS-AIID/Project5-Supabase-Demos/tree/main/apps/sql-examples

### **2.3.1 **`CREATE`** - 作成表结构**

`CREATE TABLE` 语句用于为新表定义模式（Schema），包括其列（Columns）、对应的数据类型（Data Types）以及すべての约束（Constraints），シンプルな理解是作成了一个数据表。

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

成功実行后，システム〜する提示脚本已完了。できます在 Table Editor 中看到对应的表被作成完了：

![](/zh-cn/stage-2/backend/database-supabase/images/image29.png)

### **2.3.2 **`INSERT`** - 填充初始数据**

表结构作成完毕后，次のステップ是使用 `INSERT INTO` 语句向表中追加数据行。

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

実行成功后，此时表中すでに插入了原始数据，できます进入到 Table Editor 界面更新后看到結果，也〜できる直接在 SQL Editor 界面中新建窗口，実行查询语句 `SELECT * FROM orders;`確認結果：

![](/zh-cn/stage-2/backend/database-supabase/images/image30.png)

### **2.3.3 **`SELECT`** - 读取与查询数据**

`SELECT` 语句用于从表中检索数据。通过使用異なる的子句，〜できる実装对数据的精确筛选、排序和格式化，我们可参考以下语句一步步実行確認結果：

```sql
-- Step 3: SELECT query examples for the orders table

-- Example 1: Select all fields for all orders
SELECT * FROM orders;
-- Expected Output: Returns all rows and fields. Columns: id, user_id, status, amount, details, placed_at, is_paid.

-- Example 2: Select only pending orders
SELECT id, user_id, amount FROM orders WHERE status = 'pending';
-- Expected Output: All rows with status 'pending'; columns: id, user_id, amount.

-- Example 3: Select specific fields and filter by payment status
SELECT id, status, is_paid, amount FROM orders WHERE is_paid = true;
-- Expected Output: All rows where is_paid is true; columns: id, status, is_paid, amount.

-- Example 4: Extract all item names from the details (JSON) for each order
SELECT id, details -> 'items' AS item_list FROM orders;
-- Expected Output: Each row shows id and an array from JSON with item details.
```

- **例 1:** 返す `orders` 表中的すべての行和列，与ステップ2的输出類似。
- **例 2:** 仅返すステータス为 'pending' 的オーダー，且只含む指定的列：

![](/zh-cn/stage-2/backend/database-supabase/images/image31.png)

- **例 3:** 仅返す已決済的オーダー，并表示指定的列：

| id  | status | is_paid | amount |
| --- | ------ | ------- | ------ |
| 2   | paid   | true    | 50.00  |
| 4   | paid   | true    | 22.98  |
| 6   | paid   | true    | 8.00   |
| 8   | paid   | true    | 26.99  |
| 10  | paid   | true    | 19.89  |

- **例 4:** 返す各オーダー的 `id` 和从 `details` フィールド中提取的 `items` 数组：

| id  | item_list                                                                                                            |
| --- | -------------------------------------------------------------------------------------------------------------------- |
| 1   | `[{"qty":1,"sku":"BGR001","name":"Beef Burger","price":12}]`                                                         |
| 2   | `[{"qty":2,"sku":"BGR002","name":"Chicken Burger","price":10},{"qty":2,"sku":"DRK001","name":"Lemonade","price":5}]` |
| 3   | `[{"qty":3,"sku":"FRY001","name":"French Fries","price":5}]`                                                         |
| ... | ...                                                                                                                  |

### **2.3.4 **`INSERT`** - 插入单条レコード**

在 2.3.2 中，我们演示的是开头时刻初始化批量插入数据，现在我们確認どのように新增插入单条数据。

```sql
-- Step 4: INSERT a new order (single row)
-- Example: Add a new paid order for user 2012 with one Chicken Burger
INSERT INTO orders (user_id, status, amount, details, is_paid)
VALUES (
  2012, 'paid', 9.99,
  '{"items":[{"sku":"BGR002","name":"AIID Burger","qty":100,"price":1000}]}',
  true
);
-- Expected Output:
-- Before (table fragment):
-- | id | user_id | status | amount | is_paid |
-- | ...|   ...   |  ...   |  ...   |  ...    |
--
-- After (last row):
-- | id | user_id | status | amount | is_paid |
-- | xx |  2012   |  paid  |  9.99  |  true   |
-- (where xx = next serial value)
```

此时再用 `SELECT * FROM orders;` 对数据进行查询，できます看到 orders 表成功从 11 个数据变成了 12 个数据。

### **2.3.5 **`UPDATE`** - 変更现有数据**

在实际工作中，必〜するがあります对数据表进行頻繁に数据更新，我们〜できる够用 `UPDATE` 语句変更表中已存在的レコード。

```sql
-- Step 5: UPDATE example
-- Example: Mark order with id=1 as paid and update its status
UPDATE orders SET status = 'paid', is_paid = true WHERE id = 1;
-- Expected Output:
-- Before (row with id=1):
-- | id | status  | is_paid |
-- | 1  | pending |  false  |
-- After (row with id=1):
-- | id | status | is_paid |
-- | 1  | paid   |  true   |
-- All other rows remain unchanged.
```

### **2.3.6 **`DELETE`** - 削除数据**

`DELETE` 语句可用于从表中移除レコード，并结合条件对指定部分的数据进行変更。

```sql
-- Step 6: DELETE example
-- Example: Delete orders older than 2 days to clean up old data
DELETE FROM orders WHERE placed_at < now() - interval '2 days';
-- Expected Output:
-- Before (filtered for affected rows):
-- | id | status    | placed_at           |
-- |  3 | shipped   | 2025-10-13 ...     |  <-- will be deleted
--
-- After:
-- No such rows remain. SELECT * FROM orders WHERE placed_at < now()-interval '2 days' yields zero rows.
-- Other rows in orders table are unaffected.
```

実行前，你可先実行 `SELECT id, status, placed_at FROM orders WHERE placed_at < now() - interval '2 days';` 进行数据表筛选結果的確認。当実行 `DELETE` コマンド后，再次実行同じ的 `SELECT` 查询 `SELECT id, status, placed_at FROM orders WHERE placed_at < now() - interval '2 days';`，〜する返す一个空的結果，表明これらの行已被成功削除。

## 2.4 行级セキュリティ

在学习了データベース的基本操作后，必〜するがあります进一步深入一个保障数据セキュリティなコア概念 ——RLS（行级セキュリティ，Row Level Security）。

不妨先思考一个实际シナリオ中的重〜するなな問題：どのように実装数据的 “隔离アクセス”？例えば，只許可ユーザー A 確認自分で的数据，而无法看到ユーザー B 的信息；再例えば，即便某角色拥有データベース的アクセス権限，どのように回避其误操作或泄露その他ユーザー的敏感数据？

RLS 正是为解決这类数据セキュリティ与隔离〜する件而生。它許可開発者为データベース表定义精细化的セキュリティ戦略，根据ユーザー的身份信息（如ユーザー ID、角色権限等），精确控制哪些ユーザー〜できるアクセス、変更表中的哪些行数据。
举个典型例：对于オーダー表（`orders`），できます定义这样一条 RLS 戦略 ——“仅当 `orders` 表中某条レコード的 `user_id` 列，与当前ログインユーザー的 ID 完全一致时，该ユーザー才〜できる查询到这条オーダー数据”，从而実装 “ユーザー只〜できる看自分で的オーダー” 的コア〜する件。

当你为某张表启用 RLS后，该表的すべての数据操作リクエスト（包括 `SELECT` 查询、`INSERT` 新增、`UPDATE` 変更、`DELETE` 削除）都〜する触发 RLS 校验：必須通过至少一条セキュリティ戦略的チェック，操作才〜できる実行。若不存在許可该操作的戦略，或リクエスト未满足すべての戦略的条件，データベース〜する直接拒绝此次操作，从底层阻断非授权アクセス。

在 Supabase 中，RLS 与ユーザー认证システム深度绑定，使用起来更为便捷。Supabase 提供了一个专用函数 `auth.uid()`，它〜できる直接返す “当前发起リクエスト的已ログインユーザー” 的唯一 ID（格式为 UUID）。借助この函数，できます轻松编写戦略，実装 “数据行与ユーザー身份” 的精准关联（例えば前文提到的 “オーダー `user_id` 匹配当前ユーザー ID”）。

启用 RLS 戦略的方式很灵活，できます在 Supabase データベース管理界面中的 “RLS” 按钮，直接設定并启用戦略：

![](/zh-cn/stage-2/backend/database-supabase/images/image32.png)

![](/zh-cn/stage-2/backend/database-supabase/images/image33.png)

![](/zh-cn/stage-2/backend/database-supabase/images/image34.png)

主动設定难免显得麻烦，通常，我们在数据表语句作成、初始化的时候就〜する自動考虑植入对应的 RLS 戦略。必〜するなのは在 SQL Editor 中実行類似如下语句，即可自動开启对应数据表的行级セキュリティ戦略。

![](/zh-cn/stage-2/backend/database-supabase/images/image35.png)

# 3. 第一个 SQL アプリケーション

掌握了データベース基本的な操作与RLSコア逻辑，我们终于进入本次チュートリアル的实践环节。漫长的学习铺垫是为了让后续“从0到1搭建アプリケーション”的过程更清晰。次に，します以“汉堡店オーダー管理”为シナリオ，手把手演示Supabase的一般的な操作：从アプリケーション与Supabase的关联設定，到データベース与ログイン機〜できる的集成，段階的に学习異なる操作逻辑。

## 3.1 克隆并実行 Supabase 例プロジェクト

〜する开展实操，まず必〜するがある获取配套的演示コードリポジトリ。できます让 Trae 或 Claude Code 协助 git clone 以下リポジトリ：https://github.com/THU-SIGS-AIID/Project5-Supabase-Demos

若已設定 SSH キー，提案使用 SSH 地址进行 clone（git@github.com:THU-SIGS-AIID/Project5-Supabase-Demos.git）以提升セキュリティ性；若 SSH 或 HTTPS 接続遇到网络問題，〜できる直接クリックリポジトリページ的 “Download ZIP”，获取压缩包后解压即可看到完全なコード。

![](/zh-cn/stage-2/backend/database-supabase/images/image36.png)

Clone 后，你同样〜できる让 Trae 或者是 Claude Code 帮你起動プロジェクト，例えば直接在 Agent 界面中说明： `帮我直接启动这个项目里面的 project 1 `，或者コピー对应想起動 project 的绝对パス，貼り付け给大規模モデル让大規模モデル直接起動。

## 3.2 プロジェクト1 - 汉堡店菜单增删改查

次に进入实操环节 —— 以 `project-burger-shop-menu-crud-1` 为例，します学习どのように通过 SQL 脚本一键初始化 Supabase データベース，并完了本地プロジェクト与 Supabase データベース的关联設定，让フロントエンド〜できる正常读写菜单数据。

### 使用脚本作成データベース

まず，必〜するがあります在 Supabase 中作成必〜するがある的数据表的相关内容。进入 Project1 プロジェクトディレクトリ看到名为 `scripts`的ファイル夹，其中含む 1 个 `init.sql`データベース脚本ファイル，它〜できる帮我们自動完了すべてのデータベース相关资源的作成（包括表结构、初始数据等），後に我们〜する经常用到该ファイル进行データベース中表的初始化。

```sql
......

-- ============================================================================
-- 2. Create Menu Items Table
-- ============================================================================

create table if not exists public.menu_items (
  id uuid primary key default gen_random_uuid(),
  name text not null,
  description text,
  category text check (category in ('burger','side','drink')) default 'burger',
  price_cents int not null check (price_cents > 0),
  available boolean default true,
  emoji text,
  created_at timestamptz not null default now(),
  updated_at timestamptz not null default now()
);

-- Comments for documentation
comment on table public.menu_items is 'Burger shop menu items for CRUD demo';
comment on column public.menu_items.id is 'Unique identifier for each menu item';
comment on column public.menu_items.name is 'Display name of the menu item';
comment on column public.menu_items.description is 'Detailed description of the menu item';
comment on column public.menu_items.category is 'Category: burger, side, or drink';
comment on column public.menu_items.price_cents is 'Price in cents (integer) to avoid floating point issues';
comment on column public.menu_items.available is 'Whether the item is currently available for order';
comment on column public.menu_items.emoji is 'Optional emoji representation of the menu item';
comment on column public.menu_items.created_at is 'Timestamp when the item was created';
comment on column public.menu_items.updated_at is 'Timestamp when the item was last updated';

......
```

在 SQL Editor 中実行初始化 sql 脚本后，即可在 Table Editor 中看见已作成的数据表。其中データベース初始化コード具体実行逻辑如下：

1. 作成 menu_items 表 :
2. この表用于存储汉堡店菜单中的すべてのプロジェクト。它含む了如 name (商品名), description (説明), price_cents (以美分为单位的価格，回避浮点数精度問題), category (分类) 和 available (是否可售) 等フィールド。这基本涵盖了一个菜单项所需的すべての信息。
3. 作成 promo_codes 表 :
4. 此表用于管理促销活动，例えば折扣码。它定义了 code (折扣码), discount_type (折扣类型，如百分比或固定金额), discount_value (折扣数值) 等フィールド。
5. 禁用行级セキュリティ (Row Level Security - RLS) :
6. を容易にするため開発和テスト，脚本中明确地禁用了 RLS。但结合我们前に学习的 RLS コア逻辑：RLS 是 Supabase 保障数据セキュリティな重〜するなな機〜できる，〜できる通过精细化戦略控制 “誰〜できるアクセス / 変更哪些数据”（例えば只許可管理员編集促销码，普通ユーザー只〜できる確認菜单）。したがって本番環境では中，必須开启 RLS 并設定合理戦略，从底层阻断非授权アクセス（如防止ユーザー恶意変更他人作成的菜单，或泄露促销码ルール）。
7. 插入种子数据 (Seed Data) :
8. 为了让フロントエンドプロジェクト起動后就〜できる看到実際の菜单与促销数据（无需手動で录入テスト数据），`init.sql`脚本还〜する向 `menu_items`和 `promo_codes`表中插入 “种子数据”（即例数据）。例えば，できます看到各种汉堡、小食、饮料以及多种多样的折扣码。

### 设置与データベース的接続

データベース准备完了，必〜するがあります〜するこのフロントエンドプロジェクト与 Supabase 进行接続，从而正常读取データベース内的数据。必〜するがあります〜する Supabase プロジェクト的 URL 和 anon key 写到指定設定中，本プロジェクト提供了两种柔軟な設定方式：

1. 通过環境変数設定

在プロジェクト根ディレクトリ作成一个 .env ファイル，并填入你的 Supabase 凭证：

```
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-anon-key
```

2. 在プロジェクトページ中直接设置

を容易にするため快速演示和切り替え異なる的 Supabase プロジェクト，首页ページ右上角提供了一个 设置 按钮。できますクリック它，在弹出的模态框中直接入力或貼り付け Supabase URL 和 anon key。

クリック “Save” 后，これらの信息〜する用于动态作成 Supabase クライアント实例，類似下列コード所示：

```JavaScript
import { createClient, type SupabaseClient } from '@supabase/supabase-js';

// Optional client factory for demos: returns null when env is not set.
export function maybeCreateBrowserClient(): SupabaseClient | null {
  const url = process.env.NEXT_PUBLIC_SUPABASE_URL;
  const anon = process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY;
  if (!url || !anon) return null;
  return createClient(url, anon);
}
```

作成完データベース，填写完对应的 Supabase Link 相关設定后，即可看到如下界面，できます尝试对商品进行增删查改，并观察 Supabase 中对应部分数据表的变化。

![](/zh-cn/stage-2/backend/database-supabase/images/image37.png)

![](/zh-cn/stage-2/backend/database-supabase/images/image38.png)

### 📚 課題

1. 尝试増加和削除已有プロジェクト，在 Table Editor 中確認変更操作对数据表内容变动的影响。

## 3.4 プロジェクト2 - 汉堡店认证ユーザー

Project1 実装了 “菜单 CRUD + データベース接続” ，Project2 〜する引入更贴近真实业务的コア〜できる力，ユーザー认证（Auth）与行级セキュリティ（RLS）権限管理。

Project2 含む独立したログイン页，サポートユーザー通过「邮箱 + 密码」的方式ログイン。其コア逻辑是调用 Supabase Auth 提供的原生方法，快速実装认证プロセス，无需手動で開発複雑なログイン校验逻辑：

```
const { error: err } = await supabaseClient.auth.signUp({
  email,
  password,
  options: {
    data: {
      full_name: fullName || null,
      birthday: birthday || null,
      avatar_url: avatarUrl || null
    }
  }
});
```

![](/zh-cn/stage-2/backend/database-supabase/images/image39.png)

ログイン成功后，Supabase 〜する自動为ユーザー作成一个〜する话（session），并在后续すべてのデータベースリクエスト中自動携带认证信息；通过 RLS 的作用，各ユーザー根据对应的认证信息只〜できる看到自分で的账户信息（已购买プロジェクト、钱包剩余额度），无法看到その他ユーザー的账户信息，这就実装了異なるユーザーログイン后的数据隔离，各人只〜できる看到自分で的内容。

和 Project 1 一样，必〜するがあります先使用 `init.sql` 进行数据表的初始化（注：もし初始化出错，请先在 Table Editor 中削除すでに作成的数据表，或者是直接削除この Supabase Project， 重新新建一个 Project）

成功使用邮箱新規登録账户、在邮箱確認新規登録账户后，ログイン后进入 Shop 界面即可看到如下内容：

![](/zh-cn/stage-2/backend/database-supabase/images/image40.png)

但此时クリック admin，你并不〜できる看到如下界面，必〜するがあります尝试在数据表中找到控制ユーザー権限的部分，〜する権限変更为 `admin`，从而〜できる够在 Admin 界面正常看到如下内容：

![](/zh-cn/stage-2/backend/database-supabase/images/image41.png)

值得提示的是，目前每次新規登録新的邮箱，你都必〜するがある在邮箱中进行新規登録確認才可ログイン；但这一步并非是必須的，できます在 Supabase 的 Authentication 栏目中找到 Sign In / Providers，クリックConfirm email 取消邮箱的强制確認。

![](/zh-cn/stage-2/backend/database-supabase/images/image42.png)

### 📚 課題

1. 请先领取新手礼包，完了商品购买操作。
2. 尝试找到ユーザー権限的设定数据表位置，〜する権限変更为 `admin`，并成功在オーダー管理界面変更商品数量
3. 尝试在数据表内定位到钱包金额相关表，通过変更使剩余钱包金额増加。

# 4. 構築你的第一个 Supabase アプリケーション

经过前的システム学习，你已掌握 Supabase 的コア〜できる力（データベース操作、ユーザー认证、RLS セキュリティ戦略），现在是时候亲自動手，搭建属于你的第一个含むデータベース、サポートユーザーログインシステム的アプリケーション了！

## 4.1 为任意アプリケーション接入 Supabase データベース的標準化プロセス

できます使用標準化プロセス〜する任意アプリケーション接入 Supabase データベース：

1. まず进行〜する件梳理与信息同步，明确目標并告知AI
   1. 必〜するがあります向AI清晰説明当前アプリケーション的コア機〜できる、待新增的データベース〜する件。例：“我现有一个本地React Todoアプリケーション，数据仅存在ブラウザ本地存储，需新增‘数据云端同步’機〜できる并接入Supabaseデータベース。手伝ってください梳理：このアプリケーション涉及哪些数据操作（如新增待办、変更ステータス、削除待办）？必〜するがある作成哪些数据表来存储これらの数据？”
   2. 补充重〜するなな约束条件（可选）：例えばフィールド格式〜する求（时间戳用 `timestamptz`、金额用整数存分）、数据権限ルール（仅自分で可见待办），让AI的分析更贴合实际〜する件。
   3. 对 AI 返す的結果进行审核，若AI思路存在遗漏（如未考虑“待办截止时间”フィールド），补充提示修正：“你漏考虑截止时间了，帮我加上。”
2. 让AI基于你確認后的表结构，生成适配Supabase的 `init.sql`脚本：“基于上述所说思路和表的结构，返す给我在 Supabase 中〜できる进行初始化的 init.sql 脚本”，後に必〜するがあります在 SQL Editor 中実行脚本；若実行报错，〜するエラー信息反馈给AI，让其修正脚本。
3. 在 Supabase 実行 init.sql 脚本后，让 AI 基于脚本重构当前コード，使得〜できる够和 Supabase 进行正常的数据交互：“请你根据我的 sql 脚本以及上讨论的设定，重构プロジェクト的コード让它サポート〜できる够和 Supabase 对应的データベース进行通信并処理数据”。
4. 重构完毕，此时只必〜するがある設定好 Supabase 地址和 key 的パラメータ（正式プロジェクト通常只用環境変数設定），随后进行チェック，若没問題则顺利実装〜するアプリケーション接入 Supabase データベース。
   1. 実行プロジェクト，テストすべてのデータベース交互機〜できる，到Supabase Table Editor リアルタイム確認数据是否同步；
   2. 若出现問題（如数据无法插入、仅〜できる看到部分数据），〜する問題现象反馈给AI，让其定位原因并修正コード。

さらに，若目標是開発ユーザーログインページ，可直接让 AI 协助集成ログインページ ：“现在必〜するがあります帮我给このアプリケーション加入 Supabase 的ユーザーログインシステム，使用邮箱〜できる新規登録和ログイン”。另外，你还必〜するがある向 AI 明确ページ的リダイレクト逻辑与パス（如ログイン成功后リダイレクト至システム首页、リダイレクト首页的地址是何、ログイン失敗时留在当前页并表示エラー提示）。集成完了后，必〜するがあります尝试新規登録ログイン后〜できる在 Supabase 的 Authentication プロジェクト中看到新增的ユーザー数据，并在ログイン后〜できる正常进入到原先未ログイン无法进入的アプリケーション界面即可。

もちろん，你还〜できる直接让 AI 参考某个 project 的実装直接迁移对应的 Supabase 機〜できる，例えば某个 Project 用到了データベース以及 Edge fuction 的高度な機〜できる，できます按照如下方式直接让 AI 迁移对应的相似機〜できる：“请你参考该プロジェクト {此处コピー貼り付け参考プロジェクト的绝对地址} 当中的 Supabase 相关機〜できる実装逻辑，给当前プロジェクト加上類似的実装逻辑（如ユーザーログイン、データベース管理、函数リクエスト等等）”。

## 4.2 案例研究：構築一个在线贪吃蛇游戏

根据上所提到的 SOP ，让我们通过一个具体的な实际案例 `Project5-Supabase-Demos/apps_snakegame`来实践：为一个已有的“贪吃蛇”游戏プロジェクト増加分数排行榜单，含むユーザーログイン与データベース基本的な機〜できる。

![](/zh-cn/stage-2/backend/database-supabase/images/image43.png)

### 4.2.1 分析プロジェクト，识别数据〜する件

まず，和在前に提到的標準化プロセス類似，できます先把〜する件澄清给 AI ，让 AI 基于我们プロジェクト和〜する件给出对应的変更ソリューション，後に我们〜する基于この変更ソリューション。

**できます使用如下的プロンプト来指导 AI：**

> “我有一个贪吃蛇游戏，ディレクトリ在 {此处貼り付け贪吃蛇游戏的绝对パス}。现在我想结合 supabase 给它増加一个在线排行榜機〜できる，并且サポートユーザーログインシステム，排行榜〜できる根据ユーザー名和邮箱表示排名。
>
> 手伝ってください分析一下，为了実装この機〜できる，我必〜するがある建立哪些数据表？各表べき含む哪些フィールド？”

此时你〜する得到類似如下返す：

![](/zh-cn/stage-2/backend/database-supabase/images/image44.png)

### 4.2.2 生成 `init.sql` 脚本

确定必〜するがある的部分，できます让 AI 生成必〜するがある在 Supabase 実行的データベース初始化脚本：“请你基于上的分析，帮我在プロジェクト中生成 scripts/init.sql 脚本用于在 Supabase 中初始化所需データベース”。

![](/zh-cn/stage-2/backend/database-supabase/images/image45.png)

### 4.2.3 改造プロジェクトコード

次に必〜するなのは让 AI 基于前的内容重构当前的贪吃蛇コード：“次に请你基于前思考的内容以及 sql 表，使用 Supabase 帮我実装排行榜機〜できる，排行榜是個別に的一页，必〜するがある〜できる根据邮箱和ユーザー名区分異なるユーザー的总分，你还必〜するがあるサポート基于邮箱的ユーザーログインシステム，新規登録ログイン才〜できる玩この游戏。”

如果当前 AI 〜する話轮次太多，你想重开一个新的〜する话进行プロジェクト重构，できます把上提到的 `init.sql`作为コンテキスト中的内容，让 AI 基于 sql ファイル进行プロジェクト重构。

若是发现 AI 実装的ユーザーログインシステム不够正常，できます直接〜する我们前に写好的 `Project5-Supabase-Demos/apps/project-burger-shop-auth-users-2` 的地址一同放入プロンプト，让 AI 基于プロジェクト直接実装ユーザーログインシステム。并チェック是否すでに正确设定了接続到 Supabase 的必〜する条件，防止なぜなら Supabase 設定エラー而报错。

在コード変更过程中，若出现实际效果与预期不符的情况（如排行榜数据不表示、ログイン検証失效等），只需完全なレコード具体现象并反馈给 AI，即可段階的に接近正确結果。改造成功的標準为：ユーザー〜できる顺利完了新規登録与ログイン操作，且ログイン后可正常確認对应的游戏排行榜单。

![](/zh-cn/stage-2/backend/database-supabase/images/image46.png)

![](/zh-cn/stage-2/backend/database-supabase/images/image47.png)

### 📚 レッスン課題

1. 〜するユーザー管理システム集成到贪吃蛇游戏演示版中
2. 〜するユーザー管理システム集成到你的アプリケーション程序中（如果前に已開発过一个アプリケーション程序）

# 5. 成为 Supabase 大师

以上是 Supabase 的基本操作，次に的旅程中します〜する接触 Supbase 的応用原理和機〜できる，你〜する理解なぜ我们〜する選択 Supabase 作为教学案例，以及どのように使用 Supbase 実装更高度な的操作，协助你実装更複雑な交互機〜できる，并且在学习これらの機〜できる后，即便面对 Supabase 以外の的その他同类ツール，你也〜できる触类旁通，从更本质的层面理解バックエンドサービス的コア原理。もちろん，你并不必〜するがある在短时间内学〜する全部，也许只必〜するがある学〜する第三方ログインサポートすでに足够，できます先浏览下列内容，直到プロジェクト遇到对应的〜する件时再倒回来深入学习。

## 5.1 なぜ我们選択 Supabase

在开始応用前に，我们再次思考この問題：众多バックエンド技术ソリューション中，为何我们最終的に選択 Supabase 作为技术底座？

初创团队在技术选型时普遍面临一个矛盾：既想完全掌控バックエンドシステム，又必須快速上线产品——而自建バックエンド通常意味着〜する投入数月时间搭建データベース与リアルタイム同步、ユーザー认证、APIサービス、ファイル存储、定时タスク、モニタリング告警等コアコンポーネント，除非团队成员已在对应领域积累了豊富な実践经验。在资金不足、市场窗口短暂的双重压力下，一旦陷入基本的な设施泥潭，极易导致迭代滞后、错失早期增长空间。

Supabase 〜するこれらのバックエンド〜できる力打包为开箱即用的サービス（PostgreSQLデータベース、リアルタイムサブスクリプション、身份认证、对象存储、边缘函数、自動生成API等），让初创团队得以〜する稀缺资源聚焦于コア機〜できる開発，回避因底层建设拖慢上线速度——这已成为当前创投環境下务实的生存戦略。もちろん，また〜できる使用别的一栈式バックエンド产品进行開発，例えば PocketBase（轻量极简）和 Appwrite（跨プラットフォーム适配）等ソリューション，但综合機〜できる完全な性、SQL 生态成熟度及 Github 社区关注度，Supabase 更适合支撑业务的长期稳定実行。

在同类产品中，Supabase 的开源戦略更具优势。 以市场占有率较高的 Firebase 为例：其闭源特性易导致プラットフォーム绑定，迁移成本极高。Supabase 采用完全开源模式，サポート私有化デプロイ，规避了供应商锁定风险，可根据〜する件切り替え至其その他竞品。

总而言之，技术选型需匹配业务规模与目標。 对于个人プロジェクト或极小范围テスト，PocketBase 等超轻量ソリューション已足够；若企业需对接複雑な身份システム，或〜する满足上市公司合规审计〜する求，WorkOS 这类企业级全身份治理ソリューション更为适用。但对于検証 MVP、承载早期ユーザー的コア业务シナリオ，Supabase 的完全な機〜できる完全够用，它不仅〜できる独立支撑至少万级ユーザー规模，更可灵活集成 Stripe（決済）、Resend（邮件）、Cloudflare（CDN）等第三方サービス；即便未来业务拡張至企业级〜する件，Supabase 的开源架构也〜できる与企业システム并行デプロイ，異なる機〜できる選択最适配的プラットフォーム进行使用。这种渐进式灵活性，使初创团队无需过早投入重型基本的な设施，又〜できる保留 future-proof 的演进空间。

## 5.2 Google 和 GitHub ログインサポート

前述のチュートリアル中，我们讲解了どのように直接使用邮箱进行新規登録和ログイン，但在实际操作中我们通常〜したい簡素化新規登録プロセス，例えば使用第三方ログイン Google 和 GitHub 进行システム的快速新規登録与ログイン，します〜する在这节チュートリアル中展开各细节； ，一个完全な认证システム也必須提供セキュリティ信頼性の高い密码重置機〜できる，また〜する〜する密码重置機〜できる集成在本节チュートリアル的プロジェクト中。

本プロジェクト `Project5-Supabase-Demos/apps/project-burger-shop-auth-advanced-supabase-6`)完全な地演示了どのように実装これらの高度な機〜できる。

![](/zh-cn/stage-2/backend/database-supabase/images/image48.png)

### 5.2.1 OAuth プロセス：第三方ログイン是どのように工作的？

第三方ログイン的コア是 OAuth 2.0 开放授权协议，它的本质是 “授权代理”：許可ユーザー授权我们的アプリケーション（汉堡店プロジェクト）アクセス其在第三方プラットフォーム（如 Google）的公开信息（如邮箱、头像），但无需〜する第三方プラットフォーム的密码暴露给我们的アプリケーション，从根本上规避了密码泄露风险。

完全なプロセス可拆解为 5 个重〜するななステップ，以 Google ログイン为例：

1. ユーザー发起授权リクエスト：ユーザークリックページ上的 “Sign in with Google” 按钮，我们的アプリケーション〜する自動〜するユーザー重定向到 Google 官方的授权ページ（確保授权过程的セキュリティ性，回避钓鱼风险）。
2. ユーザー完了第三方授权：ユーザー在 Google ページログイン自分で的账户（検証ユーザー身份），并同意我们的アプリケーションリクエスト的権限（如 “获取邮箱地址”）。
3. Google 返す一次性授权码：授权通过后，Google 〜する〜するユーザー重定向回我们提前约定的 “回调 URL（Callback URL）”，并在 URL パラメータ中附带一个一次性、短期有效的授权码（而非直接返すユーザー信息，进一步提升セキュリティ性）。
4. Supabase 交换アクセス令牌（Access Token）：我们的バックエンド（由 Supabase 托管，无需自建）〜する拿着この授权码，向 Google 官方インターフェース发起リクエスト，换取可用于获取ユーザー信息的 Access Token（授权码仅用于换 Token，回避 Token 直接在フロントエンド传输）。
5. 作成账户并建立〜する话：Supabase 使用 Access Token 从 Google 拉取ユーザー的公开信息（如邮箱、头像），并在我们的プロジェクト中为该ユーザー自動作成账户（若首次ログイン）或直接关联现有账户，最終的に生成一个有效的ユーザー〜する话（Session），完了ログイン。

![](/zh-cn/stage-2/backend/database-supabase/images/image49.png)

### 5.2.2 設定 Google Cloud 获取 Client ID 和 Secret

无论是何种第三方ログイン方式，我们通常都必〜するがある获取 Client ID 与 Secret 进行設定；对于 Google 的第三方ログイン，你まず必〜するがある在 Google Cloud Platform 中作成一个 OAuth 2.0 クライアント ID 进行对应パラメータ的获取。

1. **进入 Google Cloud Console** :
2. アクセス [Google Cloud Console]( PROT185 )。
3. 作成一个新プロジェクト或選択一个现有プロジェクト。
4. **設定 OAuth 同意屏幕 (OAuth consent screen)** :
5. 在左侧导航栏中，找到 “APIs & Services” -> “OAuth consent screen”。
6. 選択 “External” ユーザー类型，次にクリック “Create”。
7. 填写アプリケーション名称、ユーザーサポート电子邮件等必填信息。
8. 在 “Authorized domains” 部分，追加你的 Supabase プロジェクト域名，格式为 `*.supabase.co`。
9. 保存并继续。在 “Scopes” 和 “Test users” ステップ中，できます暂时跳过，直接保存。
10. **作成凭据 (Create Credentials)** :
11. 进入 “APIs & Services” -> “Credentials”。
12. クリック “+ CREATE CREDENTIALS”，選択 “OAuth client ID”。
13. 在 “Application type” 中選択 “Web application”。
14. 为它取一个名字，例えば “Supabase Auth”。
15. 在 “Authorized redirect URIs” 部分，クリック “ADD URI”，并填入你的 Supabase プロジェクト的回调 URL。できます在 Supabase Dashboard 的 “Authentication” -> “Providers” -> “Google” 中找到この URL，它的格式通常是 `https://<你的项目ID>.supabase.co/auth/v1/callback`。
    ![](/zh-cn/stage-2/backend/database-supabase/images/image50.png)
16. クリック “CREATE”。
17. **获取 Client ID 和 Client Secret** :
18. 作成成功后，一个弹窗〜する表示你的 **Client ID** 和 **Client Secret** 。请务必**立即コピー并妥善保存** 它们。

### 5.2.3 設定 GitHub 获取 Client ID 和 Secret

同样地，你也必〜するがある在 GitHub 上新規登録一个 OAuth アプリケーション。

1. **进入 \*\***GitHub\*\* ** Developer Settings** :
   1. ログイン你的 GitHub 账户。
   2. クリック右上角的头像，进入 “Settings”。
   3. 在左侧导航栏的底部，找到 “Developer settings”。

2. **新規登録新アプリケーション (Register a new application)** :
3. 選択 “OAuth Apps”，次にクリック “New OAuth App”。
4. 填写アプリケーション名称，例えば “My Burger Shop”。
5. **Homepage URL** : 填写你アプリケーション的线上地址，或者本地開発地址 `http://localhost:3000`。
6. **Authorization \*\***callback\*\* ** URL** : 填入你的 Supabase プロジェクト的回调 URL。同样，できます在 Supabase Dashboard 的 “Authentication” -> “Providers” -> “GitHub” 中找到它，格式为 `https://<你的项目ID>.supabase.co/auth/v1/callback`。
7. クリック “Register application”。
8. **获取 Client ID 和 Client Secret** :
9. 新規登録成功后，ページ〜する表示你的 **Client ID** 。
   ![](/zh-cn/stage-2/backend/database-supabase/images/image51.png)
10. クリック “Generate a new client secret” 来生成你的 **Client Secret** 。同样，请**立即コピー并保存** 它。

### 5.2.4 在 Supabase 中設定 Provider

现在，〜する我们获取到的凭证設定到 Supabase 中。

1. **进入 Supabase Dashboard** :
2. 選択你的プロジェクト，进入 “Authentication” -> “Providers”。
3. **启用并設定 Google** :
4. 找到 “Google” 并启用它。
5. 〜する你从 Google Cloud 获取的 **Client ID** 和 **Client Secret** 貼り付け到对应的入力框中。
6. クリック “Save”。
7. **启用并設定 ** **GitHub** :
   1. 找到 “GitHub” 并启用它。
   2. 〜する你从 GitHub 获取的 **Client ID** 和 **Client Secret** 貼り付け到对应的入力框中。
   3. クリック “Save”。

![](/zh-cn/stage-2/backend/database-supabase/images/image52.png)

至此，你すでに〜できる够使用第三方账户在構築的网站中进行ログイン，できます直接让 AI 基于 `Project5-Supabase-Demos/apps/project-burger-shop-auth-advanced-supabase-6`プロジェクト作为参考，在你的プロジェクト的基本的な上サポートユーザーログインシステム，以最小成本集成含む github 与 google 鉴权的ユーザーログイン界面。

### 5.2.6 密码重置実装

作为一个成熟的ユーザーログインコンポーネント，密码重置也是極めて重〜するなな一环，本プロジェクト `project-burger-shop-auth-advanced-supabase-6`也含む了该機〜できる的完全な実装，できます直接让 AI 基于本プロジェクト的密码重置機〜できる复刻完全な密码重置コンポーネント。其主な分为以下几步：

1. 发起リクエスト ：ユーザー在忘记密码ページ入力邮箱，フロントエンド调用 `supabase.auth.resetPasswordForEmail()` 函数，并指定一个重定向 redirectTo URL（例えば /auth/reset ）。
2. 送信邮件 ：Supabase 〜する向该邮箱送信一封含む唯一重置リンク的邮件。
3. アクセスリンク ：ユーザークリック邮件中的リンク，被重定向到アプリケーション内指定的重置ページ。
4. 更新密码 ：在重置ページ，ユーザー入力新密码。フロントエンド调用 `supabase.auth.updateUser()` ，〜する新密码提出给 Supabase。Supabase 〜する自動検証リンク的有效性并完了密码更新。

最後に，如果你觉得当前的密码重置邮件过于简陋，できます 在 Supabase Dashboard 的 Authentication -> Email Templates 中自定义“Reset Password”邮件テンプレート。

除了 Reset password 機〜できる外，你还〜できる看到许多その他与ユーザー管理相关的高度な機〜できる设定（例えば Invite user 等），你可根据对应機〜できる各自的開発ドキュメント，结合 Vibe coding ツール自行追加对应機〜できる。

![](/zh-cn/stage-2/backend/database-supabase/images/image53.png)

## 5.3 リアルタイム機〜できる

Supabase 的リアルタイム機〜できる是其最強力な特性之一，为構築协作ドキュメント、リアルタイム仪表盘、游戏大厅或客服システム提供了极大的便利。

本プロジェクト `Project5-Supabase-Demos/apps/project-burger-shop-realtime-orders-3 `通过構築一个 多人リアルタイムチャット室、光标位置共享 機〜できる，表示了 Supabase Realtime 涉及到的三大コア〜できる力：データベース变更监听 (Postgres Changes)、广播 (Broadcast) 和 在线ステータス (Presence)。

![](/zh-cn/stage-2/backend/database-supabase/images/image54.png)

如果你觉得相关コード部分有一定难度，〜できる直接让 AI 参考该部分ドキュメント内容，对你的程序进行変更。

### 5.3.1 データベースリアルタイム变动 Postgres Changes

最よくある Realtime 機〜できる是对データベース的变更进行リアルタイム监听 Postgres Changes 。它許可クライアントサブスクリプションデータベース中特定表、特定行甚至特定列的 INSERT 、 UPDATE 或 DELETE 事件。一旦データベース发生变动（无论是通过 API 调用、Supabase Dashboard 操作，还是 SQL 脚本実行），Supabase 都〜する利用 PostgreSQL 的底层コピー机制，立即通过 WebSocket 〜する变更的数据推送到すべてのサブスクリプション了该频道的フロントエンドクライアント，而无需フロントエンド通过轮询（Polling）去反复查询。

一般的に而言，该機〜できる〜できる在 Table Editor 中找到 Enable Realtime クリック后起動， 但更方便的是通过 SQL 脚本初始化実行，例えば：

```sql
-- Enable realtime replication
ALTER TABLE public.chat_messages REPLICA IDENTITY FULL;
DO $$
BEGIN
  IF NOT EXISTS (
    SELECT 1 FROM pg_publication_tables
    WHERE pubname = 'supabase_realtime'
      AND schemaname = 'public'
      AND tablename = 'chat_messages'
  ) THEN
    ALTER PUBLICATION supabase_realtime ADD TABLE public.chat_messages;
  END IF;
END $$;
```

该语句〜する `chat_messages` 表追加到了 Supabase 预设的 `supabase_realtime` 中，而一旦一个表被加入到この特殊的 `publication` 中，Supabase 的リアルタイムサーバー就〜する开始监听它的すべての数据变更。

基于上的特殊数据表，我们〜できる够使用监听コード对表内数据变动进行リアルタイム监听。必〜するがあります実装的是当一个ユーザー送信消息时，その他すべての在线ユーザー都〜できる立刻在屏幕上看到这条消息。通过サブスクリプション chat_messages 表的 INSERT 事件〜できる够実装这一点。

```typescript
    const sub = supabase
      .channel('chat_messages_channel')
      .on('postgres_changes', {
        event: 'INSERT',
        schema: 'public',
        table: 'chat_messages'
      }, (payload: any) => {
        console.log('New message received:', payload.new);
        const newMessage = payload.new as Message;
        // ... //
      .subscribe((status: string) => {
        console.log('Chat subscription status:', status);
      });
```

- `.channel('chat_messages_channel')`: 作成一个隔离的通信频道。
- `.on('postgres_changes', ...)`: 这是コアとなるサブスクリプション方法。我们告诉 Supabase 我们只关心 `chat_messages` 表的 `INSERT` 事件。
- `payload.new`: 当有新消息被插入データベース时，Supabase 〜する〜する这条新数据的完全な内容通过 `payload.new` 推送给すべてのサブスクリプション的クライアント。
- `.subscribe()`: 起動サブスクリプション。

### 5.3.2 信息广播同步 Broadcast & Presence

对于それらの不必〜するがある存入データベース的、更“即时”的交互，例えば光标移动、在线ステータス等，Supabase 提供了 Broadcast 和 Presence 機〜できる。

- Presence: 用于跟踪频道内すべてのクライアント的 **共享ステータス** 。适合用来実装“誰在线”的機〜できる。
- Broadcast: 用于向频道内的すべてのその他クライアント送信**低遅延**的 **临时消息** 。

Presence 的コア思想是： 让各クライアント声明自分で的在线ステータス，并由 Supabase 的サーバー负责〜するこれらのステータス可靠地同步给频道内的すべてのその他クライアント。実装 Presence 分为以下几个重〜するななステップ：

1. 作成一个サポート Presence 的频道

まず，我们作成了一个频道 `lobby_presence` 来专门処理これらの交互，并在設定中指定一个唯一的 key 来标识当前ユーザー。この key 通常是ユーザー的 ID。

```
const ch = supabase.channel
('lobby_presence', {
  config: {
    presence: { key: anonymousUser.id },
  }
});
```

2. サブスクリプション频道宣告“我在线”的信息

一旦频道作成成功，必〜するがありますサブスクリプション它。在サブスクリプション成功的回调（ status === 'SUBSCRIBED' ）中，我们调用 channel.track() 方法。この方法〜する〜する当前ユーザー的信息（例えばユーザーID、名称、头像颜色等）广播给频道内的すべてのその他クライアント，宣告自分で的“在线”ステータス。

```
const me = {
  id: anonymousUser.id,
  name: anonymousUser.name,
  color: anonymousUser.color
};

ch.subscribe(async (status) => {
  if (status === 'SUBSCRIBED') {
    await ch.track(me);
  }
});
```

3. 同步完全な在线列表

当一个新ユーザー加入频道时，他们必〜するがある获取当前すべてのすでに在线的ユーザー列表。这通过监听 presence 的 sync 事件来実装。 sync 事件〜する在你首次加入频道时触发，为你提供一个完全な“快照”。

channel.presenceState() 方法〜する返す一个对象，含む了当前频道内すべての在线ユーザー的ステータス信息。します其処理后更新到アプリケーション的 state 中，从而渲染出完全な在线ユーザー列表。

```
ch.on('presence', { event: 'sync' }, () 
=> {
  const state = ch.presenceState();
  const flat = {};
  Object.values(state).forEach((arr) => {
    arr.forEach((u) => { flat[u.id] = 
    { ...u }; });
  });
  setOnline(flat);
});
```

4. 监听单个ユーザー的加入与离开

除了 sync 事件，我们还〜できる监听 join 和 leave 事件，以便在有新ユーザー进入或离开时做出即时レスポンス，例えば表示一个 "User has joined" 的通知。

```
ch.on('presence', { event: 'join' }, ({ 
key, newPresences }) => {
  console.log('User joined:', key, 
  newPresences);
});

ch.on('presence', { event: 'leave' }, ({ 
key, leftPresences }) => {
  console.log('User left:', key, 
  leftPresences);
});
```

通过以上ステップ，我们便構築了一个機〜できる完备的在线ステータスシステム。Supabase 自動処理了ユーザー意外断开接続（如关闭ブラウザ或断网）的情况，并在适当的时候触发 leave 事件，確保了在线列表的准确性。

当 Presence 让我们知道了“誰在场”後に， Broadcast 〜できる够让他们間の〜できる够进行“〜する話”，但〜する話的内容是短暂存储的。一个典型的例子であるリアルタイム光标追踪。如果每次鼠标移动都去读写データベース，〜する造成巨大的性〜できる浪费和遅延。 Broadcast 完美地解決了この問題，它許可消息在各个クライアント間の直接通过 WebSocket 传递，完全绕过データベース。

Broadcast 的工作模式主な依赖两个コア方法： channel.send() 用于送信，channel.on() 用于受信。】

1. 送信端：广播我的光标位置

我们为 mousemove 事件追加了一个监听器。当鼠标移动时，我们构造一个含むユーザー ID、坐标和颜色的 payload，次に通过 channel.send() 〜する其广播出去，并指定事件名称为 'cursor'。

```typescript
const handleMouseMove = (e) => {
  const payload = {
    id: anonymousUser.id,
    x: e.clientX,
    y: e.clientY,
    name: anonymousUser.name,
    color: anonymousUser.color
  };

  channelRef.current?.send({
    type: 'broadcast',
    event: 'cursor',
    payload
  });
};

document.addEventListener('mousemove', handleMouseMove);
```

2. 受信端：监听并渲染他人的光标

在同一个频道内，すべてのクライアント都使用 channel.on() 来监听 broadcast 类型的、且 event 为 'cursor' 的消息。一旦收到匹配的消息，回调函数就〜する被触发。我们从 payload 中解析出送信方的数据，并用它来更新本地的 online ステータス，从而在屏幕上リアルタイム渲染出その他ユーザー光标的位置。

```typescript
ch.on('broadcast', { event: 'cursor' }, ({ payload }) => {
  setOnline((prev) => ({
    ...prev,
    [payload.id]: {
      ...(prev[payload.id] || {}),
      x: payload.x,
      y: payload.y
    }
  }));
});
```

この方法で， Presence 和 Broadcast 协同工作；Presence メンテナンス在线ユーザー列表，而 Broadcast 则负责在これらのユーザー間の传递像光标位置这样的临时ステータス，最終的に以较低的成本実装了豊富なリアルタイム互动機〜できる。

## 5.4 存储

除了ユーザー信息、オーダー这类可规整定义的结构化数据，一个完全なアプリケーション通常还必〜するがある処理大量非结构化ファイル —— 例えばユーザー头像、商品表示图、ユーザーアップロード的オーダードキュメント等。这类ファイル的特点是体积差异大、数量可〜できる性がある极多（例えば电商プラットフォーム的商品图可〜できる性がある达数万甚至数十万张），若直接存储在アプリケーション自身的业务サーバー中，〜する显著増加サーバー的存储负载，还可〜できる性がある拖慢数据读写速度，影响アプリケーション整体性〜できる。

实际開発中，这类非结构化ファイル〜する统一交由 “对象存储サービス” 管理，OSS、Amazon S3 均属于这类サービス，它们是专门为海量ファイル存储設計的 “专业存储ツール”，〜できる高效应对ファイル的存储、备份与快速读取〜する件。而我们在アプリケーション中获取これらのファイル时，并不〜する直接从对象存储サービス的 “底层リポジトリ” 调取，而是通过 URL 地址実装：各存储在对象存储中的ファイル，都〜する被分配一个唯一的 URL（類似 “[https://xxx.oss.com/avatar/user123.jpg]( PROT187 )” 的地址，可シンプルな理解为この“网站”只有一张图片），この URL 就像ファイル的 “专属アクセス地址”，フロントエンドページ只需通过该地址，就〜できる直接ダウンロード或加载头像、商品图，无需依赖アプリケーション业务サーバー中转，既提升了ファイル加载速度，也减轻了业务サーバー的压力。

本プロジェクト `project-burger-shop-storage-uploads-4` 便通过一个ユーザー头像アップロード機〜できる，深入演示了どのように利用 Supabase Storage 構築现代化的ファイルアップロードシステム，让開発者直观理解非结构化ファイル从アップロード到通过 URL アクセス的完全なプロセス。さらに，本プロジェクト使用 `Uppy` 库来提供一个优秀的ファイルアップロード界面，并结合 `Tus` 插件実装了可续传アップロード，通过〜する Uppy 的アップロード端点指向 Supabase 的標準 API (`<supabaseUrl>/storage/v1/upload/resumable`) 进行工作，できます参考類似的方式実装アップロード機〜できるコンポーネント。

![](/zh-cn/stage-2/backend/database-supabase/images/image55.png)

![](/zh-cn/stage-2/backend/database-supabase/images/image56.png)

### 5.4.1. 存储桶

Supabase Storage 的组成单元是存储桶 Bucket。できます把它想象成电脑操作システム中的ファイル夹。各 Bucket 都〜できる有自分で独立したセキュリティ戦略和設定。

Storage 内的すべてのファイル都〜できる通过一个公开的 URL 直接アクセス，但并不意味着すべての人都〜できる随意アップロード或変更，具体的なアクセス権限〜する由更精细的戦略来控制。和データベース一样，Storage 的アクセス権限也是通过行级セキュリティ戦略来管理的。SQL 戦略写在 storage.objects 和 storage.buckets 这两张特殊表上，〜できる精确定义誰〜できる读取 (SELECT)、アップロード (INSERT)、更新 (UPDATE) 或削除 (DELETE) ファイル。

例えば，できます作成一条戦略，只許可ユーザーアップロード到以自分で user_id 命名的ファイル夹下，并且只〜できるアップロード图片类型的ファイル：

```
CREATE POLICY "Allow authenticated 
uploads to avatars bucket"
ON storage.objects FOR INSERT
TO authenticated
WITH CHECK (
  bucket_id = 'avatars' AND
  auth.uid() = (storage.foldername(name))
  [1]::uuid AND
  (storage.extension(name) IN ('png', 
  'jpg', 'jpeg'))
);

CREATE POLICY "Allow public read access 
to avatars"
ON storage.objects FOR SELECT
USING ( bucket_id = 'avatars' );
```

### 5.4.2 获取可アクセスファイル URL

本プロジェクト必〜するがある你手動で作成一个名为 avatars 的公共桶，すべてのファイル〜するアップロード至该公共桶下进行存储。ファイルアップロード成功后，我们只得到了它在 Storage 中的存储パス ，例えば public/avatar1.png 。这只是存储在データベース中的一个字符串，〜する让ブラウザ〜できる够渲染这张图片，必〜するがあります〜する其转换为一个可アクセス的 HTTP URL。

Supabase 提供了两种截然異なる的戦略来获取この URL，它们在セキュリティ性、持久性和成本控制上有着本质的区别。

#### 1. 公开 URL (Public URL) - 永久リンク

这是最直接的方式。如果你的ファイル存放在一个**Public Bucket** 中，できます获取一个固定、永久的公开リンク。

```typescript
const { data } = supabase.storage
  .from('avatars')
  .getPublicUrl('public/avatar1.png');
const publicUrl = data.publicUrl;
```

这类リンク具有两大コア特点：一是シンプルな直接，其 URL 结构固定，在实际操作中易于拼接和管理，削減了技术使用门槛；二是利于缓存，作为永久リンク，它〜できる被 CDN（内容分发网络）和ブラウザ有效缓存，从而大幅提升资源的アクセス速度，最適化ユーザー体验。基于これらの特点，它适用于真正意义上的公共资源シナリオ，例えば网站 Logo、产品ディレクトリ图片、博客文章配图等，〜できる很好地满足这类资源的アクセス和管理〜する件。

ただし本番環境では中，这类リンク存在明显的被盗刷流量（Hotlinking）风险。由于リンク是永久公开的，外部人员〜できる轻易〜する你的图片リンクエンベディング到他们自分で的高流量网站中，导致流量被非法占用。这一行为〜する让你的 Supabase プロジェクト产生大量不必〜する的流量费用，而これらの消耗的流量并未サービス于你自身的アプリケーション，属于典型的成本浪费，是生产環境中必〜するがある高度警惕和防范的問題；したがって，必〜するがあります转向临时签名 URL 実装对外资源的暴露。

#### 2. 签名 URL (Signed URL) - 临时授权リンク

を解決するため公开 URL 的セキュリティ和成本問題，Supabase 提供了生成临时签名 URL 的方式。这是绝大多数线上アプリケーションお勧め的ベストプラクティス，例えば文生图アプリケーション给ユーザー生成限时確認的图片リンク、电商プラットフォーム仅让下单ユーザー获取临时发票ダウンロード地址、有料内容プラットフォーム为サブスクリプションユーザー提供短期有效的レッスン播放リンク，既防ファイル盗用又〜できる回避流量盗刷，适配性极强。

```typescript
const { data, error } = await supabase.storage
  .from('avatars')
  .createSignedUrl('private/user-invoice.pdf', 3600); // 链接有效期为 3600 秒 (1小时)
const signedUrl = data?.signedUrl;
```

临时签名 URL（Signed URL）有三大コア优势：セキュリティ可控是指リンク带セキュリティ标记、有有效期，过期就用不了；権限绑定很シンプルな —— 只有〜できる看这ファイル的人，才〜できる生成このリンク，就算ファイル藏在私有存储里（Private Bucket），他用このリンク也〜できる正常打开；杜绝盗刷是なぜならリンク是临时的，コピー到别处很快就失效，不〜する被恶意刷流量。靠これらの优势，像ユーザー头像、私人照片、有料内容、オーダー发票これらの必〜するがある管権限的ファイル，都〜できる用它。

从セキュリティ保障和成本控制的角度，提案养成优先使用临时签名 URL 的习惯。只有当某个资源明确必〜するがある永久公开、无限制アクセス（例えばアプリケーション的公开 Logo、公共活动宣传图等）时，才考虑使用 Public URL。这样既〜できる满足特定业务〜する件，又〜できる最大程度规避不必〜する的风险和成本消耗。

## 5.5 边缘函数

Edge Function 是 Serverless（无サーバー架构）生态中极具コア价值的形态之一，它为 “无自建バックエンド” シナリオ提供了轻量、効率的な函数実行サポート。

何是 Serverless？ Serverless（无サーバー架构）并不意味着真的没有サーバー，而是指開発者无需关心サーバー的购买、运维、設定和扩容 。你只必〜するがある编写业务コード（函数），云サービス商〜する在特定事件触发时自動为你分配资源実行コード，并按实际実行时间计费。

当你的アプリケーション必〜するがある実行いくつかの不〜できる或不应在クライアント（ブラウザ）上完了的逻辑时——例えば与必〜するがある私密キー的第三方 API 交互、実行计算密集型タスク、或强制実行複雑な业务ルール——Edge Functions 就派上了用场。Supabase Edge Functions 基于 Deno 和 TypeScript，它们被デプロイ在全球的边缘节点上，物理距离上靠近你的ユーザー，从而提供极低的函数実行遅延。

目前主流云厂商都推出了各自的 Edge Function サービス，よくある包括：

- AWS Lambda@Edge：基于 AWS Lambda 延伸的边缘函数サービス，可与 CloudFront CDN 联动，サポート Node.js、Python 等语言；
- Cloudflare Workers：Cloudflare 推出的边缘函数，デプロイ在其全球 275+ 边缘节点，サポート JavaScript/TypeScript，以 “毫秒级遅延” 为コア优势；
- Vercel Edge Functions：适配 Vercel フロントエンドプロジェクト的边缘函数，与 Next.js 深度集成，サポート TypeScript，主打 “フロントエンド与边缘逻辑无缝衔接”；

回到 Supabase ，当你的アプリケーション必〜するがある実行 “不〜できる在クライアント（ブラウザ）完了” 的逻辑时，例えば用私密キー调用第三方 API（如 LLM インターフェース）、処理计算密集型タスク（如图片压缩）、或强制実行権限校验（如ファイルアクセスルール）时，Supabase Edge Functions 就〜できる发挥作用。它基于 Deno runtime 和 TypeScript 構築，デプロイ在全球边缘节点上，〜できる以 “靠近ユーザー的物理距离” 実装极低的実行遅延，是编写自定义、可信サーバー端逻辑的コアツール。

本プロジェクト `Project5-Supabase-Demos/apps/project-burger-shop-edge-function-5`通过一个与大規模言語モデル（LLM）リアルタイム流式〜する話的機〜できる，表示了 Edge Functions 的最简アプリケーションプロセス。

![](/zh-cn/stage-2/backend/database-supabase/images/image57.png)

### 5.5.1 LLM Chat 案例解析

假设你想在アプリケーション中集成一个類似 ChatGPT 的チャット机器人。必〜するがあります在サーバー端调用 OpenAI 的 API，但这必〜するがある一个私密的 API Key。 この Key 绝对不〜できる暴露在フロントエンドコード中 ，否则すべての人都〜できる通过確認网页源码盗用你的 Key，产生高昂的费用。这正是 Edge Function 的用武之地。します作成一个名为 llm-chat 的函数，它充当了フロントエンド和 OpenAI API 間の的一个 セキュリティ代理 。

参考 `project-burger-shop-edge-function-5/scripts/llm-chat.ts`的コード，我们来看看它是どのように工作的：

```typescript
// scripts/llm-chat.ts
import "jsr:@supabase/functions-js/edge-runtime.d.ts";
import { OpenAI } from "npm:openai";

const OPENAI_API_KEY = Deno.env.get("OPENAI_API_KEY");

Deno.serve(async (req) => {
  try {
    const openai = new OpenAI({ apiKey: OPENAI_API_KEY });
    const { prompt } = await req.json();

    const stream = await openai.chat.completions.create({
      model: "gpt-3.5-turbo",
      messages: [{ role: "user", content: prompt }],
      stream: true,
    });

    return new Response(stream.toReadableStream(), {
      headers: { "Content-Type": "text/event-stream" },
    });
  } catch (err) {
  }
});
```

在该案例中，对于キーセキュリティ，OPENAI_API_KEY 作为環境変数被セキュリティ存储于 Supabase 的サーバー。本地フロントエンドコード完全无法接触到该キー，从而有效保障了キー的セキュリティ性。

### 5.5.2 作成并デプロイ函数

Supabase 提供了非常に友好的界面，让你无需接触コマンド行即可完了デプロイ。

1. **进入 Edge Functions 面板** :
2. ログイン你的 Supabase プロジェクト Dashboard。
3. 在左侧导航栏中，クリック像コード一样的图标，进入 “Edge Functions”。
4. **作成新函数** :
5. クリック “Create a new function” 按钮。
   ![](/zh-cn/stage-2/backend/database-supabase/images/image58.png)
6. 为函数命名，例えば `llm-chat`。
7. **貼り付けコード** :
   ![](/zh-cn/stage-2/backend/database-supabase/images/image59.png)
8. 在弹出的在线編集器中， **削除すべてのデフォルトの占位コード** 。
9. 打开你本地的 `llm-chat.ts` ファイル， **コピー其全部内容** 。
10. 〜するコピー的コード**貼り付け**到 Supabase 的在线編集器中。
11. **設定\*\***環境変数\*\* ** (Secrets)** :
    1. 在侧边栏找到 Secrets。
       ![](/zh-cn/stage-2/backend/database-supabase/images/image60.png)
    2. Name: 入力 `OPENAI_API_KEY`。
    3. Value: 貼り付け你自分で的 OpenAI API Key。
    4. クリック “Save”。在ここ设置的 Secret 〜する被加密存储，并セキュリティ地注入到你的函数実行时環境中。

若有函数必〜するがある更新，记得在 Edge Function 部分実行 Deploy updates。Supabase 〜する在云端为你構築并デプロイこの函数。几分钟后，你的函数就〜できる在线アクセス。

除了作为语言モデル的セキュリティ代理，Edge Functions 的アプリケーションシナリオ远不止于此。実際には，すべての必〜するがあるサーバー端逻辑処理的タスク，无论是シンプルな API 调用、数据検証，还是更複雑な计算，都〜できる通过 Edge Function 実装。它为你提供了一个轻量级、可拡張的バックエンド，而无需管理すべてのサーバー基本的な设施。

如果你想探索更多可〜できる性がある性，〜できる参考プロジェクト中的その他例。例えば：

- 图片生成 ( txt2img.ts ) : この函数表示了どのように利用 Edge Function 调用第三方的文生图（Text-to-Image）API（如 Stability AI, Midjourney 等）来动态生成图片。这是1 つの典型的计算密集型或必〜するがあるセキュリティ调用外部サービス的シナリオ。与 llm-chat 案例一样，API キー被セキュリティ地存储在 Supabase バックエンド，フロントエンド只负责送信文本説明，次に受信并表示生成的图片，全体の过程セキュリティ、高效。
- 送信邮件 ( send-email.ts ) : 在アプリケーション中送信欢迎邮件、交易通知或密码重置邮件是一般的な〜する件。 send-email.ts 例演示了どのように通过 Edge Function 集成邮件サービス（如 Resend, SendGrid）。你无需在クライアントコード中暴露敏感的邮件サービス API Key，只需作成一个函数，让フロントエンド通过调用この函数来触发邮件送信。

## 5.6 Clerk ログイン

Clerk 是一款专注于身份认证与ユーザー管理的专业開発ツール，コア〜できる力覆盖ユーザー新規登録、ログイン、账号セキュリティMFA、権限控制、〜する话管理等全链路身份认证相关〜する件，〜できる帮助開発者快速搭建セキュリティ、灵活且符合现代アプリケーション標準的なユーザー体系，无需从零開発複雑な身份逻辑。

本部分〜する介绍どのように从零开始設定 Clerk サービス，并〜する其与 Supabase 进行整合。できます在プロジェクト `project-burger-shop-auth-advanced-clerk-7` 中体验全プロセス。

![](/zh-cn/stage-2/backend/database-supabase/images/image61.png)

### 5.6.1 作成 Clerk アプリケーション与获取キー

在使用本プロジェクト前に，必〜するがあります拥有一个 Clerk 账号并作成一个アプリケーション。

1. 新規登録与作成:
   1. アクセス [dashboard.clerk.com]( PROT188 ) 并新規登録账号。
   2. クリック "Create application" 。
      ![](/zh-cn/stage-2/backend/database-supabase/images/image62.png)
   3. 入力アプリケーション名称（例えば "Burger Shop"）。
   4. 在 "How will your users sign in?" 中，デフォルト勾选 Email , Google , GitHub 。
   5. クリック Create application 。
2. 获取 API Keys:
   1. 作成成功后，你〜する被引导至 API Keys ページ。
      ![](/zh-cn/stage-2/backend/database-supabase/images/image63.png)
   2. 找到 Publishable key (以 `pk_` 开头) 和 Secret key (以 `sk_` 开头)。
      ![](/zh-cn/stage-2/backend/database-supabase/images/image64.png)
   3. 〜する它们コピー到你的 `.env.local` ファイル中（参考本プロジェクト `.env.example`）：

      ```bash
      NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_test_...
      CLERK_SECRET_KEY=sk_test_...
      ```

### 5.6.2 設定 Supabase 和 Clerk 的原生集成

在进一步使用前，必〜するがあります集成 Supabase 与 Clerk 的关联关系，方便後にログイン的鉴权リダイレクト以及控制对特定データベース的アクセス権限。Supabase 与 Clerk 提供官方原生集成〜できる力，通过该集成可快速実装两者的身份认证打通，无需手動で設定複雑な适配逻辑，大幅簡素化ユーザーログイン、権限校验等機〜できる的開発プロセス：

1. 在 Clerk 中激活对 Supab ase 的官方集成
   1. ログイン [Clerk Dashboard]( PROT189 )。
   2. 在左侧菜单导航至 Integrations (集成)。
   3. 在列表中找到并クリック Supabase。
   4. 开启 Enable Supabase 开关（或クリック Activate integration）。
   5. 重〜するななステップ：激活成功后，ページ〜する表示你的 Clerk Domain（格式通常为 `https://<your-id>.clerk.accounts.dev` 或你的自定义域名）。请コピーこの Domain 地址，次のステップ〜する用到。
2. 在 Supabase 中追加 Clerk 提供商
   1. ログイン [Supabase Dashboard]( PROT190 ) 并进入你的プロジェクト。
   2. 在左侧菜单导航至 Authentication > Sign In / Up (或者直接クリック Providers)。
   3. クリック Add provider 按钮，从下拉列表中選択 Clerk。
   4. 在弹出的 Clerk Domain 入力框中，貼り付け你刚才从 Clerk コピー的 Domain 地址。
   5. クリック Save 保存設定。

### 5.6.3 通过 Webhook 同步ユーザー数据至 Supabase

仅仅是集成只满足了鉴定権限的〜する件，但这并不〜する〜する Clerk 中すでに新規登録的ユーザー信息同步到 Supabase，を容易にするため管理，我们还必〜するがある在 Supabase 的 `public.users` 表中保留一份ユーザー备份，以便进行关联查询或数据分析。できます通过 Clerk Webhooks 実装这一機〜できる，完全な过程如下：

1. **Clerk 送信通知** : 当ユーザー在 Clerk 新規登録或更新资料时，Clerk 〜する向我们設定的 Webhook URL 送信一个 POST リクエスト。
2. **Supabase 受信并写入** : Edge Function 受信リクエスト，検証签名（確保セキュリティ），次に〜するユーザー数据更新到 Supabase 的データベース表中。

始める前に，必〜するがあります設定同步信息所需的数据表：

```sql
-- File: init.sql

-- 1. Create `users` table for synced Clerk users
-- This table will store user data pushed from Clerk Webhooks.
CREATE TABLE public.users (
  id TEXT NOT NULL PRIMARY KEY, -- Corresponds to Clerk User ID
  email TEXT,
  first_name TEXT,
  last_name TEXT,
  image_url TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- 2. Enable Row Level Security (RLS) on the table
-- This is an important security measure to ensure users cannot access any data by default.
ALTER TABLE public.users ENABLE ROW LEVEL SECURITY;

-- 3. Create RLS policies
-- Policy 1: Allow authenticated users to read their own user info.
-- `auth.jwt()->>'sub'` extracts the user ID from the JWT provided by Clerk.
CREATE POLICY "Authenticated users can view their own user record"
ON public.users FOR SELECT
TO authenticated
USING ( (SELECT auth.jwt()->>'sub') = id );

-- Policy 2: Allow users to update their own info.
CREATE POLICY "Authenticated users can update their own user record"
ON public.users FOR UPDATE
TO authenticated
USING ( (SELECT auth.jwt()->>'sub') = id );
```

以及在 Supabase 中启用对应的 Edge function：

```JavaScript
// File path: supabase/functions/clerk-webhooks/index.ts

import { serve } from 'https://deno.land/std@0.177.0/http/server.ts'
import { Webhook } from 'npm:svix'
import { createClient } from 'https://esm.sh/@supabase/supabase-js@2'

// Get Clerk Webhook signing secret from environment variables
const CLERK_WEBHOOK_SECRET = Deno.env.get('CLERK_WEBHOOK_SECRET')

if (!CLERK_WEBHOOK_SECRET) {
  throw new Error('CLERK_WEBHOOK_SECRET is not set in environment variables')
}
const supabaseAdmin = createClient(
  Deno.env.get('SUPABASE_URL')!,
  Deno.env.get('SUPABASE_SERVICE_ROLE_KEY')!
)

serve(async (req) => {
  try {
    // 1. Get Svix signature info from request headers
    const headers = Object.fromEntries(req.headers)
    const svix_id = headers['svix-id']
    const svix_timestamp = headers['svix-timestamp']
    const svix_signature = headers['svix-signature']

    if (!svix_id || !svix_timestamp || !svix_signature) {
      return new Response('Missing Svix headers', { status: 400 })
    }

    const payload = await req.json()
    const body = JSON.stringify(payload)

    // 2. Verify Webhook signature validity using the secret
    const wh = new Webhook(CLERK_WEBHOOK_SECRET)
    const evt = wh.verify(body, {
      'svix-id': svix_id,
      'svix-timestamp': svix_timestamp,
      'svix-signature': svix_signature,
    })

    const { id } = evt.data
    const eventType = evt.type
    console.log(`Received webhook event: ${eventType} for user: ${id}`)

    // 3. Execute database operations based on event type
    switch (eventType) {
      case 'user.created': {
        const { id, first_name, last_name, image_url, email_addresses } = evt.data
        const { error } = await supabaseAdmin.from('users').insert({
          id,
          first_name,
          last_name,
          image_url,
          email: email_addresses[0]?.email_address,
        })
        if (error) throw error
        console.log(`User ${id} created in Supabase.`)
        break
      }

      case 'user.updated': {
        const { id, first_name, last_name, image_url, email_addresses } = evt.data
        const { error } = await supabaseAdmin
          .from('users')
          .update({
            first_name,
            last_name,
            image_url,
            email: email_addresses[0]?.email_address,
            updated_at: new Date().toISOString(), // Update timestamp
          })
          .eq('id', id)
        if (error) throw error
        console.log(`User ${id} updated in Supabase.`)
        break
      }

      case 'user.deleted': {
        // For delete events, ID might be at the top level
        const deletedId = id
        if (!deletedId) {
          return new Response('Deleted user ID not found', { status: 400 })
        }
        const { error } = await supabaseAdmin.from('users').delete().eq('id', deletedId)
        if (error) throw error
        console.log(`User ${deletedId} deleted from Supabase.`)
        break
      }
    }

    return new Response('Webhook processed successfully', { status: 200 })
  } catch (err) {
    console.error('Error processing webhook:', err.message)
    return new Response(`Webhook Error: ${err.message}`, { status: 400 })
  }
})
```

初始化 Supabase 数据表与函数结束后，你还必〜するがある在 Clerk 中启用 Webhooks サポート：

- 在 Clerk Dashboard -> **Webhooks** 中追加 Endpoint，填入Supabase Edge Function 的 URL。
- 勾选 `user.created`, `user.updated`, `user.deleted` 等事件。

![](/zh-cn/stage-2/backend/database-supabase/images/image65.png)

一旦设置成功，你〜できる够在 Message Attempts 中看到異なるリクエスト信息，クリック后可看到詳細なリクエスト返すパラメータ結果；如果 webhook 在リクエスト Edge function 时出现問題，できます快速在返す值中找到详细原因結果。お勧め你 对照 Clerk 和 Supabase 的リクエストログ信息，用于分析各个函数设定是否正确。

### 5.6.4 Clerk 中的第三方ログインサポート

在深入了解どのように对 Clerk サポート第三方ログイン前，我们先明确两个コア概念：開発環境与生产環境，这是软件从 “開発テスト” 到 “上线可用” 的两个重〜するなな阶段，二者的定位、用途和セキュリティ〜する求截然異なる：

- 開発環境：開発者本地或テストサーバー使用的環境，仅用于機〜できる開発、デバッグ和内部検証（如本地 localhost:3000 サービス），不对外开放
- 生产環境：アプリケーション正式上线后，面向真实ユーザー的公开環境（如デプロイ在 Vercel、阿里云等プラットフォーム的 https://my-app.com）

而 Clerk 对社交ログイン区分这两种環境，本质是平衡 “開発效率” 与 “生产セキュリティ”：開発阶段需減少冗余設定以快速検証機〜できる，生产阶段需通过专属凭证保障数据セキュリティ， 符合 Google、GitHub 等第三方 OAuth プラットフォーム的ルール（线上アプリケーション必須绑定专属域名与凭证，不許可使用共享资源）。下具体说明两种環境下 Clerk 社交ログイン的差异設定：

1. **開発環境快速検証**

開発環境中，Clerk 已预置共享 OAuth 凭证和デフォルト重定向 URI，无需前往 GitHub/Google 申请专属凭证，操作ステップ如下：

- ログイン Clerk Dashboard ，在左侧导航栏进入 SSO connections （SSO 接続）ページ。
- クリック Add connection （追加接続），選択 For all users （对すべてのユーザー生效）。
- 在 Choose provider （選択提供商）下拉菜单中，按需選択 GitHub 或 Google 。
- 直接クリック Add connection （追加接続），Clerk 〜する自動用共享凭证完了绑定。

  設定后，本地起動アプリケーション（如 `localhost:3000`）并クリック“Sign in with GitHub/Google”，Clerk 〜する自動代理ログインリクエスト，快速検証機〜できる是否正常。

2. **生产環境自定义凭证設定**

（注：もし有环节和预期不一致，提案阅读官方ドキュメント进行最新方式的尝试）

アプリケーションデプロイ上线（如 Vercel、阿里云）并切り替え到 Clerk Production Instance 后，共享凭证失效，需为 GitHub/Google 設定自定义 OAuth 凭证（提案 打开 Clerk Dashboard 和第三方プラットフォームページ，方便同步操作）：

- 前置通用操作（Clerk 控制台）：
  - 进入 Clerk SSO connections ページ，クリック Add connection → 選択 For all users 。
  - 選択目標プラットフォーム（GitHub/Google），確保开启 Enable for sign-up and sign-in （許可新規登録ログイン）和 Use custom credentials （使用自定义凭证）。
  - コピーページ中的 Authorization Callback URL （GitHub）或 Authorized Redirect URI （Google），保存到セキュリティ位置，不〜する关闭当前ページ/弹窗。
- 2.1 GitHub プラットフォーム設定：
  - ログイン GitHub，进入 Developer Settings （パス：头像 → Settings → Developer settings → OAuth Apps）。
  - クリック New OAuth app ，填写信息：`Application name`（アプリケーション名称）、`Homepage URL`（生产域名，如 `https://my-app.com`）、`Authorization Callback URL`（貼り付け从 Clerk コピー的地址）。
  - クリック Register application ，再クリック Generate a new client secret ，保存生成的 Client ID 和 Client Secret （Secret 仅表示一次）。
  - 回到 Clerk 弹窗，貼り付け Client ID 和 Client Secret，クリック Add connection 完了設定（若关闭弹窗，可在 SSO connections 找到 GitHub 接続，在“Use custom credentials”モジュール补填）。
- 2.2 Google プラットフォーム設定：
  - ログイン Google Cloud Console ，選択已有プロジェクト或新建プロジェクト（如“My App Production”）。
  - クリック左上角菜单 → APIs & Services → Credentials ，クリック Create Credentials → OAuth client ID （首次設定需先完了 OAuth consent screen 设置，選択“External”并填写アプリケーション信息）。
  - 選択 Application type 为 Web application ，設定：
    1. `Authorized JavaScript origins`：追加生产域名（如 `https://my-app.com`、`https://www.my-app.com`），本地検証可补充 `http://localhost:端口号`。
    2. `Authorized Redirect URIs`：貼り付け从 Clerk コピー的地址。
  - クリック Create ，保存弹窗中的 Client ID 和 Client Secret ，回到 Clerk 弹窗貼り付け并クリック Add connection 。
  - 重〜するなな注意事项：
    1. 禁止 WebView ログイン：Google OAuth 不サポートアプリケーション内ブラウザログイン，需参考 [Google 官方ドキュメント]( PROT192 ) 调整。
    2. 切り替え公開ステータス：デフォルト“Testing”ステータス仅サポート 100 个テストユーザー，需在 OAuth consent screen 〜する“Publishing status”改为 In production （需通过 Google 审核）。
    3. 阻止子邮箱：Clerk デフォルト拦截含 `+`/`=`/`#` 的 Google 邮箱（如 `user+alias@example.com`），可在 Google 接続详情页开启/关闭 Block email subaddresses （提案开启提升セキュリティ性）。
    4. サポート Google One Tap：設定完了后，可集成 Clerk `<GoogleOneTap />` コンポーネント実装“一键ログイン”，参考 [Clerk コンポーネントドキュメント]( PROT193 )。

3. テスト第三方ログイン接続

設定完了后，通过 Clerk 内置 Account Portal 検証機〜できる：

- 进入 Clerk Dashboard，左侧导航栏进入 Account Portal ページ。
- 在“Sign-in”モジュール右侧，クリック“アクセスログインページ”按钮，リダイレクト至对应環境ログイン页：
  - 開発環境：`https://你的域名.accounts.dev/sign-in`（如 `https://my-app.accounts.dev/sign-in`）。
  - 生产環境：`https://accounts.你的域名.com/sign-in`（如 `https://accounts.my-app.com/sign-in`）。
- クリック“Sign in with GitHub/Google”，用对应プラットフォーム账号ログイン，若〜できる成功リダイレクト并返すアプリケーション，说明接続設定正常。

# 6. 从 Supabase 到更多バックエンド開発コンポーネント（応用）

在上文中，我们主な是站在 Supabase 的视角，去看“一个以 Postgres 为コアとなる一站式バックエンドプラットフォーム”〜できる帮我们解決哪些問題：认证、データベース、ファイル存储、リアルタイム通信、边缘函数等，都被集成在同一个控制台里，开箱即用、体验统一，非常に适合快速起步和中小型プロジェクト。

但从更长期、更工程化的角度来看， **Supabase 提供的每一块〜できる力（Auth / Storage / Edge Functions / Realtime / Database），在业界几乎都有对应的专业替代ソリューション** ——既包括同类 BaaS プラットフォーム，也包括更“单点突破”的云サービス和开源コンポーネント。作为上进的个人開発者和初创团队来说，了解これらの替代选项有几个好处：

- 判断当前プロジェクト是否“全用 Supabase 就够了”，还是某一块必〜するがある更专业/更便宜/更易合规的专用サービス；
- 当プロジェクト规模变大或〜する件变複雑な时，是否〜できる把某个モジュール从 Supabase 替换出去（例えば改用专门的 Auth プラットフォーム或对象存储），而不是一开始就被プラットフォーム彻底锁死；
- 拓宽技术选型视野，即使暂时不更换，也〜できる大致知道“如果不用 Supabase 的 X 機〜できる，我また哪些一般的な選択”。

本节〜するそれぞれ介绍 Supabase 所覆盖的几大〜できる力在市场上的主流替代ソリューション，例えば：认证（Auth）、ファイル存储（Storage）、边缘函数（Edge Functions）、リアルタイム通信（Realtime）、データベース托管等。シンプルな对比它们在機〜できる特性、無料额度/定价、易用性以及社区流行度等方面的差异， 让你对バックエンドコンポーネントツール库有更全面的理解。

## 同类 Baas プラットフォーム

始める前に，できます浏览同类的 Baas プラットフォーム，若觉得 Supabase 不够好用，できます根据〜する件選択異なる替代品进行尝试。

| プラットフォーム/サービス                | 类型                                                                           | 無料额度/定价                                                              | 特点 / 适用シナリオ                                                                                                                       |
| ------------------------ | ------------------------------------------------------------------------------ | -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Firebase（Google）       | 全托管 BaaS（Auth + Firestore + Storage + Functions + Hosting）                | Spark：無料轻量额度；Blaze：按量计费（Firestore/Storage/Functions それぞれ算） | 行业最成熟、ドキュメント好、上手快、リアルタイム〜できる力强。适用于中小型产品、移动/フロントエンド主导团队。缺点：计费複雑な、锁定性强、查询限制多（尤其 Firestore）。 |
| Supabase                 | 开源 BaaS（Postgres + Auth + Storage + Edge Functions + Realtime）             | 無料：500MB DB、1GB Storage、无サーバー函数少量调用；Pro：按实例计费         | 最像 Firebase 的 SQL 版；界面优秀、体验现代、可自托管。适用于必〜するがある强 SQL、BI、事务〜できる力的アプリケーション。缺点：高并发或複雑な函数成本较高。         |
| Appwrite Cloud           | 开源一站式 BaaS（DB + Auth + Storage + Functions + Realtime）                  | 無料：含む基本 DB/Storage/FaaS；有料按资源级别计费                         | 体验现代化、API 统一、可自托管；适合開発者友好的アプリケーション快速迭代。缺点：生态还不如 Firebase/Supabase 成熟；性〜できる在大型アプリケーション中必〜するがあるテスト。     |
| Nhost                    | Postgres + GraphQL + Auth + Storage + Functions                                | 無料：1GB DB、1GB Storage、少量函数调用                                    | 類似“Supabase + Hasura”；天然 GraphQL；适合フロントエンド团队与 React/Next.js プロジェクト。缺点：生态小、成本随用量升高。                              |
| AWS Amplify              | AWS 一站式バックエンド（Cognito + AppSync + DynamoDB + Storage + Functions + Hosting） | 無料：Hosting 额度 + Cognito 10k MAU + 部分函数额度                        | 大而全，适合已有 AWS 基本的な的团队；企业级可靠性。缺点：最难上手，サービス碎片化；初创团队メンテナンス成本高。                                       |
| Xata（近两年快速增长）   | 多モデルデータベース + Auth + Edge Functions                                           | 無料：250k レコード、15GB 带宽                                                 | 虽然更偏「DB + API」，但提供 Auth、ファイル、逻辑，可作为轻量全栈バックエンド。UI/開発体验极佳。缺点：機〜できる不如 Firebase/Supabase 全面。           |
| Convex（開発者体验极强） | 托管データベース + Auth + Functions（フロントエンド优先）                                      | 無料開発版；有料按リクエスト量计费                                               | 极简上手；无需 schema；フロントエンド写函数即可用バックエンド。适合 MVP/快速検証。缺点：高度绑定プラットフォーム，迁移成本高；不算完全传统 BaaS。                   |

## 认证 (Auth)

| ツール/プラットフォーム               | 機〜できる特点                                                                                                               | 無料额度/定价                        | 适用シナリオ与优缺点                                                                                                                                   |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------- | ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| Firebase Authentication | Google 提供的 BaaS 身份検証サービス，サポート邮箱/密码、手机、社交ログイン、匿名等一般的な方式。Spark 無料ソリューションサポート最高50k 月活跃ユーザー。 | Spark（無料）50k MAU；Blaze 按量计费 | 集成 Google 生态，ドキュメント丰富，上手シンプルな；機〜できる全面（MFA、阻塞函数等），适合快速開発。但与 Firebase プラットフォーム绑定，拡張到その他サービス需额外設定。                |
| Auth0 (Okta)            | 全托管身份认证プラットフォーム，サポート社交ログイン、企业 SSO、多因子认证、ルール拡張等强大機〜できる。                                           | 無料ソリューション25k MAU，有料按 MAU 计费     | 企业级機〜できる齐全（RBAC、审计ログ等），适合中大型アプリケーション；界面友好。缺点是 MAU 上升时成本高，無料版機〜できる有限（如不含 MFA/RBAC）。社区知名度高，ユーザー众多。 |
| AWS Cognito             | 亚马逊云原生身份サービス，サポート社交及 SAML 联合ログイン。直接ログインユーザー池提供每月10k MAU 無料，超过部分按 0.0055 美元/MAU 收费。  | 無料10k MAU/月，超出按量有料         | 与 AWS 生态深度集成（可无缝配合 API Gateway、Lambda 等），入門门槛略高，ドキュメント较複雑な；無料额度有限，适合已有 AWS 使用习惯的团队。                    |
| Logto                   | 开源身份认证プラットフォーム，自托管版無料，云サービス计划無料50k MAU。サポート多语言、多租户、OAuth/OIDC 等。                             | 社区版無料；Logto Cloud 無料50k MAU  | 近期流行的 Auth0 开源替代ソリューション，GitHub 已有 10k+ Stars。易拡張，自托管削減成本；缺点是生态和ドキュメント比較的较新，社区规模略逊于 Firebase/Auth0。           |
| Keycloak                | 知名开源 IAM/SSO 解決ソリューション，サポートユーザー名密码、LDAP、SAML、OAuth2 等。                                                     | 完全無料，需自托管                   | 機〜できる强大、可拡張（サポート细粒度権限控制），企业级機〜できる丰富；但デプロイ和メンテナンス複雑な度高，对小团队而言学习曲线较陡。缺点是对容器化和集群运维〜する求较高。         |

## ファイル存储 (Storage)

| プラットフォーム/サービス                                | 类型                 | 無料额度/定价                                                      | 特点/适用シナリオ                                                                                                                                         |
| ---------------------------------------- | -------------------- | ------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| Amazon S3                                | 云对象存储（AWS）    | AWS 無料プラン提供 5GB 存储、20k GET/PUT リクエスト/月，超出按使用量有料   | 行业標準的な对象存储，可靠性高、全球多区域デプロイ。機〜できる全面，与 AWS 生态整合良好；定价较複雑な，新ユーザー需了解计费ルール。                                       |
| Google Cloud Storage（Firebase Storage） | 云对象存储（Google） | Firebase Spark ソリューション提供無料额度（1GB 存储 + 流量限制），Blaze 有料 | 与 Firebase/Google Cloud 紧密集成，易于管理；サポート CDN 加速、细粒度セキュリティルール。                                                                          |
| 腾讯云 COS / 阿里云 OSS                  | 云对象存储（国内）   | 按量有料（各有新ユーザー赠送额度，如OSS有首年40GB無料等）              | 面向国内市场，高性〜できる、大规模对象存储；与中国云生态整合，ドキュメント较完善。阿里OSS 機〜できる全面、全球加速；七牛KODO 专注多媒体処理，成本较低，适合个人和小团队。 |
| MinIO                                    | 开源 S3 兼容存储     | 开源無料（自建）                                                   | 轻量级、高性〜できる、与 S3 API 兼容，适合在私有云或本地搭建对象存储。ドキュメント和社区活跃；需自分でメンテナンス基本的な设施。                                                  |
| Cloudinary / Imgix 等                    | 媒体存储+CDN         | 基本無料ソリューション（如 Cloudinary 無料 25GB/月带宽）                     | 针对图片/视频最適化的云存储+CDN サービス，提供リアルタイム转码、压缩等高度な機〜できる。适合媒体プロジェクト，但機〜できる较专一，作为通用ファイル存储使用成本偏高。                          |

## 边缘函数 (Edge Functions)

| プラットフォーム/サービス                              | 特点                                       | 無料额度/定价                                                          | 适用シナリオ与优缺点                                                                                                                                                           |
| -------------------------------------- | ------------------------------------------ | ---------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Cloudflare Workers                     | 全球分布式 JavaScript/Wasmtime 環境        | 無料计划：每天 100k リクエスト；標準计划$5/月含1,000万リクエスト                   | 実行在 Cloudflare 边缘节点，遅延极低；适合全局分发的逻辑、静态资源渲染等。無料配额较少（相当于每月约300万リクエスト），上手シンプルな。缺点是実行时（JS/Wasmtime）限制与デバッグツール有限。 |
| Vercel Edge Functions                  | 随 Next.js/フロントエンドフレームワーク无缝集成，サポート JS/TS/Go | Hobby 無料：每月 100万 函数调用，100万 边缘リクエスト                        | 深度集成フロントエンドフレームワーク，自動デプロイ；适合现代 Web アプリケーション。無料额度充足，デフォルト実行时 10s，可提升至 60s。缺点是無料版团队协作機〜できる受限；依赖 Vercel プラットフォーム。                                |
| Netlify Edge / Functions               | Node.js 云函数＋边缘路由（NFT）            | 無料：300 代币/月（约相当于每月 1M リクエスト）；按信用点计费                | サポート Node.js 函数、边缘処理路由等。無料额度用于構築、函数和带宽，适合フロントエンド全栈デプロイ。优点是简便易用，集成 Git デプロイ；缺点是無料额度使用需算计（10k リクエスト = 3 点）。            |
| AWS Lambda@Edge / CloudFront Functions | AWS 无サーバー边缘计算                       | AWS Lambda（1M 無料リクエスト/月+400k GB-s）+ CloudFront $0.085/每10万调用起 | 与 CloudFront 集成，可在边缘実行コード。适合必〜するがある AWS 生态（如在节点层面做権限或 A/B テスト）。优点是灵活强大；缺点是設定複雑な，遅延略高于 Cloudflare/Vercel。                   |

## リアルタイム通信 (Realtime)

| プラットフォーム/サービス                              | 機〜できる特点                                         | 無料额度/定价                                         | 适用シナリオ与优缺点                                                                                                     |
| -------------------------------------- | ------------------------------------------------ | ----------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| Firebase Realtime Database / Firestore | Google BaaS リアルタイムデータベース；サポート数据变更推送         | Spark 無料：リアルタイムデータベース1GB 存储 & 限额；Blaze 按量有料 | 强集成 Firebase 生态，リアルタイム监听シンプルな。优点是無料起步快；缺点是データベース类型（JSON/NoSQL），複雑な查询〜できる力弱。               |
| Ably                                   | リアルタイム消息与 pub/sub プラットフォーム，サポート WebSocket、MQTT 等 | 無料包：每月 6,000,000 条消息                         | 機〜できる全面的リアルタイム消息サービス，高并发サポート；無料额度可达600万消息/月。社区与ドキュメント较好，适合全球分布。                         |
| Pusher Channels                        | 事件推送サービス，サポート频道/事件机制                  | Sandbox 無料：每日 200k 消息，100 并发接続            | 易用的 WebSocket サービス，ドキュメント齐全，适合快速実装チャット和通知機〜できる。無料版限制消息量和接続数；有料后拡張性好。              |
| 自建 WebSocket/Socket.IO               | 自分で搭建サーバー（Node.js、Elixir 或 Go 等）       | 自行托管成本（如サーバー费用）                          | 灵活度最高，可根据〜する件定制协议和拓扑。适合对成本控制严格且技术成熟的团队。缺点是需自行処理可用性、拡張和跨域等問題。 |

## データベース

| プラットフォーム/ツール                    | データベース类型                              | 無料额度/定价                                         | 主な特点                                                                                                                            |
| ---------------------------- | --------------------------------------- | ----------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| Neon (Serverless PostgreSQL) | 关系型（PostgreSQL）                    | 無料计划：0.5GB 存储，主ブランチ永久在线，20h ブランチ计算/月 | 云原生无サーバー Postgres，サポート自動伸缩和ブランチ（fork テスト）。無料额度对小プロジェクト够用，适合现代開発プロセス。ブランチ機〜できる强大，但無料额度较小。    |
| Aiven PostgreSQL             | 关系型（PostgreSQL/MySQL）              | 無料计划：1GB 存储，1 vCPU，1GB 内存                  | 托管级データベースサービス，サポート跨云多区域迁移。提供有 MySQL、Redis 等可选。無料额度适合開発和小型プロジェクト；商业版サポート高可用集群和モニタリング。          |
| CockroachDB Cloud            | 分布式 SQL（兼容 PostgreSQL）           | 無料计划：10GB 存储                                   | 類似 Google Spanner 的分布式 SQL データベース，自動分片拡張。無料10GB 空间较慷慨；适合必〜するがある横向拡張和高一致性的アプリケーション。商业版 SLA 高。       |
| TiDB Cloud                   | 分布式关系型（MySQL 兼容）              | 無料计划：每节点5GB，总计最多25GB                     | 开源 TiDB 的云版，兼容 MySQL 协议，分布式架构。無料额度充足，适合熟悉 MySQL 的团队，性〜できる优秀；缺点是运维比較的複雑な（针对大型シナリオ）。  |
| MongoDB Atlas                | ドキュメント型（NoSQL MongoDB）                 | 無料 M0 集群：0.5GB 存储                              | 云端 MongoDB，柔軟なドキュメントモデル，サポート丰富查询和索引。無料 0.5GB データベース适合テスト和小型アプリケーション；可按需横向拡張。学习曲线略高于关系型データベース。 |
| SQLPub                       | 多データベース（MySQL、PostgreSQL、Redis 等） | 無料计划：36,000 リクエスト/小时，30 并发接続，500MB 存储   | 一站式データベースプラットフォーム，サポート多种データベース类型。無料版适合学习和小プロジェクト；优点是サポート多种 DB，缺点是存储额度较小。                               |

以上替代ソリューション各有侧重：开源更灵活可控（Keycloak、MinIO、Socket.IO、Neon、CockroachDB 等），云托管サービス更易上手（Firebase、Auth0、Cloudflare、Vercel、Netlify、AWS、Aiven、MongoDB Atlas 等）。選択时可根据プロジェクト〜する件、团队技术栈、预算和社区生态等权衡。个人プロジェクト可优先选用無料配额充足、易集成的サービス（如 Firebase 系列、七牛存储、Cloudflare Workers、Neon、CockroachDB 等），而对企业级或特定セキュリティ〜する件，则可考虑機〜できる更丰富但收费较高的ソリューション（Auth0、Alibaba/Tencent 云、AWS、TiDB/Aiven 等）。できます在实际アプリケーション中不断尝试，直到選択出最适合的バックエンド開発ツールコンポーネント。

# 总结

在今天的レッスン中，我们システム学习了データベース的基本的な概念、Supabase 的コア定义及其操作细节。后续在实践过程中，你可根据プロジェクト的实际アプリケーションシナリオ与〜する件，随时回头翻阅这份ドキュメント作为参考。

请时刻记住一个重〜するな原则： **先完了，再完美！** 无需追求一步到位，我们完全〜できる通过継続的に迭代最適化，段階的に靠近更优的成果。祝你在后续的プロジェクト实践中一切顺利！

# 📚 課題

1. 開発一个含むユーザー管理システム和データベース的アプリケーション程序。最好含む更多的Supabase 機〜できる (Realtime / cloud storage / Edge function).
