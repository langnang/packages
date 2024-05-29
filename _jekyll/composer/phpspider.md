# phpspider

> https://doc.phpspider.org/

## configs--成员

爬虫的整体框架是这样：首先定义了一个\$configs数组, 里面设置了待爬网站的一些信息, 然后通过调用`\$spider = new phpspider(\$configs);`和`\$spider->start();`来配置并启动爬虫.

\$configs数组中可以定义下面这些元素

### `name`

> 定义当前爬虫名称

**String类型 可选设置**

举个栗子:

```php
'name' => '糗事百科'
```

### `log_show`

> 是否显示日志  
> 为true时显示调试信息  
> 为false时显示爬取面板

**布尔类型 可选设置**

**log\_show默认值为false，即显示爬取面板**

栗子1:

```php
'log_show' => false
```

![](https://doc.phpspider.org/pachong.gif)

栗子2:

```php
'log_show' => true
```

![](https://doc.phpspider.org/pachong_false.gif)

**注意：显示爬取面板时，也可以通过tail命令来查看日志**

`tail -f data/phpspider.log`

### `log_file`

> 日志文件路径

**String类型 可选设置**

**log\_file默认路径为data/phpspider.log**

举个栗子:

```php
'log_file' => data/qiushibaike.log
```

### `log_type`

> 显示和记录的日志类型  
> 普通类型: info  
> 警告类型: warn  
> 调试类型: debug  
> 错误类型: error

**String类型 可选设置**

**log\_type默认值为空，即显示和记录所有日志类型**

栗子1:  
显示错误日志

```php
'log_type' => 'error'
```

举个栗子:  
显示错误和调试日志

```php
'log_type' => 'error,debug'
```

### `input_encoding`

> 输入编码  
> 明确指定输入的页面编码格式(UTF-8,GB2312,…..)，防止出现乱码,如果设置null则自动识别

**String类型 可选设置**

**input\_encoding默认值为null，即程序自动识别页面编码**

举个栗子:

```php
'input_encoding' => 'GB2312'
```

### `output_encoding`

> 输出编码  
> 明确指定输出的编码格式(UTF-8,GB2312,…..)，防止出现乱码,如果设置null则为utf-8

**String类型 可选设置**

**output\_encoding默认值为utf-8, 如果数据库为gbk编码，请修改为gb2312**

举个栗子:

```php
'output_encoding' => 'GB2312'
```

### `tasknum`

> 同时工作的爬虫任务数  
> 需要配合redis保存采集任务数据，供进程间共享使用

**整型 可选设置**

**tasknum默认值为1，即单进程任务爬取**

举个栗子:

开启5个进程爬取网页

```php
'tasknum' => 5
```

> `tasknum`在[如何实现多任务爬虫](https://doc.phpspider.org/development_skills/multitasking_crawler.html)中作详细介绍。

### `multiserver`

> 多服务器处理  
> 需要配合redis来保存采集任务数据，供多服务器共享数据使用

**布尔类型 可选设置**

**multiserver默认值为false**

举个栗子:

```php
'multiserver' => true
```

> `multiserver`在[如何实现多服务器集群爬虫](https://doc.phpspider.org/development_skills/crawler_cluster.html)中作详细介绍。

### `serverid`

> 服务器ID

**整型 可选设置**

**serverid默认值为1**

举个栗子:  
启用第二台服务器

```php
'serverid' => 2
```

### `save_running_state`

> 保存爬虫运行状态  
> 需要配合redis来保存采集任务数据，供程序下次执行使用  
> 注意：多任务处理和多服务器处理都会默认采用redis，可以不设置这个参数

**布尔类型 可选设置**

**save\_running\_state默认值为false，即不保存爬虫运行状态**

举个栗子:

```php
'save_running_state' => true
```

### `queue_config`

> redis配置

**数组类型 可选设置**

保存爬虫运行状态、多任务处理 和 多服务器处理 都需要`redis`来保存采集任务数据

举个栗子:

```php
'queue_config' => array(
    'host'      => '127.0.0.1',
    'port'      => 6379,
    'pass'      => '',
    'db'        => 5,
    'prefix'    => 'phpspider',
    'timeout'   => 30,
)
```

### `proxy`

> 代理服务器  
> 如果爬取的网站根据IP做了反爬虫, 可以设置此项

**数组类型 可选设置**

栗子1:  
普通代理

```php
'proxy' => array('http://host:port')
```

栗子2:  
验证代理

```php
'proxy' => array('http://user:pass@host:port')
```

**注意：如果对方根据IP做了反爬虫技术，你可能需要到** [阿布云代理](http://www.abuyun.com/) **申请代理通道 或者 第三方免费代理IP，然后在这里填写代理信息**

### `interval`

> 爬虫爬取每个网页的时间间隔  
> 单位：毫秒

**整型 可选设置**

举个栗子:  
设置爬取时间间隔为1秒

```php
'interval' => 1000
```

### `timeout`

> 爬虫爬取每个网页的超时时间  
> 单位：秒

**整型 可选设置**

**timeout默认值为5秒**

举个栗子:

```php
'timeout' => 5
```

### `max_try`

> 爬虫爬取每个网页失败后尝试次数  
> 网络不好可能导致爬虫在超时时间内抓取失败, 可以设置此项允许爬虫重复爬取

**整型 可选设置**

**max\_try默认值为0，即不重复爬取**

举个栗子:

```php
'max_try' => 5 // 重复爬取5次
```

### `max_depth`

> 爬虫爬取网页深度，超过深度的页面不再采集  
> 对于抓取最新内容的增量更新，抓取好友的好友的好友这类型特别有用

**整型 可选设置**

`max_depth`**默认值为0，即不限制**

举个栗子:  
采集知乎好友时只采集到5级深度

```php
'max_depth' => 5
```

### `max_fields`

> 爬虫爬取内容网页最大条数  
> 抓取到一定的字段后退出

**整型 可选设置**

`max_fields`**默认值为0，即不限制**

举个栗子:

```php
'max_fields' => 100
```

### `user_agent`

> 爬虫爬取网页所使用的浏览器类型  
> `phpspider::AGENT_ANDROID`  
> `phpspider::AGENT_IOS`  
> `phpspider::AGENT_PC`  
> `phpspider::AGENT_MOBILE`

**枚举类型 可选设置**

栗子1:  
使用内置的枚举类型

`phpspider::AGENT_ANDROID`**, 表示爬虫爬取网页时, 使用安卓手机浏览器**  
`phpspider::AGENT_IOS`**, 表示爬虫爬取网页时, 使用苹果手机浏览器**  
`phpspider::AGENT_PC`**, 表示爬虫爬取网页时, 使用PC浏览器**  
`phpspider::AGENT_MOBILE`**, 表示爬虫爬取网页时, 使用移动设备浏览器**

```php
'user_agent' => phpspider::AGENT_ANDROID
```

栗子2:  
使用自定义类型

```php
'user_agent' => "Mozilla/5.0"
```

栗子3:

随机浏览器类型，用于破解防采集

```php
'user_agent' => array(
    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.116 Safari/537.36",
    "Mozilla/5.0 (iPhone; CPU iPhone OS 9_3_3 like Mac OS X) AppleWebKit/601.1.46 (KHTML, like Gecko) Version/9.0 Mobile/13G34 Safari/601.1",
    "Mozilla/5.0 (Linux; U; Android 6.0.1;zh_cn; Le X820 Build/FEXCNFN5801507014S) AppleWebKit/537.36 (KHTML, like Gecko)Version/4.0 Chrome/49.0.0.0 Mobile Safari/537.36 EUI Browser/5.8.015S",
)
```

### `client_ip`

> 爬虫爬取网页所使用的伪IP，用于破解防采集

**String类型 或 数组类型 可选设置**

栗子1:

```php
'client_ip' => '192.168.0.2'
```

栗子2:

随机伪造IP，用于破解防采集

```php
'client_ip' => array(
    '192.168.0.2', 
    '192.168.0.3',
    '192.168.0.4',
)
```

### `export`

> 爬虫爬取数据导出
>
> > type：导出类型 csv、sql、db  
> > file：导出 csv、sql 文件地址  
> > table：导出db、sql数据表名

**注意导出到数据库的表和字段要和上面的fields对应**

**数组类型 可选设置**

栗子1:  
导出CSV结构数据到文件

```php
'export' => array(
    'type' => 'csv', 
    'file' => './data/qiushibaike.csv', // data目录下
)
```

栗子2:  
导出SQL语句到文件

```php
'export' => array(
    'type'  => 'sql',
    'file'  => './data/qiushibaike.sql',
    'table' => '数据表',
)
```

栗子3:  
导出数据到Mysql

```php
'export' => array(
    'type' => 'db',
    'table' => '数据表',  // 如果数据表没有数据新增请检查表结构和字段名是否匹配
)
```

### `db_config`

数据库配置

```php
'db_config' => array(
    'host'  => '127.0.0.1',
    'port'  => 3306,
    'user'  => 'root',
    'pass'  => 'root',
    'name'  => 'demo',
)
```

### `domains`

> 定义爬虫爬取哪些域名下的网页, 非域名下的url会被忽略以提高爬取速度

**数组类型 不能为空**

举个栗子:

```php
'domains' => array(
    'qiushibaike.com',
    'www.qiushibaike.com'
)
```

### `scan_urls`

> 定义爬虫的入口链接, 爬虫从这些链接开始爬取,同时这些链接也是监控爬虫所要监控的链接

**数组类型 不能为空**

举个栗子:

```php
'scan_urls' => array(
    'http://www.qiushibaike.com/'
)
```

### `content_url_regexes`

> 定义内容页url的规则  
> 内容页是指包含要爬取内容的网页 比如`http://www.qiushibaike.com/article/115878724` 就是糗事百科的一个内容页

**数组类型 正则表达式 最好填写以提高爬取效率**

举个栗子:

```php
'content_url_regexes' => array(
    "http://www.qiushibaike.com/article/\d+",
)
```

### `list_url_regexes`

> 定义列表页url的规则  
> 对于有列表页的网站, 使用此配置可以大幅提高爬虫的爬取速率  
> 列表页是指包含内容页列表的网页 比如`http://www.qiushibaike.com/8hr/page/2/?s=4867046` 就是糗事百科的一个列表页

**数组类型 正则表达式**

举个栗子:

```php
'list_url_regexes' => array(
    "http://www.qiushibaike.com/8hr/page/\d+\?s=\d+"
)
```

### `fields`

> 定义内容页的抽取规则  
> 规则由一个个`field`组成, 一个`field`代表一个数据抽取项

**数组类型 不能为空**

举个栗子:

```php
'fields' => array(
    array(
        'name' => "content",
        'selector' => "//*[@id='single-next-link']",
        'required' => true,
    ),
    array(
        'name' => "author",
        'selector' => "//div[contains(@class,'author')]//h2",
    )
)
```

上面的例子从网页中抽取内容和作者, 抽取规则是针对糗事百科的内容页写的

> `field`在[configs详解——之field](https://doc.phpspider.org/configs-field.html)中作详细介绍。

## configs--fileds

`field`定义一个抽取项, 一个`field`可以定义下面这些东西

### `fields[].name`

> 给此项数据起个变量名  
> 变量名中不能包含.  
> 如果抓取到的数据想要以文章或者问答的形式发布到网站(WeCenter,  
> WordPress, Discuz!等), field的命名请参考两个完整demo中的命名, 否则无法发布成功

**String类型 不能为空**

举个栗子:

给`field`起了个名字叫`content`

```php
array(
    'name' => "content",
    'selector' => "//*[@id='single-next-link']"
)
```

### `fields[].selector`

> 定义抽取规则, 默认使用xpath  
> 如果使用其他类型的, 需要指定selector\_type

**String类型 不能为空**

举个栗子:  
使用xpath来抽取糗事百科的笑话内容，selector的值就是内容的xpath

```php
array(
    'name' => "content",
    'selector' => "//*[@id='single-next-link']"
)
```

### `fields[].selector_type`

抽取规则的类型

> 目前可用[xpath](http://www.w3school.com.cn/xpath/index.asp), [jsonpath](http://www.cnblogs.com/draem0507/p/5111002.html), [regex](http://www.runoob.com/regexp/regexp-tutorial.html)  
> 默认`xpath`

**枚举类型**

栗子1:  
selector默认使用xpath

```php
array(
    'name' => "content",
    'selector' => "//*[@id='single-next-link']" // xpath抽取规则
)
```

栗子2:  
使用正则表达式来抽取数据

```php
array(
    'name' => "content",
    'selector_type' => 'regex',
    'selector' => '#<div\sclass="content">([^/]+)</div>#i' // regex抽取规则
)
```

### `fields[].required`

> 定义该`field`的值是否必须, 默认false  
> 赋值为true的话, 如果该`field`没有抽取到内容, 该field对应的整条数据都将被丢弃

**布尔类型**

举个栗子:

```php
array(
    'name' => "content",
    'selector' => "//*[@id='single-next-link']",
    'required' => true
)
```

### `fields[].repeated`

> 定义该`field`抽取到的内容是否是有多项, 默认`false`  
> 赋值为true的话, 无论该`field`是否真的是有多项, 抽取到的结果都是数组结构

**布尔类型**

举个栗子:  
爬取的网页中包含多条评论，所以抽取评论的时候要将repeated赋值为true

```php
array(
    'name' => "comments",
    'selector' => "//*[@id='zh-single-question-page']//a[contains(@class,'zm-item-tag')]",
    'repeated' => true
)
```

### `fields[].filter`

TODO

### `fields[].default`

TODO

### `fields[].children`

> 为此`field`定义子项  
> 子项的定义仍然是一个`fields`数组  
> 没错, 这是一个树形结构

**数组类型**

举个栗子:  
抓取糗事百科的评论，每个评论爬取了内容，点赞数

```php
array(
    'name' => "article_comments",
    'selector' => "//div[contains(@class,'comments-wrap')]",
    'children' => array(
        array(
            'name' => "replay",
            'selector' => "//div[contains(@class,'replay')]",
            'repeated' => true,
        ),
        array(
            'name' => "report",
            'selector' => "//div[contains(@class,'report')]",
            'repeated' => true,
        )
    )
)
```

### `fields[].source_type`

该field的数据源, 默认从当前的网页中抽取数据  
选择`attached_url`可以发起一个新的请求, 然后从请求返回的数据中抽取  
选择`url_context`可以从当前网页的url附加数据（点此查看“url附加数据”实例解析）中抽取

**枚举类型**

### `attached_url`

当source\_type设置为`attached_url`时, 定义新请求的url

**String类型**

举个栗子:  
当爬取的网页中某些内容需要异步加载请求时，就需要使用attached\_url，比如，抓取知乎回答中的评论部分，就是通过AJAX异步请求的数据

```php
array(
    'name' => "comment_id",
    'selector' => "//div/@data-aid",
),
array(
    'name' => "comments",
    'source_type' => 'attached_url',
    // "comments"是从发送"attached_url"这个异步请求返回的数据中抽取的
    // "attachedUrl"支持引用上下文中的抓取到的"field", 这里就引用了上面抓取的"comment_id"
    'attached_url' => "https://www.zhihu.com/r/answers/{comment_id}/comments",
    'selector_type' => 'jsonpath'
    'selector' => "$.data",
    'repeated => true,
    'children' => array(
        ...
    )
}
```

## configs--requests

`requests`表示当前正在爬取的网站的对象，下面介绍了可以调用的函数

### requests成员

### `input_encoding`

> 输入编码  
> 明确指定输入的页面编码格式(UTF-8,GB2312,…..)，防止出现乱码,如果设置null则自动识别

**String类型 可选设置**

**input\_encoding默认值为null，即程序自动识别页面编码**

举个栗子:

```php
requests::$input_encoding = 'GB2312';
```

### `output_encoding`

> 输出编码  
> 明确指定输出的编码格式(UTF-8,GB2312,…..)，防止出现乱码,如果设置null则为utf-8

**String类型 可选设置**

**output\_encoding默认值为utf-8, 如果数据库为gbk编码，请修改为gb2312**

举个栗子:

```php
requests::$output_encoding = 'GB2312';
```

### requests方法

### `set_timeout($timeout)`

一般在`on_start`回调函数（在[爬虫进阶开发——之回调函数](https://doc.phpspider.org/callback.html)中会详细描述）中调用, 设置请求超时时间

> @param $timeout 需添加的timeout

**默认值为5，即5秒超时**

栗子1:  
传入单一的值作为timeout，会同时设置connect和read

```php
$spider->on_start = function($phpspider) 
{
    requests::set_timeout(10);
};
```

栗子2：  
传入数组，会分别设置connect和read二者的timeout

```php
requests::set_timeout( array(3, 27) );
```

栗子3：  
如果远端服务器很慢，你可以让requests永远等待，传入一个 0 作为 timeout 值，然后就冲咖啡去吧。

```php
requests::set_timeout(0);
```

### `set_proxy($proxy)`

一般在`on_start`回调函数（在[爬虫进阶开发——之回调函数](https://doc.phpspider.org/callback.html)中会详细描述）中调用, 设置请求代理

> @param $proxy 需添加的代理，用于破解防采集，支持字符串和数组类型传入

栗子1：

字符串类型

```php
$spider->on_start = function($phpspider) 
{
    requests::set_proxy('http://user:pass@host:port);
};
```

栗子2:

数组类型，代理有多个

```php
$spider->on_start = function($phpspider) 
{
    // 代理如果有多个，请求时会随机采用
    $proxy = array(
        'http://user1:pass1@host:port',
        'http://user2:pass2@host:port',
    );
    requests::set_proxy($proxy);
};
```

### `set_useragent($useragent)`

一般在`on_start`回调函数（在[爬虫进阶开发——之回调函数](https://doc.phpspider.org/callback.html)中会详细描述）中调用, 设置浏览器useragent

> @param $useragent 需添加的useragent

**默认使用useragent: requests/2.0.0**

[点击查看“常见浏览器useragent大全”](http://taro.iteye.com/blog/1634253)

栗子1:

```php
$spider->on_start = function($phpspider) 
{
    requests::set_useragent("Mozilla/4.0 (compatible; MSIE 6.0; ) Opera/UCWEB7.0.2.37/28/");
};
```

栗子2:

随机伪造useragent，传递数组即可，爬虫在请求的时候就会随机取一个useragent访问对方网站，让对方网站对useragent的反爬虫限制失

```php
$spider->on_start = function($phpspider) 
{
    requests::set_useragent(array(
        "Mozilla/4.0 (compatible; MSIE 6.0; ) Opera/UCWEB7.0.2.37/28/",
        "Opera/9.80 (Android 3.2.1; Linux; Opera Tablet/ADR-1109081720; U; ja) Presto/2.8.149 Version/11.10",
        "Mozilla/5.0 (Android; Linux armv7l; rv:9.0) Gecko/20111216 Firefox/9.0 Fennec/9.0"
    ));
};
```

### `set_referer($referer)`

一般在`on_start`回调函数（在[爬虫进阶开发——之回调函数](https://doc.phpspider.org/callback.html)中会详细描述）中调用, 设置请求来路URL

> @param $referer 需添加的来路URL，用于破解防采集

举个栗子:

```php
$spider->on_start = function($phpspider) 
{
    requests::set_referer("http://www.qiushibaike.com");
};
```

一般在`on_start`回调函数（在[爬虫进阶开发——之回调函数](https://doc.phpspider.org/callback.html)中会详细描述）中调用, 用来添加一些HTTP请求的Header

> @param $key Header的key, 如User-Agent,Referer等  
> @param $value Header的值

举个栗子:  
Referer是HTTP请求Header的一个属性，`http://www.9game.cn/kc/`是Referer的值

```php
$spider->on_start = function($phpspider) 
{
    requests::set_header("Referer", "http://www.9game.cn/kc/");
};
```

### `set_cookie($key, $value, $domain='')`

一般在`on_start`回调函数（在[爬虫进阶开发——之回调函数](https://doc.phpspider.org/callback.html)中会详细描述）中调用, 用来添加一些HTTP请求的Cookie

> @param $key Cookie的key  
> @param $value Cookie的值  
> @param $domain 默认放到全局Cookie，设置域名后则放到相应域名下

举个栗子:  
cookie是由键-值对组成的，BAIDUID是cookie的key，FEE96299191CB0F11954F3A0060FB470:FG=1则是cookie的值

```php
$spider->on_start = function($phpspider) 
{
    requests::set_cookie("BAIDUID", "FEE96299191CB0F11954F3A0060FB470:FG=1");
    // 把Cookie设置到 www.phpspider.org 域名下
    requests::set_cookie("NAME", "phpspider", "www.phpspider.org");
};
```

### `get_cookie($name, $domain = '')`

一般在`on_start`回调函数（在[爬虫进阶开发——之回调函数](https://doc.phpspider.org/callback.html)中会详细描述）中调用, 用来得到某个域名所附带的某个Cookie

> @param $name Cookie的名称  
> @param $domain configs的domains成员中的元素

举个栗子:  
得到`s.weibo.com`域名所附带的`Cookie`, 并将`Cookie`添加到`weibo.com`的域名中

```php
$configs = array(
    'domains' => array(
        's.weibo.com',
        'weibo.com'
    )
    // configs的其他成员
    ...
);

$spider->on_start = function($phpspider) 
{
    $cookie = requests::get_cookie("SUB", "s.weibo.com");
    // 把Cookie设置到 weibo.com 域名下
    requests::set_cookie("SUB", $cookie, "weibo.com");
};
```

### `set_cookies($cookies, $domain='')`

一般在`on_start`回调函数（在[爬虫进阶开发——之回调函数](https://doc.phpspider.org/callback.html)中会详细描述）中调用, 用来添加一些HTTP请求的Cookie

> @param $cookies 多个Cookie组成的字符串  
> @param $domain 默认放到全局Cookie，设置域名后则放到相应域名下

举个栗子:  
cookies是多个cookie的键-值对组成的字符串，用;分隔。BAIDUID和BIDUPSID是cookie的key，FEE96299191CB0F11954F3A0060FB470:FG=1和FEE96299191CB0F11954F3A0060FB470是cookie的值，键-值对用=相连

```php
$spider->on_start = function($phpspider) 
{
    requests::set_cookies("BAIDUID=FEE96299191CB0F11954F3A0060FB470:FG=1; BIDUPSID=FEE96299191CB0F11954F3A0060FB470;");
    // 把Cookie设置到 www.phpspider.org 域名下
    requests::set_cookies("NAME", "www.phpspider.org");
};
```

### `get_cookies($domain = '')`

一般在`on_start`回调函数（在[爬虫进阶开发——之回调函数](https://doc.phpspider.org/callback.html)中会详细描述）中调用, 用来得到某个域名所附带的所有Cookie

> @param $domain configs的domains成员中的元素  
> @return array 返回的是所有Cookie的数组

举个栗子:  
得到`s.weibo.com`域名所附带的`Cookie`, 并将`Cookie`添加到`weibo.com`的域名中

```php
$configs = array(
    'domains' => array(
        's.weibo.com',
        'weibo.com'
    )
    // configs的其他成员
    ...
);

$spider->on_start = function($phpspider) 
{
    $cookies = requests::get_cookies("s.weibo.com");
    // 返回的是数组，可以输出看看所有的Cookie内容
    print_r($cookies);
    // 数组转化成String
    $cookies = implode(";", $cookies);
    // 把Cookie设置到 weibo.com 域名下
    requests::set_cookies($cookies, "weibo.com");

};
```

### `set_client_ip($ip)`

一般在`on_start`回调函数（在[爬虫进阶开发——之回调函数](https://doc.phpspider.org/callback.html)中会详细描述）中调用, 设置请求伪IP

> @param $ip 需添加的伪IP

栗子1:

```php
$spider->on_start = function($phpspider) 
{
    requests::set_client_ip("192.168.0.2");
};
```

栗子2:

随机伪造IP，只需要添加数组即可

```php
$spider->on_start = function($phpspider) 
{
    $ips = array(
        "192.168.0.2",
        "192.168.0.3",
        "192.168.0.4"
    );
    requests::set_client_ip($ips);
};
```

### `set_hosts($host, $ips)`

一般在`on_start`回调函数（在[爬虫进阶开发——之回调函数](https://doc.phpspider.org/callback.html)中会详细描述）中调用, 设置请求的第三方主机和IP

> @param $hosts 需添加的主机和IP，用于采集第三方不同的服务器

举个栗子:

```php
$spider->on_start = function($phpspider) 
{
    $host = "www.qiushibaike.com";
    $ips = array(
        "203.195.143.21",
        "203.195.143.22"
    );
    requests::set_hosts($host, $ips);
};
```

### `get($url, $params, $allow_redirects, $cert)`

可以在任何地方调用, 用来获取某个网页

> @param $url 请求URL  
> @param $params 请求参数  
> @param $allow\_redirects 是否允许获取跳转后的页面  
> @param $cert 证书

举个栗子:  
获取 Github 的公共时间线

```php
$json = requests::get("https://github.com/timeline.json");
$data = json_decode($json, true);
print_r($data);
```

### `post($url, $params, $files, $allow_redirects, $cert)`

可以在任何地方调用, 用来获取某个网页

> @param $url 请求URL  
> @param $params 请求参数  
> @param $files 上传文件  
> @param $allow\_redirects 是否允许获取跳转后的页面  
> @param $cert 证书

栗子1:  
用户登录

```php
$params = array(
    'username' => 'test888',
    'password' => '123456',
);
$html = requests::post("http://www.domain.com", $params);
```

栗子2：  
文件上传

```php
$files = array(
    'file1' => 'test.jpg',
    'file2' => 'test.png'
);
$html = requests::post("http://www.domain.com", NULL, $files);
```

服务端代码

```php
if ($_FILES["file1"]["error"] > 0)
{
    echo "错误：" . $_FILES["file1"]["error"] . "<br\>";
}
else
{
    echo "上传文件名: " . $_FILES["file1"]["name"] . "<br\>";
    echo "文件类型: " . $_FILES["file1"]["type"] . "<br\>";
    echo "文件大小: " . ($_FILES["file1"]["size"] / 1024) . " kB<br\>";
    echo "文件临时存储的位置: " . $_FILES["file1"]["tmp_name"];
}
```

### `put($url, $params, $allow_redirects, $cert)`

可以在任何地方调用, 用来获取某个网页

> @param $url 请求URL  
> @param $params 请求参数  
> @param $allow\_redirects 是否允许获取跳转后的页面  
> @param $cert 证书

举个栗子:  
添加用户 test888

```php
$params="{username:\"test888\",username:\"123456\"}";
$html = requests::put("http://www.domain.com", $params);
```

### `delete($url, $params, $allow_redirects, $cert)`

可以在任何地方调用, 用来获取某个网页

> @param $url 请求URL  
> @param $params 请求参数  
> @param $allow\_redirects 是否允许获取跳转后的页面  
> @param $cert 证书

举个栗子:  
删除用户 test888

```php
$params="{username:\"test888\"}";
$html = requests::delete("http://www.domain.com", $params);
```

### `获取网页编码`

可以使用 requests::$encoding 来获取网页编码

```php
requests::get("http://www.domain.com");
echo requests::$encoding;
// utf-8
```

当你发送请求时，requests会根据HTTP头部来猜测网页编码，当HTTP头部无法获取时，会继续用网页内容来继续猜测，当你使用 requests::$text 时，requests就会使用这个编码。当然你还可以修改reuqests的编码形式。

```php
requests::$output_encoding = 'gbk';
requests::get("http://www.domain.com");
echo requests::$encoding;
// gbk
```

像上面的例子，请求前先指定要输出的编码为gbk，获取到的网页内容就是gbk编码的内容。

### `json`

现在很多PHP环境都默认自带json扩展，并不需要像python、golang那样需要去引入新模块，下面是查询IP的API 例子

```php
$json = requests::get('http://ip.taobao.com/service/getIpInfo.php?ip=122.88.60.28');
$rs = json_decode($json, true);
echo $rs['data']['country'];
// 中国
```

### `获取响应内容`

可通过 requests::$content、requests::$text 来获取转码前后网页的内容。

```php
requests::get("http://www.domain.com");
// 转码前内容
echo requests::$content;
// 转码后内容
echo requests::$text;
```

### `网页状态码`

可以用 requests::$status\_code 来检查网页的状态码

```php
requests::get("http://www.domain.com");
// 状态码
echo requests::$status_code;
// 如果是302跳转，获取跳转前状态码
echo requests::$history;
```

### `响应头内容`

可以通过 requests::$headers 来获取响应头内容

```php
requests::get("http://www.domain.com");
print_r(requests::$headers);
echo requests::$headers['Content-Type'];
```

### `请求头内容`

可以通过 requests::$request\['headers'\] 来获取请求头内容。

```php
requests::get("http://www.domain.com");
print_r(requests::$request['headers']);
```

### `自定义请求头部`

伪装请求头部是采集时经常用的，我们可以用这个方法来隐藏：

```php
requests::get("http://www.domain.com")
print_r(requests::$request['headers']['User-Agent']);
// php-requests/1.2.3 PHP/5.6 Windows/XP

requests::$request['headers']['User-Agent'] = 'phpspider';
requests::get("http://www.domain.com");
print_r(requests::$request['headers']['User-Agent']);
// phpspider
```

### `curl`

curl -X PUT [http://www.domain.com/demo.php](http://www.domain.com/demo.php) -d "id=1" -d "title=a"

## configs--selector

`selector`是页面元素选择器类，下面介绍此类可以调用的方法

### `select($html, $selector, $selector_type = 'xpath')`

> @param $html 需筛选的网页内容  
> @param $selector 选择器规则  
> @param $selector\_type 选择器类型: xpath、regex、css, 默认为xpath选择类型

栗子1:

通过xpath选择器提取网页内容的标题

```php
$html = requests::get(<span>"http://www.epooll.com/archives/806/"</span>);
$data = selector::select($html, <span>"//div[contains(@class,'page-header')]//h1//a"</span>);
var_dump($data);
```

栗子2:

通过css选择器提取网页内容的标题

```php
$html = requests::get(<span>"http://www.epooll.com/archives/806/"</span>);
$data = selector::select($html, <span>".page-header > h1 > a"</span>, <span>"css"</span>);
var_dump($data);
```

栗子3:

通过正则匹配提取网页内容的标题

```php
$html = requests::get(<span>"http://www.epooll.com/archives/806/"</span>);
$data = selector::select($html, <span>'@<title>(.*?)</title>@'</span>, <span>"regex"</span>);
var_dump($data);
```

### `remove($html, $selector, $selector_type = 'xpath')`

> @param $html 需过滤的网页内容  
> @param $selector 选择器规则  
> @param $selector\_type 选择器类型: xpath、regex、css, 默认为xpath选择类型

举个例子:

```php
$html =<span><<<STR
   <div id="demo">
        aaa
       <span class="tt">bbb</span>
       <span>ccc</span>
       <p>ddd</p>
   </div>
STR;</span>

<span>// 获取id为demo的div内容</span>
$html = selector::select($html, <span>"//div[contains(@id,'demo')]"</span>);
<span>// 在上面获取内容基础上，删除class为tt的span标签</span>
$data = selector::remove($html, <span>"//span[contains(@class,'tt')]"</span>);
print_r($data);
```

## configs--db

**本节介绍db类用法**

## 数据库配置链接

```php
$db_config = array(
    'host'  => '127.0.0.1',
    'port'  => 3306,
    'user'  => 'root',
    'pass'  => 'root',
    'name'  => 'qiushibaike',
);
// 数据库配置
db::set_connect('default', $db_config);
// 数据库链接
db::init_mysql();
```

## 原生SQL操作

### `query($sql)`

举个栗子:

```php
// 查询
$rsid = db::query("Select * From `content`");
while ( $row = db::fetch($rsid) )
{
    echo "id = {$row['id']}; name = {$row['name']}\n";
}

// 新增
db::query("Insert Into `content`(`name`) Value('test'));

// 更新
db::query("Update `content` Set `name`='test' Where `id`=1");

// 删除
db::query("Delete From `content` Where `id`='1'");
```

## CRUD操作

### `get_one($sql)`

**单条查询**

举个栗子:

```php
$row = db::get_one("Select * From `content` Where `id`='1'");
```

### `get_all($sql)`

**多条查询**

举个栗子:

```php
$rows = db::get_all("Select * From `content` Limit 5");
```

### `insert($table, $data)`

**单条插入**

举个栗子:

```php
$data = array(
    'name' => 'test',
    'url'  => 'http://www.baidu.com'
);
$rows = db::insert('content', $data);
```

### `insert_batch($table, $data)`

**单条修改**

举个栗子:

```php
$data = array(
    array(
        'name' => 'test111',
        'url'  => 'http://www.baidu.com'
    ),
    array(
        'name' => 'test222',
        'url'  => 'http://www.baidu.com'
    ),
);
$rows = db::insert_batch('content', $data);
```

### `update_batch($table, $data, $index)`

**批量修改**

举个栗子:

```php
$data = array(
    array(
        'name' => 'test111',
        'url'  => 'http://www.baidu.com'
    ),
    array(
        'name' => 'test222',
        'url'  => 'http://www.baidu.com'
    ),
);
// 以name为条件进行修改
$rows = db::update_batch('content', $data, "name");
```

### `delete($table, $where)`

**单条删除**

举个栗子:

```php
$rows = db::delete('content', "`id`='1'");
```

## configs--log

## 内置方法

**本节介绍爬虫的内置方法**

### `add_url($url, $options = array())`

一般在`on_scan_page`和`on_list_page`回调函数（在[爬虫进阶开发——之回调函数](https://doc.phpspider.org/callback.html)中会详细描述）中调用, 用来往待爬队列中添加url

> @param $url 待添加的url  
> @param $options 成员包括method, headers, params, context\_data, reserve和 proxy, 如下所示:
>
> > @param $options\['method'\] 默认为"get"请求, 也支持"post"请求  
> > @param $options\['headers'\] 此url的Headers, 可以为空  
> > @param $options\['params'\] 发送请求时需添加的参数, 可以为空  
> > @param $options\['context\_data'\] 此url附加的数据, 比如内容页需要列表页一些数据, 可以为空  
> > @param $options\['proxy'\] 访问此url时使用的代理服务器，不使用请留空

栗子1:

```php
$spider->on_scan_page = function($page, $content, $phpspider) 
{
    $regex = "#http://pic.qiushibaike.com/system/pictures/\d+#";
    $urls = array();
    preg_match_all($regex, $content, $out);
    $urls = empty($out[0]) ? array() : $out[0];
    if (!empty($urls)) {
        foreach ($urls as $url) 
        {
            $phpspider->add_url($url);
        }
    }
    ...
    return false;
};
```

### `add_scan_url($url, $options = array())`

一般在`on_start`回调函数（在[爬虫进阶开发——之回调函数](https://doc.phpspider.org/callback.html)中会详细描述）中调用, 用来往待爬队列中添加scan\_url

> @param $url 待添加的scan\_url  
> @param $options 成员包括method, headers, params, context\_data, reserve和 proxy, 如下所示:
>
> > @param $options\['method'\] 默认为"get"请求, 也支持"post"请求  
> > @param $options\['headers'\] 此url的Headers, 可以为空  
> > @param $options\['params'\] 发送请求时需添加的参数, 可以为空  
> > @param $options\['context\_data'\] 此url附加的数据, 比如内容页需要列表页一些数据, 可以为空  
> > @param $options\['proxy'\] 访问此url时使用的代理服务器，不使用请留空

举个栗子:

```php
$spider->on_start = function($page, $content, $phpspider) 
{
    for ($i = 0; $i< 10; $i++) 
    {
        $phpspider->add_scan_url("http://www.qiushibaike.com/page.php?page=".$i);
    }
};
```

栗子2:

```php
$spider->on_list_page = function($page, $content, $phpspider) 
{
    ...
    $next_url = str_replace($page['url'], "page=".$current_page_num, "page=".$page_num);
    $options = array(
        'method' => 'post',
        'params' => array(
            'page' => $page_num,
            'size' => 18
        )
    );
    $phpspider->add_url($next_url, $options);
    return false;
};
```

### `request_url($url, $options = array())`

一般在`on_start`,`on_download_page`,`on_scan_page`和`on_list_page`回调函数（在[爬虫进阶开发——之回调函数](https://doc.phpspider.org/callback.html)中会详细描述）中调用, 下载网页, 得到网页内容

> @param $url 待添加的url  
> @param $options 成员包括method, headers, params, context\_data, reserve 和 proxy, 如下所示:
>
> > @param $options\['method'\] 默认为"get"请求, 也支持"post"请求  
> > @param $options\['headers'\] 此url的Headers, 可以为空  
> > @param $options\['params'\] 发送请求时需添加的参数, 可以为空  
> > @param $options\['context\_data'\] 此url附加的数据, 比如内容页需要列表页一些数据, 可以为空  
> > @param $options\['proxy'\] 访问此url时使用的代理服务器，不使用请留空

举个栗子:

```php
$spider->on_download_page = function($page, $phpspider) 
{
    $url = "https://checkcoverage.apple.com/cn/zh/?sn=FK1QPNCEGRYD";
    $options = array(
        'method' => 'post',
        'params' => array(
            'sno' => 'FK1QPNCEGRYD',
            'CSRFToken' => $result[1]
        )
    );
    // 通过发送带参数的post请求，下载网页，并将网页内容赋值给result
    $request = $phpspider->request_url($url, $options);
    ...
    return $page;
};
```

## 回调函数

**回调函数是在爬虫爬取并处理网页的过程中设置的一些系统钩子, 通过这些钩子可以完成一些特殊的处理逻辑.**

下图是PHP蜘蛛爬虫爬取并处理网页的流程图，矩形方框中标识了爬虫运行过程中所使用的重要回调函数：

![](https://doc.phpspider.org/pachongliuchengtu.png)

### `on_start($phpspider)`

**爬虫初始化时调用, 用来指定一些爬取前的操作**

> @param $phpspider 爬虫对象

在函数中可以调用`requests::set_header($key,$value)`, `requests::set_cookie($key,$value)`,`requests::set_cookies($cookies)`等

举个栗子:

在爬虫开始爬取之前给所有待爬网页添加一个Header

```php
$spider->on_start = function($phpspider) 
{
    requests::set_header("Referer", "http://buluo.qq.com/p/index.html");
};
```

### `on_status_code($status_code, $url, $content, $phpspider)`

**判断当前网页是否被反爬虫了, 需要开发者实现**

> @param $status\_code 当前网页的请求返回的HTTP状态码  
> @param $url 当前网页URL  
> @param $content 当前网页内容  
> @param $phpspider 爬虫对象  
> @return $content 返回处理后的网页内容，不处理当前页面请返回false

举个栗子:

```php
$spider->on_status_code = function($status_code, $url, $content, $phpspider) 
{
    // 如果状态码为429，说明对方网站设置了不让同一个客户端同时请求太多次
    if ($status_code == '429') 
    {
        // 将url插入待爬的队列中,等待再次爬取
        $phpspider->add_url($url);
        // 当前页先不处理了
        return false; 
    }
    // 不拦截的状态码这里记得要返回，否则后面内容就都空了
    return $content;
};
```

### `is_anti_spider($url, $content, $phpspider)`

**判断当前网页是否被反爬虫了, 需要开发者实现**

> @param $url 当前网页的url  
> @param $content 当前网页内容  
> @param $phpspider 爬虫对象  
> @return 如果被反爬虫了, 返回true, 否则返回false

举个栗子:

```php
$spider->is_anti_spider = function($url, $content, $phpspider) 
{
    // $content中包含"404页面不存在"字符串
    if (strpos($content, "404页面不存在") !== false) 
    {
        // 如果使用了代理IP，IP切换需要时间，这里可以添加到队列等下次换了IP再抓取
        // $phpspider->add_url($url);
        return true; // 告诉框架网页被反爬虫了，不要继续处理它
    }
    // 当前页面没有被反爬虫，可以继续处理
    return false;
};
```

### `on_download_page($page, $phpspider)`

**在一个网页下载完成之后调用. 主要用来对下载的网页进行处理.**

> @param $page 当前下载的网页页面的对象  
> @param $phpspider 爬虫对象  
> @return 返回处理后的网页内容
>
> > @param $page\['url'\] 当前网页的URL  
> > @param $page\['raw'\] 当前网页的内容  
> > @param $page\['request'\] 当前网页的请求对象

举个栗子:  
比如下载了某个网页，希望向网页的body中添加html标签，处理过程如下：

```php
$spider->on_download_page = function($page, $phpspider) 
{
    $page_html = "<div id=\"comment-pages\"><span>5</span></div>";
    $index = strpos($page['row'], "</body>");
    $page['raw'] = substr($page['raw'], 0, $index) . $page_html . substr($page['raw'], $index);
    return $page;
};
```

### `on_download_attached_page($content, $phpspider)`

**在一个网页下载完成之后调用. 主要用来对下载的网页进行处理.**

> @param $content 当前下载的网页内容  
> @param $phpspider 爬虫对象  
> @return 返回处理后的网页内容

举个栗子:  
比如下载的网页需去掉前后两边的中括号，并将处理后的数据返回，处理过程如下：

```php
$spider->on_download_attached_page = function($content, $phpspider) 
{
    $content = trim($content);
    $content = ltrim($content, "[");
    $content = rtrim($content, "]");
    $content = json_decode($content, true);
    return $content;
};
```

### `on_fetch_url($url, $phpspider)`

**在一个网页获取到URL之后调用. 主要用来对获取到的URL进行处理.**

> @param $url 当前获取到的URL  
> @param $phpspider 爬虫对象  
> @return 返回处理后的URL，为false则此URL不入采集队列

栗子1:  
如果获取到URL包含#filter，不入URL采集队列

```php
$spider->on_fetch_url = function($url, $phpspider) 
{
    if (strpos($url, "#filter") !== false)
    {
        return false;
    }
    return $url;
};
```

栗子2:  
把获取到URL的&替换成&，并将处理后的数据返回

```php
$spider->on_fetch_url = function($url, $phpspider) 
{
    $url = str_replace("&amp;amp;", "&amp;", $url);
    return $url;
};
```

### `on_scan_page($page, $content, $phpspider)`

**在爬取到入口url的内容之后, 添加新的url到待爬队列之前调用. 主要用来发现新的待爬url, 并且能给新发现的url附加数据（点此查看“url附加数据”实例解析）.**

> @param $page 当前下载的网页页面的对象  
> @param $content 当前网页内容  
> @param $phpspider 当前爬虫对象  
> @return 返回false表示不需要再从此网页中发现待爬url
>
> > @param $page\['url'\] 当前网页的URL  
> > @param $page\['raw'\] 当前网页的内容  
> > @param $page\['request'\] 当前网页的请求对象

此函数中通过调用$phpspider->add\_url($url, $options)函数来添加新的url到待爬队列。

栗子1:  
实现这个回调函数并返回false，表示爬虫在处理这个scan\_url的时候，不会从中提取待爬url

```php
$spider->on_scan_page = function($page, $content, $phpspider) 
{
    return false;
};
```

栗子2:  
生成一个新的url添加到待爬队列中，并通知爬虫不再从当前网页中发现待爬url

```php
$spider->on_scan_page = function($page, $content, $phpspider) 
{
    $array = json_decode($page['raw'], true);
    foreach ($array as $v) 
    {
        $lastid = $v['id'];
        // 生成一个新的url
        $url = $page['url'] . $lastid;
        // 将新的url插入待爬的队列中
        $phpspider->add_url($url);
    }
    // 通知爬虫不再从当前网页中发现待爬url
    return false;
};
```

### `on_list_page($page, $content, $phpspider)`

**在爬取到入口url的内容之后, 添加新的url到待爬队列之前调用. 主要用来发现新的待爬url, 并且能给新发现的url附加数据（点此查看“url附加数据”实例解析）.**

> @param $page 当前下载的网页页面的对象  
> @param $content 当前网页内容  
> @param $phpspider 当前爬虫对象  
> @return 返回false表示不需要再从此网页中发现待爬url
>
> > @param $page\['url'\] 当前网页的URL  
> > @param $page\['raw'\] 当前网页的内容  
> > @param $page\['request'\] 当前网页的请求对象

此函数中通过调用$phpspider->add\_url($url, $options)函数来添加新的url到待爬队列。

栗子1:  
实现这个回调函数并返回false，表示爬虫在处理这个scan\_url的时候，不会从中提取待爬url

```php
$spider->on_list_page = function($page, $content, $phpspider) 
{
    return false;
};
```

栗子2:  
生成一个新的url添加到待爬队列中，并通知爬虫不再从当前网页中发现待爬url

```php
$spider->on_list_page = function($page, $content, $phpspider) 
{
    $array = json_decode($page['raw'], true);
    foreach ($array as $v) 
    {
        $lastid = $v['id'];
        // 生成一个新的url
        $url = $page['url'] . $lastid;
        // 将新的url插入待爬的队列中
        $phpspider->add_url($url);
    }
    // 通知爬虫不再从当前网页中发现待爬url
    return false;
};
```

### `on_content_page($page, $content, $phpspider)`

**在爬取到入口url的内容之后, 添加新的url到待爬队列之前调用. 主要用来发现新的待爬url, 并且能给新发现的url附加数据（点此查看“url附加数据”实例解析）.**

> @param $page 当前下载的网页页面的对象  
> @param $content 当前网页内容  
> @param $phpspider 当前爬虫对象  
> @return 返回false表示不需要再从此网页中发现待爬url
>
> > @param $page\['url'\] 当前网页的URL  
> > @param $page\['raw'\] 当前网页的内容  
> > @param $page\['request'\] 当前网页的请求对象

此函数中通过调用$phpspider->add\_url($url, $options)函数来添加新的url到待爬队列。

栗子1:  
实现这个回调函数并返回false，表示爬虫在处理这个scan\_url的时候，不会从中提取待爬url

```php
$spider->on_content_page = function($page, $content, $phpspider) 
{
    return false;
};
```

栗子2:  
生成一个新的url添加到待爬队列中，并通知爬虫不再从当前网页中发现待爬url

```php
$spider->on_content_page = function($page, $content, $phpspider) 
{
    $array = json_decode($page['raw'], true);
    foreach ($array as $v) 
    {
        $lastid = $v['id'];
        // 生成一个新的url
        $url = $page['url'] . $lastid;
        // 将新的url插入待爬的队列中
        $phpspider->add_url($url);
    }
    // 通知爬虫不再从当前网页中发现待爬url
    return false;
};
```

### `on_handle_img($fieldname, $img)`

**在抽取到field内容之后调用, 对其中包含的img标签进行回调处理**

> @param $fieldname 当前field的name. 注意: 子field的name会带着父field的name, 通过.连接.  
> @param $img 整个img标签的内容  
> @return 返回处理后的img标签的内容

**很多网站对图片作了延迟加载, 这时候就需要在这个函数里面来处理**

举个栗子:  
汽车之家论坛帖子的图片大部分是延迟加载的，默认会使用`http://x.autoimg.cn/club/lazyload.png` 图片url，我们需要找到真实的图片url并替换，具体实现如下：

```php
$spider->on_handle_img = function($fieldname, $img) 
{
    $regex = '/src="(https?:\/\/.*?)"/i';
    preg_match($regex, $img, $rs);
    if (!$rs) 
    {
        return $img;
    }
    $url = $rs[1];
    if ($url == "http://x.autoimg.cn/club/lazyload.png") 
    {
        $regex2 = '/src9="(https?:\/\/.*?)"/i';
        preg_match($regex, $img, $rs);
        // 替换成真是图片url
        if (!$rs) 
        {
            $new_url = $rs[1];
            $img = str_replace($url, $new_url);
        }
    }
    return $img;
};
```

当一个field的内容被抽取到后进行的回调, 在此回调中可以对网页中抽取的内容作进一步处理

> @param $fieldname 当前field的name. 注意: 子field的name会带着父field的name, 通过.连接.  
> @param $data 当前field抽取到的数据. 如果该field是repeated, data为数组类型, 否则是String  
> @param $page 当前下载的网页页面的对象  
> @return 返回处理后的数据, 注意数据类型需要跟传进来的`$data`类型匹配
>
> > @param $page\['url'\] 当前网页的URL  
> > @param $page\['raw'\] 当前网页的内容  
> > @param $page\['request'\] 当前网页的请求对象

举个栗子:  
比如爬取知乎用户的性别信息, 相关网页源码如下:

`<span class="item gender" ><i class="icon icon-profile-male"></i></span>`  
那么可以这样写:

```php
$configs = array(
    // configs的其他成员
    'fields' => array(
        array(
            'name' => "gender",
            'selector' => "//span[contains(@class, 'gender')]/i/@class",
        ),
        ...
    )
);

$spider->on_extract_field = function($fieldname, $data, $page) 
{
    if ($fieldname == 'gender') 
    {
        // data中包含"icon-profile-male"，说明当前知乎用户是男性
        if (strpos($data, "icon-profile-male") !== false) 
        {
            return "男";
        }
        // data中包含"icon-profile-female"，说明当前知乎用户是女性
        elseif (strpos($data, "icon-profile-female") !== false) 
        {
            return "女";
        }
        else 
        {
            return "未知"; 
        }
    }
    return $data;
};
```

在一个网页的所有field抽取完成之后, 可能需要对field进一步处理, 以发布到自己的网站

> @param $page 当前下载的网页页面的对象  
> @param $data 当前网页抽取出来的所有field的数据  
> @return 返回处理后的数据, 注意数据类型需要跟传进来的`$data`类型匹配
>
> > @param $page\['url'\] 当前网页的URL  
> > @param $page\['raw'\] 当前网页的内容  
> > @param $page\['request'\] 当前网页的请求对象

举个栗子:  
比如从某网页中得到time和title两个field抽取项, 现在希望把time的值添加中括号后拼凑到title中，处理过程如下：

```php
$spider->on_extract_page = function($page, $data) 
{
    $title = "[{$data['time']}]" . $data['title'];
    $data['title'] = $title;
    return $data;
    // 返回false不处理，当前页面的字段不入数据库直接过滤
    // 比如采集电影网站，标题匹配到“预告片”这三个字就过滤
    //if (strpos($data['title'], "预告片") !== false)
    //{
    //    return false;
    //}
};
```

## xpath选择器常见用法

俗话说，工欲上其事，必先利其器，学好xpath选择器，能极高的提升在爬虫的数据提取环节中的提取速度，下面我们来认识认识xpath。

**选取节点**

XPath 使用路径表达式在 XML 文档中选取节点。节点是通过沿着路径或者 step 来选取的。

**下面列出了最有用的路径表达式**

| 表达式 | 描述 |
| --- | --- |
| nodename | 选取此节点的所有子节点。 |
| / | 从根节点选取。 |
| // | 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置。 |
| . | 选取当前节点。 |
| .. | 选取当前节点的父节点。 |
| @ | 选取属性。 |

**实例**

**1、精确查询**

```php
$html =<<<STR
   <div id="demo">
       <span class="tt">bbb</span>
       <span>ccc</span>
       <p rel="pnode">ddd</p>
   </div>
STR;

// 获取id为demo的div内容
$data = selector::select($html, "//div[@id='demo']");
// 获取class为tt的span内容
$data = selector::select($html, "//div[@class='tt']");
// 获取rel为pnode的p内容
$data = selector::select($html, "//div[@rel='pnode']");
```

**2、模糊查询**

contains 匹配一个属性值中包含的字符串

```php
$html =<<<STR
   <div id="demo1">
        demo1
   </div>
   <div id="demo2">
        demo2
   </div>
STR;

// 查找id属性中包含demo关键字的页面元素
// 这里能获取id为demo1和demo2的内容
$data = selector::select($html, "//div[contains(@id,'demo')]");
```

**3、获取节点属性**

```php
$html =<<<STR
   <td data-value="3.80">3.80</td>    
   <td data-value="3.80">3.80</td>    
   <td data-value="3.80">3.80</td>    
   <td data-value="3.80">3.80</td>    
STR;

// 获取 td 的 data-value 属性
$data = selector::select($html, "//td@data-value");
```

**XPATH的几个常用函数**

1.contains ()： //div\[contains(@id, 'in')\] ,表示选择id中包含有’in’的div节点

2.text()：由于一个节点的文本值不属于属性，比如`<a class=”baidu“ href=”http://www.baidu.com“>baidu</a>`,所以，用text()函数来匹配节点：//a\[text()='baidu'\]

3.last()：//div\[contains(@id, 'in')\]\[las()\]，表示选择id中包含有'in'的div节点的最后一个节点

4.starts-with()： //div\[starts-with(@id, 'in')\] ，表示选择以’in’开头的id属性的div节点

5.not()函数，表示否定，//input\[@name=‘identity’ and not(contains(@class,‘a’))\] ，表示匹配出name为identity并且class的值中不包含a的input节点。 not()函数通常与返回值为true or false的函数组合起来用，比如contains(),starts-with()等，但有一种特别情况请注意一下：我们要匹配出input节点含有id属性的，写法如下：//input\[@id\]，如果我们要匹配出input节点不含用id属性的，则为：//input\[not(@id)\]
