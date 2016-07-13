Single
======

### 介紹

RxJava（以及它派生出來的RxGroovy和RxScala）中有一個名為**Single**的Observable變種。

Single類似於Observable，不同的是，它總是只發射一個值，或者一個錯誤通知，而不是發射一系列的值。

因此，不同於Observable需要三個方法onNext, onError, onCompleted，訂閱Single只需要兩個方法：

* onSuccess - Single發射單個的值到這個方法
* onError - 如果無法發射需要的值，Single發射一個Throwable物件到這個方法

Single只會調用這兩個方法中的一個，而且只會調用一次，調用了任何一個方法之後，訂閱關係終止。

### Single的操作符

Single也可以組合使用多種操作，一些操作符讓你可以混合使用Observable和Single：

操作符 | 返回值 | 說明
------| ------|-----
compose | Single |  創建一個自定義的操作符
concat and concatWith   | Observable |  連接多個Single和Observable發射的數據
create  | Single |  調用觀察者的create方法創建一個Single
error   | Single |  返回一個立即給訂閱者發射錯誤通知的Single
flatMap | Single |  返回一個Single，它發射對原Single的數據執行flatMap操作後的結果
flatMapObservable   | Observable |  返回一個Observable，它發射對原Single的數據執行flatMap操作後的結果
from    | Single |  將Future轉換成Single
just    | Single |  返回一個發射一個指定值的Single
map | Single |  返回一個Single，它發射對原Single的數據執行map操作後的結果
merge   | Single |  將一個Single(它發射的數據是另一個Single，假設為B)轉換成另一個Single(它發射來自另一個Single(B)的數據)
merge and mergeWith | Observable |  合併發射來自多個Single的數據
observeOn   | Single |  指示Single在指定的調度程序上調用訂閱者的方法
onErrorReturn   | Single |  將一個發射錯誤通知的Single轉換成一個發射指定數據項的Single
subscribeOn | Single |  指示Single在指定的調度程序上執行操作
timeout | Single |  它給原有的Single添加超時控制，如果超時了就發射一個錯誤通知
toSingle    | Single |  將一個發射單個值的Observable轉換為一個Single
zip and zipWith | Single |  將多個Single轉換為一個，後者發射的數據是對前者應用一個函數後的結果

### 操作符圖示

詳細的圖解可以參考英文文檔：[Single](http://reactivex.io/documentation/single.html)
