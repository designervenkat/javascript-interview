### How do you find the largest and smallest numbers in an array?

-  Solution one : Using Math.max and Math.min with the spread operator

```js
const array = [1, 2, 3, 4, 5]
const max = Math.max(...array)
const min = Math.min(...array)
console.log(`Max: ${max}, Min: ${min}`) // Max: 5, Min: 1
```

-  Solution two : Using reduce method

```js
const max = array.reduce((a, b) => Math.max(a, b))
const min = array.reduce((a, b) => Math.min(a, b))
console.log(`Max: ${max}, Min: ${min}`)
```
