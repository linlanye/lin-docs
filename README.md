# Lin/1.0 说明文档

## 介绍

本文档为php框架[Lin](https://github.com/linlanye/lin)所使用的组件[Lin-components](https://github.com/linlanye/lin-components)的说明文档。文档内容分为以下三个部分：

* [basement一般组件](basement/README.md)
* [basement的服务器组件](basement_server/README.md)
* [一般组件](components/README.md)

（basement项目参见：[https://github.com/linlanye/basement](https://github.com/linlanye/basement)）


## 组件列表

#### [Basement](basement/README.md)

* [Config](basement/config/Config.md) （配置类）
* [Debug](basement/debug/Debug.md)	  （调试类）
* [Event](basement/event/Event.md)    （事件类）
* [Exception](basement/exception/GeneralException.md) （扩展异常类）
* [Lang](basement/lang/Lang.md) （语言映射类）
* [Log](basement/log/Log.md) （日志类）
* [Request](basement/request/Request.md) (请求类)

#### [Basement Server](basement_server/README.md)

* [KV](basement_server/kv/KV.md) （Key-Value型服务器操作类）
* [KVLocal](basement_server/kv/KVLocal.md) （Key-Value型模拟服务器操作类）
* [Local](basement_server/local/Local.md)  （本地文件操作类）
* [Queue](basement_server/queue/Queue.md)   （队列型服务器操作类）
* [QueueLocal](basement_server/queue/QueueLocal.md) （模拟队列型服务器操作类）
* [SQL](basement_server/sql/SQLPDO.md) （关系数据库操作类）

#### [Components](components/README.md)

* [LSE](components/algorithms/LSE.md) （对称加密类）
* [Exception](components/exception/Exception.md) （自定义异常类）
* [Layer](components/layer/Layer.md) （架构类，见[LBA说明](https://github.com/linlanye/lin)）
* [Model](components/orm/Model.md) （数据模型类）
* [Query](components/orm/Query.md) （数据查询类）
* [Formatter](components/processor/Formatter.md) （数据格式化类）
* [Mapper](components/processor/Mapper.md) （数据映射类）
* [Validator](components/validator/Validator.md) （数据校验类）
* [Response](components/response/Response.md) （响应类）
* [Route](components/route/Route.md) （路由类）
* [Security](components/security/Security.md) （安全类）
* [URL](components/url/URL.md) （url构建类）
* [View](components/view/View.md) （视图类）


## 约定
对本文档一些术语进行解释，并在整个说明文档中都保持一致的意思。

* **参见basement**：由于部分接口函数符合[basement](https://github.com/linlanye/basement)的规范要求，所以可直接前往[basement](https://github.com/linlanye/basement)项目查看具体说明。
* **动、静态配置类**：每个组件类都依赖于一个专用的配置内容。有些类的配置项在整个生命器只加载一次，若后续调整配置内容，则不会产生任何变化，称为静态配置类；有些类的配置项则在每一次实例化都会重新加载一次配置，在生命期内调整配置内容会产生实际变化，称为动态配置类。
* **重置**：有些类会提供`reset()`函数，一般代表重置的意思。重置不仅清空数据，还清空配置项，在这种情况下，再次运行静态配置的类会重新加载配置项。
而`clean()`函数一般只清空数据，不清空配置项。


## 捐赠
![捐赠林澜叶](https://img.lin-php.com/donations.png)

## 版权信息
* 作者：林澜叶(linlanye)版权所有。