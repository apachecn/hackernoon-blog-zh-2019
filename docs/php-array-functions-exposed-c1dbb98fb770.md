# PHP 数组函数(公开)

> 原文：<https://medium.com/hackernoon/php-array-functions-exposed-c1dbb98fb770>

![](img/a90aebdef2c80e52aa59ba02c6a76f8f.png)

先说清楚！

如果你想成为一名 PHP 开发者，你需要学习数组函数。

如果你是一名 [PHP](https://hackernoon.com/tagged/php) 开发者，你需要知道[数组](https://hackernoon.com/tagged/arrays)函数。

即使不需要知道所有这些，掌握基本知识并知道它们的存在将会在你的职业生涯中节省大量的时间。

在本文中，你将浏览 PHP 中最流行的数组函数，仔细阅读所有的函数，记下可以直接重构代码的函数，并尝试理解所有其他的函数，我保证有一天你会需要它们。

# 简介:什么是数组？

在 PHP 和一般的编程中，**数组是一组存储在唯一名称下的数据元素。**

[阅读我关于 PHP 中复合变量的文章，了解更多信息](http://anastasionico.uk/blog/composite-variable-in-php)

一个数组可以包含任何类型的数据，每一个元素都可以被分配和读取。

**数组可以包含数字、字符串等数组。**

您可以在一个阵列上执行的操作数量是无限的。

因此，PHP 提供了广泛的内置特性，允许您在其中读取和编辑元素。

这里是关于数组的官方 PHP 手册的链接。我的建议是看一看它，即使它可能看起来势不可挡。为此，我整理了 2109 年最重要的数组函数列表。

我相信你会从这些例子中学到很多:

# 数组()

这是一个 PHP 构造，非常容易理解。接受值或键值对的列表，并创建一个数组。

```
$dwarfs = array('Doc', 'Grumpy', 'Happy', 'Sleepy', 'Dopey', 'Bashful', 'Sneezy'); 
$disney = array ( 
    "dwarfs" => array('Doc', 'Grumpy', 'Happy', 'Sleepy', 'Dopey', 'Bashful', 'Sneezy'), 
    "originals" => array("Mickey Mouse","Pete", "Goofy", "Minnie Mouse", "Pluto"));
```

# is_array()

它将一个变量作为属性，并返回一个回答问题的布尔值:这个属性是数组吗？

```
$dwarfs = array('Doc', 'Grumpy', 'Happy', 'Sleepy', 'Dopey', 'Bashful', 'Sneezy'); If (is_array($dwarfs)){     
    echo '$dwarfs is an array'; 
}
```

# In_array()

这个 PHP 函数检查你正在寻找的元素是否在一个数组中

```
$dwarfs = array('Doc', 'Grumpy', 'Happy', 'Sleepy', 'Dopey', 'Bashful', 'Sneezy'); 
if (in_array("Snow White", $dwarfs)) {     
    echo "Snow White is not a dwarf"; 
}
```

# array_merge()

函数的作用是:将一个或几个数组合并成一个唯一的数组。

如果不同的元素有相同的键，最后一个元素会覆盖其他元素吗

该数组函数**可用于重置数组**的索引号(如果键是数字)

```
$dwarfs = array('Doc', 'Grumpy', 'Happy', 'Sleepy', 'Dopey', 'Bashful', 'Sneezy'); 
$originals = array("Mickey Mouse","Pete", "Goofy", "Minnie Mouse", "Pluto"); array_merge($dwarfs, $originals); 
Array(
    [0] => Doc 
    [1] => Grumpy 
    [2] => Happy 
    [3] => Sleepy 
    [4] => Dopey 
    [5] => Bashful 
    [6] => Sneezy 
    [7] => Mickey Mouse 
    [8] => Pete 
    [9] => Goofy 
    [10] => Minnie Mouse 
    [11] => Pluto) 
$dwarfs = array(
    3 => 'Doc', 
    4 =>'Grumpy', 
    5 => 'Happy'); array_merge($dwarfs); 
Array ( 
    [0] => "Doc" 
    [1] => "Grumpy" 
    [2] => "Happy" 
)
```

# 数组关键字()

array_keys()返回数组中的键。

它们可以是字符串或数字

该函数的第二个属性称为 *search_value* ，如果指定了该属性，则该函数只返回该值的关键字

```
$disney = array ("dwarf"  => 'Grumpy', "originals" => "Mickey Mouse"); 
array_keys($disney); 
Array (
     [0] => dwarfs
     [1] => originals ) 
$dwarfs = array('Doc', 'Grumpy', 'Doc', 'Sleepy', 'Doc', 'Bashful', 'Sneezy'); 
array_keys($dwarfs, "Doc"); Array(
     [0] => 0
     [1] => 2
     [2] => 4
 ) 
$disney = array(
     "dwarfs"  => array('Doc', 'Grumpy', 'Happy', 'Sleepy', 'Dopey', 'Bashful', 'Sneezy'),
     "originals" => array("Mickey Mouse","Pete", "Goofy", "Minnie Mouse", "Pluto");array_keys($disney); 
Array (
     [0] => dwarfs
     [1] => originals 
)
```

# 数组关键字存在()

这个函数检查数组中是否存在确定的键或索引。

它有两个参数，第一个是你要找的键，第二个是数组。

此函数返回的值取决于结果，并且是布尔类型，如果值存在，则返回 true，否则返回 false。

在条件语句中非常有用

```
$disney = array("dwarf"  => 'Grumpy', "originals" => "Mickey Mouse"); 
if (array_key_exists('dwarf', $disney)) {
     echo "the array disney contains the key dwarf"; 
}
```

# 数组值()

*array_values* 函数将一个数组作为属性，并返回一个数组，其中所有的值都用数字索引。

如果您想从数组中删除键或将数组从关联数组转换为数值数组，这将非常有用。

```
$disney = array ("dwarf"  => 'Grumpy', "originals" => "Mickey Mouse"); 
print_r(array_values($disney));  
Array(
     [0] => Grumpy
     [1] => Mickey Mouse 
)
```

# 数组计数值()

让我们假设你有一组名字，

你想找出这个列表中最常见的名字是什么？

函数 *array_count_values()* 将一个数组作为属性，并返回一个数组，该数组将属性的值作为键，将找到的实例数作为值。

我们来看一个例子；

```
$dwarfs = array('Doc', 'Grumpy', 'Happy', 'Doc', 'Dopey', 'Doc', 'Grumpy'); 
print_r(array_count_values($dwarfs )); 
Array(
     [Doc] => 3
     [Grumpy] => 2
     [Happy] => 1 
)
```

# array_shift()

函数 *array_shift()* 将数组的第一个值移出并作为变量返回，同时它将作为元素属性的数组缩短并全部下移。

数组中的所有数字键将被修改为从零开始计数，而关联数组中的字母键将不会被触动。

注意:函数 array_shift()需要对数组进行重新索引，因此必须对所有元素执行该函数并建立索引。

这意味着这种做法相当缓慢。

为了加速这个过程，你可以使用 *array_reverse()，*然后使用 *array_pop()* ，它不需要重新索引数组，如果你愿意的话，它会保留这些键。

```
$dwarfs = array('Doc', 'Grumpy', 'Happy', 'Sleepy', 'Dopey', 'Bashful', 'Sneezy'); 
$dwarf = array_shift($dwarfs); 
print_r($dwarfs); Array (
      [0] => Grumpy
      [1] => Happy
      [2] => Sleepy
      [3] => Dopey
      [4] => Bashful
      [5] => Sneezy
  ) 
// The value of $dwarf is ‘Doc’
```

# array_unshift()

*array_unshift()* 将作为属性传递的元素放在所选数组的前面。

元素列表是作为一个整体建立的，因此**前缀元素保持相同的顺序**。

数组中的所有数字键将被修改为从零开始计数，而字母键将保持不变。

您可以插入任意多的元素。

```
$dwarfs = array('Sleepy', 'Dopey', 'Bashful', 'Sneezy');
array_unshift($dwarfs, 'Doc', 'Grumpy', 'Happy'); print_r($dwarfs);
Array (
     [0] => Doc
     [1] => Grumpy
     [2] => Happy
     [3] => Sleepy
     [4] => Dopey
     [5] => Bashful
     [6] => Sneezy  
)
```

# array_push()

顾名思义 *array_push()* 将作为属性传递的变量推到数组的末尾。

它的作用与: *$array[] =* 相同

**矩阵的长度根据插入变量的数量增加。**

如果使用 *array_push()* 只向数组添加一个元素，最好使用 *$array[] =* ，因为不需要使用函数重载调用。

注意，与创建新数组的 *$var[] =* 不同，如果第一个参数不是数组， *array_push()* 函数会生成一个警告。

```
$dwarfs = array('Sleepy', 'Dopey', 'Bashful', 'Sneezy');
array_push($dwarfs, 'Doc', 'Grumpy', 'Happy'); 
print_r($dwarfs); 
Array(
     [0] => Sleepy
     [1] => Dopey
     [2] => Bashful
     [3] => Sneezy
     [4] => Doc
     [5] => Grumpy
     [6] => Happy  
)
```

# 数组 _ 弹出()

*array_pop()* 移除并返回数组中最后一个元素的值，缩短一个元素的数组。

简单来说，**函数 *array_pop()* 删除一个数组**的最后一个元素

这个函数将在使用后恢复数组输入指针。

```
$dwarfs = array('Doc', 'Grumpy', 'Happy', 'Sleepy', 'Dopey', 'Bashful', 'Sneezy'); 
$dwarf = array_pop(dwarfs); 
print_r(dwarfs); 
Array(
     [0] => Doc
     [1] => Grumpy
     [2] => Happy
     [3] => Sleepy
     [4] => Dopey
     [5] => Bashful  
) 
// The value of $dwarf is 'Sneezy'
```

# array_slice()

函数 *array_slice()* 接受四个属性(其中两个是强制的),并按照 offset 和 length 参数指定的规则从指定的数组中返回一系列元素。

这四个参数是:

**输入数组。**偏移量，可以是正整数或负整数，表示新序列开始的位置。

请注意，偏移量指示数组中的位置，而不是键。

**Length**
Length 不是强制的，如果省略，序列将拥有从偏移量到数组末尾的所有内容。

如果长度是给定的，并且是正数，那么序列将拥有它所包含的元素数。

如果长度是给定的并且是负的，序列将从数组的末尾停止许多元素。

**preserve _ keys 属性**这意味着他们将从零开始。

您可以通过将 *preserve_keys* 设置为 TRUE 来改变这种行为。不管此参数如何，字符串键总是被保留。

```
$dwarfs = array('Doc', 'Grumpy', 'Happy', 'Sleepy', 'Dopey', 'Bashful', 'Sneezy');
$output = array_slice($dwarfs, 2);
// returns 'Happy', 'Sleepy', 'Dopey', 'Bashful' and 'Sneezy' $output = array_slice($dwarfs, -2, 1);
// returns 'Bashful' $output = array_slice($dwarfs, 0, 3);
// returns 'Doc', 'Grumpy', 'Happy' 
print_r(array_slice($input, 2, -4)); 
Array (
     [0] => Happy
     [1] => Sleepy 
) 
print_r(array_slice($input, 2, -4, true)); 
Array (
     [2] => Happy
     [3] => Sleepy 
)
```

# array_splice()

函数的作用是:从一个数组中移除元素，如果指定的话，用新的元素替换它。

它有四个参数(其中两个是强制的)，

原始数组中的数字键不会被保留。

这些参数是:

**原阵**

所谓的**偏移量是 integer** 类型的强制参数，如果该数字为正数，则函数从数组的开头删除该部分，如果该数字为负数，则从指定数组的结尾开始该数字。

**长度是一个整数值，**

可以省略，正的，负的。

如果省略，那么该函数将删除从指示的偏移位置到数组末尾的所有内容，

如果数字是正数 *array_splice()* 将删除数组中指定的元素。

如果是负的(注意这里),函数将在离最后一个元素很远的地方停止。

你可以使用 *count($input)* 删除从偏移量到数组末尾的所有元素。

**替换数组**是一个参数，如果被指定，它将替换被删除的元素，如果替换的只是一个元素，它可以是一个字符串，而不必是一个数组。

array_splice()返回一个删除了元素的数组。

注意下面不同的例子:

```
$dwarfs = array('Doc', 'Grumpy', 'Happy', 'Sleepy', 'Dopey', 'Bashful', 'Sneezy'); 
array_splice($dwarfs, 2); 
// $input is now array('Doc', 'Grumpy') $dwarfs = array('Doc', 'Grumpy', 'Happy', 'Sleepy', 'Dopey', 'Bashful', 'Sneezy'); 
array_splice($dwarfs, 1, -1); 
// $input is now array("Doc", "Sneezy") $dwarfs = array('Doc', 'Grumpy', 'Happy', 'Sleepy', 'Dopey', 'Bashful', 'Sneezy'); 
array_splice($dwarfs, 1, count($dwarfs), "Mickey Mouse"); 
// $input is now array("Doc", "Mickey Mouse") $dwarfs = array('Doc', 'Grumpy', 'Happy', 'Sleepy', 'Dopey', 'Bashful', 'Sneezy'); 
array_splice($dwarfs, -1, 1, array("Mickey Mouse", "Pete")); 
// $input is now array("Doc", "Grumpy", "Happy", "Mickey Mouse", "Pete") $dwarfs = array('Doc', 'Grumpy', 'Happy', 'Sleepy', 'Dopey', 'Bashful', 'Sneezy'); 
array_splice($dwarfs, 3, 0, "Mickey Mouse"); 
// $input is now array("Doc", "Grumpy", "Happy", "Mickey Mouse", "Sleepy", "Dopey", "Bashful", "Sneezy")
```

# 数组搜索()

函数 *array_search()* 搜索元素数组中的值。

如果它成功地找到元素，它返回找到的第一个元素的键(只返回第一个)，如果它没有找到任何元素，它返回 false 或类似的结果。

该函数有三个参数:

第一个是**要搜索的元素**，它可以是混合型的。

第二个参数是要搜索到中的实际**数组。**

第三个参数不是强制性的，它是一个表示搜索严格性的布尔型**，**

默认情况下，它被设置为 false，但如果设置为 true，array_search()将搜索相同的元素(相同的值和相同的类型)。

```
$dwarfs = array(0 => 'Doc', 1 => 'Grumpy', 2 => 'Happy', 3 => 'Sleepy', 4 => 'Dopey', 5 => 'Bashful', 6 => 'Sneezy'); $key = array_search('Happy', $dwarfs); 
// return $key = 2; 
$key = array_search('Grumpy', $dwarfs);   
// return $key = 1; 
$key = array_search('Mickey Mouse', $dwarfs);   
// return false or 0;
```

# array_map()

在我看来，这个函数相当复杂，

*array_map()* 将一个用户自定义函数(通常称为回调函数)作为第一个参数，并将该回调应用于作为第二个参数传递的每个元素 *array_map()本身*；

回调中设置的参数数量需要等于作为参数传递给 *array_map()* 函数的数组数量。

```
function uppercase($dwarf) {
     return strtoupper($dwarf); 
} 
$dwarfs = array('Doc', 'Grumpy', 'Happy', 'Sleepy', 'Dopey', 'Bashful', 'Sneezy'); 
$uppercase = array_map("uppercase", $dwarfs); 
print_r($uppercase); 
Array (
      [0] => DOC
     [1] => GRUMBY
     [2] => HAPPY
     [3] => SLEEPY
     [4] => DOPEY
     [5] => BASHFUL
 ) 
// ** Two arrays as parameters ** 
function matchCharacter($character, $brand) {
     return("The character $character belongs to $brand"); 
} 
$character = array('Doc', 'Mickey Mouse', 'Happy', 'Sleepy', 'Pete', 'Goofy', 'Sneezy'); 
$brand = array("dwarfs","original", "dwarfs", "dwarfs", "original", "original", "dwarfs") 
$result = array_map("matchCharacter", $character, $brand);
print_r($result); Array (
      [0] => The character Doc belongs to dwarfs
     [1] => The character Mickey Mouse belongs to original
     [2] => The character Happy belongs to dwarfs
     [3] => The character Sleepy belongs to dwarfs
     [4] => The character Pete belongs to original
     [5] => The character Goofy belongs to original
     [6] => The character Sneezy belongs to dwarfs
)
```

# 数组唯一()

这个函数移除并返回一个没有重复值的数组。

它需要两个参数。

第一个参数是强制的，它是您需要评估的**数组；**

第二个参数通常称为 **sort_flag** ，它用于修改所指示数组的排序行为。

可用标志有:
**SORT_REGULAR** 正常比较项目(不改变类型)；
**SORT_NUMERIC** 对项目进行数值比较；
**将项目作为字符串进行比较的 SORT _ STRING**；
**SORT _ LOCALE _ STRING**根据当前区域设置，将项目作为字符串进行比较。

此数组函数不适用于多维数组。
这里快速解释一下什么是[多维数组](https://www.tutorialspoint.com/php/php_arrays.htm)。

```
$dwarfs = array("a" => 'Doc', 'Grumpy', "b" => 'Doc', 'Sleepy', 'Grumpy'); 
$result = array_unique($dwarfs); 
print_r($result); 
Array (
     [a] => Doc
     [0] => Grumpy
     [1] => Sleepy
 )
```

# array_diff()

该函数至少接受两个或更多参数，所有参数都必须是数组类型，并将第一个数组与提供的所有其他数组进行比较。

最终，

它返回第一个数组中存在的值，这些值在任何其他数组中都不存在。

请注意，当且仅当第一个求值===将产生 true 时，元素才被视为相等。

```
$dwarfsA = array('Doc', 'Grumpy', 'Happy', 'Sleepy', 'Dopey', 'Bashful', 'Sneezy'); 
$dwarfsB = array('Doc', 'Grumpy', 'Happy', 'Dopey', 'Sneezy');
$result = array_diff($dwarfsA, $dwarfsB);
print_r($result); 
Array (
     [3] => Sleepy
     [5] => Bashful 
)
```

如果需要，您可以使用 *array_diff($dwarfsA[0]，$dwarfsB[0])* 来检查更深的维度。

# 第一部分结束

数组函数是一个几乎无限延伸的话题。

你做笔记了吗？

这是一个要一下子吃下的巨大的一口，

为此，我将这篇文章分成了一系列不同的博文。

我建议你做的一个练习是回到你最近写的代码，看看你是否能实现一个或多个数组函数。

这样做的好处是，编写的代码更少，是最容易实现的，而且内置函数比我们 PHP 开发人员自己实现要快得多。

如果您喜欢这一内容，并希望看到下一组关于数组函数的示例，请继续关注并订阅时事通讯，这样您会在文章的下一部分发布时得到通知。

# 从这里去哪里？

正如我所说的，这只是 PHP 数组的第一轮，我将很快发布其他部分，与此同时，根据你目前的水平，你可以在这里获得更多关于 PHP 的信息:

[](http://anastasionico.uk/blog/php-basics-for-web-developer)**——这里有新手吗？一切似乎势不可挡？别担心，本系列将带您从零开始，准备构建您的第一个 web 应用程序**

**[**《面向对象编程完全指南》**](http://anastasionico.uk/blog/object-oriented-programming)——你知道语言的基础，现在是认真做事的时候了**

**[**PHP 7.3 及其新特性**](http://anastasionico.uk/blog/php-73)——如果你想从事 web 开发，就必须保持更新**

**[**24 个 PHP 框架指南**](http://anastasionico.uk/blog/guide-to-php-frameworks-part-1) —你觉得够好了吗？通过选择你的第一个 PHP 框架来提升你的技能。**

*******

**![](img/2991f2a34c8589d2f762a6d68dbb6d58.png)**

# **学习编码，获得一项新技能，获得一份新工作**

## **无论你的目标是什么——树屋都能帮你实现**

**[他们目前提供 4 个月的免费服务(价值 100 美元)。](http://treehouse.7eer.net/c/1374240/294479/3944)**

**[看看吧！。](http://treehouse.7eer.net/c/1374240/294479/3944)**

**([附属链接](http://treehouse.7eer.net/c/1374240/294479/3944))**

***最初发表于*[*anastasionico . uk*](http://anastasionico.uk/blog/php-array-functions-exposed)*。***

*******

**[![](img/81f240e8dd073aab6ad31da0e0d28d60.png)](http://eepurl.com/dIZqjf)**