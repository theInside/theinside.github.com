---
layout: page
title: "About us"
date: 2011-09-30 10:45
comments: false
sharing: false
footer: true
---

我們是一群熱愛網頁技術、行動應用開發的工程師，在 Inside Hack 上透過持續的寫作，分享自己的程式開發經驗並與讀者交流，希望藉由持續的互動，讓彼此都可以有所收穫。

## Inside Hack 善用開放原始碼專案與開放平台
Inside Hack 網站利用了以下幾個有趣的玩具來進行維護：

* 網頁托管：**[Github](http://www.github.com/)** 目前世界上最熱門的 Social Coding 平台。
* 網頁生成：**[Octopress](http://octopress.org/)** 官方網頁上寫著 Octopress 是 *A blogging framework for hackers*. 雖然我們不是 Hacker 等級的高手，但我們覺得這個系統真是好用，可以讓我們專注在寫作、分享，寫作的過程，要貼程式碼也很簡單，真的是工程師寫作的好幫手。
* 網頁撰寫：**[Markdown](http://markdown.tw/)**，讓我們專注在寫作上的好物。
* 網頁版本維護：每一篇文章的撰寫以及編輯，都是利用 Git 這個強大的分散式版本控制系統在進行 commit，所以對於部落格經營過程中需要的文章狀態，像是草稿、已發佈文章就可以各自控制，要整合多位作者的文章就只需要利用 merge，投稿則是利用 pull request 的方式進行，很有趣吧！ :p

## 投稿方式

* 請在 Github 上 fork 本網站： [https://github.com/theInside/theinside.github.com](https://github.com/theInside/theinside.github.com)
* 在 **source** 這個 branch 裡面透過以下指令新增一篇文章
```	
rake new_post["文章標題"]
```
* 文章的格式可以參考[Octopress網站上的說明](http://octopress.org/docs/blogging/)，以及[Markdown 的語法說明](http://markdown.tw/)
* 你肯定會需要貼程式碼的功能，記得看看 [Codeblock](http://octopress.org/docs/plugins/codeblock/) & [Backtick Codeblock](http://octopress.org/docs/plugins/backtick-codeblock/) 的使用
* 投稿前（或說送出你的 pull request 前），請愛用 [rebase](http://book.git-scm.com/4_rebasing.html) 整理你的 commit log.
	