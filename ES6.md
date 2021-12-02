# 1.let
##  1.不能重复声明
```
let a = 'apple';
let a = 'pear';
```
## 2.块级作用域
就是{出现在这里}，或者for if while...等里面能行，出了{}就不行了

## 3.不存在变量提升
符合编程思维的

## 不影响作用域链
```
let school = '五中';
function fn(){
  console.log(school);
}
fn();
```
# 解构赋值
ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构。能解构数组和对象
`let [a, b, c] = [1, 2, 3];`
上面代码表示，可以从数组中提取值，按照对应位置，对变量赋值。（一一对应）
 - 定义数组 const num = [1, 2, 3];
 - 声明模板 let [a, b, c] = num;
 - 访问元素 a //1  b //2  c //3

本质上，这种写法属于“模式匹配”，只要等号两边的模式相同，左边的变量就会被赋予对应的值。这种写法能减去重复的代码。例如调用对象属性使用，zhao.a,利用解构赋值就能直接a访问。
下面有些例子，理解解构赋值  
```
let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1
bar // 2
baz // 3

let [ , , third] = ["foo", "bar", "baz"];
third // "baz"

let [x, , y] = [1, 2, 3];
x // 1
y // 3

let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []
```
# 模板字符串
模板字符串是增强版的字符串，用反引号（`）标识。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。

```
// 普通字符串
`In JavaScript '\n' is a line-feed.`
// 多行字符串
`In JavaScript this is
 not legal.`

console.log(`string text line 1
string text line 2`);

// 字符串中嵌入变量
let name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`

//如果使用模板字符串表示多行字符串，所有的空格和缩进都会被保留在输出之中。
$('#list').html(`
<ul>
  <li>first</li>
  <li>second</li>
</ul>
`);
```


## 扩展
### 能使用Unicode  
ES6 加强了对 Unicode 的支持，允许采用\uxxxx形式表示一个字符，其中xxxx表示字符的 Unicode 码点。
```
"\u0061"
// "a"
```
但是，这种表示法只限于码点在\u0000~\uFFFF之间的字符。  
### 改进  
只要将码点放入大括号，就能正确解读该字符。

"\u{20BB7}"
// "𠮷"

"\u{41}\u{42}\u{43}"
// "ABC"

let hello = 123;
hell\u{6F} // 123

# 函数
## rest 参数
ES6 引入 rest 参数（形式为...变量名），用于获取函数的多余参数，这样就不需要使用arguments对象了。rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中。

function add(...values) {
  let sum = 0;

  for (var val of values) {
    sum += val;
  }
  return sum;
}

add(2, 5, 3) // 10
上面代码的add函数是一个求和函数，利用 rest 参数，可以向该函数传入任意数目的参数。

下面是一个 rest 参数代替arguments变量的例子。

// arguments变量的写法
function sortNumbers() {
  return Array.from(arguments).sort();
}

// rest参数的写法
const sortNumbers = (...numbers) => numbers.sort();
上面代码的两种写法，比较后可以发现，rest 参数的写法更自然也更简洁。


## 箭头函数
如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。
```
//没有参数
var f = () => 5;
// 等同于
var f = function () { return 5 };

//一个参数(括号能省略)
var f = a => a + 5;
// 等同于
var f = function (a) { return a + 5 };

//两个参数
var sum = (num1, num2) => num1 + num2;
// 等同于
var sum = function(num1, num2) {
  return num1 + num2;
};

//如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用return语句返回。
var sum = (num1, num2) => {
  let result = num1 + num2;
  console.log(result);
  return result;
}

//由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号，否则会报错。

// 报错
let getTempItem = id => { id: id, name: "Temp" };

// 不报错
let getTempItem = id => ({ id: id, name: "Temp" });

//箭头函数可以与变量解构结合使用。并指定默认值
const full = ({ first = 'morenzhi', last }) => first + ' ' + last;

// 等同于
function full(person) {
  return person.first + ' ' + person.last;
}


```
### 注意
>（1）它没有自己的this对象，内部的this就是定义时上层作用域中的this.是静态固定的,不能通过call改变

>（2）不可以当作构造函数，也就是说，不可以对箭头函数使用new命令，否则会抛出一个错误。

>（3）不可以使用arguments对象，该对象在函数体内不存>在。如果要用，可以用 rest 参数代替。

>（4）不可以使用yield命令，因此箭头函数不能用作 Generator 函数。

>（5）箭头函数不适合与this有关的回调,例如事件回调,对象的方法.
     箭头函数适合与this无关的回调,例如定时器,数组方法的回调

最重要的是第一点。对于普通函数来说，内部的this指向函数运行时所在的对象，但是这一点对箭头函数不成立。它没有自己的this对象，内部的this就是定义时上层作用域中的this。也就是说，箭头函数内部的this指向是固定的，相比之下，普通函数的this指向是可变的。

