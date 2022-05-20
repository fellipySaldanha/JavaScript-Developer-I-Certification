# Indexed Collections

## **Indexed collections**

This chapter introduces collections of data which are ordered by an index value. This includes arrays and array-like constructs such as `Array` objects and `TypedArray` objects.

### **`Array`**

The following statements create equivalent arrays:

```jsx
let arr = new Array(element0, element1, ..., elementN)
let arr = Array(element0, element1, ..., elementN)
let arr = [element0, element1, ..., elementN]
```

arrays can also be assigned as a property of a new or an existing object:

```jsx
let obj = {}
// ...
obj.prop = [element0, element1, ..., elementN]

// OR
let obj = {prop: [element0, element1, ...., elementN]}
```

If you wish to initialize an array with a single element, and the element happens to be a `Number`, you must use the bracket syntax. When a single `Number`value is passed to the `Array()` constructor or function, it is interpreted as an `arrayLength`, not as a single element.

```jsx
let arr = [42]       // Creates an array with only one element:
                     // the number 42.

let arr = Array(42)  // Creates an array with no elements
                     // and arr.length set to 42.
                     //
                     // This is equivalent to:
let arr = []
arr.length = 42
```

In ES2015, you can use the [Array.of](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/of) static method to create arrays with single element.

```jsx
let wisenArray = Array.of(9.3)   // wisenArray contains only one element 9.3
```

If you supply a non-integer value to the array operator in the code above, a property will be created in the object representing the array, instead of an array element.

```jsx
let arr = []
arr[3.4] = 'Oranges'
console.log(arr.length)                 // 0
console.log(arr.hasOwnProperty(3.4))    // true
```

You can also use [property accessors](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_Accessors) to access other properties of the array, like with an object.

```jsx
let arr = ['one', 'two', 'three']
arr[2]          // three
arr['length']   // 3
```

You can also assign to the `length` property.

Writing a value that is shorter than the number of stored items truncates the array. Writing `0` empties it entirely:

```jsx
let cats = ['Dusty', 'Misty', 'Twiggy']
console.log(cats.length)  // 3

cats.length = 2
console.log(cats)  // logs "Dusty, Misty" - Twiggy has been removed

cats.length = 0
console.log(cats)  // logs []; the cats array is empty

cats.length = 3
console.log(cats)  // logs [ <3 empty items> ]
```

## **Iterating over arrays**

if your array consists only of [DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model) nodes, for example—you can use a more efficient idiom:

```jsx
let divs = document.getElementsByTagName('div')
for (let i = 0, div; div = divs[i]; i++) {
  /* Process div in some way */
}
```

This avoids the overhead of checking the length of the array, and ensures that the `div` variable is reassigned to the current item each time around the loop for added convenience.

### `forEach`

```jsx
let colors = ['red', 'green', 'blue']
colors.forEach(function(color) {
  console.log(color)
})
// red
// green
// blue
```

Alternatively, you can shorten the code for the forEach parameter with ES2015 Arrow Functions:

```jsx
let colors = ['red', 'green', 'blue']
colors.forEach(color => console.log(color))
// red
// green
// blue
```

Note that the elements of an array that are omitted when the array is defined are not listed when iterating by `forEach`, but *are* listed when `undefined` has been manually assigned to the element:

```jsx
let array = ['first', 'second', , 'fourth']

array.forEach(function(element) {
  console.log(element)
})
// first
// second
// fourth

if (array[2] === undefined) {
  console.log('array[2] is undefined')  // true
}

array = ['first', 'second', undefined, 'fourth']

array.forEach(function(element) {
  console.log(element)
})
// first
// second
// undefined
// fourth
```

### **Array methods**

[concat()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat) joins two or more arrays and returns a new array.

```jsx
let myArray = new Array('1', '2', '3')
myArray = myArray.concat('a', 'b', 'c')
// myArray is now ["1", "2", "3", "a", "b", "c"]
```

[join(delimiter = ',')](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join)  joins all elements of an array into a string.

```jsx
let myArray = new Array('Wind', 'Rain', 'Fire')
let list = myArray.join(' - ') // list is "Wind - Rain - Fire"
```

[pop()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop) removes the last element from an array and returns that element.

```jsx
let myArray = new Array('1', '2', '3')
let last = myArray.pop()
// myArray is now ["1", "2"], last = "3"
```

[shift()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/shift) removes the first element from an array and returns that element.

```jsx
let myArray = new Array('1', '2', '3')
let first = myArray.shift()
// myArray is now ["2", "3"], first is "1"
```

[push()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push) adds one or more elements to the end of an array and returns the resulting `length`
 of the array.

```jsx
let myArray = new Array('1', '2')
myArray.push('3')  // myArray is now ["1", "2", "3"]
```

[unshift()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift) adds one or more elements to the front of an array and returns the new length of the array.

