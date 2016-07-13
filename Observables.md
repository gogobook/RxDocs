# Observable

## 概述

在ReactiveX中，一個觀察者(Observer)訂閱一個可觀察物件(Observable)。觀察者對Observable發射的數據或數據序列作出響應。這種模式可以極大地簡化並發操作，因為它創建了一個處於待命狀態的觀察者哨兵，在未來某個時刻響應Observable的通知，不需要阻塞等待Observable發射數據。

這篇文章會解釋什麼是響應式編程模式(reactive pattern)，以及什麼是可觀察物件(Observables)和觀察者(observers)，其它幾篇文章會展示如何用操作符組合和改變Observable的行為。

![Observable](images/legend.png)

#### 相關參考：

* [Single](Single.md) - 一個特殊的Observable，只發射單個數據。


## 背景知識

在很多軟件編程任務中，或多或少你都會期望你寫的代碼能按照編寫的順序，一次一個的順序執行和完成。但是在ReactiveX中，很多指令可能是並行執行的，之後他們的執行結果才會被觀察者捕獲，順序是不確定的。為達到這個目的，你定義一種獲取和變換數據的機制，而不是調用一個方法。在這種機制下，存在一個可觀察物件(Observable)，觀察者(Observer)訂閱(Subscribe)它，當數據就緒時，之前定義的機制就會分發數據給一直處於等待狀態的觀察者哨兵。

這種方法的優點是，如果你有大量的任務要處理，它們互相之間沒有依賴關係。你可以同時開始執行它們，不用等待一個完成再開始下一個（用這種方式，你的整個任務隊列能耗費的最長時間，不會超過任務裡最耗時的那個）。

有很多術語可用於描述這種異步編程和設計模式，在在本文裡我們使用這些術語：**一個觀察者訂閱一個可觀察物件** (*An observer subscribes to an Observable*)。通過調用觀察者的方法，Observable發射數據或通知給它的觀察者。

在其它的文檔和場景裡，有時我們也將**Observer**叫做*Subscriber*、*Watcher*、*Reactor*。這個模型通常被稱作*Reactor模式*。


## 創建觀察者

本文使用類似於Groovy的偽代碼舉例，但是ReactiveX有多種語言的實現。

普通的方法調用（不是某種異步方法，也不是Rx中的並行調用），流程通常是這樣的：

1. 調用某一個方法
2. 用一個變量保存方法返回的結果
3. 使用這個變量和它的新值做些有用的事

用代碼描述就是：

```groovy
// make the call, assign its return value to `returnVal`
returnVal = someMethod(itsParameters);
// do something useful with returnVal
```

在異步模型中流程更像這樣的：

1. 定義一個方法，它完成某些任務，然後從異步調用中返回一個值，這個方法是觀察者的一部分
2. 將這個異步調用本身定義為一個Observable
3. 觀察者通過訂閱(Subscribe)操作關聯到那個Observable
4. 繼續你的業務邏輯，等方法返回時，Observable會發射結果，觀察者的方法會開始處理結果或結果集

用代碼描述就是：

```groovy

// defines, but does not invoke, the Subscriber's onNext handler
// (in this example, the observer is very simple and has only an onNext handler)
def myOnNext = { it -> do something useful with it };
// defines, but does not invoke, the Observable
def myObservable = someObservable(itsParameters);
// subscribes the Subscriber to the Observable, and invokes the Observable
myObservable.subscribe(myOnNext);
// go on about my business

```


### 回調方法 (onNext, onCompleted, onError)

Subscribe方法用於將觀察者連接到Observable，你的觀察者需要實現以下方法的一個子集：

* **onNext(T item)**

    Observable調用這個方法發射數據，方法的參數就是Observable發射的數據，這個方法可能會被調用多次，取決於你的實現。

* **onError(Exception ex)**

    當Observable遇到錯誤或者無法返回期望的數據時會調用這個方法，這個調用會終止Observable，後續不會再調用onNext和onCompleted，onError方法的參數是拋出的異常。

* **onComplete**

    正常終止，如果沒有遇到錯誤，Observable在最後一次調用onNext之後調用此方法。

根據Observable協議的定義，onNext可能會被調用零次或者很多次，最後會有一次onCompleted或onError調用（不會同時），傳遞數據給onNext通常被稱作發射，onCompleted和onError被稱作通知。

下面是一個更完整的例子：

```groovy

def myOnNext     = { item -> /* do something useful with item */ };
def myError      = { throwable -> /* react sensibly to a failed call */ };
def myComplete   = { /* clean up after the final response */ };
def myObservable = someMethod(itsParameters);
myObservable.subscribe(myOnNext, myError, myComplete);
// go on about my business

```

