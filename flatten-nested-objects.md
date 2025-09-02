### Flatten nested objects with prefix and without prefix

#### Recursion in JavaScript is a programming technique where a function calls itself directly or indirectly to solve a problem.

-  A recursive function in JavaScript typically consists of two main parts:

#### Base Case:

-  This is the stopping condition that prevents the function from calling itself infinitely, thereby avoiding an infinite loop and potential stack overflow errors. When the base case is met, the function returns a value without making further recursive calls.

#### Recursive Case:

-  This is the part of the function where it calls itself with a modified argument, moving closer to the base case with each subsequent call. The problem is broken down into a simpler version of itself in this step.

```js
let user = {
   id: 1,
   name: 'Leanne Graham',
   username: 'Bret',
   email: 'Sincere@april.biz',
   address: {
      street: 'Kulas Light',
      suite: 'Apt. 556',
      city: 'Gwenborough',
      zipcode: '92998-3874',
      geo: {
         lat: '-37.3159',
         lng: '81.1496',
      },
   },
   phone: '1-770-736-8031 x56442',
   website: 'hildegard.org',
   company: {
      name: 'Romaguera-Crona',
      catchPhrase: 'Multi-layered client-server neural-net',
      bs: 'harness real-time e-markets',
   },
}
```

-  Solution one : with presvering key as prefix

```js
function flattenObj(obj, parentKey = '', result = {}) {
   // Handle edge case: must be a non-null object and NOT an array
   if (obj === null || typeof obj !== 'object' || Array.isArray(obj)) {
      return 'Please pass proper object'
   }

   // loop through for in method
   for (let key in obj) {
      if (Object.hasOwnProperty.call(obj, key)) {
         const newKey = parentKey ? `${parentKey}.${key}` : key
         const value = obj[key]

         if (
            typeof value === 'object' &&
            value !== null &&
            !Array.isArray(value)
         ) {
            // Recursively flatten nested object
            flattenObj(value, newKey, result)
         } else {
            result[newKey] = value
         }
      }
   }

   return result
}

console.log(flattenObj(user))
```

-  Solution twp : without presvering key as prefix

```js
function flattenObj(obj) {
   if (obj === null || typeof obj !== 'object' || Array.isArray(obj)) {
      return 'Please pass proper object'
   }

   let result = {}

   for (let key in obj) {
      if (Object.hasOwnProperty.call(obj, key)) {
         const value = obj[key]

         if (
            typeof value === 'object' &&
            value !== null &&
            !Array.isArray(value)
         ) {
            const flattened = flattenObj(value)
            Object.assign(result, flattened) // Merge without prefix
         } else {
            result[key] = value
         }
      }
   }

   return result
}

console.log(flattenObj(user))
```
