### How do you find the frequency of elements in an array?

-  Solutions one : Using reduce to create a frequency map

```js
const array = [1, 2, 2, 3, 4, 4, 4, 5]

const frequency = array.reduce((acc, value) => {
   acc[value] = (acc[value] || 0) + 1
   return acc
}, {})

console.log(frequency) // {1: 1, 2: 2, 3: 1, 4: 3, 5: 1}
```

-  Solution two :
   -  We loop through each item in the array.
   -  If the value already exists as a key in the frequency object, we increment its count.
   -  If it doesn’t exist yet, we initialize it with 1.

```js
const array = [1, 2, 2, 3, 4, 4, 4, 5]
const frequency = {}

for (let i = 0; i < array.length; i++) {
   const value = array[i]
   if (frequency[value]) {
      frequency[value]++
   } else {
      frequency[value] = 1
   }
}

console.log(frequency) // Output: { '1': 1, '2': 2, '3': 1, '4': 3, '5': 1 }
```
