# Quiz-02
**04-20-17**

*Time Limit: 25min*
	
**Q1:** 

Which of the following methods removes the last element from an array and returns that element?

1.	get()
2.	pop()
3.	last()
4. 	delete()

**Answer: 1**
[Reference: MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)

```js
var a = [1, 2, 3];
a.pop();

console.log(a); // [1, 2]
```

---

**Q2:** 

```js
var a = 6
function test() {
    var a = 7
    function again() {
        var a = 8
        console.log(a)  // A
    }
    again()
    console.log(a)  // B
}
test()
console.log(a) // C
```

When executed, the below JavaScript code will pop up three alerts. Identify the corret answer?

1.	8; 7; 6
2.	6; 7; 8
3.	8; 6; 7
4.	7; 6; 8

**Answer: 1**

In the snippet, first `var a = 6 ` is defined globally, then `test()` is defined but not called yet. At the end of it's definition the function is called.
- `var a` is now assigned the value 7, locally scoped within `test()`
- `again()` is defined and then called
- `var a` is now assigned the value 8, locally scoped within `again()`
- The `console.log(a)  // A` gets executed, which outputs `8`
- `again()` is now finished executing and then `console.log(a)  // B` executes, which outputs `7`
- `test()` finished executing and finally `console.log(a) // C` is executed, which outputs `6`

---

**Q3:**

```js
var fs = require('fs')
fs.readFile('questions.txt', 'utf8', (err, data) => {
	if (!err) {
		console.log(data)
	}
})
console.log('Done!')
```

What will be the output? Explain

**Answer:**

The code reads the file `questions.txt` asynchronously that takes some time to return the file data in a callback.
Hence, `Done!` is printed out first and then the file contents `console.log(data)`. 

---

**Q4:**

```js
var fs = require('fs')
var content = null
fs.readFile('questions.txt', 'utf8', (err, data) => {
	if (!err) {
		content = data
	}
})
console.log(content)
console.log('Done!')
```

What will be the output? Explain

**Answer:**

The code reads the file `questions.txt` asynchronously that takes some time to return the file data in a callback.
While `fs.readFile()` executes, it runs `console.log(content)`. However, the content is `null`, because `fs.readFile()` hasn't finished executing. It'll output `undefined`, then `Done!` and file contents are never logged in console.

--- 

**Q5:**

```js
var fs = require('fs')
var fileContent = fs.readFileSync('sample.json', 'utf8')
console.log(fileContent)
console.log('Done!')
```

What will be the output? Explain
The code reads the file `questions.txt` synchronously.
- It then, prints out file content `console.log(fileContent)`
- and then print `console.log('Done!')`

---

**Q6:**

What is the difference between `for` loop and `foreach` statement?

[Reference: Zsolt Fabók Blog ](http://zsoltfabok.com/blog/2012/08/javascript-foreach/)

Consider this array: `var elements = ["element1", "element2", "element3"]`

For an asynhronous function being executed within a `forEach` loop

```js
elements.forEach(function(element) {
  // The asynchronous action simulator
  setTimeout(function() {
    console.log(element)
  }, 1000)
})
```
It will out output all the elements of the array after 1000 ms.

```
element1
element2
element3
```


However, in case of `for` loop

```js
for (var i = 0; i < elements.length; i++) {
  setTimeout(function() {
     console.log(elements[i])
  }, 100)
}
```
It will output `undefined` for all the elements

```
undefined
undefined
undefined
```
**Javascript always passes the arguments of a function as references, there is no block scope in Javascript**
`for` loop finishes before the asynchronous methods start, and the value of i is 3 at that time, so when `console.log()` is finally executed, it prints the value of `elements[3]` - undefined - three times.

---

**Q7:**

Write a dummy function to show how callbacks work.

```js
// define the function with the callback argument
function doSomething(a, b, callback) {
  var my_number = a+b // Add a and b parameters
  callback(my_number) // Call the callback and pass our result
}

// Call the doSomething function
doSomething(5, 15, function(num) {		// a = 5 ; b = 15 ; callback is function(num)
  console.log("callback called! " + num) // this anonymous function will run when the callback is called
})

```


---

**Q8:**

```js
var deferred = Promise.defer()
var promise = deferred.promise

promise
	.then((value) => {
		console.log(value * 10)
	})
	.catch((value) => {
		console.log(value * 2)
	})

setTimeout(x, 3000)
setTimeout(y, 1000)

function x() {
	deferred.reject(5)
	console.log('Promise Status: ', promise)
}

function y() {
	deferred.resolve(10)
	console.log('Promise Status: ', promise)
}
```

What will be the output? Explain

**Answer:**

```js
Promise Status:  Promise { 10 }
100
Promise Status:  Promise { 10 }
```

We create a deferred object and its promise is stored in `var promise`. It can either be resolved or rejected. The timing of which depends on the two `setTimeout()` statements.
`setTimeout(y, 1000)` executes after 1 second and `setTimeout(x, 3000)` executes after 3 seconds. So `y()` is called before `x()`. `y()` resolves the promise with a value of `10` and outputs ` Promise { 10 }`.
Once that's done, the `then()` statement is called and `console.log(value * 10)` gets executed with the resolved value. So the output is 10x10 = `100`.
Next, `x()` gets executed. Since the promise is already resolved it can't be rejected. Next, it outputs ` Promise { 10 }` with the resolved value. Lastly, `catch` statement never executes.

---

**Q9:**

```js
var io = require('socket.io').listen(server)

io.sockets.on('connection', function(socket){
	// Difference between following?
	socket.emit(identifier, data)             
	socket.broadcast.emit(identifier, data)   
	io.sockets.emit(identifier,data)
})
```

Explain the difference.

**Answer:**

- `socket.emit(identifier, data)` : Emits data back to the client `socket` itself
- `socket.broadcast.emit(identifier, data)` : Emits data to all connected clients except the `socket` client itself
- `io.sockets.emit(identifier,data)` : Emits data to everyone

---

**Q10:**
 What does the following query do when performed on the posts collection?

`db.posts.update({_id:1},{$set:{Author:"Tom"}})`

1. Sets the complete document with _id as 1 with the document specified in second parameter by replacing it completely
2. Adds a new field Author in the searched collection if not already present
3. Updates only the Author field of the document with _id as 1
4. Both 2 and 3

**Answer: 4**

[Reference](https://docs.mongodb.com/manual/reference/method/db.collection.update/)