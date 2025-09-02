#### Palindrome Words

-  A palindrome is a word, phrase, number, or other sequence of characters that reads the same forward and backward (ignoring spaces, punctuation, and capitalization). For example, words like "madam" and "racecar" are palindromes.

#### How to Check if a String is a Palindrome in JavaScript?

-  To check if a string is a palindrome in JavaScript, we need to
   -  Normalize the string: Convert it to lowercase and remove any non-alphanumeric characters like spaces and punctuation.
   -  Reverse the string: Reverse the string to compare it with the original.
   -  Compare the original and reversed string: If they are the same, it’s a palindrome.

#### Solution one : Using for loop (Efficient Approach)

```js
function isPalindrome(str) {
   str = str.toLowerCase().replace(/[^a-z0-9]/g, '')
   let j = str.length - 1

   for (let i = 0; i < str.length / 2; i++) {
      if (str[i] !== str[j]) {
         return false
      }
      j--
   }

   return true
}

console.log(isPalindrome('racecar')) // true
console.log(isPalindrome('Rama')) // false
```

#### Solution two : Checking the String from Last

```js
function isPalindrome(str) {
   let rev = ''
   for (let i = str.length - 1; i >= 0; i--) {
      rev += str[i]
   }
   if (rev == str) {
      return true
   } else {
      return false
   }
}

console.log(isPalindrome('racecar')) // true
console.log(isPalindrome('Rama')) // false
```

#### Solution three :

```js
function isPalindrome(str) {
   let rev = str.split('').reverse().join('')
   return rev == str
}

console.log(isPalindrome('racecar')) // true
console.log(isPalindrome('Rama')) // false
```
