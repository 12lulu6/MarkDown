


#正则表达式
###1.解释
它的设计思想是用一种描述性的语言来给字符串定义一个规则，凡是符合规则的字符串，我们就认为它“匹配”了，否则，该字符串就是不合法的。
###2.语法
```
\d	查找数字。
\w  可以匹配一个字母或数字	
\s	查找空白字符。	
\b	匹配单词边界。	
*   表示任意个字符（包括0个），
+   表示至少一个字符， 
^   表示行的开头，^\d表示必须以数字开头。
$   表示行的结束，\d$表示必须以数字结束。
?   表示0个或1个字符， 
{n} 表示n个字符，
{n,m} 表示n-m个字符
[]    表示范围
[abc]	查找方括号之间的任何字符。
[0-9]	查找任何从 0 至 9 的数字。
(x|y)	查找由 | 分隔的任何选项。

i 是修饰符（把搜索修改为大小写不敏感）。写在后面
g	执行全局匹配（查找所有匹配而非在找到第一个匹配后停止）。
m	执行多行匹配。
\uxxxx	查找以十六进制数 xxxx 规定的 Unicode 字符。
n+	匹配任何包含至少一个 n 的字符串。	
n*	匹配任何包含零个或多个 n 的字符串。	
n?	匹配任何包含零个或一个 n 的字符串。
```
#####2.2举例
 \d{3}\s+\d{3,8}:三个数字 空格 3-8个数字
###3.使用正则表达式
####3.1创建正则表达式
---直接通过正则表达式写出来  `var re1 = /ABC\-001/;`  
---通过new RegExp('正则表达式')创建一个RegExp对象。`var re2 = new RegExp('ABC\\-001');`
      因为字符串的转义问题，字符串的两个\\实际上是一个\。
####3.2判断是否匹配

```
var re = /^\d{3}\-\d{3,8}$/;
    re.test('010-12345'); // true
    re.test('010-1234x'); // false
    re.test('010 12345'); // false
```
 RegExp对象的test()方法用于测试给定的字符串是否符合条件。
####3.3分组
()  表示的就是要提取的分组（Group）
`^(\d{3})-(\d{3,8})$`
分别定义了两个组，可以直接从匹配的字符串中提取出区号和本地号码：
```
var re = /^(\d{3})-(\d{3,8})$/;
re.exec('010-12345'); // ['010-12345', '010', '12345']
re.exec('010 12345'); // null
```
如果正则表达式中定义了组，就可以在RegExp对象上用exec()方法提取出子串来。
exec()方法在匹配成功后，会返回一个Array，第一个元素是正则表达式匹配到的整个字符串，后面的字符串表示匹配成功的子串。
exec()方法在匹配失败时返回null。

#字符串
####1.''和""的混合使用
`'I\'m \"OK\"!';`
表示的字符串内容是：I'm "OK"!
转义字符\可以转义很多字符，比如\n表示换行，\t表示制表符，字符\本身也要转义，所以\\表示的字符就是\。
####2.多行字符串
由于多行字符串用\n写起来比较费事，所以最新的ES6标准新增了一种多行字符串的表示方法，用反引号` `表示：
    `这是一个 (换行)
    多行(换行)
    字符串`;
####3.连接字符串
ES6新增了一种模板字符串，表示方法和上面的多行字符串一样，但是它会自动替换字符串中的变量：
字符串
1.''和""的混合使用
'I\'m \"OK\"!';
表示的字符串内容是：I'm "OK"!
转义字符\可以转义很多字符，比如\n表示换行，\t表示制表符，字符\本身也要转义，所以\\表示的字符就是\。
2。多行字符串
由于多行字符串用\n写起来比较费事，所以最新的ES6标准新增了一种多行字符串的表示方法，用反引号` `表示：
    `这是一个
    多行
    字符串`;
