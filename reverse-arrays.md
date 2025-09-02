### How do you reverse an array in place?

-  Solution One: using built in method

   ```js
   let array = [1, 2, 3, 4, 5]
   array.reverse()
   console.log(array) // [5, 4, 3, 2, 1]
   ```

-  Solution Two:
   // You use two pointers: i starts at the beginning, j at the end.
   // Swap array[i] and array[j].
   // Move i forward and j backward until they meet or cross.
   // This avoids creating a new array and modifies the original one directly.

   ```js
   for (let i = 0, j = array.length - 1; i < j; i++, j--) {
      ;[array[i], array[j]] = [array[j], array[i]]
   }
   console.log(array) // [5, 4, 3, 2, 1]
   ```

-  Solution Three: Classic Swap with Temporary Variable

   ```js
   for (let i = 0; i < array.length / 2; i++) {
      let temp = array[i]
      array[i] = array[array.length - 1 - i]
      array[array.length - 1 - i] = temp
   }

   console.log(array) // [5, 4, 3, 2, 1]
   ```
