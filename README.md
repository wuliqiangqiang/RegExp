# javascript正则表达式<sup>shine</sup>

## 基础

### 1、什么是正则表达式

来源于Perl的正则表达式是一门简单语言的语法规范，主要应用于字符串的信息实现查找、替换和提取的`技巧`操作。<br/>
这里强调是技巧操作，其处理字符串的速度是相当慢的，远不如indexOf、lastIndexOf、includes的速度快，所以勿滥用！<br/>
[正则表达式30分钟入门教程](http://deerchao.net/tutorials/regex/regex.htm)<br/>
先看几个题目：<br/>

- 1.要求用户名必须包含数字、大写字母、小写字母、长度8位以上，且不能以数字开头？<br/>
- 2.禁止密码出现三个连续相同的字符？<br/>
- 3.怎么剔除字符串中的html标签？

### 2、测试正则表达式

[正则表达式测试器](http://deerchao.net/tools/regex_tester/index.htm)<br/>
[可视化正则 https://regexper.com](https://regexper.com)

### 3、创建正则表达式

```javascript
var regex = new RegExp('xzy', 'i');
var regex = new RegExp(/xzy/i);
var regex = new RegExp(/xzy/i, 'i');//ES5不允许；ES6可以并且第二个参数指定的修饰符会覆盖前面的修饰符
// 等价于
var regex = /xzy/i;
var regex = eval("/xzy/i");//不建议
var regex = new Function("return /xzy/i")();//不建议

```

### 4、原子

原子是正则表达式的最基本组成单位，而且必须至少要包含一个原子。只要一个正则表达式可以单独使用的字符，就是原子。<br/>
.匹配除换行符以外的任意字符。

| 正则        　| 意思          | 说明  |
| ------------ |:-------------:| -----|
|\d |匹配一个数字字符|等价于 \[0-9]|
|\D |匹配一个非数字字符|等价于 \[^0-9]|
|\f |匹配一个换页符|等价于 \x0c 和 \cL|
|\n |匹配一个换行符|等价于 \x0a 和 \cJ|
|\r |匹配一个回车符|等价于 \x0d 和 \cM|
|\s |匹配任何空白字符，包括空格、制表符、换页符等等|等价于\[\f\n\r\t\v\u000B\u0020\u00A0\u2028\u2029]|
|\S |匹配任何非空白字符|等价于 \[^ \f\n\r\t\v]|
|\t |匹配一个制表符|等价于 \x09 和 \cI|
|\v |匹配一个垂直制表符|等价于 \x0b 和 \cK|
|\w |匹配包括下划线的任何单词字符|等价于\[A-Za-z0-9_]|
|\W |匹配任何非单词字符|等价于\[^A-Za-z0-9_]|
|\[A-Za-z]|匹配所有大小写字母| |
|\[a-f1-5]|自定义原子表又称`范围集合类`| |

更多参见基本多语言面（Basic Multilingual Plane,BMP）详细信息[基本多文种平面](http://baike.baidu.com/view/628163.htm)

### 4、元字符
元字符是一种特殊的字符，是用来修饰原子用的，不可以单独出现；<br/>
`量词类`

| 正则        | 说明|
|------------|-----|
|?   |等价{0,1}表示其前面的原子可以出现0次或1次，有只能有一次，要么没有|
|\+  |等价{1,}表示其前的原子可以出现1次 或多次，不能没有最少要有一个|
|\*  |等价{0,}表示其前的原子可以出现0次、1次、或多次|
|{m} |表示前面的原子出现m次|
|{m,}|表示前面的原子最少出现m次,最多无限|
|{m,n}|m要小于n,表示前面出现的原子，最少m次，最多n次，包括m和n次|

`边界类`

^ : 直接在一个正则表达式的第一个字符出现，则表达必须以这个正则表达式开始；<br/>
$ : 直接在一个正则表达式的最后一个字符出现，则表达必须以这个正则表达式结束；<br/>
\b : 表示一个边界；<br/>
\B : 表示一个非边界；<br/>
| : 表示或的关系 , 它的优先级号是最低的，最后考虑它的功能；<br/>
() : 重点，作用如下：<br/>
>A、作为大原子使用；<br/>
B、改变优先级,加上括号可以提高优先级别；<br/>
C、作为子模式使用；<br/>
D、可以取消子模式(?:)；<br/>
E、反向引用 \1 \2；<br/>

优先级：
>\ 最高<br/>
()(?:)[]<br/>
*+?{}<br/>
^$\b<br/>
| 最低<br/>

### 5、字符转义
\/[\](){}?+*|.^$-作为字符来匹配必须转义，才能作为正则的原子。
```JavaScript
function escapeRegExp(str) {
  return str.replace(/[\/[\](){}?+*|.^$-]/g, '\\$&');
}

//ES7将新增RegExp对象的静态方法RegExp.escape()
RegExp.escape('(*.*)');
// "\(\*\.\*\)"
```

## 提升

### 1、正则相关方法

| 方法        　| 返回          | 注意  |
| ------------- |:-------------:| -----|
| regexp.exec(string) | 数组 or null | 与g、lastIndex有关 |
| regexp.test(string) | true or false| 与g、lastIndex有关，不应该使用g |
| string.match(regexp)| 数组 or null | 与g相关,没g时与exec类似 |
| string.replace(search,replace)| string | 与g相关$$,$&,$number,$`,$'|
| string.search(regexp)| index or -1 | 与indexOf类似，忽略g |
| string.split(separator,limit)| 数组 | 忽略g，分组内容会存入数组 |

### 2、RegExp实例属性
| 属性       | 用法        |
| -----------|-------------|
| global     |如果使用了g，值为true|
| ignoreCase |如果使用了i，值为true|
| multiline  |如果使用了m，值为true|
| sticky 　　|如果使用了y，值为true, ES6|
| unicode　　|如果使用了u，值为true, ES6|
| dotAll　　|如果使用了s，值为true, ES7|
| flags  　　|返回正则表达式的修饰符, ES6|
| lastIndex  |下一次匹配开始的索引。初始值为0|
| source 　　|正则表达式源码文本|

```JavaScript
/^.$/u.test("𠮷") ;// true
/\u{20BB7}/u.test("𠮷") // true

var reg = /a\d/y;
var str = "a1a2ka3";
console.log(reg.lastIndex,reg.exec(str));//0,["a1"]
console.log(reg.lastIndex,reg.exec(str));//2,["a2"]
console.log(reg.lastIndex,reg.exec(str));//4,null
console.log(reg.lastIndex,reg.exec(str));//0,["a1"]

var reg = /a\d/giyu;
console.log(reg.flags);//giuy
```
### 3、范围集合类（分枝条件）

\[abc]，表示a或者b或者c中的任意一个字符；<br/>
\[a-z]、\[A-Z]、\[0-9]，表示小写字母，大写字母，0到9的数字；<br/>
\[^a-z]、\[^A-Z]、\[^0-9]，表示非小写字母，非大写字母，非0到9的数字；<br/>
\[abc|def]，表示abc和def中的任意一个；类似(abc|def)但会分组；

### 4、贪婪和非贪婪(懒惰)匹配
当正则表达式中包含能接受重复的限定符时，通常的行为是匹配尽可能多的字符。
```JavaScript
"aababxb".match(/a.*b/);
"aababxb".match(/a.*?b/);//懒惰匹配 返回aab
```
它将会匹配整个字符串。这被称为贪婪匹配。

| 代码/语法| 说明       |
| ---------|-----------|
| \*?	     |重复任意次，但尽可能少重复|
| +? 	     |重复1次或更多次，但尽可能少重复|
| ??  	     |重复0次或1次，但尽可能少重复|
|{n,m}?      |重复n到m次，但尽可能少重复|
|{n,}?       |重复n次以上，但尽可能少重复|

## 高级
### 1、分组
用小括号来指定`子表达式`(也叫做分组)。
```JavaScript
(\d{1,3}\.){3}\d{1,3}
((2[0-4]\d|25[0-5]|[01]?\d\d?)\.){3}(2[0-4]\d|25[0-5]|[01]?\d\d?)
```
| 代码/语法| 说明        |
| ---------|-------------|
| (exp)	   |匹配exp,并捕获文本到自动命名的组里|
| (?:exp)  |匹配exp,不捕获匹配的文本，也不给此分组分配组号|
| (?<name>exp) |匹配exp,并捕获文本到名称为name的组里，也可以写成(?'name'exp),js不支持|
|(?#comment)|这种类型的分组不对正则表达式的处理产生任何影响，用于提供注释让人阅读,js不支持|
#### 2、反向引用
使用小括号指定一个子表达式后，默认情况下，每个分组会自动拥有一个组号，规则是：从左向右，以分组的左括号为标志，第一个出现的分组的组号为1，第二个为2，以此类推。你可以使用(?:exp)这样的语法来剥夺一个分组对组号分配的参与权。
```JavaScript
/<(\w)>.*?<\1>/
"%12sd%32%sdf sdf%3".replace(/(%)(\d+)/g,"$2$1")
"123456789.00".replace(/\d(?=(\d{3})+\b)/g,"$&,");
```
| 代码/语法| 说明        |
| ---------|-------------|
| $$	   |直接量符号，即$字符|
| $n  |第n个子表达式相匹配的文本,n等于[1-99]|
| $&  |与 regexp 相匹配的子串|
| $`  |位于匹配子串左侧的文本|
| $'  |位于匹配子串右侧的文本|

### 3、零宽断言
| 代码/语法| 说明        |名称        |
| ---------|-------------|-------------|
| (?=exp) |匹配exp前面的位置|零宽度正预测先行断言,正向前瞻,正向预查|
| (?<=exp)  |匹配exp后面的位置，ES7支持|零宽度正回顾后行断言|
| (?!exp)  |匹配后面跟的不是exp的位置|零宽度负预测先行断言,负向前瞻,负向预查,先行否定断言|
| (?<!exp)  |匹配前面不是exp的位置，ES7支持|零宽度负回顾后发断言,后行否定断言|

```JavaScript
var str = "abaca";
//我们需要匹配a,且a后面不能是c。如果写成/a[^c]/g将无法匹配最后一个a,因为[^c]占了一个字符宽度
str.match(/a(?!c)/g);//(?!c)是不占宽度，也不计入返回结果

var reg = /(?=^[A-Za-z0-9]{8,}$)(?=[^\d]+\d)(?=.*[a-z])(?=.*[A-Z])\w*/;

/\d+(?=%)/.exec('99 and 100%')  // ["100"]

"mmxxsdfsdfmmmsdfsdfmm".match(/(?<!m)mm(?!m)/g);
```
目前，有一个提案，在ES7加入后行断言。V8引擎4.9版已经支持，Chrome浏览器49版在地址栏键入about:flags，然后开启“实验性 JavaScript”功能并重启浏览器，就可以使用这项功能。
```JavaScript
/(?<=(\d+)(\d+))$/.exec('1053') // ["", "1", "053"]
/(\d+)(\d+)/.exec('1053') // ["1053", "105", "3"]

/(?<=(o)d\1)r/.exec('hodor')  // null
/(?<=\1d(o))r/.exec('hodor')  // ["r", "o"]
```
后行断言的特点是从右向左匹配，分组编号虽然一样从左到右分配，但引用时必须在编号的左边引用。
### 应用
```JavaScript
//去除字符串两端的空格
String.prototype.trim = String.prototype.trim || function (){ return this.replace(/^\s*|\s*$/g,"")};

//格式化HTML
String(text).replace(/&/g,'&amp;').replace(/\</g,'&lt;').replace(/\>/g,'&gt;')
.replace(/"/g, '&quot;').replace(/'/g, '&#39;');

//格式化时间
function formatDate(str,time){
    time = time && time.toUTCString() != "Invalid Date" ? time : new Date();
    var obj = {
        YYYY :  time.getFullYear(),
        YY   :  (""+ time.getFullYear()).slice(-2),
        M    :  time.getMonth()+1,
        MM   :  ("0"+ (time.getMonth()+1)).slice(-2),
        D    :  time.getDate(),
        DD   :  ("0" + time.getDate()).slice(-2),
        H    :  time.getHours(),
        HH   :  ("0" + time.getHours()).slice(-2),
        m    :  time.getMinutes(),
        mm   :  ("0" + time.getMinutes()).slice(-2),
        s    :  time.getSeconds(),
        ss   :  ("0" + time.getSeconds()).slice(-2),
        w    :  ['日', '一', '二', '三', '四', '五', '六'][time.getDay()]
    };
    return str.replace(/([a-z]+)/ig,function(a){return obj[a]||""});
}
console.log(formatDate("YYYY-MM-DD HH:mm:ss 星期w"));

//获取url参数
function getUrlParam(key,url){
    url = url ? url.split("#")[0] : location.search;
    var arr = [];
    url.replace(new RegExp("[&?]"+ key + "=([^&#]*)","ig"), function(a,b) {
        arr.push(decodeURIComponent(b));
    });
    return arr.length > 1 ? arr : arr.join("");
}
console.log(getUrlParam("a","http://www.baidu.com/?a=我们&a=2#a=3&a=4"));

//字符规则
var reg1 = /^[a-z\d!@＃$％^&*()_ \- +=?]{3,6}$/i;
var reg2 = /^(?=.*\d)[a-z\d!@＃$％^&*()_ \- +=?]{3,6}$/i;//必须含有数字
var reg3 = /^(?!\d+)[a-z\d!@＃$％^&*()_ \- +=?]{3,6}$/i;//不能全是数字
var reg4 = /^(?!\d+$|[a-z]+$|[!@＃$％^&*()_ \- +=?]+$)[a-z\d!@＃$％^&*()_ \- +=?]{3,6}$/i;//不能只含一种字符（即必须含有两种以上）
var reg5 = /^(?=.*\d)(?=.*[a-z])(?=.*[!@＃$％^&*()_ \- +=?])[a-z\d!@＃$％^&*()_ \- +=?]{3,6}$/i;//必须含有三种以字符
```
### 性能
核心：减少“回溯”次数，尽快匹配结果：<br/>
1、尽量使用边界符（^、$、\b、\B等），限定搜索字符串位置<br/>
2、使用具体的元字符（\d、\w、\s等），少用”.”字符<br/>
3、多使用确定的量词（{n}、{n,m}），少用贪婪匹配<br/>
4、减少分支，减少“回溯”<br/>

====

### ECMAScript 2018正则相关扩展

#### 正则表达式的“dotall”标志

以前介绍dot（“.”）是`匹配除换行符（\n \r \f）之外的任何单个字符`，虽然其实它本来就是`匹配任何单个字符`。