#### How do you find the intersection of two arrays?

#### Solution one : Using filter and includes

```js
const arr1 = [1, 2, 3, 4, 5]
const arr2 = [4, 5, 6, 7, 8]
```

```js
const intersection1 = arr1.filter((value) => arr2.includes(value))
console.log(intersection) // [4, 5]
```

#### Solution two : Using Set for better performance on large arrays

```js
const set2 = new Set(arr2)
const intersection2 = arr1.filter((value) => set2.has(value))
console.log(intersection) // [4, 5]
```

#### Solution three : Using loops

```js
const lookup = {}
const intersection = []

for (let i = 0; i < arr1.length; i++) {
   lookup[arr1[i]] = true
}

for (let j = 0; j < arr2.length; j++) {
   if (lookup[arr2[j]]) intersection.push(arr2[j])
}

console.log(intersection) // [ 4, 5 ]
```
