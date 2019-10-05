JS中所有用单引号或者双引号抱起来的都是字符串，每一个字符串是由零到多个字符组成的。
字符串是基本数据类型，字符串的每一次操作都是值直接地进行操作，不像数组一样是基于空间地址来操作的，所以不存在原有字符串是否改变这一说，肯定都是不变的。

## 前言

> 在日常开发中，String对象的使用频率是非常高的。所以有必要详细介绍。通过String.prototype查看方法

### 基本数据类型不能绑定属性和方法

**1、基本数据类型：**

注意，基本数据类型`string`是**无法绑定属性和方法**的。比如说：

```javascript
    var str = "qianguyihao";

    str.aaa = 12;
    console.log(typeof str);  //打印结果为：string
    console.log(str.aaa);     //打印结果为：undefined
    但是：
    str.length 是可以的，因为临时转换成了对象，把str，详见之后的面向对象
```

上方代码中，当我们尝试打印`str.aaa`的时候，会发现打印结果为：undefined。也就是说，不能给 `string` 绑定属性和方法。

当然，我们可以打印str.length、str.indexOf("m")等等。因为这两个方法的底层做了数据类型转换（**临时**将 `string` 字符串转换为 `String` 对象，然后再调用内置方法），也就是我们在上一篇文章中讲到的**包装类**。

**2、引用数据类型：**

引用数据类型`String`是可以绑定属性和方法的。如下：


```javascript
    var strObj = new String("smyhvae");
    strObj.aaa = 123;
    console.log(strObj);
    console.log(typeof strObj);  //打印结果：Object
    console.log(strObj.aaa);
```

打印结果：

