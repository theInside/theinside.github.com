---
layout: post
title: "你下一個 iOS 專案可以參考的開放原始碼專案"
date: 2011-10-08 15:48
comments: true
categories: [ios]
author: dlackty
---

雖然相對 Android 的開放原始碼策略 iOS 系統似乎沒有這麼的「自由開放」，但是在 App 的開發上， iOS 就如同其他許多熱門平台一樣，有很多優質的開放原始碼專案值得參考、投入。

本篇文章便是要介紹一些 iOS 開放原始碼專案，方便開發者快速的打造 iOS apps ，或許下次你要開發 iOS apps 也可以參考。

<!-- more -->

### Three20

[Three20](http://three20.info/) 的熱門程度連許多非 iOS 開發者都有耳聞，這一個由 Facebook 所公開的開放原始碼專案，目前排名在 GitHub 熱門關注（Popular Wathced）排行的第八名，而前十名則主要包含了像是 jQuery 、 Rails 、 Node.js 等其他重量級的 Web 解決方案。

Three20 主要包含了以下的功能：

* 豐富的 UI 元件，包含模仿原生的 iOS 相簿頁面以及訊息發送介面等
* App 中共用的 Stylesheet ，讓我們可以快速的打造客製化介面
* 進階的 HTTP Request ，支援上傳進度顯示以及資料驗證等
* 方便的 URL-Based Navigation ，讓開發者可以透過定義 URL 來切換介面，像是撰寫網頁程式一般
* 加強許多 UIKit / Foundation 的基本功能（例如 NSString 等）


### Nibmus

[Nimbus](http://jverkoey.github.com/nimbus/) 是 Three20 的主要開發者 Jeff Verkoey 所主導開發的新專案，目標是為了解決 Three20 日益肥大的程式庫所造成的種種問題。

相對於完整的 Three20 而言， Nimbus 的輕量化設計以及徹底的模組化很適合有一定經驗的開發者，只要將自己所需要的模組加入即可，不需要加入整個函式庫。

截至目前為止， Nimbus 已經實作了包含：

* 進階的 HTTP Request ，基於 ASIHTTPRequest
* 類似 iOS 內建的相簿瀏覽介面
* 容易使用的 UITableViewDataSource
* 許多的 UIKit / Foundation 基本功能加強

### iOS Boilerplate

[iOS Boilerplate](http://iosboilerplate.com/) 是模仿熱門的 HTML5 Boilerplate 專案，替 Cocoa Touch Developer 打造一個方便使用的專案模版，在數個月前推出後也獲得很多開發者的矚目。

iOS Boilerplate 相對於 Nimbus 和 Three20 ，提供了更多更基本的函式庫讓開發者可以方便使用，也和 Nimbus 一般加入了 Cocoa 平台上最熱門的 HTTP 函式庫 ASIHTTPRequest 。

iOS Boilerplate 目前提供了像是：

* 結合 ASIHTTPRequest 的遠端圖片下載以及管理介面
* 下拉更新的 UITableView header 以及許多 UITableViewCells
* 第三方的專案，包含 JSONKit 、 SVProgressHUD
* 以及其他 NSData 、 NSString 和 NSDictionary 的一些 helpers

### 我的個人偏好

因為先前曾經參與 Three20 的專案開發，我和我的工作伙伴在 [Polydice](http://thepolydice.com) 的工作主要都使用 Three20 為主，且基於 Three20 也開發了許多介面元件，方便我們快速的能夠完成各種專案。

然而在最近的專案當中，我也開始嘗試使用 Nimbus 來進行開發，主要原因在於 Nimbus 提供了更好的客製化空間，讓我們在效能以及介面上有更多著墨。