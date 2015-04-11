rest-service : https://apigility.org/documentation/intro/first-rest-service


创建一个Rest服务
这个章节中，我们将会创建一个简单的REST服务

假设
这个章节假设你已经阅读并且跟着做了"installation guide"和“getting started chapter”所交代的事情。 如果没有，还是去重复以前的章节要求

你需要安装并且配置"zfcampus/statuslib-example"模块来引导安装创建api。步骤如下：

第一步：
在application 根目录下，执行如下操作
$ php composer.phar require "zfcampus/statuslib-example:~1.0-dev"

第二步：
编辑config/application.config.php  增加StatusLib模块
array(
	'modules' => array(
		'StatusLib',
	)
)

第三步
创建一个php文件“data/statuslib.php”,返回一个空数组即可
<?PHP
return array();
一定要确认这个文件是可写的

第四步
编辑config/autoload/local.php增加下面的配置
return array(
	'statuslib'=>array(
		'array_mapper_path'=>'data/statuslib.php',
	)
)

第五步
最后,您将需要一个有效的HTTP基本认证文件,通常名为htpasswd。你可以生成一个使用Apache提供的标准htpasswd工具,或者使用一个在线htpasswd文件，将htpasswd放在data/htpasswd目录下，然后就可以使用这个东西了
一旦这些步骤都做完了，就可以继续下面的教程了。

专业术语
apigility或者下面的部分中，都会用到的一些专业术语

Entity
	一个将会返回信息东西，Entities由唯一的标识URI定义。
Collection
	一个设置entities的东西，通常在collection中包含的所有entities是同一个类型，将共用一个基本的uri
Resource
	接收请求参数，确定collection和entities是否能被uri所解析,并确定要执行什么操作
Relational Links
	一个描述关系的资源URI，关系连接允许你描述不同的entities和collections之间的区别，你也可以直接链接到这个地方，以便服务客户端可以在这些关系上执行操作，这些有时也叫做hypermedia links
REST服务会返回entities和collections,还提供entities和collection中间的超媒体链接，资源对象协调操作后，返回entities和collections

创建一个rest service
这个章节中我们将会创建一个简单的rest service
进入APIs显示页，我们在前一个章节中创建了“Status”API，接下来悬着个这个“REST Service”菜单项
	
点击创建“Create New REST Service”按钮
接下来的对话框中有两个tabs，一个似乎创建“Code-Connected”服务，另一个是创建"Db-Connected"服务

	Code-Connected 和 DB-Connected service
		当你创建一个Code-Connected服务是，apigility创建了一个资源类来定义所有可以在rest服务中被操作的变量，这些操作会返回“405 Method Not Allowed”响应，你把这些写到代码中就不会出现这种情况了。



