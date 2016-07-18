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

```js
	// Use one or more map, concatAll, and filter calls to create an array with the following items
	// [
	//	 {"id": 675465,"title": "Fracture","boxart":"http://cdn-0.nflximg.com/images/2891/Fracture150.jpg" },
	//	 {"id": 65432445,"title": "The Chamber","boxart":"http://cdn-0.nflximg.com/images/2891/TheChamber150.jpg" },
	//	 {"id": 654356453,"title": "Bad Boys","boxart":"http://cdn-0.nflximg.com/images/2891/BadBoys150.jpg" },
	//	 {"id": 70111470,"title": "Die Hard","boxart":"http://cdn-0.nflximg.com/images/2891/DieHard150.jpg" }
	// ];

return movieLists.map(x => x.videos.map(y => {
	return {id:y.id, title:y.title, boxart:y.boxarts.filter(z =>z.width==150).map(a=>a.url)};
}));
Received Output
[[{"id":70111470,"title":"Die Hard","boxart":["http://cdn-0.nflximg.com/images/2891/DieHard150.jpg"]},
{"id":654356453,"title":"Bad Boys","boxart":["http://cdn-0.nflximg.com/images/2891/BadBoys150.jpg"]}],
[{"id":65432445,"title":"The Chamber","boxart":["http://cdn-0.nflximg.com/images/2891/TheChamber150.jpg"]},
{"id":675465,"title":"Fracture","boxart":["http://cdn-0.nflximg.com/images/2891/Fracture150.jpg"]}]]

return movieLists.map(x => x.videos.map(y => {
	return {id:y.id, title:y.title, boxart:y.boxarts.filter(z =>z.width==150).map(a=>a.url)};
})).concatAll();
Received Output
[{"id":675465,"title":"Fracture","boxart":["http://cdn-0.nflximg.com/images/2891/Fracture150.jpg"]},
{"id":65432445,"title":"The Chamber","boxart":["http://cdn-0.nflximg.com/images/2891/TheChamber150.jpg"]},
{"id":70111470,"title":"Die Hard","boxart":["http://cdn-0.nflximg.com/images/2891/DieHard150.jpg"]},
{"id":654356453,"title":"Bad Boys","boxart":["http://cdn-0.nflximg.com/images/2891/BadBoys150.jpg"]}]

return movieLists.map(x => x.videos.map(y => {
	return {id:y.id, title:y.title, boxart:y.boxarts.filter(z =>z.width==150).map(a=>a.url).concatAll()};
})).concatAll();
TypeError: second argument to Function.prototype.apply must be an array

return movieLists.map(x => x.videos.map(y => {
	return {id:y.id, title:y.title, boxart:y.boxarts.filter(z =>z.width==150).map(a=>a.url)}.concatAll();
})).concatAll();
TypeError: (intermediate value).concatAll is not a function

return movieLists.map(x => x.videos.map(y => {
	return {id:y.id, title:y.title, boxart:y.boxarts.filter(z =>z.width==150).map(a=>a.url)};
}).concatAll()).concatAll();
Received Output
[]

return movieLists.
	  map(function(movieList) {
		return movieList.videos.
		  map(function(video) {
			return video.boxarts.
			  filter(function(boxart) {
				return boxart.width === 150;
			  }).
			  map(function(boxart) {
				return {id: video.id, title: video.title, boxart: boxart.url};
			  });
		  }).
		  concatAll();
	  }).
	  concatAll();//解答，直接到filter完成，再進行return{}的動作，concatAll()是在第二層map後處理，及最後一層上。

return movieLists.map(x => x.videos.map(y => y.boxarts.filter(z=>z.width==150).map(z => {return {id:y.id, title:y.title, boxart:z.url};}
))).concatAll();
Received Output
[[{"id":70111470,"title":"Die Hard","boxart":"http://cdn-0.nflximg.com/images/2891/DieHard150.jpg"}],
[{"id":654356453,"title":"Bad Boys","boxart":"http://cdn-0.nflximg.com/images/2891/BadBoys150.jpg"}],
[{"id":65432445,"title":"The Chamber","boxart":"http://cdn-0.nflximg.com/images/2891/TheChamber150.jpg"}],
[{"id":675465,"title":"Fracture","boxart":"http://cdn-0.nflximg.com/images/2891/Fracture150.jpg"}]]

return movieLists.map(x => x.videos.map(y => y.boxarts.filter(z=>z.width==150).map(z => {return {id:y.id, title:y.title, boxart:z.url};}
))).concatAll().concatAll();//依照解答修改出來的我的解答，之前的答案沒有用到y.boxarts

}
```

##13.Fantastic job! Now you've learned to use concatAll() alongside map() and filter() to query trees. Notice that map() and concatAll() are very commonly chained together. Let's create a small helper function to help us with this common pattern.
超棒的，現在你學過concatAll()與map()及filter()對一個查詢樹的使用。注意，map()與concatAll()連用是非常常見的。我們已創造一個小小的輔助函數以幫助我在此常見模式

Exercise 13: Implement concatMap()

Nearly every time we flatten a tree we chain map() and concatAll(). Sometimes, if we're dealing with a tree several levels deep, we'll repeat this combination many times in our code. To save on typing, let's create a concatMap function that's just a map operation, followed by a concatAll.
幾乎每次我要平面化一個樹時，我們都會連用map()與concatAll(), 有時，假如我已處理多層深的樹，我們將重覆此情況非常多次，為了解決這種情形，我們創造了concatMap函數，這就只是一個map操作，並接著一個concatAll。

```js
Array.prototype.concatMap = function(projectionFunctionThatReturnsArray) {
	return this.
		map(function(item) {
			// ------------   INSERT CODE HERE!  ----------------------------
			// Apply the projection function to each item. The projection
			// function will return an new child array. This will create a
			// two-dimensional array.
			// ------------   INSERT CODE HERE!  ----------------------------
		}).
		// apply the concatAll function to flatten the two-dimensional array
		concatAll();
};

/*
	var spanishFrenchEnglishWords = [ ["cero","rien","zero"], ["uno","un","one"], ["dos","deux","two"] ];
	// collect all the words for each number, in every language, in a single, flat list
	var allWords = [0,1,2].
		concatMap(function(index) {
			return spanishFrenchEnglishWords[index];
		});

	return JSON.stringify(allWords) === '["cero","rien","zero","uno","un","one","dos","deux","two"]';
*/
Array.prototype.concatMap = function(projectionFunctionThatReturnsArray) {
	return this.
		map(function(item) {
			return projectionFunctionThatReturnsArray(item);
		}).
		// apply the concatAll function to flatten the two-dimensional array
		concatAll();//解答
};
```
##14.Exercise 14: Use concatMap() to retrieve id, title, and 150x200 box art url for every video