3.连接字符串
ES6新增了一种模板字符串，表示方法和上面的多行字符串一样，但是它会自动替换字符串中的变量：
```
    var name = '小明';
    var age = 20;
    var message = `你好, ${name}, 你今年${age}岁了!`;
    alert(message);
```
####4.操作字符串
```
var s = 'Hello, world!';
--s.length; // 13
--s.toUpperCase(); // 返回'HELLO WORLD'
--s.toLowerCase(); //返回小写

--s.substring(0, 5); // 从索引0开始到5（不包括5），返回'hello'
  s.substring(7); // 从索引7开始到结束，返回'world'

//charAt() 方法从一个字符串中返回指定的字符。
s.charAt(0)  //B

//indexOf(searchValue [, fromIndex]) 方法返回调用它的String 对象中第一次出现的指定值的索引，
//从 `fromIndex` 处进行搜索。如果未找到该值 and fromIndex 的值大于或等于 str.length，则返回 -1。
const searchTerm = 'new';      //这里的s是hello new world
console.log(s.indexOf(searchTerm)) //6
console.log(s.indexOf(searchTerm,5)) //0 从索引5开始找

//concat() 方法将一个或多个字符串与原字符串连接合并，形成一个新的字符串并返回。
let hello = 'Hello, '
console.log(hello.concat('Kevin', '. Have a nice day.'))
// Hello, Kevin. Have a nice day.

//slice() 方法提取某个字符串的一部分，并返回一个新的字符串，且不会改动原字符串。
//str.slice(beginIndex[, endIndex])
console.log(s.slice(5, 8));     //new

// match() 方法检索返回一个字符串匹配正则表达式的结果。
const regex = /[A-Z]/g;
let found = s.match(regex);
console.log(found); //H 找大写  found是一个数组
   ```
```
 var name = '小明';
    var age = 20;
    var message = `你好, ${name}, 你今年${age}岁了!`;
    alert(message);
```



# 数组
#### 1.创建数组
```
var arr = [1, 2, 3.14, 'Hello', null, true];
new Array(1,3,4)
```
#### 2.操作数组
```
var arr = [10, 20, '30', 'xyz'];
--arr.indexOf(10); // 元素10的索引为0
  arr.indexOf(30); // 元素30没有找到，返回-1

--arr.slice(0, 3); // 从索引0开始，到索引3结束，但不包括索引3: [1, 2, 3.14]
  arr.slice(3); // 从索引3开始到结束: ['Hello', null, true]

  如果不给slice()传递任何参数，它就会从头到尾截取所有元素

--arr.push('A', 'B'); // 返回Array新的长度: 8
  arr; // [1, 2, 3.14, 'Hello', null, true, 'A', 'B']

--arr.pop(); // pop()返回'B'
  arr; // [1, 2, 3.14, 'Hello', null, true, 'A']
  arr.pop(); arr.pop(); arr.pop(); // 连续pop 7次
  arr; // []
  arr.pop(); // 空数组继续pop不会报错，而是返回undefined

unshift和shift
--同上
  如果要往Array的头部添加若干元素，使用unshift()方法，shift()方法则把Array的第一个元素删掉

  var arr = ['B', 'C', 'A'];
--arr.sort();
  arr; // ['A', 'B', 'C']
 arr1.sort(function(a , b){
        return a - b;//这里这个是升序排序；a + b为降序排序 
    })
 //sort()可以对当前Array进行排序，它会直接修改当前Array的元素位置，直接调用时，按照默认顺序排序

  var arr = ['one', 'two', 'three'];
--arr.reverse(); 
  arr; // ['three', 'two', 'one']
  reverse()把整个Array的元素给调个个，也就是反转：

//splice()方法是修改Array的“万能方法”，它可以从指定的索引开始删除若干元素，然后再从该位置添加若干元素：
var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
// 从索引2开始删除3个元素,然后再添加两个元素:
arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', 'Excite']
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
// 只删除,不添加:
arr.splice(2, 2); // ['Google', 'Facebook']
arr; // ['Microsoft', 'Apple', 'Oracle']
// 只添加,不删除:
arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素
arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']

 // concat()方法把当前的Array和另一个Array连接起来，并返回一个新的Array：
  var arr = ['A', 'B', 'C'];
--var added = arr.concat([1, 2, 3]);
  added; // ['A', 'B', 'C', 1, 2, 3]
  arr; // ['A', 'B', 'C']

 // join()方法是一个非常实用的方法，它把当前Array的每个元素都用指定的字符串连接起来，
//然后返回连接后的字符串。如果Array的元素不是字符串，将自动转换为字符串后再连接。
  var arr = ['A', 'B', 'C', 1, 2, 3];
  arr.join('-'); // 'A-B-C-1-2-3'

//concat() 连接两个数组并返回一个新的数组。
var myArray = new Array("1", "2", "3");
myArray = myArray.concat("a", "b", "c"); // myArray is now ["1", "2", "3", "a", "b", "c"]

```

#### 3.Map
如果用Map实现，只需要一个“名字”-“成绩”的对照表，直接根据名字查找成绩，无论这个表有多大，查找速度都不会变慢。用JavaScript写一个Map如下：
```
var m = new Map([['Michael', 95], ['Bob', 75], ['Tracy', 85]]);
m.get('Michael'); // 95
```
初始化Map需要一个二维数组，或者直接初始化一个空Map。Map具有以下方法：
```
var m = new Map(); // 空Map
m.set('Adam', 67); // 添加新的key-value
m.set('Bob', 59);
m.has('Adam'); // 是否存在key 'Adam': true
m.get('Adam'); // 67
m.delete('Adam'); // 删除key 'Adam'
m.get('Adam'); // undefined
由于一个key只能对应一个value，所以，多次对一个key放入value，后面的值会把前面的值冲掉：

var m = new Map();
m.set('Adam', 67);
m.set('Adam', 88);
m.get('Adam'); // 88
```
### Object和Map的比较
一般地，[objects](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object)会被用于将字符串类型映射到数值。Object允许设置键值对、根据键获取值、删除键、检测某个键是否存在。而Map具有更多的优势。

*   的键均为Strings类型，在Map里键可以是任意类型。
*   必须手动计算Object的尺寸，但是可以很容易地获取使Map的尺寸。
*   Map的遍历遵循元素的插入顺序。

