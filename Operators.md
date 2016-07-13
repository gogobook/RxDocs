# 操作符分類

ReactiveX的每種編程語言的實現都實現了一組操作符的集合。不同的實現之間有很多重疊的部分，也有一些操作符只存在特定的實現中。每種實現都傾向於用那種編程語言中他們熟悉的上下文中相似的方法給這些操作符命名。

本文首先會給出ReactiveX的核心操作符列表和對應的文檔連結，後面還有一個決策樹用於幫助你根據具體的場景選擇合適的操作符。最後有一個語言特定實現的按字母排序的操作符列表。

如果你想實現你自己的操作符，可以參考這裡：[`實現自定義操作符`](topics/Implementing-Your-Own-Operators.md)

## 創建操作

用於創建Observable的操作符

* [`Create`](operators/Create.md) — 通過調用觀察者的方法從頭創建一個Observable
* [`Defer`](operators/Defer.md) — 在觀察者訂閱之前不創建這個Observable，為每一個觀察者創建一個新的Observable
* [`Empty/Never/Throw`](operators/Empty.md) — 創建行為受限的特殊Observable
* [`From`](operators/From.md) — 將其它的物件或數據結構轉換為Observable
* [`Interval`](operators/Interval.md) — 創建一個定時發射整數序列的Observable
* [`Just`](operators/Just.md) — 將物件或者物件集合轉換為一個會發射這些物件的Observable
* [`Range`](operators/Range.md) — 創建發射指定範圍的整數序列的Observable
* [`Repeat`](operators/Repeat.md) — 創建重複發射特定的數據或數據序列的Observable
* [`Start`](operators/Start.md) — 創建發射一個函數的返回值的Observable
* [`Timer`](operators/Timer.md) — 創建在一個指定的延遲之後發射單個數據的Observable

## 變換操作

這些操作符可用於對Observable發射的數據進行變換，詳細解釋可以看每個操作符的文檔

* [`Buffer`](operators/Buffer.md) — 緩存，可以簡單的理解為緩存，它定期從Observable收集數據到一個集合，然後把這些數據集合打包發射，而不是一次發射一個
* [`FlatMap`](operators/FlatMap.md) — 扁平映射，將Observable發射的數據變換為Observables集合，然後將這些Observable發射的數據平坦化的放進一個單獨的Observable，可以認為是一個將嵌套的數據結構展開的過程。
* [`GroupBy`](operators/GroupBy.md) — 分組，將原來的Observable分拆為Observable集合，將原始Observable發射的數據按Key分組，每一個Observable發射一組不同的數據
* [`Map`](operators/Map.md) — 映射，通過對序列的每一項都應用一個函數變換Observable發射的數據，實質是對序列中的每一項執行一個函數，函數的參數就是這個數據項
* [`Scan`](operators/Scan.md) — 掃瞄，對Observable發射的每一項數據應用一個函數，然後按順序依次發射這些值
* [`Window`](operators/Window.md) — 窗口，定期將來自Observable的數據分拆成一些Observable窗口，然後發射這些窗口，而不是每次發射一項。類似於Buffer，但Buffer發射的是數據，Window發射的是Observable，每一個Observable發射原始Observable的數據的一個子集

## 過濾操作

這些操作符用於從Observable發射的數據中進行選擇

* [`Debounce`](operators/Debounce.md) — 只有在空閒了一段時間後才發射數據，通俗的說，就是如果一段時間沒有操作，就執行一次操作
* [`Distinct`](operators/Distinct.md) — 去重，過濾掉重複數據項
* [`ElementAt`](ElementAt.md) — 取值，取特定位置的數據項
* [`Filter`](operators/Filter.md) — 過濾，過濾掉沒有通過謂詞測試的數據項，只發射通過測試的
* [`First`](operators/First.md) — 首項，只發射滿足條件的第一條數據
* [`IgnoreElements`](operators/IgnoreElements.md) — 忽略所有的數據，只保留終止通知(onError或onCompleted)
* [`Last`](operators/Last.md) — 末項，只發射最後一條數據
* [`Sample`](operators/Sample.md) — 取樣，定期發射最新的數據，等於是數據抽樣，有的實現裡叫ThrottleFirst
* [`Skip`](operators/Skip.md) — 跳過前面的若干項數據
* [`SkipLast`](operators/SkipLast.md) — 跳過後面的若干項數據
* [`Take`](operators/Take.md) — 只保留前面的若干項數據
* [`TakeLast`](operators/TakeLast.md) — 只保留後面的若干項數據

## 組合操作

組合操作符用於將多個Observable組合成一個單一的Observable

* [`And/Then/When`](operators/And.md) — 通過模式(And條件)和計劃(Then次序)組合兩個或多個Observable發射的數據集
* [`CombineLatest`](operators/CombineLatest.md) — 當兩個Observables中的任何一個發射了一個數據時，通過一個指定的函數組合每個Observable發射的最新數據（一共兩個數據），然後發射這個函數的結果
* [`Join`](operators/Join.md) — 無論何時，如果一個Observable發射了一個數據項，只要在另一個Observable發射的數據項定義的時間窗口內，就將兩個Observable發射的數據合併發射
* [`Merge`](operators/Merge.md) — 將兩個Observable發射的數據組合併成一個
* [`StartWith`](operators/StartWith.md) — 在發射原來的Observable的數據序列之前，先發射一個指定的數據序列或數據項
* [`Switch`](operators/Switch.md) — 將一個發射Observable序列的Observable轉換為這樣一個Observable：它逐個發射那些Observable最近發射的數據
* [`Zip`](operators/Zip.md) — 打包，使用一個指定的函數將多個Observable發射的數據組合在一起，然後將這個函數的結果作為單項數據發射


