# ReactiveX 

> http://reactivex.io/intro.html

## Rx介紹

### ReactiveX的歷史

ReactiveX是Reactive Extensions的縮寫，一般簡寫為Rx，最初是LINQ的一個擴展，由微軟的架構師Erik Meijer領導的團隊開發，在2012年11月開源，Rx是一個編程模型，目標是提供一致的編程接口，幫助開發者更方便的處理異步數據流，Rx庫支持.NET、JavaScript和C++，Rx近幾年越來越流行了，現在已經支持幾乎全部的流行編程語言了，Rx的大部分語言庫由ReactiveX這個組織負責維護，比較流行的有RxJava/RxJS/Rx.NET，社區網站是 [reactivex.io](http://reactivex.io/)。

### 什麼是ReactiveX

微軟給的定義是，Rx是一個函數庫，讓開發者可以利用可觀察序列和LINQ風格查詢操作符來編寫異步和基於事件的程序，使用Rx，開發者可以用Observables表示異步數據流，用LINQ操作符查詢異步數據流， 用Schedulers參數化異步數據流的並發處理，Rx可以這樣定義：Rx = Observables + LINQ + Schedulers。

ReactiveX.io給的定義是，Rx是一個使用可觀察數據流進行異步編程的編程接口，ReactiveX結合了觀察者模式、迭代器模式和函數式編程的精華。

### ReactiveX的應用

很多公司都在使用ReactiveX，例如Microsoft、Netflix、Github、Trello、SoundCloud。

### ReactiveX宣言

ReactiveX不僅僅是一個編程接口，它是一種編程思想的突破，它影響了許多其它的程序庫和框架以及編程語言。

## Rx模式

### 使用觀察者模式

* 創建：Rx可以方便的創建事件流和數據流
* 組合：Rx使用查詢式的操作符組合和變換數據流
* 監聽：Rx可以訂閱任何可觀察的數據流並執行操作

### 簡化代碼

* 函數式風格：對可觀察數據流使用無副作用的輸入輸出函數，避免了程序裡錯綜複雜的狀態
* 簡化代碼：Rx的操作符通通常可以將複雜的難題簡化為很少的幾行代碼
* 異步錯誤處理：傳統的try/catch沒辦法處理異步計算，Rx提供了合適的錯誤處理機制
* 輕鬆使用並發：Rx的Observables和Schedulers讓開發者可以擺脫底層的線程同步和各種並發問題

### 使用Observable的優勢

Rx擴展了觀察者模式用於支持數據和事件序列，添加了一些操作符，它讓你可以聲明式的組合這些序列，而無需關注底層的實現：如線程、同步、線程安全、並發數據結構和非阻塞IO。

Observable通過使用最佳的方式訪問異步數據序列填補了這個間隙

|     |        單個數據       |         多個數據         |
|-----|---------------------|-------------------------|
| 同步 | `T getData()`         | `Iterable<T> getData()`   |
| 異步 | `Future<T> getData()` | `Observable<T> getData()` |

Rx的Observable模型讓你可以像使用集合數據一樣操作異步事件流，對異步事件流使用各種簡單、可組合的操作。

#### Observable可組合

對於單層的異步操作來說，Java中Future物件的處理方式是非常簡單有效的，但是一旦涉及到嵌套，它們就開始變得異常繁瑣和複雜。使用Future很難很好的組合帶條件的異步執行流程（考慮到運行時各種潛在的問題，甚至可以說是不可能的），當然，要想實現還是可以做到的，但是非常困難，或許你可以用`Future.get()`，但這樣做，異步執行的優勢就完全沒有了。從另一方面說，Rx的Observable一開始就是為組合異步數據流準備的。

#### Observable更靈活

Rx的Observable不僅支持處理單獨的標量值（就像Future可以做的），也支持數據序列，甚至是無窮的數據流。`Observable`是一個抽象概念，適用於任何場景。Observable擁有它的近親Iterable的全部優雅與靈活。

Observable是異步的雙向push，Iterable是同步的單向pull，對比：


|   事件  |  Iterable(pull)  |  Observable(push)  |
|---------|------------------|-------------------|
| 獲取數據 | `T next()`         | `onNext(T)`          |
| 異常處理 | throws `Exception` | `onError(Exception)` |
| 任務完成 | `!hasNext()`       | `onCompleted()`      |

#### Observable無偏見

Rx對於對於並發性或異步性沒有任何特殊的偏好，Observable可以用任何方式實現，線程池、事件循環、非阻塞IO、Actor模式，任何滿足你的需求的，你擅長或偏好的方式都可以。無論你選擇怎樣實現它，無論底層實現是阻塞的還是非阻塞的，客戶端代碼將所有與Observable的交互都當做是異步的。

**Observable是如何實現的？**

```java
public Observable<data> getData();
```

* 它能與調用者在同一線程同步執行嗎？
* 它能異步地在單獨的線程執行嗎？
* 它會將工作分發到多個線程，返回數據的順序是任意的嗎？
* 它使用Actor模式而不是線程池嗎？
* 它使用NIO和事件循環執行異步網絡訪問嗎？
* 它使用事件循環將工作線程從回調線程分離出來嗎？

從Observer的視角看，這些都無所謂，重要的是：使用Rx，你可以改變你的觀念，你可以在完全不影響Observable程序庫使用者的情況下，徹底的改變Observable的底層實現。

#### 使用回調存在很多問題

回調在不阻塞任何事情的情況下，解決了`Future.get()`過早阻塞的問題。由於響應結果一旦就緒Callback就會被調用，它們天生就是高效率的。不過，就像使用Future一樣，對於單層的異步執行來說，回調很容易使用，對於嵌套的異步組合，它們顯得非常笨拙。

#### Rx是一個多語言的實現

Rx在大量的編程語言中都有實現，並尊重實現語言的風格，而且更多的實現正在飛速增加。

#### 響應式編程

Rx提供了一系列的操作符，你可以使用它們來過濾(filter)、選擇(select)、變換(transform)、結合(combine)和組合(compose)多個Observable，這些操作符讓執行和復合變得非常高效。

你可以把Observable當做Iterable的推送方式的等價物，使用Iterable，消費者從生產者那拉取數據，線程阻塞直至數據準備好。使用Observable，在數據準備好時，生產者將數據推送給消費者。數據可以同步或異步的到達，這種方式更靈活。

下面的例子展示了相似的高階函數在Iterable和Observable上的應用

```java
// Iterable
getDataFromLocalMemory()
  .skip(10)
  .take(5)
  .map({ s -> return s + " transformed" })
  .forEach({ println "next => " + it })

// Observable
getDataFromNetwork()
  .skip(10)
  .take(5)
  .map({ s -> return s + " transformed" })
  .subscribe({ println "onNext => " + it })
```

Observable類型給GOF的觀察者模式添加了兩種缺少的語義，這樣就和Iterable類型中可用的操作一致了：

1. 生產者可以發信號給消費者，通知它沒有更多數據可用了（對於Iterable，一個for循環正常完成表示沒有數據了；對於Observable，就是調用觀察者的`onCompleted`方法）
2. 生產者可以發信號給消費者，通知它遇到了一個錯誤（對於Iterable，迭代過程中發生錯誤會拋出異常；對於Observable，就是調用觀察者(Observer)的`onError`方法）

有了這兩種功能，Rx就能使Observable與Iterable保持一致了，唯一的不同是數據流的方向。任何對Iterable的操作，你都可以對Observable使用。

## 名詞定義

這裡給出一些名詞的翻譯

* Reactive 直譯為反應性的，有活性的，根據上下文一般翻譯為反應式、響應式
* Iterable 可迭代物件，支持以迭代器的形式遍歷，許多語言中都存在這個概念
* Observable 可觀察物件，在Rx中定義為更強大的Iterable，在觀察者模式中是被觀察的物件，一旦數據產生或發生變化，會通過某種方式通知觀察者或訂閱者
* Observer 觀察者物件，監聽Observable發射的數據並做出響應，Subscriber是它的一個特殊實現
* emit 直譯為發射，發佈，發出，含義是Observable在數據產生或變化時發送通知給Observer，調用Observer對應的方法，文章裡一律譯為發射
* items 直譯為項目，條目，在Rx裡是指Observable發射的數據項，文章裡一律譯為數據，數據項