#### 4.Set
Set和Map类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在Set中，没有重复的key。要创建一个Set，需要提供一个Array作为输入，或者直接创建一个空Set：
```
    var s1 = new Set(); // 空Set
    var s2 = new Set([1, 2, 3]); // 含1, 2, 3
   //重复元素在Set中自动被过滤：
   var s = new Set([1, 2, 3, 3, '3']);
   s; // Set {1, 2, 3, "3"}  注意数字3和字符串'3'是不同的元素。

//通过add(key)方法可以添加元素到Set中，可以重复添加，但不会有效果：
s.add(4);
s; // Set {1, 2, 3, 4}
s.add(4);
s; // 仍然是 Set {1, 2, 3, 4}
s.size; // 4
通过delete(key)方法可以删除元素：

var s = new Set([1, 2, 3]);
s; // Set {1, 2, 3}
s.delete(3);
s; // Set {1, 2}
```
#### 5.filter
filter() 方法创建一个新数组, 其包含通过所提供函数实现的测试的所有元素。
```
//var newArray = arr.filter(callback(element[, index[, array]])[, thisArg])
//在这个回调函数中能传入参数：
//element:  数组中当前正在处理的元素。
//index可选:  正在处理的元素在数组中的索引。
//array可选:  调用了 filter 的数组本身。
function isBigEnough(element) {
  return element >= 10;
}
var filtered = [12, 5, 8, 130, 44].filter(isBigEnough);
// filtered is [12, 130, 44]
```
####6.reduce
reduce() 方法对数组中的每个元素执行一个由您提供的reducer函数(升序执行)，将其结果汇总为单个返回值。
```
//arr.reduce(callback(accumulator, currentValue[, index[, array]])[, initialValue])
//accumulator 累计器
//currentValue 当前值
//currentIndex 当前索引
//array 数组
[0, 1, 2, 3, 4].reduce(function(accumulator, currentValue, currentIndex, array){
  return accumulator + currentValue;
});
//将上一次得到的结果给accumulator,相当于迭代 累加器 accumulator相当于上一个值

//写为箭头函数
[0, 1, 2, 3, 4].reduce((prev, curr) => prev + curr );

//给accumulator一个初始值10
[0, 1, 2, 3, 4].reduce((accumulator, currentValue, currentIndex, array) => {
    return accumulator + currentValue
}, 10)
```
#### 7.遍历数组
--使用for...in..
--使用for...of..都能遍历Array Map Set
```   
 var a = ['A', 'B', 'C'];
    var s = new Set(['A', 'B', 'C']);
    var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
    for (var x of a) { // 遍历Array
        console.log(x);
    }
    for (var x of s) { // 遍历Set
        console.log(x);
    }
    for (var x of m) { // 遍历Map
        console.log(x[0] + '=' + x[1]);
    }
```
**for...in和for...of的区别**
for in 遍历的是对象的属性，数组也能看为一个对象，每个元素的索引看为属性
当我们手动给Array对象添加了额外的属性后，for ... in循环将带来意想不到的意外效果：
```
    var a = ['A', 'B', 'C'];
    a.name = 'Hello';
    for (var x in a) {
        console.log(x); // '0', '1', '2', 'name'
    }
```
for ... in循环将把name包括在内，但Array的length属性却不包括在内。
for ... of循环则完全修复了这些问题，它只循环集合本身的元素：
```
    var a = ['A', 'B', 'C'];
    a.name = 'Hello';
    for (var x of a) {
        console.log(x); // 'A', 'B', 'C'
    }
```
这就是为什么要引入新的for ... of循环。

然而，更好的方式是直接使用iterable内置的forEach方法，它接收一个函数，每次迭代就自动回调该函数。以Array为例：
```
    'use strict';
    var a = ['A', 'B', 'C'];
    a.forEach(function (element, index, array) {
         //这个回调函数是每个元素都要执行的
        // element: 指向当前元素的值  
        // index: 指向当前索引
        // array: 指向Array对象本身
    console.log(element + ', index = ' + index);
    });
```
# 函数
#### 1.arguments的使用
    
就是当用户不知道要传多少个实参时,可以用arguments来获取。其实是当前函数的一个内置对象。存储了传递的额所有实参,第一个传来的参数会是arguments[0],arguments.length来获取实际传递给函数的参数长度
```
function myConcat(separator) {
   var result = ''; // 把值初始化成一个字符串，这样就可以用来保存字符串了！！
   var i;
   // iterate through arguments
   for (i = 1; i < arguments.length; i++) {
      result += arguments[i] + separator;
   }
   return result;
}
myConcat(", ", "red", "orange", "blue");// returns "red, orange, blue, "
```

####2.变量提升
我们JS引擎运行JS分为两步： **预解析   代码执行**
    --预解析  js引擎会把JS里面的所有var 还有function 替身到档期那作用域的最前面
      *变量预解析(变量提升) 和函数预解析(函数提升)
        ---变量提升就是把所有的变量声明提升到档期那的作用域最前面 不提升赋值操作
        例如：
```
fun();
              var fun = function(){
                  console.log(11);
              }
              通过变量提升变为以下代码
              var fun;
              fun();
              fun = function(){
                  console.log(11);
              }
```
        ---函数提升就是把所有的函数声明提升到当前作用域的最前面  不调用函数
        例如：