Let's repeat the exercise we just performed. However this time we'll simplify the code by replacing the map().concatAll() calls with concatMap().

```js
	return movieLists.concatMap(
		x => x.videos.concatMap(
			y => y.boxarts.filter(
				z=>z.width==150
			).map(
					z => {return {id:y.id, title:y.title, boxart:z.url};}
			  	 )
		)
	)
```
##15.Reducing Arrays

Sometimes we need to perform an operation on more than one item in the array at the same time. For example, let's say we need to find the largest integer in an array. We can't use a filter() operation, because it only examines one item at a time. To find the largest integer we need to compare items in the array to each other.

One approach could be to select an item in the array as the assumed largest number (perhaps the first item), and then compare that value to every other item in the array. Each time we come across a number that was larger than our assumed largest number, we'd replace it with the larger value, and continue the process until the entire array was traversed.

If we replaced the specific size comparison with a closure, we could write a function that handled the array traversal process for us. At each step our function would apply the closure to the last value and the current value and use the result as the last value the next time. Finally we'd be left with only one value. This process is known as reducing because we reduce many values to a single value.
Exercise 15: Use forEach to find the largest box art

In this example we use forEach to find the largest box art. Each time we examine a new boxart we update a variable with the currently known maximumSize. If the boxart is smaller than the maximum size, we discard it. If it's larger, we keep track of it. Finally we're left with a single boxart which must necessarily be the largest.

```js
function() {
	var boxarts = [
			{ width: 200, height: 200, url: "http://cdn-0.nflximg.com/images/2891/Fracture200.jpg" },
			{ width: 150, height: 200, url: "http://cdn-0.nflximg.com/images/2891/Fracture150.jpg" },
			{ width: 300, height: 200, url: "http://cdn-0.nflximg.com/images/2891/Fracture300.jpg" },
			{ width: 425, height: 150, url: "http://cdn-0.nflximg.com/images/2891/Fracture425.jpg" }
		],
		currentSize,
		maxSize = -1,
		largestBoxart;

	boxarts.forEach(function(boxart) {
		currentSize = boxart.width * boxart.height;
		if (currentSize > maxSize) {
			largestBoxart = boxart;
			maxSize = currentSize;
		}
	});

	return largestBoxart; //解答
}
		
```
##16.This process is a reduction because we're using the information we derived from the last computation to calculate the current value. However in the example above, we still have to specify the method of traversal. Wouldn't it be nice if we could just specify what operation we wanted to perform on the last and current value? Let's create a helper function to perform reductions on arrays.
Exercise 16: Implement reduce()

Let's add a reduce() function to the Array type. Like map. Take note this is different from the reduce in ES5, which returns an value instead of an Array!

```js
// [1,2,3].reduce(function(accumulatedValue, currentValue) { return accumulatedValue + currentValue; }); === [6];
// [1,2,3].reduce(function(accumulatedValue, currentValue) { return accumulatedValue + currentValue; }, 10); === [16];
// 這是範例，[1,2,3]反正就是陣列

// 下面是解釋，看起來做很多事。
Array.prototype.reduce = function(combiner, initialValue) {
	var counter,
		accumulatedValue;

	// If the array is empty, do nothing
	if (this.length === 0) {
		return this;
	}
	else {
		// If the user didn't pass an initial value, use the first item.
		if (arguments.length === 1) {
			counter = 1;
			accumulatedValue = this[0];
		}
		else if (arguments.length >= 2) {
			counter = 0;
			accumulatedValue = initialValue;
		}
		else {
			throw "Invalid arguments.";
		}

		// Loop through the array, feeding the current value and the result of
		// the previous computation back into the combiner function until
		// we've exhausted the entire array and are left with only one value.
		while(counter < this.length) {
			accumulatedValue = combiner(accumulatedValue, this[counter])
			counter++;
		}

		return [accumulatedValue];
	}
};
		
```

##17.Exercise 17: Retrieve the largest rating.

Let's use our new reduce function to isolate the largest value in an array of ratings.

```js
function() {
	var ratings = [2,3,1,4,5];

	// You should return an array containing only the largest rating. Remember that reduce always
	// returns an array with one item.
	return ratings.
		 reduce(function(acc, curr) {
		if(acc > curr) {
		  return acc;
		}
		else {
		  return curr;
		}
	  }); //這是解答，就二個參數，一個函式(用來比較或累積)

	  return ratings.
		 reduce((x,y) => {
		if(x > y) {
		  return x;
		}
		else {
		  return y;
		}
	  });//依據解答修改過來的。 
}
		
```
##18.Exercise 18: Retrieve url of the largest boxart

Let's try combining reduce() with map() to reduce multiple boxart objects to a single value: the url of the largest box art.
試併合reduce()與map()以得到一個單一的值，其值為最大box art

```js
unction() {
	var boxarts = [
			{ width: 200, height: 200, url: "http://cdn-0.nflximg.com/images/2891/Fracture200.jpg" },
			{ width: 150, height: 200, url: "http://cdn-0.nflximg.com/images/2891/Fracture150.jpg" },
			{ width: 300, height: 200, url: "http://cdn-0.nflximg.com/images/2891/Fracture300.jpg" },
			{ width: 425, height: 150, url: "http://cdn-0.nflximg.com/images/2891/Fracture425.jpg" }
		];

	// You should return an array containing only the URL of the largest box art. Remember that reduce always
	// returns an array with one item.
	return boxarts.
		reduce((x,y) => {
			a_box_art=x.map(a => {a.width*a.height};);
			b_box_art=y.map(b => {b.width*b.height};);
		if(a_box_art > b_box_art) {
		  return x;
		}
		else {
		  return y;
		}
		});//答錯，x不是陣列

	return boxarts.
		reduce((x,y) => {
			if(x.width*x.height > y.width*y.height) {
				return x;
			}
			else {
				return y;
			}
		});
	Received Output
	[{"width":425,"height":150,"url":"http://cdn-0.nflximg.com/images/2891/Fracture425.jpg"}]

	return boxarts.
		reduce((x,y) => {
			if(x.width*x.height > y.width*y.height) {
				return x;
			}
			else {
				return y;
			}
		}).map(z=>z.url);//加上map把url取出即可。
}
```
##19.Exercise 19: Reducing with an initial value

