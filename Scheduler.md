# 調度器 Scheduler

如果你想給Observable操作符鏈添加多線程功能，你可以指定操作符（或者特定的Observable）在特定的調度器(Scheduler)上執行。

某些ReactiveX的Observable操作符有一些變體，它們可以接受一個Scheduler參數。這個參數指定操作符將它們的部分或全部任務放在一個特定的調度器上執行。

使用ObserveOn和SubscribeOn操作符，你可以讓Observable在一個特定的調度器上執行，ObserveOn指示一個Observable在一個特定的調度器上調用觀察者的onNext, onError和onCompleted方法，SubscribeOn更進一步，它指示Observable將全部的處理過程（包括發射數據和通知）放在特定的調度器上執行。

## RxJava示例

### 調度器的種類

下表展示了RxJava中可用的調度器種類：

調度器類型 | 效果
------|------
Schedulers.computation( )	 | 用於計算任務，如事件循環或和回調處理，不要用於IO操作(IO操作請使用Schedulers.io())；默認線程數等於處理器的數量
Schedulers.from(executor)	 | 使用指定的Executor作為調度器
Schedulers.immediate( )	 | 在當前線程立即開始執行任務
Schedulers.io( )	 | 用於IO密集型任務，如異步阻塞IO操作，這個調度器的線程池會根據需要增長；對於普通的計算任務，請使用Schedulers.computation()；Schedulers.io( )默認是一個CachedThreadScheduler，很像一個有線程緩存的新線程調度器
Schedulers.newThread( )	 | 為每個任務創建一個新線程
Schedulers.trampoline( )	 | 當其它排隊的任務完成後，在當前線程排隊開始執行

### 默認調度器

在RxJava中，某些Observable操作符的變體允許你設置用於操作執行的調度器，其它的則不在任何特定的調度器上執行，或者在一個指定的默認調度器上執行。下面的表格個列出了一些操作符的默認調度器：

操作符 | 調度器
------|------
buffer(timespan)	 | computation
buffer(timespan, count)	 | computation
buffer(timespan, timeshift)	 | computation
debounce(timeout, unit)	 | computation
delay(delay, unit)	 | computation
delaySubscription(delay, unit)	 | computation
interval	 | computation
repeat	 | trampoline
replay(time, unit)	 | computation
replay(buffersize, time, unit)	 | computation
replay(selector, time, unit)	 | computation
replay(selector, buffersize, time, unit)	 | computation
retry	 | trampoline
sample(period, unit)	 | computation
skip(time, unit)	 | computation
skipLast(time, unit)	 | computation
take(time, unit)	 | computation
takeLast(time, unit)	 | computation
takeLast(count, time, unit)	 | computation
takeLastBuffer(time, unit)	 | computation
takeLastBuffer(count, time, unit) | 	computation
throttleFirst	 | computation
throttleLast	 | computation
throttleWithTimeout	 | computation
timeInterval	 | immediate
timeout(timeoutSelector)	 | immediate
timeout(firstTimeoutSelector, timeoutSelector)	 | immediate
timeout(timeoutSelector, other)	 | immediate
timeout(timeout, timeUnit)	 | computation
timeout(firstTimeoutSelector, timeoutSelector, other)	 | immediate
timeout(timeout, timeUnit, other)	 | computation
timer	 | computation
timestamp	 | immediate
window(timespan)	 | computation
window(timespan, count)	 | computation
window(timespan, timeshift)	 | computation


### 使用調度器

除了將這些調度器傳遞給RxJava的Observable操作符，你也可以用它們調度你自己的任務。下面的示例展示了Scheduler.Worker的用法：

```java

worker = Schedulers.newThread().createWorker();
worker.schedule(new Action0() {

    @Override
    public void call() {
        yourWork();
    }

});
// some time later...
worker.unsubscribe();

```

#### 遞歸調度器

要調度遞歸的方法調用，你可以使用schedule，然後再用schedule(this)，示例：

```java

worker = Schedulers.newThread().createWorker();
worker.schedule(new Action0() {

    @Override
    public void call() {
        yourWork();
        // recurse until unsubscribed (schedule will do nothing if unsubscribed)
        worker.schedule(this);
    }

});
// some time later...
worker.unsubscribe();

```

#### 檢查或設置取消訂閱狀態

Worker類的物件實現了Subscription接口，使用它的isUnsubscribed和unsubscribe方法，所以你可以在訂閱取消時停止任務，或者從正在調度的任務內部取消訂閱，示例：

```java

Worker worker = Schedulers.newThread().createWorker();
Subscription mySubscription = worker.schedule(new Action0() {

    @Override
    public void call() {
        while(!worker.isUnsubscribed()) {
            status = yourWork();
            if(QUIT == status) { worker.unsubscribe(); }
        }
    }

});

```

Worker同時是Subscription，因此你可以（通常也應該）調用它的unsubscribe方法通知可以掛起任務和釋放資源了。

#### 延時和週期調度器

你可以使用schedule(action,delayTime,timeUnit)在指定的調度器上延時執行你的任務，下面例子中的任務將在500毫秒之後開始執行：

```java
someScheduler.schedule(someAction, 500, TimeUnit.MILLISECONDS);
```

使用另一個版本的schedule，schedulePeriodically(action,initialDelay,period,timeUnit)方法讓你可以安排一個定期執行的任務，下面例子的任務將在500毫秒之後執行，然後每250毫秒執行一次：

```java
someScheduler.schedulePeriodically(someAction, 500, 250, TimeUnit.MILLISECONDS);
```

### 測試調度器

TestScheduler讓你可以對調度器的時鐘表現進行手動微調。這對依賴精確時間安排的任務的測試很有用處。這個調度器有三個額外的方法：

* advanceTimeTo(time,unit) 向前波動調度器的時鐘到一個指定的時間點
* advanceTimeBy(time,unit) 將調度器的時鐘向前撥動一個指定的時間段
* triggerActions( ) 開始執行任何計劃中的但是未啟動的任務，如果它們的計劃時間等於或者早於調度器時鐘的當前時間