```
fun();
            function fun(){
                console.log(22);
            }
            通过函数提升变为以下代码
             function fun(){
                console.log(22);
            }
            fun();
    而采用匿名函数不能使用这种提升 
    代码执行  按照代码书写的顺序从上往下执行
```
#### 3.解构赋值
从ES6开始，JavaScript引入了解构赋值，可以同时对一组变量进行赋值。
    `var [x, y, z] = ['hello', 'JavaScript', 'ES6'];`
    // x, y, z分别被赋值为数组对应元素:
`    console.log('x = ' + x + ', y = ' + y + ', z = ' + z);`

注意，对数组元素进行解构赋值时，多个变量要用[...]括起来。如果数组本身还有嵌套，也可以通过下面的形式进行解构赋值，注意嵌套层次和位置要保持一致：
```
    let [x, [y, z]] = ['hello', ['JavaScript', 'ES6']];
    x; // 'hello'
    y; // 'JavaScript'
    z; // 'ES6'
```
解构赋值还可以忽略某些元素：
`let [, , z] = ['hello', 'JavaScript', 'ES6']; // 忽略前两个元素，只对z赋值第三个元素
z; // 'ES6'`
一般情况var创建的变量可以为全局或者函数内部的变量域，但是没有块级变量域，如for,while等,因此引入了let在for等地方创建块级变量

# 对象
#### 1.属性访问
`objectName.property           // person.age`
`objectName["property"]       // person["age"]`

访问属性是通过.操作符完成的，但这要求属性名必须是一个有效的变量名。如果属性名包含特殊字符，就必须用''括起来：
 ```
           var xiaohong = {
                name: '小红',
                'middle-school': 'No.1 Middle School'
            };
```
xiaohong的属性名middle-school不是一个有效的变量(有了-)，就需要用''括起来。访问这个属性也无法使用.操作符，必须用['xxx']来访问：
`xiaohong['middle-school']; // 'No.1 Middle School'`
#### 2.判断该对象是否有该属性
-使用in,如果in判断一个属性存在，这个属性不一定是xiaoming的，它可能是xiaoming继承得到的。
    `console.log('toString' in xiaohong)  //true`
    因为toString定义在object对象中，而所有对象最终都会在原型链上指向object，所以xiaoming也拥有toString属性。

-要判断一个属性是否是xiaoming自身拥有的，而不是继承得到的，可以用hasOwnProperty()方法
    `xiaohong.hasOwnProperty('toString'); // false`
`    xiaohong.hasOwnProperty(name); // true`