### 取消訂閱 (Unsubscribing)

在一些ReactiveX實現中，有一個特殊的觀察者接口*Subscriber*，它有一個*unsubscribe*方法。調用這個方法表示你不關心當前訂閱的Observable了，因此Observable可以選擇停止發射新的數據項（如果沒有其它觀察者訂閱）。

取消訂閱的結果會傳遞給這個Observable的操作符鏈，而且會導致這個鏈條上的每個環節都停止發射數據項。這些並不保證會立即發生，然而，對一個Observable來說，即使沒有觀察者了，它也可以在一個while循環中繼續生成並嘗試發射數據項。

### 關於命名約定

ReactiveX的每種特定語言的實現都有自己的命名偏好，雖然不同的實現之間有很多共同點，但並不存在一個統一的命名標準。

而且，在某些場景中，一些名字有不同的隱含意義，或者在某些語言看來比較怪異。

例如，有一個*onEvent*命名模式(onNext, onCompleted, onError)，在一些場景中，這些名字可能意味著事件處理器已經註冊。然而在ReactiveX裡，他們是事件處理器的名字。


## Observables的"熱"和"冷"

Observable什麼時候開始發射數據序列？這取決於Observable的實現，一個"熱"的Observable可能一創建完就開始發射數據，因此所有後續訂閱它的觀察者可能從序列中間的某個位置開始接受數據（有一些數據錯過了）。一個"冷"的Observable會一直等待，直到有觀察者訂閱它才開始發射數據，因此這個觀察者可以確保會收到整個數據序列。

在一些ReactiveX實現裡，還存在一種被稱作*Connectable*的Observable，不管有沒有觀察者訂閱它，這種Observable都不會開始發射數據，除非Connect方法被調用。

## 用操作符組合Observable

對於ReactiveX來說，Observable和Observer僅僅是個開始，它們本身不過是標準觀察者模式的一些輕量級擴展，目的是為了更好的處理事件序列。

ReactiveX真正強大的地方在於它的操作符，操作符讓你可以變換、組合、操縱和處理Observable發射的數據。

Rx的操作符讓你可以用聲明式的風格組合異步操作序列，它擁有回調的所有效率優勢，同時又避免了典型的異步系統中嵌套回調的缺點。

下面是常用的操作符列表：

1. [創建操作](operators/Creating-Observables.md) Create, Defer, Empty/Never/Throw, From, Interval, Just, Range, Repeat, Start, Timer
2. [變換操作](operators/Transforming-Observables.md) Buffer, FlatMap, GroupBy, Map, Scan和Window
3. [過濾操作](operators/Filter-Observables.md) Debounce, Distinct, ElementAt, Filter, First, IgnoreElements, Last, Sample, Skip, SkipLast, Take, TakeLast
4. [組合操作](operators/Combining-Observables.md) And/Then/When, CombineLatest, Join, Merge, StartWith, Switch, Zip
5. [錯誤處理](operators/Error-Handling-Observables.md) Catch和Retry
6. [輔助操作](operators/Observable-Utility-Operators.md) Delay, Do, Materialize/Dematerialize, ObserveOn, Serialize, Subscribe, SubscribeOn, TimeInterval, Timeout, Timestamp, Using
7. [條件和布爾操作](operators/Conditional-Observables.md) All, Amb, Contains, DefaultIfEmpty, SequenceEqual, SkipUntil, SkipWhile, TakeUntil, TakeWhile
8. [算術和集合操作](operators/Mathematical-and-Aggregate-Operators.md) Average, Concat, Count, Max, Min, Reduce, Sum
9. [轉換操作](operators/To.md) To
10. [連接操作](operators/Connectable-Observable-Operators.md) Connect, Publish, RefCount, Replay
11. [反壓操作](topics/Backpressure.md)，用於增加特殊的流程控制策略的操作符

這些操作符並不全都是ReactiveX的核心組成部分，有一些是語言特定的實現或可選的模組。

## RxJava

在RxJava中，一個實現了_Observer_接口的物件可以訂閱(_subscribe_)一個_Observable_ 類的實例。訂閱者(subscriber)對Observable發射(_emit_)的任何數據或數據序列作出響應。這種模式簡化了並發操作，因為它不需要阻塞等待Observable發射數據，而是創建了一個處於待命狀態的觀察者哨兵，哨兵在未來某個時刻響應Observable的通知。
