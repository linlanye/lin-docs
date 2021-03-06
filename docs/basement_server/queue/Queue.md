KV类
----
namespace: `lin\basement\queue`

包含如下类：

* **lin\basement\queue\QueueRedis**

---

### 说明

`lin\basement\queue\QueueRedis`也是采用一致性哈希算法实现集群访问，具体说明参考[KV](../kv/KV.md)的使用。不同的时只在**实例化和调用setName()**根据队列名选择不同服务器。


---

### 功能

* basement部分，[点击查看](../../docs_basement/ServerQueue.md)
* 扩展功能：
    * 多服务器和集群功能，参见[KV](../kv/KV.md)的使用



#### 配置项

动态配置

分为两部分：

* `servers`相关部分，[点击查看](../README.md)。
* 组件部分如下：

~~~php
<?php

'server'=>[
    'queue' => [ //队列服务器
        'driver'  => [
            'redis' => [ //使用redis驱动的配置
                'use' => 'queue*', //使用所有queue标识的服务器
            ],
            'local' => [ //使用本地文件驱动的配置
                'path' =>  'path/to/queue/data/', //数据存放路径
            ],
        ],
        'default' => [
            'name' => 'lin', //默认的队列名
        ],
        'debug'   => true, //调试模式
    ],
]

~~~

#### 其他

[参见KV](../kv/KV.md)
