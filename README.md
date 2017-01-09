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



