---
layout: post
title: 如何合併多個commits
date: 2011-12-30 15:31
comments: true
categories: [Git]
author: chiachialee
---


如果你使用git做版本控管，在一個branch上開發一段時間後，commits看起來又多又雜亂，這個時候你可能會需要整理你的commits，將多個commits合併成1個，而git提供了一個指令可以幫助你完成這件事情：git rebase -i

一個簡單的範例，目標是：
將原本git log
	a
	b1
	b2


變成
	a
	b

也就是將兩個commits，b1和b2，合併成為b。
那麼方法和步驟如下：

**Step1：$ git rebase -i <不變動的commit的SHA-1>**

例如此例就是a的SHA-1 (可用git log查看該commit的版本號)

	$ git rebase -i 49687a0a646954afdf3f4dae1f914ea793341ea2

按下enter後會進到編輯模式

**Step2：squash**

一開始編輯視窗的內容是這樣
{% codeblock %}
pick 033beb4 b1
pick d426a8a b2

# Rebase 49687a0..d426a8a onto 49687a0
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#  x, exec = run command (the rest of the line) using shell
#
# If you remove a line here THAT COMMIT WILL BE LOST.
# However, if you remove everything, the rebase will be aborted.
#
{% endcodeblock %}

分成兩個區塊：第一是rebase interactive要執行的指令，第二是每個指令的簡單說明('#'後的是注解)。在這個例子裡面，我們至少要搞懂以下兩個：
	pick = use commit
	squash = use commit, but meld into previous commit
也就是說，pick是會執行該commit，而squash會把這個版本的commit合併到**前一個**commit。
所以我們要將它改成：
{% codeblock %}
pick 033beb4 b1
squash d426a8a b2
{% endcodeblock %}

也就是將d426a8a這個版本的commit併到版本號033beb4的commit，對應我們的例子，就是將b2合併到b1這個commit。

在編輯完成後按下esc，打上:wq 儲存離開。

**Step3：接著輸入新的commit message**

剛進來這個畫面，它會告訴你原本兩個commit messages分別是b1和b2，現在你要輸入新的commit message，也就是b，建議可以把原本的訊息註解掉。
```
b
# This is a combination of 2 commits.
# The first commit's message is:
# b1
# 
# This is the 2nd commit message:
#
# b2
#
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# Not currently on any branch.
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   a.txt
#
```

編輯完成後一樣儲存離開。沒意外的話，會出現這樣的訊息：_Successfully rebased and updated refs/heads/develop._。最後可以用git log這個指令來檢驗commits是不是變成我們要的樣子：
{% codeblock %}
-> % git log
commit 27a1ff53137c6a35da1a83f4aa9ab7f652a5b4b2
Author: ChiaChia Lee <spiderman0103@gmail.com>
Date:   Fri Dec 30 15:07:38 2011 +0800

    b

commit 49687a0a646954afdf3f4dae1f914ea793341ea2
Author: ChiaChia Lee <spiderman0103@gmail.com>
Date:   Fri Dec 30 15:07:29 2011 +0800

    a

{% endcodeblock %}