for...in 循环中的代码块会为每个属性执行一次。它可以把一个对象的所有属性依次循环出来
```
for (var key in xiaohong) {
    console.log(key); // 'name', 'middle-school'
    console.log(xiaohong[key]); // '小红', 'xxxschool'
}
```
#### 3.使用构造函数
```
    function Star(uname , age , sex){
        this.name = uname;
        this.age = age;
        this.sex = sex; 
        this.sing = function(sang){
            console.log(sang);
        }
    }
    // 构造函数返回的是对象
    var ldh = new Star('刘德华', 18 , '男');
    console.log(ldh.sing);//不使用 () 访问函数将返回函数声明而不是函数结果
```
#### 4.使用create创建对象
```
// Animal properties and method encapsulation
var Animal = {
  type: "Invertebrates", // 属性默认值
  displayType : function() {  // 用于显示type属性的方法
    console.log(this.type);
  }
}

// 创建一种新的动物——animal1
var animal1 = Object.create(Animal);
animal1.displayType(); // Output:Invertebrates

// 创建一种新的动物——Fishes
var fish = Object.create(Animal);
fish.type = "Fishes";
fish.displayType(); // Output:Fishes
```
#### 5.操作对象属性
```
var person = {
  firstName: "Bill",
  lastName : "Gates",
  language : "EN" 
};
```
// 更改属性：
`Object.defineProperty(person, "language", {value:"ZH"})`
// 添加属性：
`Object.defineProperty(person, "year", {value:"2019"})`
####对象模型
- JavaScript 是一种基于原型而不是基于类的基于对象(object-based)语言
- 所有对象均为实例。
- 通过构造器函数来定义和创建一组对象。
- 通过 new 操作符创建单个对象。
- 指定一个对象作为原型并且与构造函数一起构建对象的层级结构
- 遵循原型链继承属性
- 构造器函数或原型指定实例的初始属性集。允许动态地向单个的对象或者整个对象集中添加或移除属性。
下面是实例
![employee.png](https://upload-images.jianshu.io/upload_images/27278342-f203b96b3d57a1de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
function Employee () {
  this.name = "";
  this.dept = "general";
}
function Manager() {
  Employee.call(this);
  this.reports = [];
}
Manager.prototype = Object.create(Employee.prototype);

function WorkerBee() {
  Employee.call(this);
  this.projects = [];
}
WorkerBee.prototype = Object.create(Employee.prototype);
function SalesPerson() {
   WorkerBee.call(this);
   this.dept = 'sales';
   this.quota = 100;
}
SalesPerson.prototype = Object.create(WorkerBee.prototype);

function Engineer() {
   WorkerBee.call(this);
   this.dept = 'engineering';
   this.machine = '';
}
Engineer.prototype = Object.create(WorkerBee.prototype);
```

#DOM
##操作DOM
```
// 返回ID为'test'的节点：
var test = document.getElementById('test');

// 先定位ID为'test-table'的节点，再返回其内部所有tr节点：
var trs = document.getElementById('test-table').getElementsByTagName('tr');

// 先定位ID为'test-div'的节点，再返回其内部所有class包含red的节点：
var reds = document.getElementById('test-div').getElementsByClassName('red');

// 获取节点test下的所有直属子节点:
var cs = test.children;

// 获取节点test下第一个、最后一个子节点：
var first = test.firstElementChild;
var last = test.lastElementChild;
第二种方法是使用querySelector()和querySelectorAll()，需要了解selector语法，然后使用条件来获取节点，更加方便：

// 通过querySelector获取ID为q1的节点：
var q1 = document.querySelector('#q1');

// 通过querySelectorAll获取q1节点内的符合条件的所有节点：
var ps = q1.querySelectorAll('div.highlighted > p');
```
##更新DOM
- innerHTML:不仅能修改文本还能识别html标签
```
p.innerHTML = ' <span style="color:red">RED</span> XYZ';
//<p> <span style="color:red">RED</span> XYZ</p>
```
- innerText:只能修改文本
```
p.innerHTML = ' <strong>RED</strong> XYZ';
//<p> <strong>RED</strong>  XYZ</p>   这里面的strong 不能被识别
```

#其他
语句块通常用于流程控制，如if，for，while等等。所以在里面定义let和var是不一样的
```
var x = 1;
{
  var x = 2;
}
alert(x); // 输出的结果为 2
let y = 1;
{
  let y = 2;
}
alert(y); // 输出的结果为 undifined
```
##Promise
允许你对延时和异步操作流进行控制。
Promise 对象有以下几种状态：
- pending：初始状态，既没有被兑现，也没有被拒绝。
- fulfilled：意味着操作成功完成。
- rejected：失败，没有完成操作。
- settled：Promise 处于 fulfilled 或 rejected 二者中的任意一个状态, 不会是 pending。

promise.then()，promise.catch() 和 promise.finally() 这些方法将进一步的操作与一个变为已敲定状态的 promise 关联起来。这些方法还会返回一个新生成的 promise 对象，这个对象可以被非强制性的用来做链式调用.
```
 // 需求： 第一次aaa   第一次请求的操作
  //       第二次aaa111   第二次请求的操作
  //        第三次aaa111222   第三次请求的操作  就只是对上一次结果拼接加一些操作
  new Promise(resolve=>{
    setTimeout(()=>{
      resolve('aaa')
    },1000)
  }).then(res=>{
    console.log("我是第一次请求的操作");

    return new Promise(resolve=>{
      resolve(res+'111')
    })
  }).then(res=>{
    console.log("我是第二次请求的操作");
    return new Promise(resolve=>{
      resolve(res+'222')
    })
  }).then(res=>{
    console.log("我是第三次请求的操作");
    console.log(res);
  })


  // promise的链式操作
  new Promise(resolve=>{
    setTimeout(()=>{
      resolve('aaa')
    },1000)
  }).then(res=>{
    console.log("我是第一次请求的操作");

    return Promise.resolve(res + '111')
  }).then(res=>{
    console.log("我是第二次请求的操作");
    return Promise.resolve(res + '222')
  }).then(res=>{
    console.log("我是第三次请求的操作");
    console.log(res);
  })


//因为需求特殊，可以省略创建new Promise
  new Promise(resolve=>{
    setTimeout(()=>{
      resolve('aaa')
    },1000)
  }).then(res=>{
    console.log("我是第一次请求的操作");

    return res + '111'
  }).then(res=>{
    console.log("我是第二次请求的操作");
    return res + '222'
  }).then(res=>{
    console.log("我是第三次请求的操作");
    console.log(res);
  })
```
####使用Promise的all()
什么时候使用？
 当某些操作是需要两个请求回来的数据一起完成，这时候就可以使用promise的all
 参数是可迭代的，就是能循环的，使用数组。
 当请求1和请求2都回来的时候，才调用then
```
Promise.all([
   new Promise((resolve,reject)=>{
    $.ajax({
      url:'url1',
      success:function(data){
        resolve(data)
      }
    })
   }),
   new Promise((resolve,reject)=>{
    $.ajax({
      url:'url2',
      success:function(data){
        resolve(data)
      }
    })
   })
  ]).then(results => {
      //results[0]是请求一 
      results[0]
      results[1]
  })
```
####使用any
什么时候使用？
当其中其中有一个成功就会返回出来，不会再执行下去。
```
const pErr = new Promise((resolve, reject) => {
  reject("总是失败");
});

const pSlow = new Promise((resolve, reject) => {
  setTimeout(resolve, 500, "最终完成");
});

const pFast = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, "很快完成");
});

