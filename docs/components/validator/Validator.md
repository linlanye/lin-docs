Validator类
----
namespace: `lin\validator`

包含如下类：

* **lin\validator\Validator**

---

### 说明

对数据进行校验，通过继承本类后定义规则，然后调用规则进行数据批量校验。提供`must`、`should`、`may`三种校验模式，分别代表：无论字段存在与否都要校验、字段存在便校验、字段存在且非空才校验。
除继承外，也可直接调用内置校验方法对数据进行单个校验。

---

### 功能

* 对数组中的字段校验。
* 使用 `'.'` 访问数组深层字段。
* 提供常见[校验方法](Function.md)。
* 提供`setting()`方法。


#### 配置项

动态配置

~~~php
<?php
'validator' => [
    'default' => [ //默认
        'info' => function ($fields) {return "${fields}字段验证失败";}, //默认字段验证失败后的信息回调，入参为字段名
        'type' => 'may', //默认验证模式。可为must, should, may分别代表，必须验证，存在才验证，存在且不为空验证(trim后长度大于0)
    ],
    'debug'   => true,
],
~~~

#### 使用

使用`setRule(string $name, array $rules)`定义规则，其中`$name`为规则名，`$rules`为具体规则，格式形如
```
['字段: 模式' => '类里面的校验方法或回调函数']

一般形式
['key1.key2, key3.key4: mode' => 'callable|method']
或
['key1.key2, key3.key4: mode' => ['callable|method', 'error_info'] ]
```
多个入参的校验函数，在键名中使用 ',' 隔开；键值为校验函数和校验失败信息（可选），优先级为：`回调函数` > `类自定义函数` > `类内置函数`

~~~php
<?php

//1.定义规则
class MyValidator extends Validator
{
    //在setting方法中定义不同的规则，以下用$data表示待校验数组
    protected function setting()
    {
        //定义一个校验规则，使用类中校验函数
        $this->setRule('my_rule', [
            //must模式，字段不存在则返回失败
            'id: must' => 'isInt', //校验$data['id']是否为整数

            //should模式，字段存在才校验，否则返回成功
            'info: should' => 'isString', //$data['info']是否为字符串

            //may模式，字段存在且非空才校验，否则返回成功
            'ip: may' => 'isIP',//$data['ip']存在且非空时，校验是否为ip地址

            //校验多个入参
            'a, b' => 'gt', //$data['a']是否大于$data['b']
        ]);

        //定义一个校验规则，使用回调或闭包
        $this->setRule('my_rule2', [
            'status' => function($v){
                return is_int($v);
            },
        ]);

        //定义校验失败时候的信息
        $this->setRule('my_rule3', [
            'status' => ['isInt', 'error status'], //校验失败时候，错误信息为'error status'
            'pwd' => [function($v){return strlen($v)>5;}, 'error password'], //错误信息为'error password'
        ]);

        //同理可定义多个不同规则
        ...
    }
}


//2.使用规则
$Validator = new MyValidator;

//数据必须全部通过校验才成功
$result = $Validator->withRule('my_rule')->valid($data);

//弱模式，数据只要有一项通过则校验成功
$result = $Validator->withRule('my_rule')->valid($data, true);

//可同时使用多个规则，用 ',' 隔开
$Validator->withRule('my_rule1, my_rule2')->valid($data);

//获得校验失败的字段信息
$Validator->getErrors(); //形如['字段' => '错误信息']

//3.直接使用
$data['id'] = $Validator->toInt($data['id']); //内置校验函数见下方

~~~


---


## API

#### 列表
~~~php
final protected function setRule(string $name, array $rules): bool
final public function validate(array $data, bool $weakMode = false): bool
final public function withRule(string $names): object
final public function getErrors(): array
~~~

#### 详细说明

**setRule()**: 只用在`setting()`中定义规则
```php
params:
    string $name  规则名
    array  $rules 规则数组，键名为原数据中的字段名和校验模式，键值为校验函数和校验失败信息，多个校验入参字段用 ',' 隔开，深层字段使用 '.' 访问。
return
    bool

$rules一般形式如：
[
    'key1.key2, key3.key4: mode' => 'callable|method', //优先级为：回调函数> 类自定义函数 > 类内置函数
    或
    'key1.key2, key3.key4: mode' => ['callable|method', 'error info']
]
```

**withRule()**: 指定当前校验使用的规则，一次性使用。校验完后，下一次校验需重新指定
```php
params:
    string $names 本次校验使用的规则名，多个规则使用 ',' 隔开
return
    $this
```

**valid()**: 校验数据
```php
params:
    array $data           待校验的数据
    bool  $weakMode=false 是否为弱模式，弱模式下任一字段通过则全部通过，否则必须所有字段通过才通过
return
    bool 是否校验成功
```

**getErrors()**: 获取校验失败的字段错误信息，来源于`setRule()`时指定，未指定则来自配置项`default.info`。下一次校验时，将被重新生成。
```php
params:
    void
return
    array 格式形如 ['失败字段' => '失败信息']
```