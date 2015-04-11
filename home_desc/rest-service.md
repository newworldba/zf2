#rest-service创建#
=============
rest-service : https://apigility.org/documentation/intro/first-rest-service


##创建一个Rest服务##
---------------

这个章节中，我们将会创建一个简单的REST服务

##假设##
---------------
这个章节假设你已经阅读并且跟着做了"installation guide"和“getting started chapter”所交代的事情。 如果没有，还是去重复以前的章节要求

你需要安装并且配置"zfcampus/statuslib-example"模块来引导安装创建api。步骤如下：

##第一步：##
---------------
在application 根目录下，执行如下操作
$ php composer.phar require "zfcampus/statuslib-example:~1.0-dev"

##第二步：##
---------------
编辑config/application.config.php  增加StatusLib模块
array(
	'modules' => array(
		'StatusLib',
	)
)

##第三步##
---------------
创建一个php文件“data/statuslib.php”,返回一个空数组即可
<?PHP
return array();
一定要确认这个文件是可写的

##第四步##
---------------
编辑config/autoload/local.php增加下面的配置
return array(
	'statuslib'=>array(
		'array_mapper_path'=>'data/statuslib.php',
	)
)

##第五步##
---------------
最后,您将需要一个有效的HTTP基本认证文件,通常名为htpasswd。你可以生成一个使用Apache提供的标准htpasswd工具,或者使用一个在线htpasswd文件，将htpasswd放在data/htpasswd目录下，然后就可以使用这个东西了
一旦这些步骤都做完了，就可以继续下面的教程了。

##专业术语##
---------------
apigility或者下面的部分中，都会用到的一些专业术语

###Entity###
---------------
	一个将会返回信息东西，Entities由唯一的标识URI定义。
###Collection###
---------------
	一个设置entities的东西，通常在collection中包含的所有entities是同一个类型，将共用一个基本的uri
###Resource###
---------------
	接收请求参数，确定collection和entities是否能被uri所解析,并确定要执行什么操作
###Relational Links###
---------------	一个描述关系的资源URI，关系连接允许你描述不同的entities和collections之间的区别，你也可以直接链接到这个地方，以便服务客户端可以在这些关系上执行操作，这些有时也叫做hypermedia links
REST服务会返回entities和collections,还提供entities和collection中间的超媒体链接，资源对象协调操作后，返回entities和collections

