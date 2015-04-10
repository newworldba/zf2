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
	