![](http://img.smyhvae.com/20180202_1351.png)

内置对象Number也有一些自带的方法，比如：

- Number.MAX_VALUE;

- Number.MIN_VALUE;

内置对象Boolean也有一些自带的方法，但是用的不多。

### 在底层，字符串以字符数组的形式保存

在底层，字符串是以字符数组的形式保存的。代码举例：

```javascript
	var str = "smyhvae";
	console.log(str.length); // 获取字符串的长度
	console.log(str[2]); // 获取字符串中的第2个字符
```

上方代码中，`smyhvae`这个字符串在底层是以`["s", "m", "y", "h", "v", "a", "e"]`的形式保存的。因此，我们既可以获取字符串的长度，也可以获取指定索引index位置的单个字符。这很像数组中的操作。

## 内置对象 String 的常见方法

### charAt()

`charAt`：返回字符串指定位置的字符。不会修改原字符串。

语法：

```javascript
    字符 = str.charAt(index);
```

解释：字符串中第一个字符的下标是 0。如果参数 index 不在 [0, string.length) 之间，该方法将返回一个空字符串。

而且，这里的 `str.charAt(index)`和`str[index]`的效果是一样的。
它们的区别在于如果获取一个不存在的索引
```
str[100];   =>undefined
str.charAt(100)  =>""
```

**代码举例**：

```javascript
   var str = new String("smyhvae");

    for (var i = 0; i < str.length; i++) {
        console.log(str.charAt(i));
    }
```

打印结果：

![](http://img.smyhvae.com/20180202_1401.png)

上面这个例子一般不用。一般打印数组和json的时候用索引，打印String不建议用索引。

### charCodeAt()

`charCodeAt`：返回字符串指定位置的字符的 Unicode 编码。不会修改原字符串。ASCII表里面的十进制编码

语法：

```javascript
    字符 = str.charCodeAt(index);
```

**代码举例**：打印字符串的占位长度

提示：一个英文占一个位置，一个中文占两个位置。

思路：判断该字符是否在0-127之间（在的话是英文，不在是非英文）。

代码实现：

```html
<script>
    //    sort();   底层用到了charCodeAt();

    var str = "I love my country!我你爱中国！";

    //需求：求一个字符串占有几个字符位。
    //思路；如果是英文，站一个字符位，如果不是英文占两个字符位。
    //技术点：判断该字符是否在0-127之间。（在的话是英文，不在是非英文）
    alert(getZFWlength(str));
    alert(str.length);

    //定义方法：字符位
    function getZFWlength(string) {
        //定义一个计数器
        var count = 0;
        for (var i = 0; i < string.length; i++) {
            //对每一位字符串进行判断，如果Unicode编码在0-127，计数器+1；否则+2
            if (string.charCodeAt(i) < 128 && string.charCodeAt(i) >= 0) {
                count++;
            } else {
                count += 2;
            }
        }
        return count;
    }
</script>
```

打印结果：

```
    30
    24
```

从打印结果可以看出：字符串的长度是24，但是却占了30个字符位（一个中文占两个字符位）。

另外，sort()方法其实底层也是用到了charCodeAt()，因为用到了Unicode编码。

### String.fromCharCode()

`String.fromCharCode()`：根据字符的 Unicode 编码获取字符。

代码举例：

```javascript
	var result1 = String.fromCharCode(72);
	var result2 = String.fromCharCode(20013);

	console.log(result1); // 打印结果：H
	console.log(result2); // 打印结果：中
```

### indexOf()/lastIndexOf()

`indexOf()/lastIndexOf()`：获取指定字符的索引。

语法：

```javascript
    索引值 = str.indexOf(想要查询的字符);
```

解释：`indexOf()` 是从前向后索引字符串的位置。同理，`lastIndexOf()`是从后向前寻找。

**作用**：可以检索一个字符串中是否含有指定内容。如果字符串中含有该内容，则会返回其**第一次出现**的索引；如果没有找到指定的内容，则返回 -1。

因此可以得出一个技巧：**如果获取的索引值为0，说明字符串是以查询的参数为开头的**。

这个方法还可以指定第二个参数，用来 指定开始查找的位置。

**代码举例1**：

```javascript
    var str = "abcdea";

    //给字符查索引(索引值为0,说明字符串以查询的参数为开头)
    console.log(str.indexOf("c"));
    console.log(str.lastIndexOf("c"));

    console.log(str.indexOf("a"));
    console.log(str.lastIndexOf("a"));

```

打印结果：

![](http://img.smyhvae.com/20180202_1420.png)

**代码举例2**：（两个参数时，需要特别注意）

```javascript
    var str = 'qianguyihao';
    result = str.indexOf('a', 3); // 从第三个位置开始查找 'a'这个字符 【重要】

    console.log(result); // 打印结果：9
```

上方代码中，`indexOf()`方法中携带了两个参数，具体解释请看注释。

### concat()

`concat()`：字符串的连接。

语法：

```javascript
    新字符串 = str1.concat(str2)； //链接两个字符串
```

这种方法基本不用，直接把两个字符串相加就好。

是的，你会发现，数组中也有`concat()`方法，用于数组的连接。这个方法在数组中用得挺多的。

代码举例：

```javascript
    var str1 = 'qiangu';
    var str2 = 'yihao';

    var result = str1.concat(str2);
    console.log(result); // 打印结果：qianguyihao
```

### slice()

`slice()`：从字符串中截取指定的内容。不会修改原字符串，而是将及截取到的内容返回。

语法：

```javascript
    字符串 = str.slice(开始索引, 结束索引); //两个参数都是索引值。包左不包右。
```

解释：上面的参数，包左不包右。参数举例如下：

- (2, 5) 截取时，包左不包右。

- (2) 表示**从指定的索引位置开始，截取到最后**。

- (-3) 表示从倒数第几个开始，截取到最后。

- (1, -1) 表示从第一个截取到倒数第一个。

- (5, 2) 表示前面的大，后面的小，返回值为空。

### substring()

`substring()`：从字符串中截取指定的内容。和`slice()`类似。

语法：

```javascript
    字符串 = str.substring(开始索引, 结束索引); //两个参数都是索引值。包左不包右。
```

`substring()`和`slice()`是类似的。但不同之处在于：

- `substring()`不能接受负值作为参数。如果传递了一个**负值**，则默认使用0。

- `substring()`还会自动调整参数的位置，如果第二个参数小于第一个，则自动交换。比如说， `substring(1, 0)`截取的是第一个字符。

### substr()

`substr()`：从字符串中截取指定的内容。不会修改原字符串，而是将及截取到的内容返回。

语法：

```javascript
   字符串 = str.substr(开始索引, 截取的长度);
```

参数举例：

- (2,4)：从索引值为2的字符开始，截取4个字符。

- (1)：从指定位置开始，截取到最后。

- (-3)：从倒数第几个开始，剪到最后.

-  不包括前大后小的情况。

备注：ECMAscript 没有对 `substr()` 方法进行标准化，因此不建议使用它。

### split() 【重要】

`split()`：将一个字符串拆分成一个数组。

语法：


```javascript
   数组 = str.split();
```


备注：`split()`这个方法在实际开发中用得非常多。一般来说，从接口拿到的json数据中，经常会收到类似于`"q, i, a, n"`这样的字符串，前端需要将这个字符串拆分成`['q', 'i', 'a', 'n']`数组，这个时候`split()`方法就排上用场了。

**代码举例1**：

```javascript

    var str = "qian, gu, yi, hao"; // 用逗号隔开的字符串
    var array = str.split(","); // 将字符串 str 拆分成数组，通过逗号来拆分

    console.log(array); // 打印结果是数组：["qian", " gu", " yi", " hao"]
```

**代码举例2**：

```javascript
    //split()方法：字符串变数组
    var str3 = "生命壹号|许嵩|smyhvae";

    console.log(str3);

    console.log(str3.split());   // 无参数，表示：把字符串作为一个元素添加到数组中。

    console.log(str3.split(""));  //参数为空字符串，则表示：分隔字符串中每一个字符，分别添加到数组中

    console.log(str3.split("|")); //参数为指定字符，表示：此字符将不会出现在数组的任意一个元素中

    console.log(str3.split("许")); //同理
```

打印结果：

![](http://img.smyhvae.com/20180202_1503.png)

### trim()

`trim()`：去除字符串前后的空白。

代码举例：

```javascript
    //去除前后的空格，trim();
    var str1 = "   a   b   c   ";
    console.log(str1);
    console.log(str1.trim());
```

打印结果：

![](http://img.smyhvae.com/20180202_1455.png)

### replace()

`replace()`：将字符串中的指定内容，替换为新的内容并返回。不会修改原字符串。
在不是用正则的情况下，如果原字符串有多个相同的部分，每执行一次replace只能替换一个。

语法：

```javascript
    新的字符串 = str.replace(被替换的内容，新的内容);
```

代码举例：

```javascript
    //replace()方法：替换
    var str2 = "Today is fine day,today is fine day !!!"
    console.log(str2);
    console.log(str2.replace("today","tomorrow"));  //只能替换第一个today
    console.log(str2.replace(/today/gi,"tomorrow")); //这里用到了正则，才能替换所有的today
```

### 大小写转换

举例：

```javascript
    var str = "abcdEFG";

    //转换成小写
    console.log(str.toLowerCase());

    //转换成大写
    console.log(str.toUpperCase());
```

## html方法

- anchor()  创建a链接

- big()

- sub()

- sup()

- link()

- bold()

注意，str.link()  返回值是字符串。

举例：

```javascript
    var str = "你好";

    console.log(str.anchor())
    console.log(str.big())
    console.log(str.sub())
    console.log(str.sup())
    console.log(str.link("http://www.baidu.com"));
    console.log(str.bold())
```

![](http://img.smyhvae.com/20180202_1536.png)


## 字符串练习

**练习1**："smyhvaevaesmyh"查找字符串中所有m出现的位置。

代码实现：

```javascript
    var str2 = "abcoefoxyozzopp";
    for(var i=0;i<str2.length;i++){
        //如果指定位置的符号=== "o"
        //str2[i]
        if( str2.charAt(i)==="o"){
            console.log(i);
        }
    }
```

**练习2**：判断一个字符串中出现次数最多的字符，统计这个次数

```html
<script>
    var str2 = "smyhvaevaesmyhvae";

    //定义一个json，然后判断json中是够有该属性，如果有该属性，那么值+1;否则创建一个该属性，并赋值为1；
    var json = {};
    for (var i = 0; i < str2.length; i++) {
        //判断：如果有该属性，那么值+1;否则创建一个该属性，并赋值为1；
        var key = str2.charAt(i);
        if (json[key] === undefined) {
            json[key] = 1;
        } else {
            json[key] += 1;
        }
    }
    console.log(json);


    console.log("----------------");
    //获取json中属性值最大的选项
    var maxKey = "";
    var maxValue = 0;
    for (var k in json) {
        //        if(maxKey == ""){
        //            maxKey = k;
        //            maxValue = json[k];
        //        }else{
        if (json[k] > maxValue) {
            maxKey = k;
            maxValue = json[k];
        }
        //        }
    }
    console.log(maxKey);
    console.log(maxValue);


</script>
```

打印结果：

![](http://img.smyhvae.com/20180202_1540.png)

## 真实项目需求
1. 时间字符串格式化
> 有一个时间字符串"2018-4-4 16:26:8",怎么基于这个字符串获取到"04月04日 16时26分"

```
function addZero(val){
	return val < 10 ? '0' + val: val;
}

var str = '2018-4-4 16:32:8'
var ary = str.split(''),
	aryLeft = ary[0].split('-'),
	aryRight = ary[1].split(':');
var month = addZero(aryLeft[1]),
	day = addZero(aryLeft[2]),
	hour = addZero(aryRight[0]),
	minute = addZero(aryRight[1]);
var result = month+'月'+day+'日' +hour+'时'+minute+'分';
//或者用正则表达式
str.split(/(?:-| |:)/g)

或者更厉害的超级处理代码：
~function(pro){
  pro.formatTime = function (template){
  	template = template || '{0}年{1}月{2}日 {3}时{4}分{5}秒';
	var ary = this.match(/\d+/g);
	template = template.replace(/\{(\d+)\g,function() {
	var n = arguments[1],
	val = ary[n] || '0';
	val < 10 ? val = '0' + val:null;
	return val;
	});
	return template;
	}
}(String.prototype);
```

## URL地址问好传参解析
https://sports.qq.com/kbsweb/game.htm?mid=100000:54444221
https://sports.qq.com/kbsweb/game.htm?mid=100000:54444219

> 有一个URL地址:"http://www.zhufengpeixun.cn/stu/?lx=1&name=AA&sex=man#teacher"地址问号后面的内容是我们需要解析出来的参数信息
#后面的成为哈希值，这个值可能有可能没有，我们需要处理，有的话我们截取的时候需要过滤掉
需要做的是：
以&进行拆分（数组）
遍历数组中的每一项，把每一项在按照=进行拆分，把拆分后的第一项作为对象的属性名，第二项作为属性值进行存储即可
```
var str = "http://www.zhufengpeixun.cn/stu/?lx=1&name=AA&sex=man#teacher"
var indexASK = str.indexOf('?');
var indexWell = str.indexOf('#')
if(indexWell>-1) //表示有#的时候
{
  str = str.substring(indexASK + 1, indexWell);
}
else
{
  str = str.substr(indexASK+1);
}
var  ary = str.split('&'),
	obj = {};
for(var i=0;i<ary.length;i++)
{
var item = ary[i],
itemAry = item.split('=');
var key = itemAry[0],
    value = itemAry[1];
  obj[key] = value;
}
console.log(obj);

//真正项目的时候是怎么做的？
~function  (pro){
	pro.queryURLParameter = function(){
	var obj ={},
	reg = /([^?=&#]+)(?:=([?=&#]+)?)/g;
	this.replace(reg,function(){
	var key = arguments[1],
	    value = arguments[2] || null;
	 ovj[key] = value;
	});
	return obj;
   }
}(String.prototype);

var str = 'http://www.zhufengpeixun.cn/stu/?lx=1&name=&sex#teacher';
console.log(str.queryURLParameter());

	
```