##创建一个rest service##
---------------
这个章节中我们将会创建一个简单的rest service
进入APIs显示页，我们在前一个章节中创建了“Status”API，接下来悬着个这个“REST Service”菜单项
![Alt text](https://apigility.org/apigility-documentation/img/intro-first-rest-service-new-rest-service.png "点击创建“Create New REST Service”按钮") 
##点击创建“Create New REST Service”按钮##
---------------
![RESt Service](https://apigility.org/apigility-documentation/img/intro-first-rest-service-new-rest-service.png)
接下来的对话框中有两个tabs，一个似乎创建“Code-Connected”服务，另一个是创建"Db-Connected"服务

##Code-Connected 和 DB-Connected service##
---------------
当你创建一个Code-Connected服务是，apigility创建了一个资源类来定义所有可以在rest服务中被操作的变量，这些操作会返回“405 Method Not Allowed”响应，你把这些写到代码中就不会出现这种情况了。

接下来，我们联系一下，我们创建一个"Code-Connected service",在“REST Service Name”选项中，我们输入Status，点击‘Create Code-Connected REST Service’按钮。这个服务被成功的创建后，会出现一个有颜色的栏，点击后可以查看这个服务。
[alt](https://apigility.org/apigility-documentation/img/intro-first-rest-service-status-settings.png "REST Service Name")

Apigility 提供了一组完善的默认项：
* Collections 只允许GET(查询一个列表) 和 POST（创建一个新的实体）项
* entities 允许GET(查询一个实体),PUT(替换一个实体)PATCH(执行部分更新)，和DELETE（移除一个实体）选项
* 如果你的collection支持分页，apiligity 将会默认每页显示25个item
* Apigility 会以这个服务名称创建一个基础路由（/status[/:status_id]）

##Uri 路由##
Apigility会在Zend Framework 2 MVC执行的最开始运行，因此就会用到它的路由引擎
apigility执行的路由都是segment路由，segment路由允许你做如下东西：
* 可选部分用[]来表示
* 用:varName或者:var_name匹配命名参数
* 指定文字匹配，未命名的或者用括号({})的就是文字

对于REST service 执行的uri是一行文字，详细介绍强制匹配规则，去解决这个文档；上面的例子就想/status这个地址，他也有一个有命名参数的segment选项，我们叫他实体标识：[/:status_id]。这个将匹配像/status/foo,/status/2,/status/9999-aaaa的uri

有一个原则：这个文档uri和实体uri中的/只能指定是个实体时指定；你不能用一个斜线来请求一个collection

为什么呢？因为rest的一个原则就是一个uri，是一个资源。如果我们允许一个斜线，我们就是允许了若干个uri去解决相同名称的资源

我们对"REST Parameters"多说一些，你将看到一个值为Zend\Stdlib\Hydrator\ArraySerializable的“Hydrator Service Name”的块名称。我们可以用StatusLib实例包去改变这个工作方式

我们把鼠标悬浮在这个服务标题栏上就会发现一个绿色的“编辑”按钮，然后点击它。然后就会展开这个“REST Parameters”部分。对于这个"Hydrator Service Name"选择一个职位Zend\Stdlib\Hydrator\ObjectProperty
[alt](https://apigility.org/apigility-documentation/img/intro-first-rest-service-settings-edit.png "options")

##Hydractor##
Hydrators 允许一个写好的对象关联另一个数组对象，每个水合器都都使用不同的策略让他执行。apigility默认的水合器类型是用ArraySerializable类型，这个类型是一个有两个方法的接口：
1. getArrayCopy()是个array代表
2. exchangeArray($array)将一个数组转为一个对象

（这些在PHP数组对象中用的是相同的方法）
这个属性对象水合器将会期望一个对象公共属性，可以创建一个代表性数组合一个流行的数组对象

举个例子，这个StatusLib 库提供他自己的实体和文档类，点击"Service class Name"仪表盘标题栏，可以编辑这个"实体类"去去就可以看到StatusLib\Entity和Collection Class 去去可以看到StatusLib\Collection
[alt](https://apigility.org/apigility-documentation/img/intro-first-rest-service-settings-classes.png)
##服务类##
当我们创建一个Code-Connected 服务时，apigility会生为你成4个PHP类文件，
* 一个实体类
* 一个文档类继承Zend\Paginator\paginator，也将允许你去提供一个分页结果集
* 一个资源类可以执行操作
* 一个工厂类，可以创建资源类

你自己的代码已经可以定义你想要的实体类和文档类，你也可以自定义区忽略apigility创建的存根类。需要注意的一点是：如果你最后的版本api，你将用到的是最后的这个版本号的指定实体和文档类，当然你也可以做一些你期望的某一个版本的属性的逻辑。

现在选择这个绿色的保存按钮，
接下来，我们可以为这个api的一些区域定义了。
##为我们的服务定义##、
我们要定义的区域如下：
* message 一个状态描述，这不能为空，并且不能多于140个字符
* user 这个user规定了一个状态信息，他不能为空，还要增加一些特定的规则
* timestamp 是一个整形时间戳，这个不需要被提交，但是他必须是个数字常量，他需要每次都要返回。

我们也有一个idfiled，但是他的唯一的目的就是显示。

导航栏的区域标签和编辑服务标签（当悬浮在服务标题栏上就会出现一个绿色的编辑按钮）
[alt](https://apigility.org/apigility-documentation/img/intro-first-rest-service-fields-edit.png)

在有"field name"的输入框中，填写message，并且按下enter键，就可以创建一个新的field
[alt](https://apigility.org/apigility-documentation/img/intro-first-rest-service-message-field.png)

接下来，以同样的方式创建user 和 timestamp区域
[alt](https://apigility.org/apigility-documentation/img/intro-first-rest-service-all-fields.png)

点击绿色的标签，展开message
[alt](https://apigility.org/apigility-documentation/img/intro-first-rest-service-message-edit.png)

介绍选项中，填写“A status message of no more than 140 characters.”，确认错误信息里填写“A status message must contain between 1 and 140 characters.”
鼠标悬浮至Filter栏上，点击“增加过滤规则”按钮，会展示一个"增加过滤规则"表单。在下来列表中选择"Zend\Filter\StringTrim"(这个类型可以过滤)，完成后点击“添加过滤规则”按钮，然后完成再按取消按钮，就会取消这个表单
[alt](https://apigility.org/apigility-documentation/img/intro-first-rest-service-message-filter-trim.png)

现在悬浮在“验证”栏上，点击“添加验证”按钮，就会显示一个“增加验证规则”的按钮。在下拉选项框中选择Zend\Validator\StringLength（这个选项是String的选择），当完成后就按“添加验证”的按钮，然后可以在添加验证按钮之后按取消按钮来移除表单。
[alt](https://apigility.org/apigility-documentation/img/intro-first-rest-service-message-validator-length.png)
鼠标悬浮在Zend\Validator\StringLength的标题上，就会显示一个添加选项的按钮，点击之后就会有一个下来列表展示出来，选择max值，然后就会有一个输入框显示出来，然后再类型里面填写140，然后可以点击增加选项的按钮，当添加选项完成后需要移除表单，可以点击取消按钮。
[alt](https://apigility.org/apigility-documentation/img/intro-first-rest-service-message-validator-max.png)
接下来要注意了，在屏幕上你会看到如下的情况：
[alt](https://apigility.org/apigility-documentation/img/intro-first-rest-service-message-complete.png)

点击标题栏，message字段就会消失

接下来，，你可以练习下
* ##修改user字段##
** 添加“The user submitting the status message.”的介绍文字
** 添加确认失败信息“You must provide a valid user”
** 添加一个Zend\Filter\StringTrim过滤项
** 添加一个Zend\Validator\Regax 验证规则，增加一个正则匹配规则，/^(mwop|andi|zeev)$/(这个你可以自由发挥)
* ##修改timestamp##字段
** 增加“The timestamp when the status message was last modified."”介绍文字
** 增加“You must provide a timestamp.”失败确认文字
**　将命令标记的开关选择“No”
** 增加Zend\Validator\Digits验证规则

做完这些后，在最底部的右侧点击绿色的保存修改按钮，下面是user和timestamp都搞定之后的截屏。
[alt](https://apigility.org/apigility-documentation/img/intro-first-rest-service-user-complete.png)
[alt](https://apigility.org/apigility-documentation/img/intro-first-rest-service-timestamp-complete.png)
下面我们再来看看文本标签好了
##文本##
REST服务允许你不仅可以通过http来编写文档，也可以为每个文档和实体的http方法来编写
写REST服务文档的方法就想我们在Gettign Started 章节中学到的，只有两个不同点：
* 你可以为集合和实体都编写http方法文档
* 一些方法也可以定义一些请求数据，因此你也需要为请求数据编写文档

“配置执行”按钮为请求和响应做了介绍，假设你已经为你的字段编写了文档，通常会有适当的负载。你会注意到响应负载中包含了_links，有时也会包含——embedded成员。这是因为apigility中的REST服务会默认用到超媒体应用语言格式，他为你的服务提供了一条和其他嵌入式服务一样的路径。（关于HAL更多的信息可以阅读HALprimer）

到现在你可以练习一下集合和实体的编写文档的操作
* 有一个“创建，操作，和检索信息状态”的介绍
** GET方法，检索状态信息的分页列表
** POST方法，创建一个新的状态信息
* 针对实体介绍“操作，检索个别状态信息”
** GET方法，检索一条状态信息
** PATCH方法 更改一个状态信息
** PUT方法 替换一条状态信息
** DELETE方法 删除一条状态信息

每个动作都有delete操作，用这个配置操作按钮可以生成重量级体积的请求和响应。你可以自由的编辑，但是辅导中并不是必须的。
我们稍后检查下这个文档，那么现在我们可以看身份验证了。


##中间暂时跳过身份验证验证##
因为大多数的api都是自个写的，不会用公用的

##定义资源##
你应该能回忆起来我们最开始创建的4个存根，他们分别是实体，集合，资源，创建资源的工厂类，这时候我们要在资源类中加入一些代码，以便可以做些东西。

apigility提供了可以重用的版本。版本的一个方面是代码可以根据命名空间来区分版本，这个特色可以同时运行过个版本的api

有个资源类如下
module/status/src/status/V1/rest/status/statusResource.php
打开这个文件进行编辑

我们可以引入StatusLib\MapperInterface 类，在use上面然后添加一行use Status\MapInterface，