Promise.any([pErr, pSlow, pFast]).then((value) => {
  console.log(value);
  // pFast fulfils first
})
// 期望输出: "很快完成"
```
不止能用于使用请求
```
const promise = doSomething();
const promise2 = promise.then(successCallback, failureCallback);

const promise2 = doSomething().then(successCallback, failureCallback);

使用Promise避免了回调地狱
```
#数字
##属性
| [`Number.MAX_VALUE`] 可表示的最大值 
| [`Number.MIN_VALUE`]  可表示的最小值 
| [`Number.NaN`] 特指”非数字“ 
...
##方法
| `Number.parseFloat()`把字符串参数解析成浮点数，和全局方法 `parseFloat()`作用一致. |
| `Number.parseInt()`  把字符串解析成特定基数对应的整型数字，和全局方法 `parseInt()`作用一致.
| `Number.isNaN()` 判断传递的值是否为NaN.

##数字对象Math
`Math.PI    // π`
##方法
```
abs()  //绝对值
sin() cos() tan()  //三角函数
pow()  //指数

//Math.round返回四舍五入最近值
Math.round10(55, 1);       // 60
Math.round10(54.9, 1);     // 50
Math.round10(-55.55, -1);  // -55.5
Math.round10(-55.551, -1); // -55.6

//Math.floor() 返回小于或等于一个给定数字的最大整数。
Math.floor10(-55.51, -1);  // -55.6
Math.floor10(-51, 1);      // -60

//Math.ceil() 函数返回大于或等于一个给定数字的最小整数。
Math.ceil(-55.59, -1);   // -55.5
Math.ceil(-59, 1);       // -50

//Math.random()  一个浮点型伪随机数字，在0（包括0）和1（不包括）之间。
Math.random()    
...
```
























****