Sometimes when we reduce an array, we want the reduced value to be a different type than the items stored in the array. Let's say we have an array of videos and we want to reduce them to a single map where the key is the video id and the value is the video's title.
有時，我們要減少一陣列，減少後的值的類型與項目不同，例如，我們有一個video陣列，並且我們想要減後的值，其key為id，其value為title。

```js
function() {
	var videos = [
		{
			"id": 65432445,
			"title": "The Chamber"
		},
		{
			"id": 675465,
			"title": "Fracture"
		},
		{
			"id": 70111470,
			"title": "Die Hard"
		},
		{
			"id": 654356453,
			"title": "Bad Boys"
		}
	];

	// Expecting this output...
	// [
	//	 {
	//		 "65432445": "The Chamber",
	//		 "675465": "Fracture",
	//		 "70111470": "Die Hard",
	//		 "654356453": "Bad Boys"
	//	 }
	// ]
	return videos.
		reduce(function(accumulatedMap={}, video) {
		var obj = {};
		obj = {video.id:video.title};

		// ----- INSERT CODE TO ADD THE VIDEO TITLE TO THE ----
		// ----- NEW MAP USING THE VIDEO ID AS THE KEY	 ----

		// Object.assign() takes all of the enumerable properties from
		// the object listed in its second argument (obj) and assigns them
		// to the object listed in its first argument (accumulatedMap).
		return Object.assign(accumulatedMap, obj);
		},
		// Use an empty map as the initial value instead of the first item in
		// the list.
		{});//答錯，要用key-value方法來設值。

		//即		
		obj[video.id] = video.title;
}

```
##20.Exercise 20: Retrieve the id, title, and smallest box art url for every video.

This is a variation of the problem we solved earlier, where we retrieved the url of the boxart with a width of 150px. This time we'll use reduce() instead of filter() to retrieve the smallest box art in the boxarts array.
這是個我們之前解過的問題的變化型，那時我們取回width是150px的url，這次要用reduce()取代filter()來取回最小的box art

```js
function() {
	var movieLists = [
		{
			name: "New Releases",
			videos: [
				{
					"id": 70111470,
					"title": "Die Hard",
					"boxarts": [
						{ width: 150, height:200, url:"http://cdn-0.nflximg.com/images/2891/DieHard150.jpg" },
						{ width: 200, height:200, url:"http://cdn-0.nflximg.com/images/2891/DieHard200.jpg" }
					],
					"url": "http://api.netflix.com/catalog/titles/movies/70111470",
					"rating": 4.0,
					"bookmark": []
				},
				{
					"id": 654356453,
					"title": "Bad Boys",
					"boxarts": [
						{ width: 200, height:200, url:"http://cdn-0.nflximg.com/images/2891/BadBoys200.jpg" },
						{ width: 140, height:200, url:"http://cdn-0.nflximg.com/images/2891/BadBoys140.jpg" }

					],
					"url": "http://api.netflix.com/catalog/titles/movies/70111470",
					"rating": 5.0,
					"bookmark": [{ id:432534, time:65876586 }]
				}
			]
		},
		{
			name: "Thrillers",
			videos: [
				{
					"id": 65432445,
					"title": "The Chamber",
					"boxarts": [
						{ width: 130, height:200, url:"http://cdn-0.nflximg.com/images/2891/TheChamber130.jpg" },
						{ width: 200, height:200, url:"http://cdn-0.nflximg.com/images/2891/TheChamber200.jpg" }
					],
					"url": "http://api.netflix.com/catalog/titles/movies/70111470",
					"rating": 4.0,
					"bookmark": []
				},
				{
					"id": 675465,
					"title": "Fracture",
					"boxarts": [
						{ width: 200, height:200, url:"http://cdn-0.nflximg.com/images/2891/Fracture200.jpg" },
						{ width: 120, height:200, url:"http://cdn-0.nflximg.com/images/2891/Fracture120.jpg" },
						{ width: 300, height:200, url:"http://cdn-0.nflximg.com/images/2891/Fracture300.jpg" }
					],
					"url": "http://api.netflix.com/catalog/titles/movies/70111470",
					"rating": 5.0,
					"bookmark": [{ id:432534, time:65876586 }]
				}
			]
		}
	];
	// Use one or more concatMap, map, and reduce calls to create an array with the following items (order doesn't matter)
	// [
	//	 {"id": 675465,"title": "Fracture","boxart":"http://cdn-0.nflximg.com/images/2891/Fracture120.jpg" },
	//	 {"id": 65432445,"title": "The Chamber","boxart":"http://cdn-0.nflximg.com/images/2891/TheChamber130.jpg" },
	//	 {"id": 654356453,"title": "Bad Boys","boxart":"http://cdn-0.nflximg.com/images/2891/BadBoys140.jpg" },
	//	 {"id": 70111470,"title": "Die Hard","boxart":"http://cdn-0.nflximg.com/images/2891/DieHard150.jpg" }
	// ];
	return movieLists.
		concatMap(function(movieList) {
			(movieList.videos).concatMap(x => {id:x.id, title:x.title, boxart:x.boxarts.reduce(
                                   (x,y)=> {
						        	  if(x.width*x.height < y.width*y.height) {
                                        return x;
                                      }
                                      else {
                                        return y;
                                      }
								    }).map(z=>z.url);
                                };)}); //答錯
	
	return movieLists.concatMap(function(movieList) {
	  return movieList.videos.concatMap(function(video) {
	    return video.boxarts.
		  reduce(function(acc,curr) {
			if (acc.width * acc.height < curr.width * curr.height) {
			  return acc;
			}
			else {
			  return curr;
			}
		  }).
		  map(function(boxart) {
			return {id: video.id, title: video.title, boxart: boxart.url};
		  });
	  });
	});//解答

}		
```

