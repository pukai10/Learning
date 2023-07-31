## 基本类型

### 数字类型 number  
双精度64位浮点数,如:
```
let binaryLiteral: number = 0b1010; //二进制
let octalLiteral: number = 0o744;//八进制
let decLiteral: number = 6;//十进制 
let hexLiteral: number = 0xf00d;//十六进制
```

### 字符串类型 string
一个字符系列，使用单引号(')或者双引号(")来表示，反引号(\`) 来定义多行文本和内嵌表达式，如：
```
let str:string = "str"; 
let str_name = 'HuaWei';
let years:number = 5; 
let words:string = \`今年是${str_name},发布${years + 1}周年`;  //今年是HuaWei，发布6周年
```

### 布尔类型 boolean
表示逻辑值:true或者false
```
let flag: boolean = false;
```

### 数组类型
声明数组有两种方式，如:
```
//在元素类型后加[]
let arr:number[] = [1,2];
//使用数组泛型
let arr:Array<number> = [3,4];
```

### 元组
元组类型用来表示已知元素数量和类型得数组，各元素得类型不必相同，对应位置得类型需要相同，如：
```
let x :[string,number];
x = ["Rub",1];    //正确
x = [1,"Rub"];    //错误
console.log(x[0]);   //输出:Rub
```

### 枚举 enum
```
enum Color {Red,Green,Blue};
let c:Color = Color.Bule;
console.log(c);  //输出:2
```

### void
用于标识方法返回值得类型，表示该方法没有返回值:
```
function hello():void{
	alert("Hello,Rub");
}
```

### 任意类型 any
声明为any的变量可以赋予任意类型的值，通常用于以下三种情况
1、变量的值会动态改变时，比如来自用户的输入，任意类型可以让这些变量跳过编译阶段的类型检查如:
```
let x:any = 1;
x = "I am rub";
x = false;
```
2、改写现有代码时，任意值允许在编译时可选择地包含或移除类型检查，如:
```
let x:any = 4;
x.ifItExists();    // 正确，ifItExists方法在运行时可能存在，但这里并不会检查
x.toFixed();    // 正确
```
3、定义储存各种类型数据的数组时，如:
```
let arrayList: any[] [1,false,'str'];
arrayList[1] = 100;
```

### null类型 null
表示对象值缺失，表示空对象引用，用typeof检测null返回是object

### undefined类型 undefined
用于初始化变量为一个未定义的值，typeof返回undefined
null和undefined是其他任何类型(包括void)的子类型，可以赋值给其他类型，此时，赋值后的类型会变成null或undefined,而typescript中启用严格空校验（--strictNullChecks）特性,就可以使得null 和 undefined 只能被赋值给 void 或本身对应的类型，示例代码如下：
```
// 启用 --strictNullChecks
let x: number;
x = 1; // 编译正确
x = undefined;    // 编译错误
x = null;    // 编译错误
```
上面的例子中变量 x 只能是数字类型。如果一个类型可能出现 null 或 undefined， 可以用 | 来支持多种类型，示例代码如下：
```
// 启用 --strictNullChecks
let x: number | null | undefined;
x = 1; // 编译正确
x = undefined;    // 编译正确
x = null;    // 编译正确
```

### never类型 never
never 是其它类型（包括 null 和 undefined）的子类型，代表从不会出现的值。这意味着声明为 never 类型的变量只能被 never 类型所赋值，在函数中它通常表现为抛出异常或无法执行到终止点（例如无限循环），示例代码如下：
```
let x: never;
let y: number;

// 编译错误，数字类型不能转为 never 类型
x = 123;

// 运行正确，never 类型可以赋值给 never类型
x = (()=>{ throw new Error('exception')})();

// 运行正确，never 类型可以赋值给 数字类型
y = (()=>{ throw new Error('exception')})();

// 返回值为 never 的函数可以是抛出异常的情况
function error(message: string): never {
    throw new Error(message);
}

// 返回值为 never 的函数可以是无法被执行到的终止点的情况
function loop(): never {
    while (true) {}
}
```



## 运算符

### 算术运算符

|运算符|例子|x运算结果|y运算结果|
| :----: | :----: | :----: | :----: |
| + |x = y + 2 | 7 | 5 |
| - |x = y - 2 | 3 | 5 |
| * |x = y * 2 | 10 | 5 |
| / |x = y / 2 | 2.5 | 5 |
| % | x = y % 2 | 1 | 5 |
| ++ | x = ++y或 x = y++ | 6或5 | 6或6 |
| -- | x = --y或 x = y-- | 4或5 | 4或4 |

```
var num1:number = 10  
var num2:number = 2  
var res:number = 0  
     
res = num1 + num2  
console.log("加:        "+res);  
  
res = num1 - num2;  
console.log("减: "+res)  
  
res = num1*num2  
console.log("乘:    "+res)  
  
res = num1/num2  
console.log("除:   "+res)  
     
res = num1%num2  
console.log("余数:   "+res)  
  
num1++  
console.log("num1 自增运算: "+num1)  
  
num2--  
console.log("num2 自减运算: "+num2)

//运算结果
加:        12
减: 8
乘:    20
除:   5
余数:   0
num1 自增运算: 11
num2 自减运算: 1
```

### 关系运算符
x = 5,如下表:

| 运算符 | 描述 | 比较 | 返回值 |
| :----: | :----: | :----: | :----: |
| == | 等于 | x == 8或x == 5 | false或true |
| != | 不等于 | x != 8 | true |
| > | 大于 | x > 8 | false |
| < | 小于 | x < 8 | true |
| >= | 大于等于 | x >= 8 | false |
| <= | 小于等于 | x <= 8 | true |

### 逻辑运算符
给定x = 6,y = 3,如下表:

| 运算符 | 描述 | 例子 | 返回值 |
| :----: | :----: | :----: | :----: |
| && | and | (x < 10 && y > 1) | true |
| \|\| | or | (x == 5 \|\| y == 5) | false |
| ! | not | !(x == y ) | true |

### 位运算符

| 运算符 | 描述 | 例子 | 类似于 | 结果 | 十进制 |
| :----: | :----: | :----: | :----: | :----: | :----: |
| & | 与 | x = 5 & 1 | 0101 & 0001 | 0001 | 1 |
| \| | 或 | x = 5 \| 1 | 0101 \| 0001 | 0101 | 5 |
| ~ | 非/取反 | x = ~5 | ~0101 | 1010 | -6 |
| ^ | 异或 | x = 5 ^ 1 | 0101 ^ 0001 | 0100 | 4 |
| << | 左移 | x = 5 << 1 | 0101 << 1 | 1010 | 10 |
| >> | 右移 | x = 5 >> 1 | 0101 >> 1 | 0010 | 2 |
| >>> | 无符号右移 | x = 2 >>> 1 | 0010 >>> 1 | 0001 | 1 |

### 赋值运算符
x = 10,y = 5

|运算符|例子|实例| x值 |
| :----: | :----: | :----: | :----: |
| =(赋值) |x = y | x = y | 5 |
| +=(先加后赋值) |x += y | x = x + y | 15 |
| -= | x -= y | x = x - y | 5 |
| \*= |x \*= y | x = x \* y | 50 |
| /= | x /= y  | x = x / y | 2 |

### 三元运算符

```
var num:number = -2 var result = num > 0 ? "大于 0" : "小于 0，或等于 0" console.log(result)    
输出 小于 0，或等于 0
```

### 类型操作符
#### typeof运算符

```
var num = 12;
console.log(typeof num);   //输出 number
```

#### instanceof
instanceof 运算符用于判断对象是否为指定的类型



## 条件语句
### 条件语句
```
//if语句
var num:number = 5 
if (num > 0) {
	console.log("数字是正数") 
}

//if else 语句
var num:number = 12; 
if (num % 2==0) { 
	console.log("偶数"); 
} else {
	console.log("奇数");
}

//if...else if...else
var num:number = 2;
if(num > 0){
	console.log(num + "是正数");
}else if(num < 0){
	console.log(num + "是负数");
}else {
	console.log(num + "是不是正数也不是负数");
}

//switch...case语句
var grade:string = "A";
switch(grade){
	case "A":{
		console.log("优");
		break;
	}
	case "B":{
		console.log("良");
		break;
	}
	case "C":{
		console.log("及格");
		break;
	}
	case "D":{
		console.log("不及格");
		break;
	}
	default:{
		console.log("非法输入");
		break;
	}
}
```



## 循环语句

### for循环
```
var num:number = 5;
var i:number;
var factorial = 1;
for(i = num; i >= 1; i--)
{
	factorial *= i;
}
console.log(factorial)
输出 120
```

### for...in循环
该语句用于一组值得集合或列表进行迭代输出

```
var j:any;
var n:any = "a b c"
for(j in n)
{
	console.log(n[j] + "," + j)
}
输出
a,0
 ,1
b,2
 ,3
c,4
```

### for...of、forEach、every和some循环

```
//for..of
let someArray = [1,"string",false]
for(let entry of someArray)
{
    console.log(entry)
}
输出
1
string
false

//forEach、every 和 some 是 JavaScript 的循环语法，TypeScript 作为 JavaScript 的语法超集，当然默认也是支持的。因为 forEach 在 iteration 中是无法返回的，所以可以使用 every 和 some 来取代 forEach。
let list = [4,5,6]
list.forEach((val,idx,array)=>{
    //var 当前值，idx当前索引， array Array
    console.log(`val:${val},idx:${idx},array:${array}`)
})
输出
val:4,idx:0,array:4,5,6
val:5,idx:1,array:4,5,6
val:6,idx:2,array:4,5,6
```

### while循环
```
var num:number = 5;
var fatorial:number  = 1;
while(num >= 1)
{
	fatorial *= num;
	num--;
}
console.log(fatorial)

输出 120
```

### do...while循环
```
var doN:number = 10;
do {
    console.log(doN);
    doN--;
} while(doN>=0);
输出
10
9
8
7
6
5
4
3
2
1
0
```

### break语句
**break** 语句有以下两种用法：
1. 当 **break** 语句出现在一个循环内时，循环会立即终止，且程序流将继续执行紧接着循环的下一条语句。
2. 它可用于终止 **switch** 语句中的一个 case。

如果您使用的是嵌套循环（即一个循环内嵌套另一个循环），break 语句会停止执行最内层的循环，然后开始执行该块之后的下一行代码。

### continue语句
**continue** 语句有点像 **break** 语句。但它不是强制终止，continue 会跳过当前循环中的代码，强迫开始下一次循环。

对于 **for** 循环，**continue** 语句执行后自增语句仍然会执行。对于 **while** 和 **do...while** 循环，**continue** 语句重新执行条件判断语句。



## 函数
### 函数定义
```
function function_name()
{

}
```

### 函数调用
```
function test()
{
	console.log("test函数调用")
}

test()   //调用函数
```

### 函数返回值
```
function greet():string
{
	return "Hello world"
}

function caller()
{
	var  msg = greet();
	console.log(msg)
}

caller()
```

### 带参数函数
```
function add(x:number,y:number):number
{
	return x + y;
}

console.log(add(1,2))
```

### 可选参数和默认参数
在TypeScript函数里，如果我们定义了函数参数，则必须传入这些参数，除非将这些参数设置为可选的，可选参数使用问号(?)标识
```
function buildName(firstName:string,lastName?:string)
{
	if(lastName)
		return firstName + " " + lastName
	else
		return firstName
}

let result1 = buildName("Bob")
let result2 = buildName("Bob","Adams")
let result3 = buildName("Bob","Adams","Sr.")  //错误，参数太多
```
默认参数

```
function calculate_discount(price:number,rate:number = 0.5)
{
	var discount = price * rate
	console.log("计算结果:" , discount)
}

calculate_discount(1000)
calculate_discount(1000,0.3)

```

### 剩余参数
有一种情况，我们不知道要向函数传入多少个参数，这时候我们就可以使用剩余参数来定义。
剩余参数语法允许我们将一个不确定数量的参数作为一个数组传入。
```
function addNumbers(...nums:number[])
{
    var i;
    var sum:number  = 0;
    for (i =0;i < nums.length;i++)
    {
        sum += nums[i];
    }
    console.log("和为:",sum)
}
addNumbers(1,2,3)
addNumbers(10,10,10,10,10)

输出 
6
50
```

### 匿名函数
匿名函数是一种没有函数名的函数，匿名函数在程序运行时动态声明，除了没有函数名外，其他的与标准函数一样。我们可以将匿名函数赋值给一个变量，这种表达式称为函数表达式。
```
//不带参数的匿名函数
var msg = function()
{
	return "hello world"
}
console.log(msg())

//带参数的匿名函数
var res = function(a:number,b:number){
	return a*b
}
console.log(res(1,2))
```
匿名函数自调用在函数后使用()即可:
```
(function(){
	var x = "hello!"
	console.log(x)
})()
```

### 构造函数
TypeScript 也支持使用 JavaScript 内置的构造函数 Function() 来定义函数：
语法格式如下：
```
var res = new Function ([arg1[, arg2[, ...argN]],] functionBody)
```
参数说明：
- **arg1, arg2, ... argN**：参数列表。
- **functionBody**：一个含有包括函数定义的 JavaScript 语句的字符串。

```
var myFunction = new Function("a","b","return a * b")
var x = myFunction(4,3);
console.log(x)
输出 12
```
### 递归函数
递归函数即在函数内调用函数本身
```
function factorial(number) {
    if (number <= 0) {         // 停止执行
        return 1;
    } else {    
        return (number * factorial(number - 1));     // 调用自身
    }
};
console.log(factorial(6));      // 输出 720
```

### Lambda函数
语法格式:
```
( [param1, param2,…param n] )=>statement;
```
```
var foo = (x:number)=> 10 + x;
console.log(foo(100)) //输出 110
```

### 函数重载
重载是方法名字相同，而参数不同，返回类型可以相同也可以不同。
每个重载的方法（或者构造函数）都必须有一个独一无二的参数类型列表。
参数类型不同：
```
function disp(string):void; 
function disp(number):void;
```

参数数量不同：
```
function disp(n1:number):void; 
function disp(x:number,y:number):void;
```

参数类型顺序不同：
```
function disp(n1:number,s1:string):void; 
function disp(s:string,n:number):void;
```
如果参数类型不同，则参数类型应设置为 **any**。
参数数量不同你可以将不同的参数设置为可选。


## Number对象
Number对象是原始数值的包装对象
语法
```
var num = new Number(value)
```

### Number对象属性
```
console.log("TypeScript Number 属性:");
console.log("最大值为:",Number.MAX_VALUE);
console.log("最小值为:" + Number.MIN_VALUE);
console.log("负无穷大:",Number.NEGATIVE_INFINITY)
console.log("正无穷大:",Number.POSITIVE_INFINITY)

输出结果
TypeScript Number 属性:
最大值为: 1.7976931348623157e+308
最小值为: 5e-324
负无穷大: -Infinity
正无穷大:Infinity
```



### NaN实例
```
var month = 0;
if(month <= 0 || month > 12){
	month = Number.NaN
	console.log("月份是:",month)
}else{
	console.log("输入月份数值正确")
}
```



### prototype实例
```
function employee(id:number,name:string){
	this.id = id;
	this.name = name
}

var emp = new employee(123,"admin");
employee.prototype.email = "admin@runoob.com"
console.log("员工ID:" + emp.id)
console.log("员工名字:" + emp.name)
console.log("员工邮箱:" + emp.email)
输出结果
员工ID:123
员工名字:admin
员工邮箱:admin@runoob.com
```




### Number对象方法
#### toExonential() 把对象的值转为指数计数法
```
var num1 = 1234.5
var val = num1.toExponential();
console.log(val)
输出
1.2345e+3
```


#### toFixed() 把数字转为字符串，并对小数点指定位数
```
var num3 = 177.234
console.log("num3.toFixed()为:" + num3.toFixed())  //输出 177
console.log("num3.toFixed(2)为:" + num3.toFixed(2))  //输出 177.23
console.log("num3.toFixed(6)为:" + num3.toFixed(6))  //输出 177.234000
```



#### toLocaleString() 把数字转换为字符串，使用本地数字格式顺序
```
var num = new Number(177.1234)
console.log(num.toLocaleString())  //输出 1.77.1234
```


#### toPrecision() 把数字格式化为指定长度
```
var num = new Number(7.123456); 
console.log(num.toPrecision());  // 输出：7.123456 
console.log(num.toPrecision(1)); // 输出：7
console.log(num.toPrecision(2)); // 输出：7.1
```


#### toString() 把数字转化为字符串，使用指定的基数，数字的基数是2~36之间的证书，若省略该参数，则使用基数10
```
var num = new Number(10); 
console.log(num.toString());  // 输出10进制：10
console.log(num.toString(2)); // 输出2进制：1010
console.log(num.toString(8)); // 输出8进制：12
```


#### valueOf() 返回一个Nunber对象的原始数字值
```
var num = new Number(10);
console.log(num.valueOf());
```



## String 字符串
语法
```
var txt = new String("string")
或
var txt = "string"
```

### String 对象的属性
#### constructor 对创建该对象的函数的引用
```
var str = new String("this is string")
console.log("str.constructor is :" , str.constructor)
输出
str.constructor is : [Function: String]
```

#### length 返回字符串长度
```
var uname = new String("Hello World");
console.log("Length:",uname.length)
输出
Length: 11
```

#### prototype 允许您向对象添加属性和方法
[[#prototype实例]]

### String方法
#### charAt() 返回指定位置的字符
```
var str = new String("abcdefg")
console.log("str.charAt(0):" + str.charAt(0));  //a
console.log("str.charAt(1):" + str.charAt(1));  //b
console.log("str.charAt(2):" + str.charAt(2));  //c
console.log("str.charAt(3):" + str.charAt(3));  //d
console.log("str.charAt(4):" + str.charAt(4));  //e
console.log("str.charAt(5):" + str.charAt(5));  //f
console.log("str.charAt(6):" + str.charAt(6));  //g
```

#### charCodeAt() 返回指定的位置的字符的Unicode编码
```
var str = new String("abcdefg")  
console.log("str.charCodeAt(0) 为:" + str.charCodeAt(0)); // 97
console.log("str.charCodeAt(1) 为:" + str.charCodeAt(1)); // 98
console.log("str.charCodeAt(2) 为:" + str.charCodeAt(2)); // 99
console.log("str.charCodeAt(3) 为:" + str.charCodeAt(3)); // 100
console.log("str.charCodeAt(4) 为:" + str.charCodeAt(4)); // 101
console.log("str.charCodeAt(5) 为:" + str.charCodeAt(5)); // 102
console.log("str.charCodeAt(6) 为:" + str.charCodeAt(6)); // 103
```

#### concat() 连接两个或更多字符串，并返回新的字符串
```
var str1 = new String("Run");
var str2 = new String("God");
var str3 = str1.concat( str2.valueOf() );
console.log(str3)   //RunGod
```

#### indexOf() 返回某个指定的字符串值在字符串中首次出现的位置
```
var str1 = new String("RUNOOB");
var index = str1.indexOf("OO")
console.log(index)   //3
```

#### lastIndexOf() 从后向前搜索字符串，并从起始位置(0)开始计算返回字符串最后出现的位置
```
var str1 = new String( "This is string one and again string" );
var index = str1.lastIndexOf( "string" );
console.log("lastIndexOf 查找到的最后字符串位置 :" + index ); // 29
index = str1.lastIndexOf( "one" );
console.log("lastIndexOf 查找到的最后字符串位置 :" + index ); // 15
```

#### localeCompare() 使用本地特定顺序来比较两个字符串
```
var str1 = new String("This is beautiful string")
var index = str1.localeCompare("This is beautiful string")
console.log(index)    // 0
```

#### match() 查找找到一个或多个正则表达式的匹配
```
var str = "The rain is SPAIN stays mainly in the plain";
var n = str.match(/ani/g)
console.log(n)   //[ 'ain', 'ain', 'ain' ]
```

#### replace() 替换与正则表达式匹配的字符串
```
var re = /(\w+)\s(\w+)/;
var str = "zara ali";
var newStr = str.replace(re,"$2,$1");
console.log(newStr)   //ali,zara
```

#### search() 检索与正则表达式匹配的值
```
var re = /apples/gi;
var str = "Apples are round,and apples are juicy."
if(str.search(re) == -1){
	console.log("Does not contain Apples")
}else{
	console.log("Contains Apples")
}
```

#### slice() 提取字符串的片断，并在新的字符串中返回被提取的部分

#### split() 把字符串分割为子字符串数组
```
var str = "Apples are round, and apples are juicy.";
var splitted = str.split(" ", 3);
console.log(splitted)  // [ 'Apples', 'are', 'round,' ]
```

#### substr() 从起始索引号提取字符串中指定数目的字符

#### substring() 提取字符串中两个指定的索引号之间的字符
```
var str = "RUNOOB GOOGLE TAOBAO FACEBOOK";
console.log("(1,2): "    + str.substring(1,2));   // U
console.log("(0,10): "   + str.substring(0, 10)); // RUNOOB GOO
console.log("(5): "      + str.substring(5));     // B GOOGLE TAOBAO FACEBOOK
```

#### toLocaleLowerCase() 根据主机的语言环境把字符串转换为小写，只有几种语言具有其他特有的大小写映射
```
var str = "Runoob Google"; 
console.log(str.toLocaleLowerCase( ));  // runoob google
```

#### toLocaleUpperCase() 据主机的语言环境把字符串转换为大写，只有几种语言（如土耳其语）具有地方特有的大小写映射。
```
var str = "Runoob Google"; 
console.log(str.toLocaleUpperCase( ));  // RUNOOB GOOGLE
```

#### toLowerCase() 把字符串转换为小写
```
var str = "Runoob Google"; 
console.log(str.toLowerCase( ));  // runoob google
```

#### toString() 返回字符串
```
var str = "Runoob"
console.log(str.toString())   //Runoob
```

#### toUpperCase() 把字符串转换成大写
```
var str = "Runoob Google"; 
console.log(str.toUpperCase( ));  // RUNOOB GOOGLE
```

#### valueOf() 返回指定字符串对象的原始值
```
var str = new String("Runoob"); 
console.log(str.valueOf( ));  // Runoob
```

## Array对象
Array对象的构造函数接受两种值:
- 表示数组大小的数值
- 初始化的数组列表，元素使用逗号分隔值
```
var arr_names:number[] = new Array(4);
var sites:string[] = new Array("Google","Runoob","Taobao","FaceBook")
```

#### 数组解构
可以把数组元素赋值给变量
```
var arr:number[] = [12,13];
var [x,y] = arr
console.log(x + "," + y); //12 13
```

### 数组迭代
```
var j:any;
var nums:number[] = [1001,1002,1003]
for(j in nums)
{
	console.log(nums[j])
}
输出
1001
1002
1003
```

### 多维数组
```
var multi:number[][] = [[1,2,3],[23,24,25]]
console.log(multi[0][0])
console.log(multi[0][1])
console.log(multi[0][2])
console.log(multi[1][0])
console.log(multi[1][1])
console.log(multi[1][2])
```

### 数组在函数中的使用
- 作为参数传递给函数
- 作为函数的返回值

### 数组方法
#### concat() 连接两个或更多的数组，并返回结果
```
var alpha = ["a","b","c"];
var numeric = ["1","2","3"];
var alphaNumeric = alpha.concat(numeric);
console.log(alphaNumeric);    // [ 'a', 'b', 'c', '1', '2', '3' ]

```
#### every() 检测数值元素的每个元素是否都符合条件
```
function isBigEnough(element,index,array){
	return element >= 10;
}

var passed = [12,5,8,130,44].every(isBigEnough);
console.log("Test Value:" + passed);    //Test Value:false
```

#### filter() 检测数值元素，并返回符合条件所有元素的数组
```
function isBigEnough(element,index,array){
	return element >= 10;
}

var passed = [12,5,8,130,44].filter(isBigEnough);
console.log("Test Value:" + passed);    //Test Value:12,130,44
```

#### forEach() 数组每个元素都执行一次回调函数
```
let num = [2,4,6]
num.forEach(function(value){
	console.log(value)
})
输出
2
4
6
```

#### indexOf() 搜索数组中的元素，并返回它所在的位置，如果搜索不到，返回值-1
```
let num = [2,4,6 ]
console.log("index is :" + num.indexOf(4))    //1
console.log("index is :" + num.indexOf(7))    //-1
```

#### join() 把数组的所有元素放入一个字符串
```
var arr = new Array("Googl","taobao","jingdong")
var str = arr.join()
console.log(str)          //Googl,taobao,jingdong
str = arr.join(" , ")    
console.log(str)          //Googl , taobao , jingdong
str = arr.join(" + ")
console.log(str)          //Googl + taobao + jingdong
```

#### lastIndexOf() 返回一个指定的字符串最后出现的位置，在一个字符串中的指定位置从后向前搜索
```
var index = [12, 5, 8, 130, 44];
console.log("index is : " + index.lastIndexOf(8));  // 2
console.log("index is : " + index.lastIndexOf(100));  // -1
```

#### map() 通过指定函数处理数组的每个元素，并返回处理后的数组
```
var numbers = [1, 4, 9]; 
var roots = numbers.map(Math.sqrt); 
console.log("roots is : " + roots );  // 1,2,3
```

#### pop() 删除数组的最后一个元素并返回删除后的元素
```
var nums = [1,4,9]
var element = nums.pop();
console.log(element)   // 9
element = nums.pop()
console.log(element)   //4
element = nums.pop()
console.log(element)   //1
element = nums.pop()
console.log(element)   //undefined
```

#### push() 向数组的末尾添加一个或更多元素，并返回新的长度

```
var numbers = new Array(1, 4, 9);
var length = numbers.push(10);
console.log("new numbers is : " + numbers  + ",length:" + length); 
// 1,4,9,10 ,length:4
length = numbers.push(20);
console.log("new numbers is : " + numbers + ",length:" + length ); 
// 1,4,9,10,20,length:5
```

#### reduce() 将数组元素计算为一个值(从左到右)
```
var total = [0,1,2,3].reduce(function(a,b){
    console.log(a,b)
    return a + b;}
);
console.log("total:",total) 
输出
0 1
1 2
3 3
total: 6
```

#### reduceRight() 将数组元素计算为一个值（从右到左）。
```
var total = [0,1,2,3].reduce(function(a,b){
    console.log(a,b)
    return a + b;}
);
console.log("total:",total) 
输出
3 2
5 1
6 0
total: 6
```

#### reverse() 反转数组的元素顺序
```
var nums: number[] = [1,2,4]
console.log(nums.reverse())    //[ 4, 2, 1 ]
```

#### shift() 删除并返回数组的第一个元素

```
var nums: number[] = [1,2,4]
console.log(nums.shift())    //1
```

#### slice() 选取数组的一部分，并返回一个新数组
```
var arr = ["orange", "mango", "banana", "sugar", "tea"]; 
console.log("arr.slice( 1, 2) : " + arr.slice( 1, 2) );  // mango
console.log("arr.slice( 1, 3) : " + arr.slice( 1, 3) );  // mango,banana
```
#### some() 检测数组元素中是否有元素符合指定条件
```
function isBigEnough(element, index, array) { 
   return (element >= 10); 
          
} 
          
var retval = [2, 5, 8, 1, 4].some(isBigEnough);
console.log("Returned value is : " + retval );  // false
          
retval = [12, 5, 8, 1, 4].some(isBigEnough); 
console.log("Returned value is : " + retval );  // true
```

#### sort() 对数组的元素进行排序
```
var arr = new Array("orange", "mango", "banana", "sugar"); 
var sorted = arr.sort(); 
console.log("Returned string is : " + sorted );  // banana,mango,orange,sugar
```

#### splice() 从数组中添加或删除元素
```
var arr = ["orange", "mango", "banana", "sugar", "tea"];  
var removed = arr.splice(2, 0, "water");  
console.log("After adding 1: " + arr );    // orange,mango,water,banana,sugar,tea 
console.log("removed is: " + removed); 
          
removed = arr.splice(3, 1);  
console.log("After removing 1: " + arr );  // orange,mango,water,sugar,tea 
console.log("removed is: " + removed);  // banana
```

#### toString() 把数组转换为字符串，并返回结果
```
var arr = new Array("orange", "mango", "banana", "sugar");         
var str = arr.toString(); 
console.log("Returned string is : " + str );  // orange,mango,banana,sugar
```

#### unshift()向数组的开头添加一个或多个元素，并返回新的长度
```
var arr = new Array("orange", "mango", "banana", "sugar"); 
var length = arr.unshift("water"); 
console.log("Returned array is : " + arr );  // water,orange,mango,banana,sugar 
console.log("Length of the array is : " + length ); // 5
```

## Map对象

Map使用new关键字来创建:
```
let map = new Map();
```
### Map相关函数和属性
#### set()/get() 设置键值对，返回Map对象 /返回键对应的值，如果不存在返回undefined
```

//设置值
map.set("key1",1);
map.set("key2",2);
map.set("key3",3);

console.log(map)   //Map(3) { 'key1' => 1, 'key2' => 2, 'key3' => 3 }

//获取值
console.log(map.get("key2"));   //2
console.log(map.get("key4")); // undefined
```

#### has() 返回一个布尔值，用于判断Map中是否包含键对应的值
```
console.log(map.has("key1"))  //true
console.log(map.has("key4"))  // false
```
#### size 返回Map对象键值对的数量
```
console.log(map.size)    // 3
```

#### keys() 返回一个iterator对象，包含了Map对象中每个元素的键
#### values() 返回一个新的iterator，包含了Map对象中每个元素的值

#### delete() 删除Map中的元素，删除成功返回true,失败返回false
```
console.log(map.delete("key1"))    //true
console.log(map.delete("key4"))    //false
console.log(map)                  //Map(2) { 'key2' => 2, 'key3' => 3 }
```
#### clear() 移除Map对象的所有键/值对
```
map.clear()
console.log(map)     //Map(0) {}
```

### Map迭代
Map对象中的元素是按顺序插入的，我们可以迭代Map对象，每一次迭代返回[k,v]数组，使用for...of来实现迭代
```
for(let key of map.keys())
{
	console.log(key)
}

for(let value of map.values())
{
	console.log(value)
}

for(let entry of map.entries())
{
	console.log(entry[0],entry[1])
}

for(let [key,value] of map)
{
	console.log(key,value)
}

输出
key1
key2
key3
1
2
3
key1 1
key2 2
key3 3
key1 1
key2 2
key3 3
```


### error TS2583: Cannot find name 'Map'. Do you need to change your target library? Try changing the 'lib' compiler option to 'es2015' or later.
typescript默认使用ES5,不支持ES6
编译单个文件
```
tsc --target es6 --module commonjs app.ts
```

## 元组
元组中允许储存不同类型的元素，元组可以作为参数传递给函数
```
var mytuple1 = [10,"Test"]
//或者
var mytuple2 = []
mytuple2[0] = 120;
mytuple2[1] = "test"

console.log(mytuple1[0],mytuple1[1])   //10 Test
console.log(mytuple2[0],mytuple1[2],mytuple2[2])   // 120 Test undefined

```

### 元组运算
#### push() 向元组添加元素，添加到最后面
#### pop() 从元组中移除最后一个元素，并返回移除的元素
```
var mytuple = [10,"Hello","World","typeScript"]; 
console.log("添加前元素个数："+mytuple.length) // 返回元组的大小 
mytuple.push(12) // 添加到元组中 
console.log(mytuple)   //[ 10, 'Hello', 'World', 'typeScript', 12 ]
console.log("添加后元素个数："+mytuple.length) 
console.log("删除前元素个数："+mytuple.length) 
console.log(mytuple.pop()+" 元素从元组中删除") // 删除并返回删除的元素 
console.log("删除后元素个数："+mytuple.length)
console.log(mytuple)   [ 10, 'Hello', 'World', 'typeScript' ]
```

### 更新元组
```
var mytuple = [10, "Runoob", "Taobao", "Google"]; // 创建一个元组
console.log("元组的第一个元素为：" + mytuple[0])
// 更新元组元素
mytuple[0] = 121    
console.log("元组中的第一个元素更新为："+ mytuple[0])
console.log( mytuple)
输出
元组的第一个元素为：10
元组中的第一个元素更新为：121
[ 121, 'Runoob', 'Taobao', 'Google' ]
```

### 解构元组
可以把元组元素赋值给变量，如下所示：
```
var a =[10,"test"] 
var [b,c] = a 
console.log( b )     //10
console.log( c )     //test
```

## 联合类型

### 实例
联合类型(Union Types)可以通过管道(|)将变量设置多种类型，赋值时可以根据设置的类型来赋值
```
var val:string | number;
val = 12;
console.log(val)    //12
val = "Test"
console.log(val)    //Test

val = true   //报错
```

联合类型可以作为函数参数使用
```
function disp(name:string | string[])
{
	if(typeof name == "string")
	{
		console.log(name)
	}
	else 
	{
		var i;
		for(i = 0;i < name.length;i++)
		{
			console.log(name[i])
		}
	}
}

disp("testName1")
disp(["1","2","4"])
输出
testName1
1
2
4
```

### 联合类型数组
```
	var arr:number[] | string[];
	var i:number;
	arr = [1,2,4]
	console.log("数字数组")
	for(i = 0;i < arr.length;i++)
	{
		console.log(arr[i])	
	}
	arr = ["Test1","test2","test4"]
	
	for(i = 0;i < arr.length;i++)
	{
		console.log(arr[i])	
	}
输出
数字数组
1
2
4
Test1
test2
test4
```