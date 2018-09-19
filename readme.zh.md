
# 喵[![Build Status](https://travis-ci.org/sindresorhus/meow.svg?branch=master)](https://travis-ci.org/sindresorhus/meow)

> CLI app helper

![](meow.gif)

## 特征

-   解析论点
-   将标志转换为[骆驼香烟盒](https://github.com/sindresorhus/camelcase)
-   使用时取消标记`--no-`字首
-   输出版本时`--version`
-   输出描述和提供的帮助文本`--help`
-   做出未经处理的被拒绝的承诺[大声失败](https://github.com/sindresorhus/loud-rejection)而不是默认的静默失败
-   将进程标题设置为package.json中定义的二进制名称

## 安装

```
$ npm install meow
```

## 用法

```
$ ./foo-app.js unicorns --rainbow
```

```js
#!/usr/bin/env node
'use strict';
const meow = require('meow');
const foo = require('.');

const cli = meow(`
	Usage
	  $ foo <input>

	Options
	  --rainbow, -r  Include a rainbow

	Examples
	  $ foo unicorns --rainbow
	  🌈 unicorns 🌈
`, {
	flags: {
		rainbow: {
			type: 'boolean',
			alias: 'r'
		}
	}
});
/*
{
	input: ['unicorns'],
	flags: {rainbow: true},
	...
}
*/

foo(cli.input[0], cli.flags);
```

## API

### 喵(选项,[minimistOptions])

返回一个`Object`有:

-   `input` *(阵列)*- 非标志参数
-   `flags` *(目的)*- 标志转换为camelCase
-   `pkg` *(目的)*-`package.json`目的
-   `help` *(串)*- 使用的帮助文本`--help`
-   `showHelp([code=2])` *(功能)*- 显示帮助文本并退出`code`
-   `showVersion()` *(功能)*- 显示版本文本并退出

#### 选项

类型:`Object` `Array` `string`

可以是一个字符串/数组`help`或选项对象.

##### 旗

类型:`Object`

定义参数标志.

键是标志名称,值是具有以下任何一个的对象:

-   `type`:值的类型.(可能的值:`string` `boolean`)
-   `alias`:通常用于定义短标志别名.
-   `default`:未指定标志时的默认值.

例:

```js
flags: {
	unicorn: {
		type: 'string',
		alias: 'u',
		default: 'rainbow'
	}
}
```

##### 描述

类型:`string` `boolean`<br>默认值:package.json`"description"`属性

说明在帮助文本上方显示.

将其设置为`false`完全禁用它.

##### 帮帮我

类型:`string` `boolean`

您想要显示的帮助文本.

输入是重新缩进的,并且开始/结束换行符被修剪,这意味着您可以使用[模板文字](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/template_strings)无需关心使用正确数量的缩进.

说明将自动显示在帮助文本上方.

##### 版

类型:`string` `boolean`<br>默认值:package.json`"version"`属性

设置自定义版本输出.

##### autoHelp

类型:`boolean`<br>默认:`true`

当时自动显示帮助文本`--help`国旗是存在的.用于将此值设置为`false`当CLI使用自己的帮助文本管理子CLI时.

##### autoVersion

类型:`boolean`<br>默认:`true`

当时自动显示版本文本`--version`国旗是存在的.用于将此值设置为`false`当CLI使用自己的版本文本管理子CLI时.

##### pkg

类型:`Object`<br>默认值:最近的package.json

package.json作为`Object`.

*您很可能不需要此选项.*

##### argv

类型:`Array`<br>默认:`process.argv.slice(2)`

自定义参数对象.

##### inferType

类型:`boolean`<br>默认:`false`

推断参数类型.

默认情况下,参数`5`在`$ foo 5`变成一个字符串.启用此功能会将其推断为数字.

##### booleanDefault

类型:`boolean` `null` `undefined`<br>默认:`false`

的价值`boolean`标志未定义`argv`.如果设置为`undefined`没有定义的标志`argv`将被排除在结果之外.该`default`价值设定`boolean`标志优先于`booleanDefault`.

例:

```js
const cli = meow(`
	Usage
	  $ foo

	Options
	  --rainbow, -r  Include a rainbow
	  --unicorn, -u  Include a unicorn
	  --no-sparkles  Exclude sparkles

	Examples
	  $ foo
	  🌈 unicorns✨🌈
`, {
	booleanDefault: undefined,
	flags: {
		rainbow: {
			type: 'boolean',
			default: true,
			alias: 'r'
		},
		unicorn: {
			type: 'boolean',
			default: false,
			alias: 'u'
		},
		cake: {
			type: 'boolean',
			alias: 'c'
		},
		sparkles: {
			type: 'boolean',
			default: true
		}
	}
});
/*
{
	flags: {rainbow: true, unicorn: false},
	…
}
*/
```

## 承诺

喵会做出未经处理的被拒绝的承诺[大声失败](https://github.com/sindresorhus/loud-rejection)而不是默认的静默失败.这意味着您不必手动操作`.catch()`CLI中使用的承诺.

## 提示

看到[`chalk`](https://github.com/chalk/chalk)如果要为终端输出着色.

看到[`get-stdin`](https://github.com/sindresorhus/get-stdin)如果你想接受来自stdin的输入.

看到[`conf`](https://github.com/sindresorhus/conf)如果你需要保留一些数据.

看到[`update-notifier`](https://github.com/yeoman/update-notifier)如果你想要更新通知.

[更有用的CLI实用程序......](https://github.com/sindresorhus/awesome-nodejs#command-line-utilities)

## 执照

麻省理工学院©[Sindre Sorhus](https://sindresorhus.com)