##21.Zipping Arrays

Sometimes we need to combine two arrays by progressively taking an item from each and combining the pair. If you visualize a zipper, where each side is an array, and each tooth is an item, you'll have a good idea of how the zip operation works.
Exercise 21: Combine videos and bookmarks by index

Use a for loop to traverse the videos and bookmarks array at the same time. For each video and bookmark pair, create a {videoId, bookmarkId} pair and add it to the videoIdAndBookmarkIdPairs array.

```js
function() {
	var videos = [
			{
				"id": 70111470,
				"title": "Die Hard",
				"boxart": "http://cdn-0.nflximg.com/images/2891/DieHard.jpg",
				"uri": "http://api.netflix.com/catalog/titles/movies/70111470",
				"rating": 4.0,
			},
			{
				"id": 654356453,
				"title": "Bad Boys",
				"boxart": "http://cdn-0.nflximg.com/images/2891/BadBoys.jpg",
				"uri": "http://api.netflix.com/catalog/titles/movies/70111470",
				"rating": 5.0,
			},
			{
				"id": 65432445,
				"title": "The Chamber",
				"boxart": "http://cdn-0.nflximg.com/images/2891/TheChamber.jpg",
				"uri": "http://api.netflix.com/catalog/titles/movies/70111470",
				"rating": 4.0,
			},
			{
				"id": 675465,
				"title": "Fracture",
				"boxart": "http://cdn-0.nflximg.com/images/2891/Fracture.jpg",
				"uri": "http://api.netflix.com/catalog/titles/movies/70111470",
				"rating": 5.0,
			}
		],
		bookmarks = [
			{id: 470, time: 23432},
			{id: 453, time: 234324},
			{id: 445, time: 987834}
		],
	counter,
	videoIdAndBookmarkIdPairs = [];

	for(counter = 0; counter < Math.min(videos.length, bookmarks.length); counter++) {
		// Insert code here to create a {videoId, bookmarkId} pair and add it to the videoIdAndBookmarkIdPairs array.
		videoIdAndBookmarkIdPairs.push({videoId: videos[counter].id, bookmarkId: bookmarks[counter].id});//解答

	}

	return videoIdAndBookmarkIdPairs;
}
```
##22.Exercise 22: Implement zip

Let's add a static zip() function to the Array type. The zip function accepts a combiner function, traverses each array at the same time, and calls the combiner function on the current item on the left-hand-side and right-hand-side. The zip function requires an item from each array in order to call the combiner function, therefore the array returned by zip will only be as large as the smallest input array.

```js
// JSON.stringify(Array.zip([1,2,3],[4,5,6], function(left, right) { return left + right })) === '[5,7,9]'
// zip()一次自二個陣列，拿項目出來做事。
Array.zip = function(left, right, combinerFunction) {
	var counter,
		results = [];

	for(counter = 0; counter < Math.min(left.length, right.length); counter++) {
		// Add code here to apply the combinerFunction to the left and right-hand items in the respective arrays
		results.push(combinerFunction(left[counter],right[counter]));

	}

	return results;
};
		
```

##23.Exercise 23: Combine videos and bookmarks by index

Let's repeat exercise 21, but this time lets use your new zip() function. For each video and bookmark pair, create a {videoId, bookmarkId} pair.

```js
function() {
	var videos = [
			{
				"id": 70111470,
				"title": "Die Hard",
				"boxart": "http://cdn-0.nflximg.com/images/2891/DieHard.jpg",
				"uri": "http://api.netflix.com/catalog/titles/movies/70111470",
				"rating": 4.0,
			},
			{
				"id": 654356453,
				"title": "Bad Boys",
				"boxart": "http://cdn-0.nflximg.com/images/2891/BadBoys.jpg",
				"uri": "http://api.netflix.com/catalog/titles/movies/70111470",
				"rating": 5.0,
			},
			{
				"id": 65432445,
				"title": "The Chamber",
				"boxart": "http://cdn-0.nflximg.com/images/2891/TheChamber.jpg",
				"uri": "http://api.netflix.com/catalog/titles/movies/70111470",
				"rating": 4.0,
			},
			{
				"id": 675465,
				"title": "Fracture",
				"boxart": "http://cdn-0.nflximg.com/images/2891/Fracture.jpg",
				"uri": "http://api.netflix.com/catalog/titles/movies/70111470",
				"rating": 5.0,
			}
		],
		bookmarks = [
			{id: 470, time: 23432},
			{id: 453, time: 234324},
			{id: 445, time: 987834}
		];

	return Array. //Array是新的，供zip用。
	
		zip(
		  videos,
		  bookmarks,
		  function(video, bookmark) {
			return {videoId: video.id, bookmarkId: bookmark.id};
		  });//解答
}
```
#Querying Observables

What's the difference between these two tasks? 這兩個動作有什麼不同呢?

    * Creating a flat list of movies with a rating of 5.0 from a bunch of movie lists.
    * Creating a sequence of all the mouse drag events from the mouseDown, mouseMove, and mouseUp events.

You might think of them as different, and might code them very differently, but these tasks are fundamentally the same. Both of these tasks are queries, and can be solved using the functions you've learned in these exercises.
...但這二個動作基本上是一樣的，這二個動作是查詢，而且可以用你曾學過的函數解答。

The difference between traversing an Array and traversing an Observable is the direction in which the data moves. When traversing an Array, the client pulls data from the data source, blocking until it gets a result. When traversing Observables, the data source pushes data at the client whenever it arrives.
輪詢一個陣列和輪詢一個Observable的差別是資料移動的方向，當輪詢一個陣列，客端拉取資料，直到拿到一個結果。當輪詢一個Observables，當資料到達時，資料源推送資料。
It turns out that the direction in which data moves is orthogonal(直角的) to querying that data. In other words, when we're querying data it doesn't matter whether we pull data, or data is pushed at us. In either case the query methods make the same transformations. The only thing that changes is the input and output type respectively. If we filter an Array, we get a new Array. If we filter an Observable, we get a new Observable, and so on.
...假如我們過濾一個陣列，我們會得到一個陣列口；假如我過濾一個Observable，我們會得到一個新的Observable。
Take a look at how the query methods transform Observables and Arrays:

