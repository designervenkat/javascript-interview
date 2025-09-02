### How do you remove duplicates from an array?

-  Solution One: Using a Set (which only stores unique values):

```js
const array = [1, 2, 2, 3, 4, 4, 5]
const uniqueArray1 = [...new Set(array)]
console.log(uniqueArray1) // [1, 2, 3, 4, 5]
```

-  Solution Two: using filter and indexOf

```js
const uniqueArray2 = array.filter((value, index) => {
   array.indexOf(value) === index
})
console.log(uniqueArray2) // [1, 2, 3, 4, 5]
```

-  Solution Three: Using manually tracking seen values using a plain object or map

```js
let seen = {}
let uniqueArray = []

for (let i = 0; i < array.length; i++) {
   let value = array[i]
   if (!seen[value]) {
      seen[value] = true
      uniqueArray.push(value)
   }
}

console.log(uniqueArray) // [1, 2, 3, 4, 5]
```

-  Solution Three:

```js
function removeDuplicates(arr) {
   let unique = []

   for (let index = 0; index < arr.length; index++) {
      if (unique.indexOf(arr[index]) === -1) {
         unique.push(arr[index])
      }
   }

   return unique
}

console.log(removeDuplicates([1, 2, 3, 3, 4, 4]))
```
