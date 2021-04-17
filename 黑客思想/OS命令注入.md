#### system函数

> **system()** **能够将字符串作为**OS **命令执行，自带输出功能**
>
> ```php
> <?php
> 
> if(isset($_GET['cmd'])){
> 
> $str=$_GET['cmd'];
> 
> system($str);
> 
> }
> 
> ?>
> ```
>
> 提交参数
> **?cmd=ipconfig**

#### **exec()**函数

> **exec()** 函数能将字符串作为OS命令执行，需要输出执行结果。测试代码如下
>
> ```php
> <?php
> 
> if(isset($_GET['cmd'])){
> 
> $str=$_GET['cmd'];
> 
> print(exec($str));
> 
> }
> 
> ?>
> ```
>
> **?cmd=whoami**

#### **shell_exec()**函数

> ```php
> <?php
> 
> if(isset($_GET['cmd'])){
> 
> $str=$_GET['cmd'];
> 
> print(shell_exec($str));
> 
> }
> 
> ?>
> ```
>
> **?cmd=whoami**

#### passthru()函数

> ```php
> <?php
> 
> if(isset($_GET['cmd'])){
> 
> $str=$_GET['cmd'];
> 
> passthru($str);
> 
> }
> 
> ?>
> ```
>
> **?cmd=whoami**

#### **popen()**

> **popen()** **也能执行****OS** **命令，但是该函数并不是返回命令结果，而是返回一个文件指针**
>
> ```php
> 
> <?php
> 
> if(isset($_GET['cmd'])){
> 
> $str=$_GET['cmd'].">> 1.txt";
> 
> popen($str,'r');
> 
> }
> 
> ?>
> ```
>
> **?cmd=whoami**

#### **反引号**

> ```php
> <?php
> 
> if(isset($_GET['cmd'])){
> 
> $str=$_GET['cmd'];
> 
> print `$str`;
> 
> }
> 
> ?>
> ```
>
> **?cmd=whoami**