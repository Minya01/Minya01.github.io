---
layout: post
title: 单元测试(1)
date:   2018-02-03
categories: unit-test
---

单元测试一周学习总结

### 1. 开篇废话
**肺话** 以前，我可能会各种借口不乐意花时间进行这种“佛性”活动--技术沉淀。“写博客是对工作有很大帮助的”这个道理似乎如影随形，但是上网一顿搜索发现大神级文章那么多，自己就懒得动手了，直接收藏，然后再也没有翻出来看过。。。佛主叹息。。。

**激励** 感谢群里的大家这么激烈讨论，瞬间压力直线上升。

### 2. 单元测试
> 因为前端开发的大多数问题都出在浏览器兼容上，或者界面的交互上，所以很少注意单元测试。接触nodejs后，会发现初始化很多框架项目都会提示是否安装单元测试（Mocha、Jasmine等），使用单元测试作为程序可用性的保障。

主要是测试一段代码（一个函数或者一个对象），代码要足够分离。

#### 2.1 TDD or BDD
```
* TDD:测试驱动开发
* BDD：行为驱动开发
```
简而言之，TDD是在执行之前编写测试来驱动代码设计的做法。BDD建议测试行为，不考虑代码具体实现，而是考虑这个场景期待什么结果。

这一周主要是学习BDD测试（Mocha）。Mocha不仅支持BDD（describe），也支持TDD（suite）。

#### 2.2 Mocha
Mocha是express作者的另一个作品。

Mocha本身是不带断言库的，可以根据自己的喜好选择，比如should.js、node内置的assert模块、chai等。

每一个test case都是以describe()开始，用以描述每个test case要测试的内容，可以包含多层describe，最里面是it()函数用来验证结果，it的第一个参数是字符串（用来描述期待的结果），比如“it should ...”，如下例子：

首先安装全局mocha，sudo npm install mocha -g

	npm install mocha should --save-dev

然后编写测试用例：

	describe('Array', function() {
		describe('#indexOf()', function() {
       		it('should return -1 when the value is not present', function() {
          		assert.equal(-1, [1, 2, 3].indexOf(123))
       		})
	  	})
	})

以上代码是同步执行的，如果想要测试异步的，就使用done().

最后执行mocha（或npm test）即可看到结果。

#### 2.3 整合Travis CI
Travis CI是一个提供持续整合服务的平台，将github于Travis CI整合后，github有任何更新，Travis CI会自动执行测试。

1.使用github登录Travis CI

2.Travis CI会列出github上所有的repository，选择我们要进行测试的项目，激活。

3.我们需要在项目里添加.travis.yml文件，用于写入travis配置。如下：

	language: node_js
	node_js:
		- "9.2.1"
	before_script:
		- npm install -g mocha

4.进入Travis查看测试结果，测试通过会显示绿色的passing

#### 2.4 Karma
Karma的作者是AngularJS团队。Karma是一个工具，可以通过cli针对浏览器运行JavaScript。Karma运行在浏览器上，而不是虚拟DOM。

全局安装：sudo npm install karma-cli -g

	npm install karma karma-chrome-launcher karma-mocha --save-dev

执行karma init进行初始化配置，将会生成一个karma.conf.js文件。具体配置项可以参阅官方配置文档。

在package.json中，配置运行命令：

	"scripts": {
    	"test": "karma start karma.conf.js"
    },

通过npm test运行，如果结果正确，则会在控制台显示如下:

	Karma v1.7.1 server started at http://0.0.0.0:9887/
	Launching browser Chrome with unlimited concurrency
	Starting browser Chrome

#### 2.5 再次整合Travis CI

> 使用chrome浏览器测试

	language: node_js
	node_js:
		- "9.2.1"
	scripts:
		- node_modules/karma/bin/karma start karma.conf.js --single-run
	before_install:
	 	- export CHROME_BIN=chromium-browser
	 	- export DISPLAY=:99.0
	 	- sh -e /etc/init.d/xvfb start

遇到的问题：

* 配置文件singleRun项，默认为false，这将会在执行完之后持续监控，直到10分钟后超时报错。所以需要设置singleRun：true，执行完后自动退出。
* chrome出现报错：

```
Cannot start Chrome
The SUID sandbox helper binary was found, but is not configured correctly. Rather than run without sandboxing I'm aborting now. You need to make sure that /opt/google/chrome/chrome-sandbox is owned by root and has mode 4755.
```
查找Travis官方文档找到解决方案[Chrome](https://docs.travis-ci.com/user/chrome):--no-sandbox

设置config：

	cfg对象：

	// travis ci 使用chrome测试
	customLaunchers: {
      Chrome_travis_ci: {
        base: 'Chrome',
        flags: ['--no-sandbox']
      }
    }
    <!--判断环境为Travis,使用自定义配置浏览器-->
    if (process.env.TRAVIS) {
        cfg.browsers = ['Chrome_travis_ci'];
    }

    config.set(cfg);

### 3. 总结
这次总结只是大概梳理一下学习的单元测试流程，还有很多需要深入学习的地方，在未来还要继续努力。感谢大家。
