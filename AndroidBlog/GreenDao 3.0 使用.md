# GreenDao 3.0 使用
[TOC]

## 3.0版本与之前版本的区别

3.0以前采用Generator项目,生成所有的Dao类与实体类.

代码生成核心代码：
1. **DaoMaster** DaoMaster保存数据库对象（SQLiteDatabase）和管理DAO类（而不是对象）为特定的模式。它的静态方法来创建表或删除它们。它的内部类OpenHelper和DevOpenHelper是创建SQLite数据库架构SQLiteOpenHelper实现。
2. **DaoSession** 管理所有可用的DAO对象，DaoSession还提供了一些通用的持久性的方法，如插入，装载，更新，刷新和删除实体。最后，DaoSession对象也跟踪标识范围。
3. **Daos** 各个表的Dao类，例如 UserDao，ImageDao等，负责增删改查