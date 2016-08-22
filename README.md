```javascript
{
  let a = 1;
  var b = 2;
}
console.log(a) // ReferenceError: a is not defined.
console.log(b) // 1

for (let i = 0; i < arr.length; i++) {}
console.log(i); //ReferenceError: i is not defined

var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6

console.log(foo); // 输出undefined
console.log(bar); // 报错ReferenceError
var foo = 2;
let bar = 2;
```

```javascript
var tmp = 123;
if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}

typeof x; // ReferenceError
let x;
```

```javascript
function () {// 报错
  let a = 10;
  var a = 1;
}
function () {// 报错
  let a = 10;
  let a = 1;
}

function func(arg) {
  let arg; // 报错
}
function func(arg) {
  {
    let arg; // 不报错
  }
}
```

```javascript
//ES5规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明。
'use strict'; // ES5严格模式
if (true) {
  function f() {}
}
// 报错

'use strict'; // ES6严格模式
if (true) {
  function f() {}
}

// ES5的浏览器环境
function f() { console.log('I am outside!'); }
(function () {
  if (false) {
    function f() { console.log('I am inside!'); }
  }
  f();//I am inside!
}());
// ES6的浏览器环境
function f() { console.log('I am outside!'); }
(function () {
  if (false) {
    // 重复声明一次函数f
    function f() { console.log('I am inside!'); }
  }
  f();// Uncaught TypeError: f is not a function
}());
```

```javascript
//const声明一个只读的常量。一旦声明，常量的值就不能改变。
const PI = 3.1415;
PI // 3.1415
PI = 3;
// TypeError: Assignment to constant variable.

const foo = {};
foo.prop = 123;
foo.prop;// 123
foo = {}; // TypeError: "foo" is read-only
```

```javascript
var [a, b, c] = [1, 2, 3];

let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []

[x, y = 'b'] = ['a']; // x='a', y='b'
[x, y = 'b'] = ['a', undefined]; // x='a', y='b'

//注意，ES6内部使用严格相等运算符（===），判断一个位置是否有值。所以，如果一个数组成员不严格等于undefined，默认值是不会生效的。
var [x = 1] = [undefined];
x // 1
var [x = 1] = [null];
x // null

//如果默认值是一个表达式，那么这个表达式是惰性求值的，即只有在用到的时候，才会求值。
function f() {
  console.log('aaa');
}
let [x = f()] = [1];

let [x = 1, y = x] = [];     // x=1; y=1
let [x = 1, y = x] = [2];    // x=2; y=2
let [x = 1, y = x] = [1, 2]; // x=1; y=2
let [x = y, y = 1] = [];     // ReferenceError

var { foo, bar } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"
baz // undefined

var { foo: baz } = { foo: 'aaa', bar: 'bbb' };
baz // "aaa"

//对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。真正被赋值的是后者，而不是前者。
var { foo: foo, bar: bar } = { foo: "aaa", bar: "bbb" };
foo //'aaa'
bar //'bbb'

var { foo: baz } = { foo: "aaa", bar: "bbb" };
baz // "aaa"
foo // error: foo is not defined

({ foo: obj.prop, bar: arr[0] } = { foo: 123, bar: true });
obj // {prop:123}
arr // [true]
foo // error: foo is undefined

var {x = 3} = {};
x // 3
var {x, y = 5} = {x: 1};
x // 1
y // 5
var {x:y = 3} = {};
y // 3
var {x:y = 3} = {x: 5};
y // 5
var { message: msg = 'Something went wrong' } = {};
msg // "Something went wrong"

var {foo: {bar}} = {baz: 'baz'};// 报错

// 错误的写法
var x;
{x} = {x: 1};
// SyntaxError: syntax error
// 正确的写法
({x} = {x: 1});

let { log, sin, cos } = Math;

var arr = [1, 2, 3];
var {0 : first, [arr.length - 1] : last} = arr;
first // 1
last // 3

const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"

let {length : len} = 'hello';
len // 5

//解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。
let {toString: s} = 123;
s === Number.prototype.toString // true
let {toString: s} = true;
s === Boolean.prototype.toString // true

//解构赋值的规则是，只要等号右边的值不是对象，就先将其转为对象。由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错。
let { prop: x } = undefined; // TypeError
let { prop: y } = null; // TypeError

function add([x, y]){
  return x + y;
}
add([1, 2]); // 3

function move({x = 0, y = 0} = {}) {
  return [x, y];
}
move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, 0]
move({}); // [0, 0]
move(); // [0, 0]

function move({x, y} = { x: 0, y: 0 }) {
  return [x, y];
}
move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, undefined]
move({}); // [undefined, undefined]
move(); // [0, 0]

[[1, 2], [3, 4]].map(([a, b]) => a + b);
// [ 3, 7 ]
```

```javascript
'\z' === 'z'  // true
'\172' === 'z' // true
'\x7A' === 'z' // true
'\u007A' === 'z' // true
'\u{7A}' === 'z' // true

var s = "𠮷";
s.length // 2
s.charAt(0) // ''
s.charAt(1) // ''
s.charCodeAt(0) // 55362
s.charCodeAt(1) // 57271

var s = '𠮷a';
s.codePointAt(0) // 134071
s.codePointAt(1) // 57271
s.charCodeAt(2) // 97

String.fromCharCode(0x20BB7)
// "ஷ"
String.fromCodePoint(0x20BB7)
// "𠮷"

var text = String.fromCodePoint(0x20BB7);
for (let i = 0; i < text.length; i++) {
  console.log(text[i]);
}
// " "
// " "
for (let i of text) {
  console.log(i);
}
// "𠮷"

'abc'.charAt(0) // "a"
'𠮷'.charAt(0) // "\uD842"
'abc'.at(0) // "a"
'𠮷'.at(0) // "𠮷"

var s = 'Hello world!';
s.startsWith('Hello') // true
s.endsWith('!') // true
s.includes('o') // true
s.startsWith('world', 6) // true
s.endsWith('Hello', 5) // true
s.includes('Hello', 6) // false

'x'.repeat(3) // "xxx"
'hello'.repeat(2) // "hellohello"
'na'.repeat(0) // ""
'na'.repeat(2.9) // "nana"
'na'.repeat(Infinity)// RangeError
'na'.repeat(-1)// RangeError
'na'.repeat(-0.9) // ""
'na'.repeat(NaN) // ""
'na'.repeat('na') // ""
'na'.repeat('3') // "nanana"

'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4, 'ab') // 'abax'
'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'
'xxx'.padStart(2, 'ab') // 'xxx'
'xxx'.padEnd(2, 'ab') // 'xxx'
'abc'.padStart(10, '0123456789')// '0123456abc'
'x'.padStart(4) // '   x'
'x'.padEnd(4) // 'x   '

```

```javascript
$('#result').append(`
  There are <b>${basket.count}</b> items
   in your basket, <em>${basket.onSale}</em>
  are on sale!
`);

var greeting = `\`Yo\` World!`;


```

```javascript

```

```javascript

```

```javascript

```

```javascript

```

```javascript

```