```js
// map()

[1,2,3].map(function(x) { return x + 1 })                       === [2,3,4]
seq([1,,,2,,,3]).map(function(x) { return x + 1})               === seq([2,,,3,,,4])
seq([1,,,2,,,3,,,]).map(function(x) { return x + 1 })           === seq([2,,,3,,,4,,,])

// filter()

[1,2,3].filter(function(x) { return x > 1; })                   === [2,3]
seq([1,,,2,,,3]).filter(function(x) { return x > 1; })          === seq([2,,,3])
seq([1,,,2,,,3,,,]).filter(function(x) { return x > 1; })       === seq([2,,,3,,,])

// concatAll()

[ [1, 2, 3], [4, 5, 6] ].concatAll()                             === [1,2,3,4,5,6]
seq([ seq([1,,,2,,,3]),,,seq([4,,,5,,,6]) ]).concatAll()         === seq([1,,,2,,,3,,,4,,,5,,,6])

// If a new sequence arrives before all the items
// from the previous sequence have arrived, no attempt
// to retrieve the new sequence's elements is made until
// the previous sequence has completed. As a result the
// order of elements in the sequence is preserved.
seq([
	seq([1,,,, ,2, ,3])
	,,,seq([,,4, ,5, ,,6]) ]).
	concatAll()                                                  === seq([1,,,,,2,,3,,4,,5,,,6])

// Notice that as long as at least one sequence being
// concatenated is incomplete, the concatenated sequence is also
// incomplete.
seq([
	seq([1,, ,,, ,,,2,,,3])
	,,,seq([4,,,5,,, ,,, ,,6,,,]) ]).
	concatAll()                                                  === seq([1,,,,,,,,2,,,3,4,,,5,,,,,,,,6,,,])

// reduce()

[ 1, 2, 3 ].reduce(sumFunction)                                 === [ 6 ]
seq([ 1,,,2,,,3 ]).reduce(sumFunction)                          === seq([,,,,,,6])

// Reduced sequences do not complete until the
// sequence does.
seq([ 1,,,2,,,3,,, ]).reduce(sumFunction)                       === seq([ ,,,,,,,,,])

// zip()

// In both Arrays and Observables, the zipped sequence
// completes as soon as either the left or right-hand
// side sequence completes.
Array.zip([1,2],[3,4,5], sumFunction)                           === [4,6]
Observable.zip(seq([1,,,2]),seq([3,,,4,,,5]), sumFunction)      === seq([4,,,6])

// take()
[1,2,3].take(2)                                                 === [1, 2]
seq([ 1,,,2,,,3 ]).take(2)                                      === seq([ 1,,,2 ])
seq([ 1,,,2,,,3,,, ]).take(2)                                   === seq([ 1,,,2 ])

// takeUntil()

// takeUntil works for Arrays, but it's not very useful
// because the result will always be an empty array.
[1,2,3].takeUntil([1])                                          === []

seq([1,,,2,,,3,,, ]).takeUntil(
seq([ ,,, ,,4 , ]))                                             === seq([ 1,,,2 ])
```
Remember when I prohibited the use of array indexers? The reason for that restriction should now become clearer to you. Whereas the 5 functions can be used on any collection, indexers can only be used on collections that support random-access (like Array). If you avoid indexers and stick to the functions you've learned in this tutorial, you'll have a unified programming model for transforming any collection. Having a unified programming model makes it trivial to convert synchronous code to asynchronous code, a process which would otherwise be very difficult. As we've demonstrated, you don't need indexers to perform complex collection transformations.
記得曾經我禁止使用陣列索引，現在可以跟你說明理由了，即這5個函數可以用在任何集合，索引只能用在支持隨意存取的集合(像陣列。)
Now that we've seen that we can query asychronous and synchronous data sources using the same programming model, let's use Observable and our query functions to create complex new events.
讓我們使用Observable和我們的查詢函數來創造複雜的新事件。
##32.Creating a mouse drag event
創一個滑鼠拖戈事件
Remember the exercise we solved earlier? The one in which we retrieved all the movies with a rating of 5.0 from an array of movie lists? If we were to describe the solution in pseudocode it might read something like this...

"For every movie list, retrieve only those videos with a rating of 5.0"

```js
var moviesWithHighRatings =
	movieLists.
		concatMap(function(movieList) {
			return movieList.videos.
				filter(function(video) {
					return video.rating === 5.0;
				});
		});
		
```
Now we're going to create a mouseDrag event for a DOM object. If we were to describe this problem as pseudocode it might read something like this...

"For every mouse down event on the sprite, retrieve only those videos with a rating of 5.0 mouse move events that occur before the next mouse up event."

```js
function(sprite, spriteContainer) {
	var spriteMouseDowns = Observable.fromEvent(sprite, "mousedown"),
		spriteContainerMouseMoves = Observable.fromEvent(spriteContainer, "mousemove"),
		spriteContainerMouseUps = Observable.fromEvent(spriteContainer, "mouseup"),
		spriteMouseDrags =
			// For every mouse down event on the sprite...
			spriteMouseDowns.concatMap(x=>spriteContainerMouseMoves.takeUntil(spriteContainerMouseUps));
				// --------------------------------------------------------
				//					  INSERT CODE HERE
				// --------------------------------------------------------
				// Complete this expression...
				// For every mouse down event, return the mouse move event
				// sequence until a mouse up event occurs.

	// For each mouse drag event, move the sprite to the absolute page position.
	spriteMouseDrags.forEach(function(dragPoint) {
		sprite.style.left = dragPoint.pageX + "px";
		sprite.style.top = dragPoint.pageY + "px";
	});
}
		
```
##33.Now we're really cooking. We just created a complex event with a few lines of code. We didn't have to deal with any subscriptions objects, or write any stateful code whatsoever. Let's try something a little harder.
Exercise 33: Improving our mouse drag event

Our mouse drag event is a little too simple. Notice that when we drag around the sprite, it always positions itself at the top-left corner of the mouse. Ideally we'd like our drag event to offset its coordinates, based on where the mouse was when the mouse down event occurred. This will make our mouse drag more closely resemble moving a real object with our finger.

Let's see if you can adjust the coordinates in the mouse drag event, based on the mousedown location on the sprite. The mouse events are sequences, and they look something like this:

```js
spriteContainerMouseMoves =
	seq([ {x: 200, y: 400, offsetX: 10, offsetY: 15},,,{x: 210, y: 410, offsetX: 20, offsetY: 26},,, ])		
```
Each item in the mouse event sequences contains an x, y value that represents that absolute location of the mouse event on the page. The moveSprite() function uses these coordinates to position the sprite. Each item in the sequence also contains a pair of offsetX and offsetY properties that indicate the position of the mouse event relative to the event target. 

```js
function(sprite, spriteContainer) {
	// All of the mouse event sequences look like this:
	// seq([ {pageX: 22, pageY: 3423, offsetX: 14, offsetY: 22} ,,, ])
	var spriteMouseDowns = Observable.fromEvent(sprite, "mousedown"),
		spriteContainerMouseMoves = Observable.fromEvent(spriteContainer, "mousemove"),
		spriteContainerMouseUps = Observable.fromEvent(spriteContainer, "mouseup"),
		// Create a sequence that looks like this:
		// seq([ {pageX: 22, pageY:4080 },,,{pageX: 24, pageY: 4082},,, ])
		spriteMouseDrags =
			// For every mouse down event on the sprite...
			spriteMouseDowns.
				concatMap(function(contactPoint) {
					// ...retrieve all the mouse move events on the sprite container...
					return spriteContainerMouseMoves.
						// ...until a mouse up event occurs.
						takeUntil(spriteContainerMouseUps).
						map(function(movePoint) {
							return {
								pageX: movePoint.pageX - contactPoint.offsetX,
								pageY: movePoint.pageY - contactPoint.offsetY
							};
						});
				});

	// For each mouse drag event, move the sprite to the absolute page position.
	spriteMouseDrags.forEach(function(dragPoint) {
		sprite.style.left = dragPoint.pageX + "px";
		sprite.style.top = dragPoint.pageY + "px";
	});
}//解答，但好像沒那個效果。
```

## 34.Exercise 34: HTTP requests
Events aren't the only source of asynchronous data in an application. There's also HTTP requests. Most of the time HTTP requests are exposed via a **callback-based API**. To receive data asynchronously from a callback-based API, the client typically passes a success and error handler to the function. When the asynchronous operation completes, the appropriate handler is called with the data. In this exercise we'll use jQuery's getJSON api to asynchronously retrieve data. 

事件不是應用程式中唯一的非同資料的來源，HTTP requests 也是，現今的http requests 是透過一個**callback-based API**的公開，來非同步的接收資料。(中略)在這個例題中，我們將使用jQuery的getJSON api來進行非同步的接收資料。

```js
function($) {
	$.getJSON(
		"http://api-global.netflix.com/queue",
		{
			success: function(json) {
				alert("Data has arrived.");
			},
			error: function(ex) {
				alert("There was an error.")
			}
		});
}
```
## 35 Exercise 35: Sequencing HTTP requests with callbacks

Let's say that we're writing the startup flow for a web application. On startup, the application must perform the following operations:

1. Download the URL prefix to use for all subsequent AJAX calls. This URL prefix will vary based on what AB test the user is enrolled in.
2. Use the url prefix to do the following actions in parallel:
	* Retrieve a movie list array
	* Retrieve configuration information and...
		* make a follow up call for an instant queue list if the config property "showInstantQueue" is truthy
3. If an instant queue list was retrieved, append it to the end of movie list.
4. If all operations were successful then display the movie lists after the window loads. Otherwise inform the user that there was a connectivity error.

假如說我們要寫一個web application的初始工作流，在初始時應用程式必須執行下列操作。

```js
function(window, $, showMovieLists, showError) {
	var error,
		configDone,
		movieLists,
		queueList,
		windowLoaded,
		outputDisplayed,
		errorHandler = function() {
			// Otherwise show the error.
			error = "There was a connectivity error";

			// We may be ready to display out
			tryToDisplayOutput();
		},
		tryToDisplayOutput = function() {
			if (outputDisplayed) {
				return;
			}
			if (windowLoaded) {
				if (configDone && movieLists !== undefined) {
					if (queueList !== undefined) {
						movieLists.push(queueList);
					}
					outputDisplayed = true;
					showMovieLists(JSON.stringify(movieLists));
				}
				else if (error) {
					outputDisplayed = true;
					showError(error);
				}
			}
		},
		windowLoadHandler = function() {
			windowLoaded = true;

			// Remember to unsubscribe from events
			window.removeEventListener("load", windowLoadHandler);

			// This may be the last task we're waiting on, so try and display output.
			tryToDisplayOutput();
		};

	// Register for the load event
	window.addEventListener("load", windowLoadHandler);

	// Request the service url prefix for the users AB test
	$.getJSON(
		"http://api-global.netflix.com/abTestInformation",
		{
			success: function(abTestInformation) {
				// Request the member's config information to determine whether their instant
				// queue should be displayed.
				$.getJSON(
					"http://api-global.netflix.com/" + abTestInformation.urlPrefix + "/config",
					{
						success: function(config) {
							// Parallel retrieval of movie list could've failed,
							// in which case we don't want to issue this call.
							if (!error) {
								// If we're supposed to
								if (config.showInstantQueue) {
									$.getJSON(
										"http://api-global.netflix.com/" + abTestInformation.urlPrefix + "/queue",
										{
											success: function(queueMessage) {
												queueList = queueMessage.list;

												configDone = true;
												tryToDisplayOutput();
											},
											error: errorHandler
										});
								}
								else {
									configDone = true;
									tryToDisplayOutput();
								}
							}
						},
						error: errorHandler
					});

				// Retrieve the movie list
				$.getJSON(
					"http://api-global.netflix.com/" + abTestInformation.urlPrefix + "/movieLists",
					{
						success: function(movieListMessage) {
							movieLists = movieListMessage.list;
							tryToDisplayOutput();
						},
						error: errorHandler
					});
			},
			error: errorHandler
		});
}
```


