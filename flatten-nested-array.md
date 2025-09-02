### How do you flatten a nested array?

-  Solution one : using built in reduce method

```js
const array = [1, [2, [3, [4]], 5]]
function flatten(arr) {
   return arr.reduce(
      (flat, toFlatten) =>
         flat.concat(Array.isArray(toFlatten) ? flatten(toFlatten) : toFlatten),
      []
   )
}
console.log(flatten(array)) // [1, 2, 3, 4, 5]
```

-  Solution two : using recursion method

```js
function flattenArray(arr) {
   let results = []
   arr.map((item) => {
      if (Array.isArray(item) && item !== null) {
         results.push(...flattenArray(item))
      } else {
         results.push(item)
      }
   })
   return results
}

console.log(flattenArray(array))
```
