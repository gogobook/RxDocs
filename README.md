# ReactiveX/RxJava文檔中文版

**項目地址：<https://github.com/mcxiaoke/RxDocs>，歡迎Star和幫忙改進。**

有任何意見或建議，到這裡提出 [Create New Issue](https://github.com/mcxiaoke/RxDocs/issues/new)

## 閱讀地址

* [ReactiveX文檔中文翻譯](http://mcxiaoke.gitbooks.io/rxdocs/content/)
* [PDF/ePub/Mobi格式下載](https://www.gitbook.com/book/mcxiaoke/rxdocs/details)

## 說明

* 大部分是翻譯自 [ReactiveX.io](http://reactivex.io/) 和 [RxJava Wiki](https://github.com/ReactiveX/rxjava/wiki)，修正了原文的一些錯誤，補充了詳細的說明和示例

## 版本歷史

* 0.9.5 - 2016.03.14 文本修正和潤色，感謝@htoooth/@AlanCheen/@Ydcool/@loshine/@ppoffice幫忙完善
* 0.9.0 - 2015.11.25 修正錯誤和文本潤色，感謝@slb1988/@htoooth/@jiyee/@donglua幫忙完善
* 0.8.0 - 2015.07.27 完成全部文檔的初步審校，修正了部分用詞不當的地方
* 0.7.0 - 2015.07.24 完成全部文檔的初譯，調整了目錄的部分連結和文本
* 0.6.0 - 2015.07.22 完成絕大部分文檔的翻譯，使用GitBook發佈初始版本

## 目錄

* [ReactiveX](Intro.md) - 什麼是Rx，Rx的理念和優勢
* [Observables](Observables.md) - 簡要介紹Observable的觀察者模型
* [Single](Single.md) - 一種特殊的只發射單個值的Observable
* [Subject](Subject.md) - Observable和Observer的複合體，也是二者的橋樑
* [Scheduler](Scheduler.md) - 介紹了各種異步任務調度和默認調度器
* [All Operators List](All-Operators-List.md) - 按字母順序的全部操作符列表
* [Operators Categories](Operators.md) - 按目錄分類的主要操作符列表
  * [Creating 創建操作](operators/Creating-Observables.md) - `Create`/`Defer`/`From`/`Just`/`Start`/`Repeat`/`Range`
  * [Transforming 變換操作](operators/Transforming-Observables.md) - `Buffer`/`Window`/`Map`/`FlatMap`/`GroupBy`/`Scan`
  * [Filtering 過濾操作](operators/Filtering-Observables.md) - `Debounce`/`Distinct`/`Filter`/`Sample`/`Skip`/`Take`
  * [Combining 結合操作](operators/Combining-Observables.md) - `And`/`StartWith`/`Join`/`Merge`/`Switch`/`Zip`
  * [Error Handling 錯誤處理](operators/Error-Handling-Operators.md) - `Catch`/`Retry`
  * [Utility 輔助操作](operators/Observable-Utility-Operators.md) - `Delay`/`Do`/`ObserveOn`/`SubscribeOn`/`Subscribe`
  * [Conditional 條件和布爾操作](operators/Conditional-and-Boolean-Operators.md) - `All`/`Amb`/`Contains`/`SkipUntil`/`TakeUntil`
  * [Mathematical 算術和聚合操作](operators/Mathematical-and-Aggregate-Operators.md) - `Average`/`Concat`/`Count`/`Max`/`Min`/`Sum`/`Reduce`
  * [Async 異步操作](operators/Async-Operators.md) - `Start`/`ToAsync`/`StartFuture`/`FromAction`/`FromCallable`/`RunAsync`
  * [Connect 連接操作](operators/Connectable-Observable-Operators.md) - `Connect`/`Publish`/`RefCount`/`Replay`
  * [Convert 轉換操作](operators/To.md) - `ToFuture`/`ToList`/`ToIterable`/`ToMap`/`toMultiMap`
  * [Blocking 阻塞操作](operators/Blocking-Observable-Operators.md) - `ForEach`/`First`/`Last`/`MostRecent`/`Next`/`Single`/`Latest`
  * [String 字符串操作](operators/String-Observables.md) - `ByLine`/`Decode`/`Encode`/`From`/`Join`/`Split`/`StringConcat`
* [RxJava文檔和教程](Topics.md)
  * [RxJava入門指南](topics/Getting-Started.md)
  * [RxJava使用示例](topics/How-To-Use-RxJava.md)
  * [實現自定義操作符](topics/Implementing-Your-Own-Operators.md)
  * [自定義插件](topics/Plugins.md)
  * [Backpressure](topics/Backpressure.md)
  * [錯誤處理](topics/Error-Handling.md)
  * [Android模組](topics/The-RxJava-Android-Module.md)
  * [參與開發](topics/How-to-Contribute.md)
  * [補充閱讀材料](topics/Additional-Reading.md)


## 連結

* [ReactiveX.io](http://reactivex.io/intro.html)
* [RxJava Wiki](https://github.com/ReactiveX/RxJava/wiki)
* [RxJava Javadoc](http://reactivex.io/RxJava/javadoc/)
* [RxMarbles](http://rxmarbles.com/)
* [Advanced RxJava](http://akarnokd.blogspot.com/)
* [Intro to Rx](http://www.introtorx.com/content/v1.0.10621.0/01_WhyRx.html)
* [MSDN](https://msdn.microsoft.com/en-us/data/gg577609.aspx)


------

## 許可協議

- [署名-非商業性使用-相同方式共享 4.0 國際](https://creativecommons.org/licenses/by-nc-sa/4.0/legalcode)

------

## 聯繫方式

* Blog: <http://blog.mcxiaoke.com>
* Github: <https://github.com/mcxiaoke>
* Email: [github@mcxiaoke.com](mailto:github#mcxiaoke.com)

## 開源項目

* Rx文檔中文翻譯: <https://github.com/mcxiaoke/RxDocs>
* MQTT協議中文版: <https://github.com/mcxiaoke/mqtt>
* Awesome-Kotlin: <https://github.com/mcxiaoke/awesome-kotlin>
* Kotlin-Koi: <https://github.com/mcxiaoke/kotlin-koi>
* Next公共組件庫: <https://github.com/mcxiaoke/Android-Next>
* PackerNg極速打包: <https://github.com/mcxiaoke/packer-ng-plugin>
* Gradle渠道打包: <https://github.com/mcxiaoke/gradle-packer-plugin>
* EventBus實現xBus: <https://github.com/mcxiaoke/xBus>
* 蘑菇飯App: <https://github.com/mcxiaoke/minicat>
* 飯否客戶端: <https://github.com/mcxiaoke/fanfouapp-opensource>
* Volley鏡像: <https://github.com/mcxiaoke/android-volley>

------
