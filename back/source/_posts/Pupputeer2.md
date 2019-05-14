---
title: UI自动化测试--Puppeteer再探
date: 2017-08-23
categories: NodeJS
---
看过上篇的同学，应该都会使用Puppeteer的高级爬虫功能了，附上姐妹篇链接：
[爬取并生成《ES6标准入门》PDF--Puppeteer初探](http://www.zhentaoo.com/2017/08/17/Puppeteer/)
除了爬虫之外，也可以使用Puppeteer完成页面上任意操作，即: **可以用来做UI自动化测试**
开门见山，今天的目标是，**爬取SF的热门文章，自动推荐到掘金！！！**

### 简要提下Puppeteer的应用场景
1. 屏幕快照，打印PDF
2. 高级爬虫（有别于传统爬虫.使用Puppeteer可以拿到渲染后的效果，传统爬虫相当于只能拿到http response）
3. UI自动化测试（使用Puppeteer可以模拟用户操作）
4. 页面性能分析

### 废话不多说，直接上动图/视频看效果
GIF图片比较大，如果不能加载成功，也可以到微博看下录制的视频
http://weibo.com/tv/v/FiHMz7dcq?fid=1034:dcc08a8eee118263f6071fb6fafcc9a9

<img src="https://raw.githubusercontent.com/zhentaoo/puppeteer-deep/master/doc/sf-jj.gif" width = "700" height = "440" align=center />

### 下面就来介绍具体流程

#### 1. 爬取 segmentfault 前30篇热门文章
  - 跳转到https://segmentfault.com/news/frontend
  - 接着分析SF首页的Dom结构，爬取每篇文章的链接
  - 然后取出每篇文章最重要的 href，title 等信息
  - 具体代码如下：
  ```js
      await page.goto('https://segmentfault.com/news/frontend')

      var SfFeArticleList = await page.evaluate(() => {
          var list = [...document.querySelectorAll('.news__list .news__item-title a')]
          return list.map(el => {
              return {href: el.href.trim(), title: el.innerText}
          })
      })

      await page.screenshot({path: './sf-juejin/sf.png', type: 'png'});
  ```

#### 2. 登录掘金 (这里我事先注册了个测试账号,大家可以替换成自己的)
- 跳转到掘金，模拟点击登录按钮
- 接着，会弹出一个的登录dialog，模拟输入用户名密码
- 模拟点击登录，稍等....嗯...掘金应该把cookie写好了....
- 代码如下：
```js
      await page.goto('https://juejin.im')

      var login = await page.$('.login')
      await login.click()

      var loginPhoneOrEmail = await page.$('[name=loginPhoneOrEmail]')
      await loginPhoneOrEmail.click()
      await page.type('18516697699@163.com', {delay: 20})

      var password = await page.$('[placeholder=请输入密码]')
      await password.click()
      await page.type('123456', {delay: 20})

      var authLogin = await page.$('.panel .btn')
      await authLogin.click()
```
#### 3.推荐文章（使用第一步从SF爬取的文章信息）
- 模拟点击推荐文章 按钮 “＋”
- 这时从SF拿到的文章信息就派上用场了，随机取出一篇: Math.floor(Math.random() * 30)
- 模拟填写推荐表单，点击发布
- 嗯，有时会提示该文章已被分享，那就换一篇吧，再执行一次。
- 代码如下
```js
      var seed = Math.floor(Math.random() * 30)
      var theArtile = SfFeArticleList[seed]

      var add = await page.$('.main-nav .ion-android-add')
      await add.click()

      var shareUrl = await page.$('.entry-form-input .url-input')
      await shareUrl.click()
      await page.type(theArtile.href, {delay: 20})

      await page.press('Tab')
      await page.type(theArtile.title, {delay: 20})

      await page.press('Tab')
      await page.type(theArtile.title, {delay: 20})

      await page.evaluate(() => {
          let li = [...document.querySelectorAll('.category-list-box .category-list .item')]
          li.forEach(el => {
              if (el.innerText == '前端')
                  el.click()
          })
      })

      var submitBtn = await page.$('.submit-btn')
      await submitBtn.click()
```


### 项目Repo && 运行
1. git clone https://github.com/zhentaoo/puppeteer-deep
2. npm install (puppeteer在win下100+M、mac下70+M，请耐心等候)
3. npm test

### 结语
1. 为了效果展示，这里使用的headless: false模式，实际使用时可以同时开n个page，模拟操作，大家可以尝试改改，也可以给我提PR
2. 目前已经带领大家，使用Puppeteer完成爬虫 和 UI自动化测试，接下来可能会出第三篇，应该会是关于前端性能分析
3. 其实Puppeteer的应用场景远不止这些，大家也可以使用它在各自的领域大放异彩！！！
4. 希望掘金小编不会打我....
