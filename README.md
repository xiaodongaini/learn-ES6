# learn-ES6

## babel 转换器 ##

Babel是一个广泛使用的 ES6 转码器，可以将 ES6 代码转为 ES5 代码，从而在现有环境执行。这意味着，你可以用 ES6 的方式编写程序，又不用担心现有环境是否支持。下面是一个例子。

转码前

```
input.map(item => item + 1);
```

转码后

```
input.map(function (item) {
  return item + 1;
});
```

### 配置文件 .babelrc ###

Babel的配置文件是.babelrc，存放在项目的根目录下。使用Babel的第一步，就是配置这个文件。用来设置转换规则和插件，基本格式如下：

```
{
  "presets": [],
  "plugins": []
}
```

presets字段设定转码规则，官方提供以下的规则集，你可以根据需要安装。

```
{
  "presets": [
    "es2015",
    "react",
    "stage-3"
  ],
  "plugins": []
}
```

presets字段设定转码规则，官方提供以下的规则集，你可以根据需要安装。

```
# ES2015转码规则
$ npm install --save-dev babel-preset-es2015

# react转码规则
$ npm install --save-dev babel-preset-react

# ES7不同阶段语法提案的转码规则（共有4个阶段），选装一个
$ npm install --save-dev babel-preset-stage-0
$ npm install --save-dev babel-preset-stage-1
$ npm install --save-dev babel-preset-stage-2
$ npm install --save-dev babel-preset-stage-3
```

然后，将这些规则加入.babelrc。

```
{
    "presets": [
      "es2015",
      "react",
      "stage-2"
    ],
    "plugins": []
  }
```

### babel-preset-stage-* 差异: ###

preset stage-0 其实包含了 stage-[1-3] ，也就是说你装了 0 ，其他三个默认就装了，别重复安装，因为它包含stage-1, stage-2以及stage-3的所有功能。另外，[0-3] 是代表了不同阶段，但， 0 是代表刚开始讨论，而 3 是代表快要成为标准规范了，切忌搞混。在进行实际开发时，可以更具需要来设置对应的stage。如果省事懒得折腾，一般设置为stage-0即可。如果为了防止开发人员使用某些太新的功能，我们可以限制到某个特定的stage。

### 更详细、更专业一点的介绍： ###
#### stage3包含以下两个插件： ####

- transform-async-to-generator
- transform-exponentiation-operator

#### stage2包含stage3的所有插件，额外还有以下插件： ####

- syntax-trailing-function-commas
- transform-object-reset-spread

#### stage1包含stage2所有插件，额外还有以下插件： ####

- transform-class-constructor-call (Deprecated)
- transform-class-properties
- transform-decorators – disabled pending proposal update
- transform-export-extensions

#### stage0包含stage1所有插件，额外还有以下插件： ####

- transform-do-expressions
- transform-function-bind

### babel转换工具 ###

#### babel-cli ####

命令行转码工具

#### babel-register ####

babel-register模块改写require命令，为它加上一个钩子。此后，每当使用require加载.js、.jsx、.es和.es6后缀名的文件，就会先用Babel进行转码。
使用时，必须首先加载babel-register。

```
require("babel-register");
require("./index.js");
```

然后，就不需要手动对index.js转码了。

需要注意的是，babel-register只会对require命令加载的文件转码，而不会对当前文件转码。另外，由于它是实时转码，所以只适合在开发环境使用。

#### bebel-core ####

如果某些代码需要调用Babel的API进行转码，就要使用babel-core模块。

```
var babel = require('babel-core');

// 字符串转码
babel.transform('code();', options);
// => { code, map, ast }

// 文件转码（异步）
babel.transformFile('filename.js', options, function(err, result) {
  result; // => { code, map, ast }
});

// 文件转码（同步）
babel.transformFileSync('filename.js', options);
// => { code, map, ast }

// Babel AST转码
babel.transformFromAst(ast, code, options);
// => { code, map, ast }
```

#### babel-polyfill ####

Babel默认只转换新的JavaScript句法（syntax），而不转换新的API，比如Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise等全局对象，以及一些定义在全局对象上的方法（比如Object.assign）都不会转码。

举例来说，ES6在Array对象上新增了Array.from方法。Babel就不会转码这个方法。如果想让这个方法运行，必须使用babel-polyfill，为当前环境提供一个垫片。

然后，在脚本头部，加入如下一行代码。

```
import 'babel-polyfill';
// 或者
require('babel-polyfill');
```

### let和const ###
ES6新增了let命令，用来声明变量。它的用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效。

#### let声明的变量不存在变量提升， ####
```
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;

// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
```

上面代码中，变量foo用var命令声明，会发生变量提升，即脚本开始运行时，变量foo已经存在了，但是没有值，所以会输出undefined。变量bar用let命令声明，不会发生变量提升。这表示在声明它之前，变量bar是不存在的，这时如果用到它，就会抛出一个错误。
#### 暂时性死区 ####
总之，在代码块内，使用let命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。
ES6明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。
#### let不允许在相同作用域内，重复声明同一个变量。 ####
```
// 报错
function () {
  let a = 10;
  var a = 1;
}

// 报错
function () {
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

#### 块级作用域 ####
ES5只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景。
第一种场景，内层变量可能会覆盖外层变量。
```
var tmp = new Date();

function f() {
  console.log(tmp);
  if (false) {
    var tmp = "hello world";
  }
}

f(); // undefined
```
上面代码中，函数f执行后，输出结果为undefined，原因在于变量提升，导致内层的tmp变量覆盖了外层的tmp变量。

第二种场景，用来计数的循环变量泄露为全局变量。
```
var s = 'hello';

for (var i = 0; i < s.length; i++) {
  console.log(s[i]);
}

console.log(i); // 5
```
上面代码中，变量i只用来控制循环，但是循环结束后，它并没有消失，泄露成了全局变量。
#### const ####
const声明一个只读的常量。一旦声明，常量的值就不能改变。
const声明的变量不得改变值，这意味着，const一旦声明变量，就必须立即初始化，不能留到以后赋值。
const的作用域与let命令相同：只在声明所在的块级作用域内有效。
const声明的常量，也与let一样不可重复声明。
### 变量的解构赋值 ###
需要注意的地方比较多