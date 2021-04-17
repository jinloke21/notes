#### php

#### file_exists() 

> ​	file_exists() 函数检查文件或目录是否存在。
>
> ​	如果指定的文件或目录存在则返回 true，否则返回 false
>
> ```php
> <?php
> echo file_exists("test.txt");
> ?>
> ```

#### getimagesize()

> getimagesize() 函数用于获取图像大小及相关信息，成功返回一个数组，失败则返回 FALSE 并产生一条 E_WARNING 级的错误信息。
>
> ```php
> <?php
> list($width, $height, $type, $attr) = getimagesize("runoob-logo.png");
> echo "宽度为：" . $width;
> echo "高度为：" . $height;
> echo "类型为：" . $type;
> echo "属性：" . $attr;
> ?>
> ```
>
> ```
> //输出结果
> 宽度为：290
> 高度为：69
> 类型为：3
> 属性：width="290" height="69"
> ```

#### image_type_to_extension()

```php
image_type_to_extension — 根据指定的图像类型返回对应的后缀名
imagecreatetruecolor - 新建一个真彩色图像
```

​	<img src="C:\Users\86184\AppData\Roaming\Typora\typora-user-images\image-20210403115139559.png" alt="image-20210403115139559" style="zoom:65%;" />

#### imagecreatetruecolor

```php 
imagecreatetruecolor() 返回一个图像标识符，代表了一幅大小为 x_size 和 y_size 的黑色图像。
```

#### stripos

- [ ] 查找 "php" 在字符串中第一次出现的位置:

> ```
> <?php
> echo stripos("You love php, I love php too!","PHP");
> ?>
> ```



#### substr() 

> 用法:substr(*string,start,length*)
>
> 返回字符串的一部分。

#### in_array() 

> 函数搜索数组中是否存在指定的值
>
> 用法: in_array(search,array,type)
>
> * 如果 *search* 参数是字符串且 *type* 参数被设置为 TRUE，则搜索区分大小写。
>
> | *search* | 必需。规定要在数组搜索的值。                                 |      |
> | -------- | ------------------------------------------------------------ | ---- |
> | *array*  | 必需。规定要搜索的数组。                                     |      |
> | *type*   | 可选。如果设置该参数为 true，则检查搜索的数据与数组的值的类型是否相同。 |      |


#### exec

> 该函数执行外部command命令
>
> ​	echo exec('whoami');