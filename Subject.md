Subject
======

Subject可以看成是一個橋樑或者代理，在某些ReactiveX實現中（如RxJava），它同時充當了Observer和Observable的角色。因為它是一個Observer，它可以訂閱一個或多個Observable；又因為它是一個Observable，它可以轉發它收到(Observe)的數據，也可以發射新的數據。

由於一個Subject訂閱一個Observable，它可以觸發這個Observable開始發射數據（如果那個Observable是"冷"的--就是說，它等待有訂閱才開始發射數據）。因此有這樣的效果，Subject可以把原來那個"冷"的Observable變成"熱"的。

## Subject的種類

針對不同的場景一共有四種類型的Subject。他們並不是在所有的實現中全部都存在，而且一些實現使用其它的命名約定（例如，在RxScala中Subject被稱作PublishSubject）。

### AsyncSubject

一個AsyncSubject只在原始Observable完成後，發射來自原始Observable的最後一個值。（如果原始Observable沒有發射任何值，AsyncObject也不發射任何值）它會把這最後一個值發射給任何後續的觀察者。
![](images/S.AsyncSubject.png)

然而，如果原始的Observable因為發生了錯誤而終止，AsyncSubject將不會發射任何數據，只是簡單的向前傳遞這個錯誤通知。
![](images/S.AsyncSubject.e.png)

### BehaviorSubject

當觀察者訂閱BehaviorSubject時，它開始發射原始Observable最近發射的數據（如果此時還沒有收到任何數據，它會發射一個默認值），然後繼續發射其它任何來自原始Observable的數據。
![](images/S.BehaviorSubject.png)

然而，如果原始的Observable因為發生了一個錯誤而終止，BehaviorSubject將不會發射任何數據，只是簡單的向前傳遞這個錯誤通知。
![](images/S.BehaviorSubject.e.png)

### PublishSubject

PublishSubject只會把在訂閱發生的時間點之後來自原始Observable的數據發射給觀察者。需要注意的是，PublishSubject可能會一創建完成就立刻開始發射數據（除非你可以阻止它發生），因此這裡有一個風險：在Subject被創建後到有觀察者訂閱它之前這個時間段內，一個或多個數據可能會丟失。如果要確保來自原始Observable的所有數據都被分發，你需要這樣做：或者使用Create創建那個Observable以便手動給它引入"冷"Observable的行為（當所有觀察者都已經訂閱時才開始發射數據），或者改用ReplaySubject。
![](images/S.PublishSubject.png)

如果原始的Observable因為發生了一個錯誤而終止，PublishSubject將不會發射任何數據，只是簡單的向前傳遞這個錯誤通知。
![](images/S.PublishSubject.e.png)

### ReplaySubject

ReplaySubject會發射所有來自原始Observable的數據給觀察者，無論它們是何時訂閱的。也有其它版本的ReplaySubject，在重放緩存增長到一定大小的時候或過了一段時間後會丟棄舊的數據（原始Observable發射的）。

如果你把ReplaySubject當作一個觀察者使用，注意不要從多個線程中調用它的onNext方法（包括其它的on系列方法），這可能導致同時（非順序）調用，這會違反Observable協議，給Subject的結果增加了不確定性。

![](images/S.ReplaySubject.png)

### RxJava的對應類

假設你有一個Subject，你想把它傳遞給其它的代理或者暴露它的Subscriber接口，你可以調用它的asObservable方法，這個方法返回一個Observable。具體使用方法可以參考Javadoc文檔。

### 串行化
如果你把 `Subject` 當作一個 `Subscriber` 使用，注意不要從多個線程中調用它的onNext方法（包括其它的on系列方法），這可能導致同時（非順序）調用，這會違反Observable協議，給Subject的結果增加了不確定性。

要避免此類問題，你可以將 `Subject` 轉換為一個 [`SerializedSubject`](http://reactivex.io/RxJava/javadoc/rx/subjects/SerializedSubject.html) ，類似於這樣：

```java
mySafeSubject = new SerializedSubject( myUnsafeSubject );
```
