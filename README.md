# 平台系统开发要求

## 系统功能相关

1.平台系统功能封闭

    所有直接与外部系统交互的操作，不得在平台系统中出现，均通过接口服务进行交互。并且这平台系统仅允许系统通用功能和性能进行开发升级，不允许出现对定制需求的开发升级。所有定制需求均只能出现在接口服务中实现。

2.暴力搜索禁止

    避免暴力搜索操作，在保证数据最终统计结果准确的前提下，允许用实时统计数量的准确性换取性能的提升。

3.“重跑”功能

    尽量单独设计重跑流程，避免异常数据在初次使用时异常触发重跑后，在重跑时继续异常。

4.异常流程及失败流程处理

    缩小try catch的范围，尽可能方便定位问题。
    出现“非正常”（包括判断分支的“非正常”以及抛出的异常）问题时，需要详细打印异常数据信息。

## 配置文件相关

1.读取配置文件配置项的功能

    “选项类”配置需要设计默认配置值，必要时需要在系统启动日志中打印是否使用了系统默认配置值。

## 代码规范相关

1.自定义常量

    代码中所有需要使用常量的地方，均需要设计编写对应的常量类（可以使用接口类），或者枚举类型，不允许直接使用整形或者字符串值。直接使用整形或者字符串值，严重影响代码的可维护性。

2.常量命名

    以简介明确为原则，禁止使用flag这类看似为类型名称，但却没有任何实际含义的名词命名。

3.POJO类设计

    需要重写toString()方法，避免打印POJO类对象信息时打印出对象内存地址（实际上是该对象内存地址经哈希算法得出的int类型的值在转换成十六进制），造成日志中出现无效信息。

## 数据库操作相关

### 缓存数据

1.禁止自定义Redis KEY值

    所有Redis操作，key的取值均必须调用设计专用的"KEY管理类"，提供Key的查询方法或常量，不要出现直接在代码中出现直接使用自定义字符串的形式作为key来操作Redis。

2.Redis数据失效处理

    如果数据库表数据在Redis中存在相应数据，则需要设计相应的更新生效机制（或者数据失效机制）。在增删（改）接口中设置删除Redis中的对应的数据，或者设置更新标识，避免接口中对Redis进行数据更新。应将“数据自动加载”操作设计在使用该数据的服务中执行，尽量减少服务之间的耦合，降低接口服务的维护难度

### 持久化数据

1.强制数据库接口操作

    所有数据库表设计时，均需要设计完整的增删（改）查接口功能，应避免运维过程中直接修改数据库数据。

2.数据库操作时间记录

    对于频繁更新数据的表，字段中必须包含创建时间、更新时间两个字段
