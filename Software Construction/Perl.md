## Perl 技巧总结
####感觉Perl是一种糅合了Shell和Python的语言，一旦代码复杂起来可读性相当难以维护。但在处理文本和遍历文件方面确实很好用。

#### Trick-1 正则匹配
##### 转义字符
```
尤其要注意转义字符的问题
比如想匹配小数\d+.\d+的时候很容易忘记转义中间的. 而且难以调试发现这种问题
```
##### 匹配捕获
```
$` 匹配成功之前的部分
$& 匹配成功的部分
$' 匹配成功之后的剩余部分

利用这些可以很容易使用递归拆分一个字符串，尤其是分隔符不规则的情况下
```

#### Trick-2 Sort排序
##### 排序规则
Perl的sort函数并不像Python那样直观，其默认的排序方式是ASCII编码，忘记这点的话会出现非常奇怪的排序结果，比如：
```
sort (1, 2, 42, 10)
 = （1, 2, 10, 42）
```
这是因为ASCII编码是根据每个字符的编码大小来比较的，和数字比较完全不同。
正确的写法是：
```
#数字比较 从小到大
@sorted = sort { $a <=> $b } @not_sorted
#数字比较 从大到小，把两个变量反过来就行了
@sorted = sort { $b <=> $a } @not_sorted
#字母比较1
@sorted = sort { $a cmp $b } @not_sorted
#字母比较2， 先转换成小写比较
@sorted = sort { lc($a) cmp lc($b) } @not_sorted
```
$a 和 $b 之间的关系	| $a <=> $b 的返回值
-------------------|------------------
$a 大于$b	1 | 1
$a 等于$b	0 | 0
$a 小于$b	-1| -1

这里的$a 和$b其实是perl固有的全局变量(build-in/package globals) , 我们把自定义排序法则的子程序写成: {$a <=>$b} ，perl编译器将得知你定义了的排序法则是采用数字比较大小。我们在对hash按key进行排序时候常用到自定义排序法。例如:
```
foreach $key （sort{$mapword{$b}<=>$mapword{$a}}keys %mapword）{
  print "$mapword{$key} \n";
}
```
'keys % mapword' 得到的是一个数组
用全局变量，$a, $b定义了对比法则，将对%mapword的key数值按从小到大顺序排列
打印出$key值和对应的hash值。
既然是自定义法则，我们还可以把比较法则定义成你自己想要的，比如说，要把一组数安字母顺序排列，但是要让dh永远排在前面，可以这么写：
```
@words = ("hi", "da", "abc", "dh", "man");
@sorted = sort {
if ($a eq 'dh') { return -1; }
elsif ($b eq 'dh') { return 1; }
else { return $a cmp $b; }
} @words;
print join(" ", @sorted);
#打印结果为：dh abc da hi man
```