It's fair to say that **sequencing HTTP requests with callbacks is very hard**. In order to perform two tasks in parallel, we have to introduce a variable to track the status of each task. Every time one of the parallel tasks completes it must check whether its sibling task has also completed. If both have completed, only then can we move forward. In the example above, every time a task is finished the tryToDisplayOutput() function is called to check if the program was ready to display output. This function checks the status of all tasks and displays the output if possible.
可以確定的**sequencing HTTP requests with callbacks is very hard**，為了同步地執行2個工作，每一次平行工作的一個完成，還要檢查另一個工作是否完成，只有二個都完才能往前走，在上例中...

With a callback-based API, asynchronous error handling is also very complicated. In synchronous programs, a unit of work is cancelled when an exception is thrown. By contrast, in our program we had to explicitly track whether an error occurred in parallel to prevent an unnecessary call for the instant queue. Javascript provides us with special support for synchronous error handling with the keywords try/catch/throw. Unfortunately no such support is available for asynchronous programs.

The Observable interface is a much more powerful way of working with asynchronous APIs than callbacks. We'll see that Observables can free us from having to track the status of tasks that are run in parallel, just as Observables free us from having to track Event Subscriptions. We'll also see that Observable gives us the same error propagation semantics in asynchronous programs that we expect in synchronous programs. Finally we'll learn that by converting callback-based APIs to Observables, we can query them along with Events to build much more expressive programs.

## 36Exercise 36: Traversing callback-based Asynchronous APIs

### If a callback API were a sequence, what kind of sequence would it be? 
We've seen that UI Event sequences can contain anywhere from 0 to infinite items, but will never complete on their own.
```
mouseMoves === seq([ {x: 23, y: 55},,,,,,,{x: 44, y: 99},,,{x:55,y:99},,,{x: 54, y:543},,, ]);
```		

In contrast, if we were to convert output from the $.getJSON() function we've been using into a sequence it would always return a sequence that completes after sending a single item.
```
getJSONAsObservable("http://api-global.netflix.com/abTestInformation") ===
	seq([ { urlPrefix: "billboardTest" } ])
```		

It might seem strange to create sequences that contain only one object. We could introduce an Observable-like type specifically for scalar values, but that would make callback-based APIs more difficult to query with Events. Thankfully, an Observable sequence is flexible enough to model both.

### So how do we convert a callback API into an Observable sequence?
Unfortunately, because callback-based APIs vary so much in their interfaces, we can't create a conversion function like we did with fromEvent(). However there is a more flexible function we can use to build Observable sequences...

### Observable.create() is powerful enough to convert any asynchronous API into an Observable. 
Observable.create() relies on the fact that all asynchronous APIs have the following semantics:

1. The client needs to be able to receive data.
2. The client needs to be able to receive error information.
3. The client needs to be able to be alerted that the operation is complete.
4. The client needs to be able to indicate that they're no longer interested in the result of the operation.

In the following example, we'll use the Observable.create() function to create an Observable that issues a request to getJSON when it's traversed. 

```js
function(window, $) {//這裡還是使用jQuery
	var getJSON = function(url) { //getJSON包其他東西
		return Observable.create(function(observer) {//使用Observable.create
													 //observer是參數
			var subscribed = true;

			$.getJSON(url,
				{
					success:
						function(data) {
							// If client is still interested in the results, send them.
							if (subscribed) {
								// Send data to the client
								observer.onNext(data);
								// Immediately complete the sequence
								observer.onCompleted();
							}
						},
					error: function(ex) {
						// If client is still interested in the results, send them.
						if (subscribed) {
							// Inform the client that an error occurred.
							observer.onError(ex);
						}
					}
				});

			// Definition of the Subscription objects dispose() method.
			return function() {
				subscribed = false;
			}
		});
	};

	var subscription = //url在這裡才出現。
		getJSON("http://api-global.netflix.com/abTestInformation").
			forEach(
				// observer.onNext()
				function(data) {
					alert(JSON.stringify(data));
				},
				// observer.onError()
				function(err) {
					alert(err)
				},
				// observer.onCompleted()
				function() {
					alert("The asynchronous operation has completed.")
				});
}
```


Understand that the function passed to Observable.create() is the definition of the forEach() function for this Observable. In other words, all we have to do to define an Observable is to define its traversal function. Notice that although we pass three functions to the Observable's forEach() function, the function we pass to Observable.create() accepts only one value: an Observer. An Observer is just a triple containing three handlers:
了解函數在forEach()中定義，然後被傳到Observable.create()，換句話說，我們所需要做的是定義observable 的輪詢函數，注意雖然我們傳了三個函數到forEach()函數，但這些函數只接受一個值--一個observer，一個observer是一個三重容器，容納了三個處理器。
* The onNext() handler used to send data to the client.
* The onError() handler used to send error information to the client.
* The onCompleted() handler used to inform the client that the sequence has completed.

The three handlers we pass to forEach() are packaged together into a single Observer object for convenience. Finally Observable.create() returns a function that defines the dispose() method of the Subscription object during Traversal. Like the Observer, the Observable.create() function creates the Subscription object for us and uses our function as the dispose() definition.
我們傳到forEach()的三個處理器被包在一起成為一個單一的observer物件，最後，observable.create()返回一個函數，其定義了dispose()方法
Now that we've built a version of the getJSON function that returns an Observable sequence, let's use it to improve our solution to the previous exercise...

## 37.Exercise 37: Sequencing HTTP requests with Observable
Let's use the getJSON function that returns Observables, and the Observable.fromEvent() to complete the exercise we completed earlier.
讓我們使用Observables 所返回的getJSON函數及Observable.fromEvent()來完成我們之前完成的例題。

```js
function(window, getJSON, showMovieLists, showError) {
	var movieListsSequence =
		Observable.zip(//zip需要二個observable, 一是getJSON，另一是Observable
			getJSON("http://api-global.netflix.com/abTestInformation").
				concatMap(function(abTestInformation) {
					return Observable.zip(//zip需要二個observable(是getJSON)
						getJSON("http://api-global.netflix.com/" + abTestInformation.urlPrefix + "/config").
							concatMap(function(config) {
								if (config.showInstantQueue) {
									return getJSON("http://api-global.netflix.com/" + abTestInformation.urlPrefix + "/queue").
										map(function(queueMessage) {
											return queueMessage.list;
										});
								}
								else {
									return Observable.returnValue(undefined);
								}
							}),
						getJSON("http://api-global.netflix.com/" + abTestInformation.urlPrefix + "/movieLists"),
						function(queue, movieListsMessage) {
							var copyOfMovieLists = Object.create(movieListsMessage.list);
							if (queue !== undefined) {
								copyOfMovieLists.push(queue);
							}

							return copyOfMovieLists;
						});
				}),
			Observable.fromEvent(window, "load"),
			function(movieLists, loadEvent) {
				return movieLists;
			});

	movieListsSequence.//這裡是執行movieListsSequence
		forEach(
			function(movieLists) {
				showMovieLists(movieLists);
			},
			function(err) {
				showError(err);
			});
}
```

