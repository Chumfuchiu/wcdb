# WCDB（Tencent 跨平台数据库）学习教程

> 仓库：`Tencent/wcdb`（upstream）｜ 镜像：`Chumfuchiu/wcdb`（私有）

WCDB 是腾讯开源的**跨平台数据库框架**，底层是（深度改造过的）SQLite，向上提供
**C++ / Java / Kotlin / Swift / Objective-C** 五种语言的 API。核心主张：

> **Easy-to-use + Efficient + Complete**——ORM 与 WINQ 查询语言免去手写 SQL；
> **连接池多 handle 并发**（多线程读写）；SQLCipher 加密、Zstd 字段级压缩、
> 自动 Vacuum/Checkpoint/Backup、损坏修复（RepairKit）、数据迁移、全文检索、SQL 注入防护。

## 本教程怎么读

每课围绕一个主线问题，首尾衔接，建议按顺序读。

| 课 | 主线问题 |
| --- | --- |
| [01 总览](01-overview.md) | WCDB 与裸 SQLite 差在哪？五种语言怎么共用一套核心？ |
| [02 核心架构](02-core-architecture.md) | `Database` / `Handle` / `Statement` 怎么分层？懒初始化怎么运作？ |
| [03 并发与装饰器](03-concurrency.md) | 一个库怎么支持多线程读写？装饰器链怎么扩展行为？ |
| [04 配置与自愈](04-config.md) | 忙重试、自动 vacuum、加密、压缩怎么配？ |
| [99 实战与自测](99-practice.md) | 怎么在 Android 上建表、CRUD、跑事务？ |

## 源码地图

| 层 | 路径 | 内容 |
| --- | --- | --- |
| C++ 公共 API | `src/cpp/core/` | `Database`、`Handle`、`PreparedStatement`、`Table` |
| 链式 DSL | `src/cpp/chaincall/` | `ChainCall`、`Select`、`Insert`、`Update`、`Delete`、`MultiSelect` |
| 引擎实现 | `src/common/core/` | `InnerDatabase`、`InnerHandle`、`HandlePool`、`HandleCounter`、`sqlite/`（装饰器） |
| 配置 | `src/common/core/config/` | `BusyRetryConfig`、`AutoVacuumConfig`、`CipherConfig`… |
| 高级能力 | `src/common/core/{repair,cipher,compression,migration,integrity,vacuum}/` | 修复、加密、压缩、迁移、完整性 |
| 编译期生成 | `src/java/builder/compiler/` | `JavaAnnotationProcessor` + `JavaCodeGenerator`（处理 `@WCDBTableCoding`，生成同包编码类） |
| Java API | `src/java/main/.../core/` | `Database`、`Handle`、`Transaction`、`PausableTransaction` |
| WINQ | `src/java/main/.../winq/` | `Statement*`、`Column`、`Schema`、`Join`… |
| Java 桥 | `src/java/main/src/main/cpp/` | `core/DatabaseExtendJNI.cpp`、`base/WCDBExtendJNI.cpp`（Java ↔ C++） |

> 官方教程在 [github.com/Tencent/wcdb/wiki](https://github.com/Tencent/wcdb/wiki)，
> 本教程只讲源码级设计，不重复官方用法文档。