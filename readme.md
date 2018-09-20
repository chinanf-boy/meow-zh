# meow [![explain]][source] [![translate-svg]][translate-list]

<!-- [![size-img]][size] -->

[explain]: http://llever.com/explain.svg
[source]: https://github.com/chinanf-boy/meow-explain
[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list
[size-img]: https://packagephobia.now.sh/badge?p=Name
[size]: https://packagephobia.now.sh/result?p=Name

「 CLI 命令行 帮助 库 」

[中文](./readme.md) | [english](https://github.com/sindresorhus/meow)

---

## 校对 ✅

<!-- doc-templite START generated -->
<!-- repo = 'sindresorhus/meow' -->
<!-- commit = '258659a6e4cf102ef09a7c27efcee1e953808725' -->
<!-- time = '2018 5.22' -->

| 翻译的原文 | 与日期       | 最新更新 | 更多                       |
| ---------- | ------------ | -------- | -------------------------- |
| [commit]   | ⏰ 2018 5.22 | ![last]  | [中文翻译][translate-list] |

[last]: https://img.shields.io/github/last-commit/sindresorhus/meow.svg
[commit]: https://github.com/sindresorhus/meow/tree/258659a6e4cf102ef09a7c27efcee1e953808725

<!-- doc-templite END generated -->

### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)

## 生活

[help me live , live need money 💰](https://github.com/chinanf-boy/live-need-money)

---

# meow [![Build Status](https://travis-ci.org/sindresorhus/meow.svg?branch=master)](https://travis-ci.org/sindresorhus/meow)

> CLI 应用 帮助库

![](https://github.com/sindresorhus/meow/blob/master/meow.gif)

## 特征

- 解析命令参数
- 将参数转换为[骆驼风格](https://github.com/sindresorhus/驼峰字形)
- 使用`--no-`,能反转参数
- 输出版本`--version`
- 输出描述和提供的帮助文本`--help`
- 让未经处理的失败的Promise[大声说出](https://github.com/sindresorhus/loud-rejection)而不是默认的静默失败
- 将进程标题设置为 package.json 中定义的二进制名称

### 目录

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [安装](#%E5%AE%89%E8%A3%85)
- [用法](#%E7%94%A8%E6%B3%95)
- [API](#api)
  - [meow(options,[minimistOptions])](#meowoptionsminimistoptions)
    - [options](#options)
      - [flags](#flags)
      - [description](#description)
      - [help](#help)
      - [version](#version)
      - [autoHelp](#autohelp)
      - [autoVersion](#autoversion)
      - [pkg](#pkg)
      - [argv](#argv)
      - [inferType](#infertype)
      - [booleanDefault](#booleandefault)
- [Promise](#promise)
- [提示](#%E6%8F%90%E7%A4%BA)
- [执照](#%E6%89%A7%E7%85%A7)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

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

const cli = meow(
  `
	Usage
	  $ foo <input>

	Options
	  --rainbow, -r  Include a rainbow

	Examples
	  $ foo unicorns --rainbow
	  🌈 unicorns 🌈
`,
  {
    flags: {
      rainbow: {
        type: 'boolean',
        alias: 'r',
      },
    },
  }
);
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

### meow(options,[minimistOptions])

返回的`Object`据有:

- [input](#input) _(Array)_ - 非标志参数
- [flags](#flags) _(Object)_ - 标志参数转换为 驼峰字形
- [pkg](#pkg) _(Object)_ - `package.json`对象
- [help](#help) _(string)_ - 使用的帮助文本`--help`
- `showHelp([code=2])` _(Function)_- 显示帮助文本并退出代码为`code`
- `showVersion()` _(Function)_- 显示版本文本并退出

#### options

type:|`Object` `Array` `string`
---|---
Desc: | 可以是一个字符串/数组的`help`文本或options对象.

##### flags

type:|`Object`
---|---
Desc:|定义参数标志.

字段关键字是标志参数名称,值是下面对象结构:

- `type`:值的类型.(可能的值:`string` `boolean`)
- `alias`:通常用于定义短标志别名.
- `default`:未指定标志时的默认值.

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

##### description

type:|`string` `boolean`
---|---
默认值:|package.json的`"description"`属性
Desc: |在帮助文本上方显示.

将其设置为`false`完全禁用它.

##### help

type:|`string` `boolean`
---|---
Desc:| 您想要显示的帮助文本.

输入是重新缩进的,并且头尾的换行符剪掉,这意味着您可以使用[模板文字](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/template_strings)无需关心使用正确数量的缩进.

此文本自动显示在帮助文本上方.

##### version

type:|`string` `boolean`
---|---
默认值:|package.json的`"version"`属性
Desc: | 设置自定义版本输出

##### autoHelp

type:|`boolean`
---|---
默认:|`true`

自动显示帮助文本,当`--help`使用时.设置为`false`有用于当 CLI 的子 CLI 要使用自己的帮助文本时.

##### autoVersion

type:|`boolean`
---|---
默认:|`true`

自动显示版本文本,当`--version`使用时.设置为`false`有用于当 CLI 的子 CLI 要使用自己的版本文本时.

##### pkg

type:|`Object`
---|---
默认值: | 最近的 package.json
Desc:| package.json 作为`Object`.

_您很可能不需要此options._

##### argv

type:|`Array`
---|---
默认: | `process.argv.slice(2)`
Desc:| 自定义参数对象.

##### inferType

type:|`boolean`
---|---
默认: | `false`
Desc:| 推断参数类型.

默认情况下,参数`5`在`$ foo 5`会变成一个字符串.启用此函数会将其推断为数字.

##### booleanDefault

type:|`boolean` `null` `undefined`
---|---
默认:|`false`

`boolean`类型的参数值若未定义在`argv`中.如果设置为`undefined`,那在`argv`没有定义的标志,将被排除在结果之外.

该`default`值设定`boolean`类型的参数会优先于`booleanDefault`.

例:

```js
const cli = meow(
  `
	Usage
	  $ foo

	Options
	  --rainbow, -r  Include a rainbow
	  --unicorn, -u  Include a unicorn
	  --no-sparkles  Exclude sparkles

	Examples
	  $ foo
	  🌈 unicorns✨🌈
`,
  {
    booleanDefault: undefined,
    flags: {
      rainbow: {
        type: 'boolean',
        default: true,
        alias: 'r',
      },
      unicorn: {
        type: 'boolean',
        default: false,
        alias: 'u',
      },
      cake: {
        type: 'boolean',
        alias: 'c',
      },
      sparkles: {
        type: 'boolean',
        default: true,
      },
    },
  }
);
/*
{
	flags: { 
		rainbow: true,
		r: true,
		unicorn: false,
		u: false,
		sparkles: true },
	…
}
*/
```

## Promise

meow会做出未经处理的失败的promise[大声失败](https://github.com/sindresorhus/loud-rejection)而不是默认的静默失败.这意味着您不必手动操作CLI 中使用的promise的`.catch()`.

## 提示

查阅[`chalk`](https://github.com/chalk/chalk)如果要为终端输出着色.

查阅[`get-stdin`](https://github.com/sindresorhus/get-stdin)如果你想接受来自 stdin 的输入.

查阅[`conf`](https://github.com/sindresorhus/conf)如果你需要保留一些数据.

查阅[`update-notifier`](https://github.com/yeoman/update-notifier)如果你想要更新通知.

[更有用的 CLI 实用程序......](https://github.com/sindresorhus/awesome-nodejs#command-line-utilities)

## 执照

MIT ©[Sindre Sorhus](https://sindresorhus.com)
