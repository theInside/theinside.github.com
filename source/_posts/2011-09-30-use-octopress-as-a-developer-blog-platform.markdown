---
layout: post
title: "使用 Octopress 作為開發者專屬部落格"
date: 2011-09-30 13:48
comments: true
categories: [octopress]
author: deduce
---

[Inside Hack](http://hack.inside.com.tw/) 部落格開張的第一篇文章就來談談本部落格所使用的開放原始碼專案：**[Octopress](http://octopress.org/)**。

身為一個軟體工程師，如果想要寫部落格分享自己的技術研究心得、程式碼，通常都滿麻煩的，目前比較常見的作法是利用 Github 所提供的 [Gist](https://gist.github.com/) 來進行程式碼片段的分享與管理，Gist 的好處是它同時幫我們處理了 Syntax highlight 以及版本的控管（每次更新其實都會被儲存成不同的版本）。使用 Gist 搭配主流的部落格平台，像是 Wordpress 或是 Blogger 其實就非常好用了。
Inside Hack 本來也打算這麼做，但我們遇到了幾個困擾...
<!--more-->
* 用 Gist 貼程式碼的話，程式碼是另外建立、另外存放，跟文章本身是分開的，這點除了造成文章不易流傳之外，使用 RSS 訂閱的讀者也就沒辦法順利看到所有的程式碼。寫作過程也變得比較麻煩，還要特地跑上 Gist 貼程式碼，沒網路的時候就不能寫文章了。
* 用 Wordpress 或 Blogger 比較不適合寫技術文章，如果我們不用 Gist，貼程式碼也會造成額外的麻煩，雖然說並非無法解決，但這類部落格平台生來就不是給 developer 用的，與其花時間去讓這些平台可以幫我們做程式碼上色、讓我們可以用 Markdown 寫文章，不如換一個開箱就有這些功能的平台。
* 我們想要用 geek 的方式寫文章，最好是程式設計師看了會笑的方式，最好文章可以跟程式碼一樣用 Git 控管，最好多人一起寫作的時候可以善用 Github 這種號稱 social coding 的平台。

於是，我們決定用 Octopress，Octopress 有以下特色：

* 基於 [Jekyll]，這是一個以 Ruby 語言撰寫的靜態網頁生成器，
