[learnrx](http://reactivex.io/learnrx/)

##2. Notice that forEach lets us specify what we want to happen to each item in the array, but hides how the array is traversed. 

```js
function(console) {
	var names = ["Ben", "Jafar", "Matt", "Priya", "Brian"];

	names.forEach((name)=>console.log(name));
}
		
```
##3.Projecting Arrays

Applying a function to a value and creating a new value is called a projection. To project one array into another, we apply a projection function to each item in the array and collect the results in a new array.
對一個值應用函數並產生另一個值稱為投射(或預估)，為了把一個陣列投射到另一個陣列，我們在一個陣列中對每一項應用投射函數，並在一個新的陣列中收集結果。
Exercise 3: Project an array of videos into an array of {id,title} pairs using forEach()

For each video, add a projected {id, title} pair to the videoAndTitlePairs array.

```js
    videoAndTitlePairs = []; //新陣列，用以收集結果。
	newReleases.forEach((video) => //對每一項，進行投射函數
		videoAndTitlePairs.push({ id: video.id, title: video.title })
	);
```
##4.Exercise 4: Implement map()

To make projections easier, let's add a map() function to the Array type. Map accepts the projection function to be applied to each item in the source array, and returns the projected array. 

```js
Array.prototype.map = function(projectionFunction) { // 這裡表示map()等於什麼事。
	var results = [];
	this.forEach(function(itemInArray) {
		results.push(projectionFunction(itemInArray));

	});

	return results;
};

// JSON.stringify([1,2,3].map(function(x) { return x + 1; })) === '[2,3,4]' //這裡表示map()發生了什麼事
```
##5.Exercise 5: Use map() to project an array of videos into an array of {id,title} pairs

Let's repeat the exercise of collecting {id, title} pairs for each video in the newReleases array, but this time we'll use our map function. 

```js
  return newReleases.map((video) => { return { id: video.id, title: video.title }; });

```
##6.Filtering Arrays

Like projection, filtering an array is also a very common operation. To filter an array we apply a test to each item in the array and collect the items that pass into a new array.
就像投射函數，過濾一個陣列也是一個非常普通的操作，我們對陣列中的每一項進行測試並且收集通過測試的項到一個新的陣列，以完成過濾。
Exercise 6: Use forEach() to collect only those videos with a rating of 5.0 使用forEach()收集rating為5.0的video。

Use forEach() to loop through the videos in the newReleases array and, if a video has a rating of 5.0, add it to the videos array. 

```js
	videos = [];

	newReleases.forEach(video => {
		if (video.rating == 5.0) {//這裡要二個或三個等號
			videos.push(video);
		}
	});

	return videos;
```
##7.Exercise 7: Implement filter()

To make filtering easier, let's add a filter() function to the Array type. The filter() function accepts a predicate. A predicate is a function that accepts an item in the array, and returns a boolean indicating whether the item should be retained in the new array.

為了使過濾操作更簡單些，讓我們為陣列型別加上filter(), filter()接受一個預測，一個預測是一個函數，其接受陣列中的一個項，並且返回一個布林值指出該項是否應留在新陣列中。

```js
Array.prototype.filter = function(predicateFunction) {
	var results = [];
	this.forEach(function(itemInArray) {
	  if (predicateFunction(itemInArray)) {
		results.push(itemInArray);
	  }
	});

	return results;
};

// JSON.stringify([1,2,3].filter(function(x) { return x > 2})) === "[3]"
```
##8.Query Data by Chaining Method Calls
Exercise 8: Chain filter and map to collect the ids of videos that have a rating of 5.0

```js

	return newReleases.
		filter(function(video) {
			return video.rating === 5.0;
		}).
		map(function(video) {
			return video.id;
		});
```
##9.Querying Trees

Sometimes, in addition to flat arrays, we need to query trees. Trees pose a challenge because we need to flatten them into arrays in order to apply filter() and map() operations on them. In this section we'll define a concatAll() function that we can combine with map() and filter() to query trees.
有時，為了額外平面化陣列，我們需要成為查詢樹，樹是個挑戰，因為我們需要把它們平面它們使成為陣列，這樣才能使用filter()與map()操作，此處我們將定義一個concatAll()函數(把它們全串成一起的函數)，然後我們可能對一個查詢樹使用filter()與map()

Exercise 9: Flatten the movieLists array into an array of video ids

Let's start by using two nested forEach loops to collect the id of every video in the two-dimensional movieLists array.
讓我們使用2個巢狀的forEach迴圈，以收集在二維陣列中的id。

```js
allVideoIdsInMovieLists = [];

	movieLists.forEach(x => x.videos.forEach(y=>allVideoIdsInMovieLists.push(y.id))); //要先指向videos再forEach
	

	return allVideoIdsInMovieLists;
```

##10.Flattening trees with nested forEach expressions is easy because we can explicitly add items to the array. Unfortunately it's exactly this type of low-level operation that we've been trying to abstract away with functions like map() and filter(). Can we define a function that's abstract enough to express our intent to flatten a tree, without specifying too much information about how to carry out the operation?
使用巢狀forEach表達句來平面化一個樹是容易的，因為我們可明確地加一個項到一個陣列，不幸是這是低階的操作，我們已經試過像map()和filter()的抽像的方法，是否我們可以定義一個夠抽象的方法來表達我們的意圖，以平面化一個樹，而不用指定太多有關於如何執行這項操作的資訊。

Exercise 10: Implement concatAll()

Let's add a concatAll() function to the Array type. The concatAll() function iterates over each sub-array in the array and collects the results in a new, flat array. Notice that the concatAll() function expects each item in the array to be another array. 
讓我加入一個concatAll()函數到陣列型態，concatAll()函數迭化過陣列中每一個次陣列，並收集結果到一個新的平面的陣列。

```js
    var results = [];
    this.forEach(
		subArray=>subArray.forEach(x=>results.push(x))
	);//我的答案是過關的，是使用二個forEach

    var results = [];
	this.forEach(function(subArray) {
		results.push.apply(results, subArray);
	});

	return results;//官方的答案，它使用array.push.apply(array, subArray)

// JSON.stringify([ [1,2,3], [4,5,6], [7,8,9] ].concatAll()) === "[1,2,3,4,5,6,7,8,9]"
// [1,2,3].concatAll(); // throws an error because this is a one-dimensional array
```
##11.concatAll is a very simple function, so much so that it may not be obvious yet how it can be combined with map() to query a tree. Let's try an example...
concatAll是一個非常簡單的函數，以致於它可能不那明顯地，可與map()合併,以對一個樹查詢化。讓我們試一個例子
Exercise 11: Use map() and concatAll() to project and flatten the movieLists into an array of video ids

Hint: use two nested calls to map() and one call to concatAll().
提示:使用2 個巢狀的map()和一個concatAll()。

```js
	return movieLists.map(x=>x.videos.map(y=>y.id)).concatAll();//我參考解答寫的答案。

    return movieLists.
	  map(function(movieList) {
		return movieList.videos.map(function(video) {
			return video.id;
		  });
	  }).
	  concatAll();//解答

```
##12.Wow! Great work. Mastering the combination of map() and concatAll() is key to effective functional programming. You're half way there! Let's try a more complicated example...
完成以上等於到一半了。
Exercise 12: Retrieve id, title, and a 150x200 box art url for every video
從每一個video取回id,title與url
You've managed to flatten a tree that's two levels deep, let's try for three! Let's say that instead of a single boxart url on each video, we had a collection of boxart objects, each with a different size and url. Create a query that selects {id, title, boxart} for every video in the movieLists. This time though, the boxart property in the result will be the url of the boxart object with dimensions of 150x200px. Let's see if you can solve this problem with map(), concatAll(), and filter().
你已經對二階的樹作過平面化，讓我們試三階樹。
There's just more one thing: you can't use indexers. In other words, this is illegal:
