chengang/joy-interaction
========================
这个扩展提供了乐换机各个系统内部调用方法的接口封装。

Installation
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

```
php composer.phar require --prefer-dist chengang/joy-interaction "*"
```

or add

```
"chengang/joy-interaction": "*"
```

to the require section of your `composer.json` file.


Usage
-----
请求系统使用方式：

一.组件方式

1.在配置文件中 components 加入配置项。如下：
```
'components'=>{
 'joyInteraction' => [ //组件名称
            'class' => \chengang\joyInteraction\HttpPush::class, 
            'signMethod' => '', //签名实现方法 需要实现chengang\joyInteraction\AuthMethod接口，返回签名字符串
            'httpHeader' => '', //头部参数 需要继承chengang\joyInteraction\HttpHeader类 无特殊需要无需设置
            'source' => '1213', //需要配置目标系统分配的来源标识
        ]
}
```
2.在需要使用时，直接引用
```
$res = Yii::$app->joyInteraction->push($method, $url, $data = [], $header = []);
$res: 为请求结果
$method 请求方式，请引用HttpPush提供的请求方式枚举
$url 请求目标地址
$data 请求数据 可空
$header 请求头部需要传入的字段 可空
```


目标系统使用方式

一、基类控制器需要继承\chengang\joyInteraction\BaseController 控制器

   1.签名验证方法 需要重新，必须实现chengang\joyInteraction\AuthMethod接口，返回签名字符串
   
   $signMethod = '\chengang\joyInteraction\SignMethod'; //签名实现方法 可重写
   
   2.头部参数 需要继承chengang\joyInteraction\HttpHeader类 无特殊需要无需设置
   
   $httpHeader = '\chengang\joyInteraction\HttpHeaderModel'; //请求头部对象 可重写
   
   3.需要在params配置文件设置httpValidate参数，参数内容为数组，允许请求系统通过的source 
```
    'httpValidate' => [
        'sources' => ['source1','source2'],
        'ips' => ['ip1','ip2'], //允许通过ip
        'outtime' => 3, //超时设置
    ],

```