## 錯誤處理

這些操作符用於從錯誤通知中恢復

* [`Catch`](operators/Catch.md) — 捕獲，繼續序列操作，將錯誤替換為正常的數據，從onError通知中恢復
* [`Retry`](operators/Retry.md) — 重試，如果Observable發射了一個錯誤通知，重新訂閱它，期待它正常終止

## 輔助操作

一組用於處理Observable的操作符

* [`Delay`](operators/Delay.md) — 延遲一段時間發射結果數據
* [`Do`](operators/Do.md) — 註冊一個動作佔用一些Observable的生命週期事件，相當於Mock某個操作
* [`Materialize/Dematerialize`](operators/Materialize.md) — 將發射的數據和通知都當做數據發射，或者反過來
* [`ObserveOn`](operators/ObserveOn.md) — 指定觀察者觀察Observable的調度程序（工作線程）
* [`Serialize`](operators/Serialize.md) — 強制Observable按次序發射數據並且功能是有效的
* [`Subscribe`](operators/Subscribe.md) — 收到Observable發射的數據和通知後執行的操作
* [`SubscribeOn`](operators/SubscribeOn.md) — 指定Observable應該在哪個調度程序上執行
* [`TimeInterval`](operators/TimeInterval.md) — 將一個Observable轉換為發射兩個數據之間所耗費時間的Observable
* [`Timeout`](operators/Timeout.md) — 添加超時機制，如果過了指定的一段時間沒有發射數據，就發射一個錯誤通知
* [`Timestamp`](operators/Timestamp.md) — 給Observable發射的每個數據項添加一個時間戳
* [`Using`](operators/Using.md) — 創建一個只在Observable的生命週期內存在的一次性資源

## 條件和布爾操作

這些操作符可用於單個或多個數據項，也可用於Observable

* [`All`](operators/Conditional.md#All) — 判斷Observable發射的所有的數據項是否都滿足某個條件
* [`Amb`](operators/Conditional.md#Amb) — 給定多個Observable，只讓第一個發射數據的Observable發射全部數據
* [`Contains`](operators/Conditional.md#Contains) — 判斷Observable是否會發射一個指定的數據項
* [`DefaultIfEmpty`](operators/Conditional.md#DefaultIfEmpty) — 發射來自原始Observable的數據，如果原始Observable沒有發射數據，就發射一個默認數據
* [`SequenceEqual`](operators/Conditional.md#SequenceEqual) — 判斷兩個Observable是否按相同的數據序列
* [`SkipUntil`](operators/Conditional.md#SkipUntil) — 丟棄原始Observable發射的數據，直到第二個Observable發射了一個數據，然後發射原始Observable的剩餘數據
* [`SkipWhile`](operators/Conditional.md#SkipWhile) — 丟棄原始Observable發射的數據，直到一個特定的條件為假，然後發射原始Observable剩餘的數據
* [`TakeUntil`](operators/Conditional.md#TakeUntil) — 發射來自原始Observable的數據，直到第二個Observable發射了一個數據或一個通知
* [`TakeWhile`](operators/Conditional.md#TakeWhile) — 發射原始Observable的數據，直到一個特定的條件為真，然後跳過剩餘的數據

## 算術和聚合操作

這些操作符可用於整個數據序列

* [`Average`](operators/Mathematical.md#Average) — 計算Observable發射的數據序列的平均值，然後發射這個結果
* [`Concat`](operators/Mathematical.md#Concat) — 不交錯的連接多個Observable的數據
* [`Count`](operators/Mathematical.md#Count) — 計算Observable發射的數據個數，然後發射這個結果
* [`Max`](operators/Mathematical.md#Max) — 計算並發射數據序列的最大值
* [`Min`](operators/Mathematical.md#Min) — 計算並發射數據序列的最小值
* [`Reduce`](operators/Mathematical.md#Reduce) — 按順序對數據序列的每一個應用某個函數，然後返回這個值
* [`Sum`](operators/Mathematical.md#Sum) — 計算並發射數據序列的和

## 連接操作

一些有精確可控的訂閱行為的特殊Observable

* [`Connect`](operators/Connect.md) — 指示一個可連接的Observable開始發射數據給訂閱者
* [`Publish`](operators/Publish.md) — 將一個普通的Observable轉換為可連接的
* [`RefCount`](operators/RefCount.md) — 使一個可連接的Observable表現得像一個普通的Observable
* [`Replay`](operators/Replay.md) — 確保所有的觀察者收到同樣的數據序列，即使他們在Observable開始發射數據之後才訂閱

## 轉換操作

* [`To`](operators/To.md) — 將Observable轉換為其它的物件或數據結構
* [`Blocking`](operators/Blocking-Observable-Operators.md) 阻塞Observable的操作符

## 操作符決策樹

幾種主要的需求

* 直接創建一個Observable（創建操作）
* 組合多個Observable（組合操作）
* 對Observable發射的數據執行變換操作（變換操作）
* 從Observable發射的數據中取特定的值（過濾操作）
* 轉發Observable的部分值（條件/布爾/過濾操作）
* 對Observable發射的數據序列求值（算術/聚合操作）