## 38.Exercise 38: Throttle Input節流閥輸入

When dealing with user input, there will be times when the user's input is too noisy, and will potentially clog your servers with extraneous requests. We want the ability to throttle the users's input so that if they interacting for one second, then we will get the user input. Let's say for example, the user clicks a button once too many times upon saving and we only want to fire after they've stopped for a second.
當處理使用者輸入，當使用者的輸入有太多雜訊時，可能需要多次輸入。過多的requests可能會塞住你的伺服器。我們希望能節流使用者的輸入，假如使用者的互動為一秒，然後我們再取得使用者的輸入，即使用者如果按一個按鍵太多次以進行儲存，而我們則會少很多次要處理的東西。

```
seq([1,2,3,,,,,,,4,5,6,,,]).throttle(1000 /* ms */) === seq([,,,,,,,3,,,,,,,,,,6,,,]);
```

```js
function (clicks, saveData, name) {
	return clicks.throttle(1000).
		// TODO: Throttle the clicks so that it only happens every one second
		concatMap(function () {
			return saveData(name);
		});
}
```
## 39.Exercise 39: Autocomplete Box
自動補完

One of the most common problems in web development is the autocomplete box. This seems like it should be an easy problem, but is actually quite challenging. For example, how do we throttle the input? How do we make sure we're not getting out of order requests coming back? For example if I type "react" then type "reactive" I want "reactive" to be my result, regardless of which actually returned first from the service.

In the example below, you will be receiving a sequence of key presses, a textbox, and a function when called returns an array of search results.

```
getSearchResultSet('react') === seq[,,,["reactive", "reaction","reactor"]]
keyPresses === seq['r',,,,,'e',,,,,,'a',,,,'c',,,,'t',,,,,]
```

```js
function (getSearchResultSet, keyPresses, textBox) {

	var getSearchResultSets =
		keyPresses.
		map(function () {
			return textBox.value;
		}).
		// TODO: Ensure that we only trigger a maximum of one search request per second
		concatMap(function (text) {

		// TODO: Ensure this sequence ends as soon as another key press arrives
		return getSearchResultSet(text);
	});

	return getSearchResultSets;
}//題目

function (getSearchResultSet, keyPresses, textBox) {

	var getSearchResultSets =
		keyPresses.
			map(function () {
				return textBox.value;
			}).
			throttle(1000).
			concatMap(function (text) {
				return getSearchResultSet(text).takeUntil(keyPresses);//我們漏了這個。
			});

	return getSearchResultSets;
}//答案

```

## 40.

Now that we're able to query with our throttled input, you'll still notice one slight problem. If you hit your arrow keys or any other non character key, the request will still fire. How do we prevent that?
Exercise 40: Distinct Until Changed Input
分辦直到輸入改變。 

You'll notice in the previous exercise that if you pressed your arrow keys while inside the textbox, the query will still fire, regardless of whether the text actually changed or not. How do we prevent that? The distinctUntilChanged filters out successive repetitive values.

```
seq([1,,,1,,,3,,,3,,,5,,,1,,,]).distinctUntilChanged() ===
seq([1,,,,,,,3,,,,,,,5,,,1,,,]);
```

```js
function (keyPresses, isAlpha) {

	return keyPresses.
		map(function (e) { return String.fromCharCode(e.keyCode); }).

		// Ensure we only have alphabetic characters
		filter(function (character) { return isAlpha(character); }).

		// TODO: Filter out successive repetitive keys

		// Building up a string of all the characters typed.
		scan('', function (stringSoFar, character) {
			return stringSoFar + character;
		});
}//問題
function (keyPresses, isAlpha) {

	return keyPresses.
		map(function (e) { return String.fromCharCode(e.keyCode); }).
		filter(function (character) { return isAlpha(character); }).
		distinctUntilChanged(). //注意不需要三個等號
		scan('', function (stringSoFar, character) {
			return stringSoFar + character;
		});
}//解答

```
## 41.

Now that we know how to get only the distinct input, let's see how it applies to our autocomplete example...
Exercise 41: Autocomplete Box Part 2: Electric Boogaloo
自動完成 第2部分，電子邦加鼓
In the previous version of the autocomplete box, there were two bugs
有二個錯誤。

* Multiple successive searches are made for the same string
* Attempts are made to retrieve results for an empty string.

The example below is the same as above, but this time, fix the bugs!

```js
getSearchResultSet('react') === seq[,,,["reactive", "reaction","reactor"]]
keyPresses === seq['r',,,,,'e',,,,,,'a',,,,'c',,,,'t',,,,,]
```

```js
function (getSearchResultSet, keyPresses, textBox) {

	var getSearchResultSets =
		keyPresses.
			map(function () {
				return textBox.value;
			}).
			throttle(1000).

			// TODO: Make sure we only get distinct values

			// TODO: Make sure the text is not empty

			concatMap(function (text) {
				return getSearchResultSet(text).takeUntil(keyPresses);
			});

	return getSearchResultSets;
}//問題

function (getSearchResultSet, keyPresses, textBox) {

	var getSearchResultSets =
		keyPresses.
			map(function () {
				return textBox.value;
			}).
			throttle(1000).
			distinctUntilChanged().//
			filter(function (s) { return s.length > 0; }).//
			concatMap(function (text) {
				return getSearchResultSet(text).takeUntil(keyPresses);
			});

	return getSearchResultSets;
}//答案
        
```


Exercise 42: Retrying after errors
A Work in Progress

Congratulations! You've made it this far, but you're not done. Learning is an on-going process. Go out and start using the functions you've learned in your day-to-day coding. Over time, I'll be adding more exercises to this tutorial. If you have suggestions for more exercises, send me a pull request!
