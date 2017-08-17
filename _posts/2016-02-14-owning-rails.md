---
layout: post
title: "2016新春計畫-owning rails course"
date: 2016-02-14 20:30 +0800
categories: programming
tag: [2016新春, rails]
---
這是長達八小時的[教學影片](http://owningrails.com/)。或許因為自己能力不足，八小時完全是看了又停（查資料），剛著影片打code，而且還不太清楚自己打了什麼。又一邊查文章、找資料，也算是把這八個小時的教學影片給看完了。完成了新春計畫的第二部分。   

<!-- more -->
這影片是去年十一月底和學長說要一起努力學習完成的。想想開始認真學rails也過了半年了，不敢說自己可以做什麼或是完成什麼，但也跟著著[rails tutorial](https://www.railstutorial.org/book), tealeaf academy(現已改名[launch school](https://launchschool.com/)), [rails101](http://growth.xdite.net/courses/rails-101/)等等教學
照著打了好幾次的code, 也正努力把russ olsen的"eloquent ruby"看完了2/3。tealeaf有一系列的"no magic rails"的影片，當時看了半小時就放棄了，因為完全看不懂。  

我不知道什麼是正確的學習方法？也不曉得去了解這一些背後運作的原理能否之後可以打code跟飛起來一樣。不過剛好昨天有人傳了篇文章給我- 張五常["思考的方法"](http://mp.weixin.qq.com/s?__biz=MzA4OTQzODA1MQ%3D%3D&mid=402065630&idx=1&sn=d3bdc1fbdd53503d3358c57a6f6e73e7&scene=4&srcid=0211CguVQmZFoXBS8W25i6zS#rd), 台灣有出「賣桔者言」，收錄在其中，是篇老文章。學習這些大抵就是訓練自己思考的方式。至少看完owning rails之後，雖然只過了一兩天，背後整個rails的架構、甚至更深層的東西，不知道要多久之後才會懂，但基礎的設計邏輯、想法等等，比較清楚，也知道自己哪裡不足，之後學習的路上還要再加強。  

不知道其他學習者有沒有這樣的問題？雖然知道MVC(model, view, controller)的架構，但一直百思不得其解為什麼會這麼神奇？很多語法雖然書本、講師都說是來自於ruby，回頭學完了基礎的ruby語法，還是不曉得rails的語法為什麼可以這樣用？可能是我愛鑽牛角尖。無法回答這樣的問題，每次跟著打完一次code，就又無法理解時，很挫折。但看完owning rails，又感覺可以繼續往下走了。知道這三者之間是如何互動的，也知道為何要這麼做了。
教學影片大抵都會先從routing看始，接著是controller, 設定一下activerecord/activemodel, 再來是把view設定好。但是為什麼要這樣做呢？ owning rails 講得很清楚。  

雖然很多評論都說要中階以上的使用者在學習，但我反而覺得大抵知道rails再幹麼，學完ruby後，就可以先看一次了。
不過真的會很挫折就是了。以下是我個人的一些想法，如果可以先知道，或許學習上更會舒服些。

先知道http是在幹麼的，以下是他在day2所提到的影片，先知道的話，會省去不少時間，我也是看完這影片才對http 又有更多的了解。  

- [caching tutorial](https://www.mnot.net/cache_docs/)
- [things caches do](http://2ndscale.com/rtomayko/2008/things-caches-do)
- 還有可以看railscast:[#321 http caching](http://railscasts.com/episodes/321-http-caching)

再來是一些ruby 的基本知識。day 1 的第一天會快速的復習，會講到不少metaprogramming的東西，但不會太深奧，可以把codeschool 的[ruby bits part 2](https://www.codeschool.com/courses/ruby-bits-part-2), 就可以理解。還有block, lambda, proc的[差異](https://awaxman11.github.io/blog/2013/08/05/what-is-the-difference-between-a-block/), 雖然影片也會提到，但是可以再用熟一點再開始看會更好。另外，instance_eval, class_eval如果可以清楚，會[更好](http://rubylearning.com/blog/2010/11/30/how-do-i-build-dsls-with-yield-and-instance_eval/)

再來第三個易卡關的是rack, rack middleware，我覺得這個部分railscast[講解的不錯:#319](http://railscasts.com/episodes/319-rails-middleware-walkthrough)。

第二大就多在教你如何讀rails source code，並使用第一天的知識。

最後是作者給的一些建議：

#### fail to browse code
1. start reading
2. get overwhelming
3. curse
4. claim you could do better
5. back to step 1 or give up
6. repeat 1 to 5

#### right way to browse code
1. take a deep breath
2. start with assumption
3. follow the flow(don't read top to bottom)
  - try to use search function(shortcut) in editor. 主題式閱讀法(find method definition)
4. **don't try to understand everything**

或許，後續還要再多看個幾次影帶。不過至少，在新春，是個很好的開始。
看完之後，在reddict 上看到有人討論這一本書[rebuilding rails](https://rebuilding-rails.com/), 看了覺得還不錯，可以再對owning rails中沒提到，或是初心者不熟的地方，有個文字方面的補強。

以上，希望可以給新年好的開始！

- tips:作者有個[rebuilding web server](https://confirmsubscription.com/h/y/52E53ED7FF30E314), signup 完成後，會有一個1hr的教學影片，還有discount coupon可以折50USD喔!
