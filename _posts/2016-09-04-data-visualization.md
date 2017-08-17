---
title: 做報告時，要選用什麼圖？-快速上手資料視覺化
date: 2016-09-04 15:02:06
tags: [data science, visualization, cs109]
categories: [data science, cs109]
---

這是在harvard cs 109 data science 的第3講。我覺得非常簡潔，因此做個筆記給自己和需要做視覺化的大家。很多時候，無論是在報告、或是寫論文，常常會對於要選用什麼圖來表達感到疑惑。這個講解很短，約一個半小時，但是有很多不錯的內容。

文末附上課程的影片和投影片。
![chart suggestions](https://dl.dropboxusercontent.com/u/22163115/Selection_008.png)

<!--more-->
## 資料視覺化的五步驟
1. ask an interesting question
2. get the data
3. explore the data
4. model the data
5. communicate and visulize the results

所以，你想要做資料視覺化的最重要的第一步就是，你**要回答什麼問題？**
而視覺化目的就是為了要用更有效率的方式傳達你的重要，想要溝通更加流暢，而不是增加麻煩。
1. communication
2. analyze

## 有效的視覺化溝通
最重要的一點就是：**不要用3D立體圖**。
3D圖是很多軟體標榜的特效，看起來很炫，但是不實用。因為比例會扭曲，而且無法*簡單*而清楚地表達我們想要的東西。

### 正確地用圖說話
長條圖就要正確的比例，不要故意放大要強調的；曲線圖要把正確的變化呈現出來，圓餅圖的比例加起來要100%。很多東西都是國中國小的時候學到的。不過好像長大了，大家都還給老師了……或是當時不知道這些東西的重要性…

### 簡單最好
要小心使用顏色（後面會再提到）、表格中的背影，想想看可以去掉什麼，越簡單越好。再強調一次**不要用3D圖**

### 用正確的圖表
就是本文一開始的圖表，完整的[pdf](http://extremepresentation.typepad.com/blog/files/choosing_a_good_chart.pdf)

#### 比較資料時
這裡推薦使用bar chart, 或是 line chart, 可以清楚的呈現出來。其他更多比較資料時可用的表格可以看chart suggestion
![beer](https://dl.dropboxusercontent.com/u/22163115/Selection_009.png)
還有一點重要的，bar chart 要從零開始[^1]
### 表達趨勢
請用line chart，如果有時間關係的話

### 表達比例時
pie chart[^2] 是大家最常見的。後面會提到, pie chart 某些狀況很好用，用的話要小心。因為視覺化上，最好的狀況是用長度比角度來呈現會更好，等等還會再提。

![pie chart](http://eagerpies.com/wp-content/uploads/2013/04/Screen-shot-2013-04-01-at-12.51.21-AM.png)


如果可以的話用stacked bar chart 或是stacked area chart
這裡有一篇文章提到為什麼要用bar chart或area chart[^3]。

要記得簡單最好，不要圓餅圖裡還有圓餅圓，太過複雜的話，看看是不是要切成兩個或多個圖來表達想法。
![stacked bart chart](https://dl.dropboxusercontent.com/u/22163115/Selection_010.png)

### 表達相關性

請用scatterplot(散佈圖)。但是小心**不要使用3D**…

### 表達分佈

可以用histogram(分佈圖)，最重要的是寬度，不同的寬度表達出來的圖形會讓人給出不一樣的解釋。或是可以使用density plots來表達。（**不要用3D**)

### seaborn
![seaborn homepage](https://dl.dropboxusercontent.com/u/22163115/Selection_013.png)

因為這堂課是使用python 做為主要的語言，這裡推薦可以去[seaborn](http://web.stanford.edu/~mwaskom/software/seaborn/tutorial/distributions.html)讀一下有如何選用正確的圖來解勢你的資料

### 小心使用顏色
![percetual effectiveness](https://dl.dropboxusercontent.com/u/22163115/Selection_012.png)

這裡有提到很多新聞、網站，很喜歡用顏色來表達資料。但人可以分辨出來的顏色大約7種上下，"五色令人盲"，大概就是這個意思。
如果可以的話，多使用**長度、位置**，來表達。人們對於這兩個的感知最正確。

如果不得已再來用的話，選用面積或是角度。（這也是為什麼stack bar chart比圓餅圖來得好的原因）

如果真的要用顏色，請用漸層色，來表達有順序的資料。我們常常在大氣資料看到用彩紅的顏色來表達，但是這樣的表達，被我們解讀時，不會是線性的，我們會放大對紅色和黃色的解讀，看起來會很大、很多、很嚴重。但如果真放在線性比較時，就沒有這樣的狀況。

有介紹一個叫[color brewer](http://colorbrewer2.org/), 可以協助選用對的漸層色。



## 建議閱讀
- Edward Tufte
  - [the visual display of quantitative information](https://www.amazon.com/Visual-Display-Quantitative-Information/dp/0961392142/ref=la_B000APET3Y_1_1?s=books&ie=UTF8&qid=1472972271&sr=1-1)
  - [envision information](https://www.amazon.com/Envisioning-Information-Edward-R-Tufte/dp/0961392118/ref=la_B000APET3Y_1_2?s=books&ie=UTF8&qid=1472972271&sr=1-2)
  - [visual explanations](https://www.amazon.com/Visual-Explanations-Quantities-Evidence-Narrative/dp/0961392126/ref=la_B000APET3Y_1_3?s=books&ie=UTF8&qid=1472972271&sr=1-3)
- Stephen Few
  <!-- - [Now you see it](https://www.amazon.com/Now-You-See-Visualization-Quantitative/dp/0970601980/ref=la_B001H6IQ5M_1_2?s=books&ie=UTF8&qid=1472972384&sr=1-2) -->
  - [Information dashborad design](https://www.amazon.com/Information-Dashboard-Design-At---Glance/dp/1938377001/ref=la_B001H6IQ5M_1_3?s=books&ie=UTF8&qid=1472972384&sr=1-3)
  - [Show me the numbers](https://www.amazon.com/Show-Me-Numbers-Designing-Enlighten/dp/0970601972/ref=la_B001H6IQ5M_1_1?s=books&ie=UTF8&qid=1472972384&sr=1-1)

## 相關資源
- [投影片下載](https://github.com/cs109/2015/raw/master/Lectures/03-EDA.pdf)
- [影片](https://matterhorn.dce.harvard.edu/engage/player/watch.html?id=a4e81697-fd86-415c-9b29-c14ea7ec15f2) 大約一個半小時，不知何時會失效
### footnote
[^1]: 長條圖的更多 [解釋](http://flowingdata.com/2015/08/31/bar-chart-baselines-start-at-zero/)
[^2]: 對圓餅圖還想知道 [更多的話](http://eagerpies.com/)
[^3]: 這裡有更多解說 [Quantitative Displays for Combining
Time-Series and Part-to-Whole Relationships](http://www.perceptualedge.com/articles/visual_business_intelligence/displays_for_combining_time-series_and_part-to-whole.pdf)