```jsx
let myArray = new Array('1', '2', '3')
myArray.unshift('4', '5')
// myArray becomes ["4", "5", "1", "2", "3"]
```

[slice(start_index, up_to_index)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice) extracts a section of an array and returns a new array.

```jsx
let myArray = new Array('a', 'b', 'c', 'd', 'e')
myArray = myArray.slice(1, 4)  // starts at index 1 and extracts all elements
                               // until index 3, returning [ "b", "c", "d"]
```

[splice(index, count_to_remove, addElement1, addElement2, ...)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice) removes elements from an array and (optionally) replaces them. It returns the items which were removed from the array.

```jsx
let myArray = new Array('1', '2', '3', '4', '5')
myArray.splice(1, 3, 'a', 'b', 'c', 'd')
// myArray is now ["1", "a", "b", "c", "d", "5"]
// This code started at index one (or where the "2" was),
// removed 3 elements there, and then inserted all consecutive
// elements in its place.
```

[reverse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse) transposes the elements of an array, in place: the first array element becomes the last and the last becomes the first. It returns a reference to the array.

```jsx
let myArray = new Array('1', '2', '3','4')
myArray.reverse()
// transposes the array so that myArray = ["4","3", "2", "1"]
```

[indexOf(searchElement[, fromIndex])](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf) searches the array for `searchElement` and returns the index of the first match.

```jsx
let a = ['a', 'b', 'a', 'b', 'a']
console.log(a.indexOf('b'))     // logs 1

// Now try again, starting from after the last match
console.log(a.indexOf('b', 2))  // logs 3
console.log(a.indexOf('z'))     // logs -1, because 'z' was not found
```

[lastIndexOf(searchElement[, fromIndex])](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/lastIndexOf) works like `indexOf`, but starts at the end and searches backwards.

```jsx
let a = ['a', 'b', 'c', 'd', 'a', 'b']
console.log(a.lastIndexOf('b'))     // logs 5

// Now try again, starting from before the last match
console.log(a.lastIndexOf('b', 4))  // logs 1
console.log(a.lastIndexOf('z'))     // logs -1
```

[forEach(callback[, thisObject])](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach) executes `callback` on every array item and returns `undefined`

```jsx
let a = ['a', 'b', 'c']
a.forEach(function(element) { console.log(element) })
// logs each item in turn
```

[map(callback[, thisObject])](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) returns a new array of the return value from executing `callback` on every array item.

```jsx
let a1 = ['a', 'b', 'c']
let a2 = a1.map(function(item) { return item.toUpperCase() })
console.log(a2) // logs ['A', 'B', 'C']
```

[filter(callback[, thisObject])](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) returns a new array containing the items for which `callback`
 returned `true`

```jsx
let a1 = ['a', 10, 'b', 20, 'c', 30]
let a2 = a1.filter(function(item) { return typeof item === 'number'; })
console.log(a2)  // logs [10, 20, 30]
```

[every(callback[, thisObject])](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every) returns `true` if `callback` returns `true` for every item in the array.

```jsx
function isNumber(value) {
  return typeof value === 'number'
}
let a1 = [1, 2, 3]
console.log(a1.every(isNumber))  // logs true

let a2 = [1, '2', 3]
console.log(a2.every(isNumber))  // logs false
```

[some(callback[, thisObject])](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some) returns `true` if `callback` returns `true` for at least one item in the array.

```jsx
function isNumber(value) {
  return typeof value === 'number'
}

let a1 = [1, 2, 3]
console.log(a1.some(isNumber))  // logs true

let a2 = [1, '2', 3]
console.log(a2.some(isNumber))  // logs true

let a3 = ['1', '2', '3']
console.log(a3.some(isNumber))  // logs false
```

[reduce(callback[, initialValue])](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce) applies `callback(accumulator, currentValue[, currentIndex[, array]])` for each value in the array for the purpose of reducing the list of items down to a single value. The `reduce` function returns the final value returned by `callback` function.

```jsx
let a = [10, 20, 30]
let total = a.reduce(function(accumulator, currentValue) { 
	return accumulator + currentValue 
}, 0)
console.log(total) // Prints 60
```

If `initialValue` is specified, then `callback` is called with `initialValue` as the first parameter value and the value of the first item in the array as the second parameter value.

If `initialValue` is *not* specified, then `callback`'s first two parameter values will be the first and second elements of the array. On *every* subsequent call, the first parameter's value will be whatever `callback` returned on the previous call, and the second parameter's value will be the next value in the array.

If `callback` needs access to the index of the item being processed, or access to the entire array, they are available as optional parameters. 

reduce((previousValue, currentValue, currentIndex, array)

[reduceRight(callback[, initialValue])](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduceRight) works like `reduce()`, but starts with the last element.