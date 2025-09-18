# 50 LeetCode Algorithms in JavaScript

## How would you reverse a linked list?

```js
// Function to reverse a linked list
function reverseLinkedList(head) {
    // Initialize three pointers for the reversal process
    let prev = null // Points to the previous node (starts as null)
    let current = head // Points to the current node (starts at head)
    let next = null // Points to the next node (will be set in loop)

    // Traverse through the linked list
    while (current !== null) {
        // Store the next node before we change the current node's pointer
        next = current.next

        // Reverse the current node's pointer to point to the previous node
        current.next = prev

        // Move prev and current one step forward
        prev = current // prev becomes current
        current = next // current becomes next
    }

    // prev now points to the new head of the reversed list
    return prev
}
```

## How would you find the middle node of a linked list?

```js
// Function to find the middle node using the two-pointer technique
function findMiddleNode(head) {
    // Handle empty list
    if (head === null) return null

    // Initialize two pointers: slow moves 1 step, fast moves 2 steps
    let slow = head // Will reach middle when fast reaches end
    let fast = head // Moves twice as fast as slow

    // Traverse until fast reaches the end or second-to-last node
    while (fast !== null && fast.next !== null) {
        slow = slow.next // Move slow pointer one step
        fast = fast.next.next // Move fast pointer two steps
    }

    // When fast reaches end, slow is at the middle
    return slow
}
```

## How would you determine if a string has all unique characters?

```js
// Function to check if string has all unique characters
function hasUniqueCharacters(str) {
    // Create a Set to store characters we've seen
    const seen = new Set()

    // Iterate through each character in the string
    for (let i = 0; i < str.length; i++) {
        const char = str[i]

        // If we've already seen this character, string is not unique
        if (seen.has(char)) {
            return false
        }

        // Add character to our set of seen characters
        seen.add(char)
    }

    // If we made it through without finding duplicates, all characters are unique
    return true
}

// Alternative approach using object (for older JavaScript environments)
function hasUniqueCharactersObject(str) {
    const charCount = {} // Object to count character occurrences

    for (let i = 0; i < str.length; i++) {
        const char = str[i]

        // If character already exists in our count object
        if (charCount[char]) {
            return false // Found duplicate
        }

        // Mark character as seen
        charCount[char] = true
    }

    return true
}
```

## How would you determine if two strings are anagrams of each other?

```js
// Function to check if two strings are anagrams
function areAnagrams(str1, str2) {
    // If lengths are different, they can't be anagrams
    if (str1.length !== str2.length) {
        return false
    }

    // Create character frequency map for first string
    const charCount = {}

    // Count characters in first string
    for (let i = 0; i < str1.length; i++) {
        const char = str1[i]
        charCount[char] = (charCount[char] || 0) + 1 // Increment count
    }

    // Check characters in second string against our count
    for (let i = 0; i < str2.length; i++) {
        const char = str2[i]

        // If character doesn't exist or count is 0, not an anagram
        if (!charCount[char]) {
            return false
        }

        // Decrease count for this character
        charCount[char]--
    }

    // If all characters match in frequency, they are anagrams
    return true
}

// Alternative approach using sorting
function areAnagramsSorted(str1, str2) {
    // If lengths are different, they can't be anagrams
    if (str1.length !== str2.length) {
        return false
    }

    // Convert to arrays, sort, and join back to strings
    const sorted1 = str1.split('').sort().join('')
    const sorted2 = str2.split('').sort().join('')

    // If sorted strings are equal, they are anagrams
    return sorted1 === sorted2
}
```

## How would you determine if a binary tree is a binary search tree?

```js
// Function to check if binary tree is a valid BST
function isValidBST(root) {
    // Helper function to validate BST with min and max bounds
    function validate(node, min, max) {
        // Empty node is valid
        if (node === null) {
            return true
        }

        // Check if current node value is within bounds
        if (node.val <= min || node.val >= max) {
            return false
        }

        // Recursively check left and right subtrees
        // Left subtree: all values must be < current node value
        // Right subtree: all values must be > current node value
        return (
            validate(node.left, min, node.val) &&
            validate(node.right, node.val, max)
        )
    }

    // Start validation with infinite bounds
    return validate(root, -Infinity, Infinity)
}

// Alternative approach using in-order traversal
function isValidBSTInOrder(root) {
    let prev = null // Store previous node value

    function inOrder(node) {
        if (node === null) return true

        // Check left subtree first
        if (!inOrder(node.left)) return false

        // Check current node against previous
        if (prev !== null && node.val <= prev) {
            return false
        }

        prev = node.val // Update previous value

        // Check right subtree
        return inOrder(node.right)
    }

    return inOrder(root)
}
```

## How would you implement a stack using an array?

```js
// Stack implementation using array
function createStack() {
    const stack = [] // Array to store stack elements

    return {
        // Add element to top of stack
        push: function (item) {
            stack.push(item) // Add to end of array (top of stack)
        },

        // Remove and return top element
        pop: function () {
            if (this.isEmpty()) {
                return null // Return null if stack is empty
            }
            return stack.pop() // Remove and return last element
        },

        // Return top element without removing
        peek: function () {
            if (this.isEmpty()) {
                return null // Return null if stack is empty
            }
            return stack[stack.length - 1] // Return last element
        },

        // Check if stack is empty
        isEmpty: function () {
            return stack.length === 0 // True if no elements
        },

        // Get current size of stack
        size: function () {
            return stack.length // Return number of elements
        },

        // Display all elements in stack
        display: function () {
            return [...stack] // Return copy of stack array
        },
    }
}

// Usage example:
// const myStack = createStack();
// myStack.push(1);
// myStack.push(2);
// console.log(myStack.pop()); // Returns 2
```

## How would you implement a queue using two stacks?

```js
// Queue implementation using two stacks
function createQueue() {
    const stack1 = [] // Stack for enqueue operations
    const stack2 = [] // Stack for dequeue operations

    return {
        // Add element to rear of queue
        enqueue: function (item) {
            stack1.push(item) // Simply push to first stack
        },

        // Remove and return front element
        dequeue: function () {
            // If second stack is empty, transfer all elements from first stack
            if (stack2.length === 0) {
                while (stack1.length > 0) {
                    stack2.push(stack1.pop()) // Move from stack1 to stack2
                }
            }

            // If still empty after transfer, queue is empty
            if (stack2.length === 0) {
                return null
            }

            return stack2.pop() // Return top of second stack
        },

        // Return front element without removing
        front: function () {
            // If second stack is empty, transfer elements
            if (stack2.length === 0) {
                while (stack1.length > 0) {
                    stack2.push(stack1.pop())
                }
            }

            if (stack2.length === 0) {
                return null
            }

            return stack2[stack2.length - 1] // Return top without removing
        },

        // Check if queue is empty
        isEmpty: function () {
            return stack1.length === 0 && stack2.length === 0
        },

        // Get current size of queue
        size: function () {
            return stack1.length + stack2.length
        },
    }
}
```

## How would you implement a binary search algorithm?

```js
// Binary search implementation (iterative)
function binarySearch(arr, target) {
    let left = 0 // Left boundary of search space
    let right = arr.length - 1 // Right boundary of search space

    // Continue searching while there's a valid search space
    while (left <= right) {
        // Calculate middle index to avoid overflow
        const mid = Math.floor(left + (right - left) / 2)

        // If target found at middle, return its index
        if (arr[mid] === target) {
            return mid
        }

        // If target is smaller, search left half
        if (arr[mid] > target) {
            right = mid - 1 // Update right boundary
        } else {
            // If target is larger, search right half
            left = mid + 1 // Update left boundary
        }
    }

    // Target not found
    return -1
}

// Binary search implementation (recursive)
function binarySearchRecursive(arr, target, left = 0, right = arr.length - 1) {
    // Base case: search space is invalid
    if (left > right) {
        return -1 // Target not found
    }

    // Calculate middle index
    const mid = Math.floor(left + (right - left) / 2)

    // If target found at middle, return its index
    if (arr[mid] === target) {
        return mid
    }

    // If target is smaller, search left half recursively
    if (arr[mid] > target) {
        return binarySearchRecursive(arr, target, left, mid - 1)
    } else {
        // If target is larger, search right half recursively
        return binarySearchRecursive(arr, target, mid + 1, right)
    }
}
```

## How would you implement a bubble sort algorithm?

```js
// Bubble sort implementation
function bubbleSort(arr) {
    const n = arr.length // Get array length

    // Outer loop: number of passes through the array
    for (let i = 0; i < n - 1; i++) {
        let swapped = false // Flag to check if any swap occurred

        // Inner loop: compare adjacent elements
        // After each pass, largest element bubbles to the end
        for (let j = 0; j < n - i - 1; j++) {
            // If current element is greater than next element
            if (arr[j] > arr[j + 1]) {
                // Swap the elements
                ;[arr[j], arr[j + 1]] = [arr[j + 1], arr[j]]
                swapped = true // Mark that a swap occurred
            }
        }

        // If no swaps occurred in this pass, array is sorted
        if (!swapped) {
            break // Early termination for optimization
        }
    }

    return arr // Return sorted array
}

// Optimized bubble sort with early termination
function bubbleSortOptimized(arr) {
    const n = arr.length
    let swapped

    do {
        swapped = false // Reset swap flag for each pass

        // Compare adjacent elements and swap if needed
        for (let i = 0; i < n - 1; i++) {
            if (arr[i] > arr[i + 1]) {
                // Swap elements
                ;[arr[i], arr[i + 1]] = [arr[i + 1], arr[i]]
                swapped = true // Mark that swap occurred
            }
        }
    } while (swapped) // Continue until no swaps occur

    return arr
}
```

## How would you implement a quicksort algorithm?

```js
// Quick sort implementation
function quickSort(arr, low = 0, high = arr.length - 1) {
    // Base case: if low >= high, subarray has 0 or 1 element
    if (low < high) {
        // Partition the array and get pivot index
        const pivotIndex = partition(arr, low, high)

        // Recursively sort elements before and after partition
        quickSort(arr, low, pivotIndex - 1) // Sort left subarray
        quickSort(arr, pivotIndex + 1, high) // Sort right subarray
    }

    return arr // Return sorted array
}

// Partition function: places pivot in correct position
function partition(arr, low, high) {
    // Choose rightmost element as pivot
    const pivot = arr[high]
    let i = low - 1 // Index of smaller element (indicates right position of pivot)

    // Traverse through all elements
    for (let j = low; j < high; j++) {
        // If current element is smaller than or equal to pivot
        if (arr[j] <= pivot) {
            i++ // Increment index of smaller element
            ;[arr[i], arr[j]] = [arr[j], arr[i]] // Swap elements
        }
    }

    // Place pivot in correct position
    ;[arr[i + 1], arr[high]] = [arr[high], arr[i + 1]]

    return i + 1 // Return pivot index
}

// Alternative quicksort with different pivot selection
function quickSortMiddlePivot(arr) {
    if (arr.length <= 1) {
        return arr // Base case: array with 0 or 1 element is sorted
    }

    // Choose middle element as pivot
    const pivotIndex = Math.floor(arr.length / 2)
    const pivot = arr[pivotIndex]

    // Partition array into three parts
    const left = [] // Elements less than pivot
    const right = [] // Elements greater than pivot
    const equal = [] // Elements equal to pivot

    // Categorize each element
    for (let i = 0; i < arr.length; i++) {
        if (arr[i] < pivot) {
            left.push(arr[i])
        } else if (arr[i] > pivot) {
            right.push(arr[i])
        } else {
            equal.push(arr[i])
        }
    }

    // Recursively sort left and right, then combine
    return [
        ...quickSortMiddlePivot(left),
        ...equal,
        ...quickSortMiddlePivot(right),
    ]
}
```

## How would you implement a mergesort algorithm?

```js
// Merge sort implementation
function mergeSort(arr) {
    // Base case: array with 0 or 1 element is already sorted
    if (arr.length <= 1) {
        return arr
    }

    // Find middle point to divide array into two halves
    const mid = Math.floor(arr.length / 2)

    // Divide array into left and right halves
    const leftHalf = arr.slice(0, mid) // Left half of array
    const rightHalf = arr.slice(mid) // Right half of array

    // Recursively sort both halves
    const sortedLeft = mergeSort(leftHalf) // Sort left half
    const sortedRight = mergeSort(rightHalf) // Sort right half

    // Merge the sorted halves
    return merge(sortedLeft, sortedRight)
}

// Merge function: merges two sorted arrays into one sorted array
function merge(left, right) {
    const result = [] // Array to store merged result
    let leftIndex = 0 // Index for left array
    let rightIndex = 0 // Index for right array

    // Compare elements from both arrays and add smaller one to result
    while (leftIndex < left.length && rightIndex < right.length) {
        if (left[leftIndex] <= right[rightIndex]) {
            result.push(left[leftIndex]) // Add from left array
            leftIndex++ // Move to next element in left array
        } else {
            result.push(right[rightIndex]) // Add from right array
            rightIndex++ // Move to next element in right array
        }
    }

    // Add remaining elements from left array (if any)
    while (leftIndex < left.length) {
        result.push(left[leftIndex])
        leftIndex++
    }

    // Add remaining elements from right array (if any)
    while (rightIndex < right.length) {
        result.push(right[rightIndex])
        rightIndex++
    }

    return result // Return merged sorted array
}

// In-place merge sort implementation
function mergeSortInPlace(arr, left = 0, right = arr.length - 1) {
    if (left < right) {
        // Find middle point
        const mid = Math.floor(left + (right - left) / 2)

        // Recursively sort first and second halves
        mergeSortInPlace(arr, left, mid) // Sort left half
        mergeSortInPlace(arr, mid + 1, right) // Sort right half

        // Merge the sorted halves
        mergeInPlace(arr, left, mid, right)
    }

    return arr
}

// In-place merge function
function mergeInPlace(arr, left, mid, right) {
    // Create temporary arrays for left and right halves
    const leftArr = arr.slice(left, mid + 1)
    const rightArr = arr.slice(mid + 1, right + 1)

    let i = 0,
        j = 0,
        k = left // Indices for left, right, and main arrays

    // Merge the temporary arrays back into main array
    while (i < leftArr.length && j < rightArr.length) {
        if (leftArr[i] <= rightArr[j]) {
            arr[k] = leftArr[i] // Place left element
            i++
        } else {
            arr[k] = rightArr[j] // Place right element
            j++
        }
        k++
    }

    // Copy remaining elements from left array
    while (i < leftArr.length) {
        arr[k] = leftArr[i]
        i++
        k++
    }

    // Copy remaining elements from right array
    while (j < rightArr.length) {
        arr[k] = rightArr[j]
        j++
        k++
    }
}
```

## How would you implement a binary tree traversal algorithm?

```js
// Binary tree node structure
function createTreeNode(val, left = null, right = null) {
    return { val, left, right }
}

// Pre-order traversal: Root -> Left -> Right
function preorderTraversal(root) {
    const result = [] // Array to store traversal result

    function traverse(node) {
        if (node === null) return // Base case: null node

        result.push(node.val) // Process current node (root)
        traverse(node.left) // Traverse left subtree
        traverse(node.right) // Traverse right subtree
    }

    traverse(root) // Start traversal from root
    return result
}

// In-order traversal: Left -> Root -> Right
function inorderTraversal(root) {
    const result = []

    function traverse(node) {
        if (node === null) return // Base case: null node

        traverse(node.left) // Traverse left subtree first
        result.push(node.val) // Process current node (root)
        traverse(node.right) // Traverse right subtree
    }

    traverse(root)
    return result
}

// Post-order traversal: Left -> Right -> Root
function postorderTraversal(root) {
    const result = []

    function traverse(node) {
        if (node === null) return // Base case: null node

        traverse(node.left) // Traverse left subtree first
        traverse(node.right) // Traverse right subtree
        result.push(node.val) // Process current node (root) last
    }

    traverse(root)
    return result
}

// Level-order traversal (Breadth-First): Level by level
function levelOrderTraversal(root) {
    if (root === null) return [] // Handle empty tree

    const result = [] // Array to store result
    const queue = [root] // Queue to store nodes for processing

    // Process nodes level by level
    while (queue.length > 0) {
        const levelSize = queue.length // Number of nodes in current level
        const currentLevel = [] // Array for current level nodes

        // Process all nodes in current level
        for (let i = 0; i < levelSize; i++) {
            const node = queue.shift() // Remove first node from queue
            currentLevel.push(node.val) // Add node value to current level

            // Add children to queue for next level
            if (node.left !== null) queue.push(node.left)
            if (node.right !== null) queue.push(node.right)
        }

        result.push(currentLevel) // Add current level to result
    }

    return result
}
```

## How would you implement a depth-first search algorithm?

```js
// DFS for graph (using adjacency list)
function depthFirstSearch(graph, start, visited = new Set()) {
    const result = [] // Array to store DFS traversal result

    function dfs(node) {
        // Mark current node as visited
        visited.add(node)
        result.push(node) // Add node to result

        // Visit all unvisited neighbors
        if (graph[node]) {
            for (const neighbor of graph[node]) {
                if (!visited.has(neighbor)) {
                    dfs(neighbor) // Recursively visit neighbor
                }
            }
        }
    }

    dfs(start) // Start DFS from given node
    return result
}

// DFS for binary tree
function dfsBinaryTree(root) {
    const result = []

    function traverse(node) {
        if (node === null) return // Base case: null node

        result.push(node.val) // Process current node
        traverse(node.left) // Visit left subtree
        traverse(node.right) // Visit right subtree
    }

    traverse(root)
    return result
}

// DFS iterative implementation using stack
function dfsIterative(graph, start) {
    const visited = new Set() // Set to track visited nodes
    const result = [] // Array to store result
    const stack = [start] // Stack to store nodes to visit

    // Process nodes while stack is not empty
    while (stack.length > 0) {
        const node = stack.pop() // Get node from top of stack

        // If node not visited, process it
        if (!visited.has(node)) {
            visited.add(node) // Mark as visited
            result.push(node) // Add to result

            // Add unvisited neighbors to stack
            if (graph[node]) {
                for (const neighbor of graph[node]) {
                    if (!visited.has(neighbor)) {
                        stack.push(neighbor) // Add to stack for later processing
                    }
                }
            }
        }
    }

    return result
}

// DFS to find path between two nodes
function findPathDFS(graph, start, end, visited = new Set(), path = []) {
    // Add current node to path and mark as visited
    path.push(start)
    visited.add(start)

    // If we reached the end node, return the path
    if (start === end) {
        return [...path] // Return copy of path
    }

    // Explore all neighbors
    if (graph[start]) {
        for (const neighbor of graph[start]) {
            if (!visited.has(neighbor)) {
                const result = findPathDFS(graph, neighbor, end, visited, path)
                if (result) {
                    return result // Return path if found
                }
            }
        }
    }

    // Backtrack: remove current node from path and visited
    path.pop()
    visited.delete(start)
    return null // No path found
}
```

## How would you implement a breadth-first search algorithm?

```js
// BFS for graph (using adjacency list)
function breadthFirstSearch(graph, start) {
    const visited = new Set() // Set to track visited nodes
    const result = [] // Array to store BFS traversal result
    const queue = [start] // Queue to store nodes for processing

    visited.add(start) // Mark start node as visited

    // Process nodes while queue is not empty
    while (queue.length > 0) {
        const node = queue.shift() // Remove first node from queue
        result.push(node) // Add node to result

        // Visit all unvisited neighbors
        if (graph[node]) {
            for (const neighbor of graph[node]) {
                if (!visited.has(neighbor)) {
                    visited.add(neighbor) // Mark neighbor as visited
                    queue.push(neighbor) // Add neighbor to queue
                }
            }
        }
    }

    return result
}

// BFS for binary tree (level-order traversal)
function bfsBinaryTree(root) {
    if (root === null) return [] // Handle empty tree

    const result = [] // Array to store result
    const queue = [root] // Queue to store nodes

    // Process nodes level by level
    while (queue.length > 0) {
        const node = queue.shift() // Remove first node from queue
        result.push(node.val) // Add node value to result

        // Add children to queue for next level
        if (node.left !== null) queue.push(node.left)
        if (node.right !== null) queue.push(node.right)
    }

    return result
}

// BFS to find shortest path (unweighted graph)
function shortestPathBFS(graph, start, end) {
    if (start === end) return [start] // Same start and end

    const visited = new Set() // Track visited nodes
    const queue = [[start, [start]]] // Queue: [node, path]

    visited.add(start) // Mark start as visited

    // Process nodes while queue is not empty
    while (queue.length > 0) {
        const [node, path] = queue.shift() // Get node and its path

        // Check all neighbors
        if (graph[node]) {
            for (const neighbor of graph[node]) {
                if (neighbor === end) {
                    return [...path, neighbor] // Found target, return path
                }

                if (!visited.has(neighbor)) {
                    visited.add(neighbor) // Mark as visited
                    queue.push([neighbor, [...path, neighbor]]) // Add to queue with extended path
                }
            }
        }
    }

    return null // No path found
}

// BFS to find minimum steps (each edge has weight 1)
function minimumStepsBFS(graph, start, end) {
    if (start === end) return 0 // Same start and end

    const visited = new Set() // Track visited nodes
    const queue = [[start, 0]] // Queue: [node, steps]

    visited.add(start) // Mark start as visited

    // Process nodes while queue is not empty
    while (queue.length > 0) {
        const [node, steps] = queue.shift() // Get node and current steps

        // Check all neighbors
        if (graph[node]) {
            for (const neighbor of graph[node]) {
                if (neighbor === end) {
                    return steps + 1 // Found target, return steps
                }

                if (!visited.has(neighbor)) {
                    visited.add(neighbor) // Mark as visited
                    queue.push([neighbor, steps + 1]) // Add to queue with incremented steps
                }
            }
        }
    }

    return -1 // No path found
}
```

## How would you find the second largest element in an array?

```js
// Function to find second largest element in array
function findSecondLargest(arr) {
    // Handle edge cases
    if (arr.length < 2) {
        return null // Need at least 2 elements
    }

    let largest = -Infinity // Track largest element
    let secondLargest = -Infinity // Track second largest element

    // Iterate through array to find largest and second largest
    for (let i = 0; i < arr.length; i++) {
        const current = arr[i]

        // If current element is greater than largest
        if (current > largest) {
            secondLargest = largest // Previous largest becomes second largest
            largest = current // Current becomes new largest
        }
        // If current is between largest and second largest
        else if (current > secondLargest && current !== largest) {
            secondLargest = current // Update second largest
        }
    }

    // Return second largest (or null if all elements are same)
    return secondLargest === -Infinity ? null : secondLargest
}

// Alternative approach using sorting
function findSecondLargestSorted(arr) {
    if (arr.length < 2) return null

    // Sort array in descending order
    const sorted = [...arr].sort((a, b) => b - a)

    // Find first element that's different from the largest
    for (let i = 1; i < sorted.length; i++) {
        if (sorted[i] !== sorted[0]) {
            return sorted[i] // Return first different element
        }
    }

    return null // All elements are same
}

// Using Set to remove duplicates first
function findSecondLargestUnique(arr) {
    // Remove duplicates and convert back to array
    const uniqueArr = [...new Set(arr)]

    if (uniqueArr.length < 2) return null

    // Sort in descending order
    uniqueArr.sort((a, b) => b - a)

    return uniqueArr[1] // Return second element
}
```

## How would you find the maximum sum of a contiguous subarray in an array?

```js
// Kadane's Algorithm - O(n) time complexity
function maxSubarraySum(arr) {
    if (arr.length === 0) return 0 // Handle empty array

    let maxSoFar = arr[0] // Maximum sum found so far
    let maxEndingHere = arr[0] // Maximum sum ending at current position

    // Iterate through array starting from second element
    for (let i = 1; i < arr.length; i++) {
        // Either extend previous subarray or start new subarray
        maxEndingHere = Math.max(arr[i], maxEndingHere + arr[i])

        // Update maximum sum found so far
        maxSoFar = Math.max(maxSoFar, maxEndingHere)
    }

    return maxSoFar // Return maximum subarray sum
}

// Kadane's Algorithm with indices (returns start and end indices)
function maxSubarraySumWithIndices(arr) {
    if (arr.length === 0) return { sum: 0, start: -1, end: -1 }

    let maxSoFar = arr[0]
    let maxEndingHere = arr[0]
    let start = 0,
        end = 0,
        tempStart = 0

    for (let i = 1; i < arr.length; i++) {
        // If current element is better than extending previous subarray
        if (arr[i] > maxEndingHere + arr[i]) {
            maxEndingHere = arr[i]
            tempStart = i // Start new subarray from current position
        } else {
            maxEndingHere += arr[i] // Extend previous subarray
        }

        // Update maximum sum and indices if we found better subarray
        if (maxEndingHere > maxSoFar) {
            maxSoFar = maxEndingHere
            start = tempStart
            end = i
        }
    }

    return { sum: maxSoFar, start, end }
}

// Brute force approach - O(n²) time complexity
function maxSubarraySumBruteForce(arr) {
    if (arr.length === 0) return 0

    let maxSum = -Infinity

    // Try all possible subarrays
    for (let i = 0; i < arr.length; i++) {
        let currentSum = 0

        // Calculate sum of subarray starting at index i
        for (let j = i; j < arr.length; j++) {
            currentSum += arr[j]
            maxSum = Math.max(maxSum, currentSum) // Update maximum sum
        }
    }

    return maxSum
}
```

## How would you find the intersection of two sorted arrays?

```js
// Function to find intersection of two sorted arrays
function findIntersection(arr1, arr2) {
    const result = [] // Array to store intersection elements
    let i = 0,
        j = 0 // Pointers for both arrays

    // Traverse both arrays simultaneously
    while (i < arr1.length && j < arr2.length) {
        // If elements are equal, add to result and move both pointers
        if (arr1[i] === arr2[j]) {
            // Avoid duplicates in result
            if (result.length === 0 || result[result.length - 1] !== arr1[i]) {
                result.push(arr1[i])
            }
            i++ // Move pointer in first array
            j++ // Move pointer in second array
        }
        // If element in first array is smaller, move its pointer
        else if (arr1[i] < arr2[j]) {
            i++
        }
        // If element in second array is smaller, move its pointer
        else {
            j++
        }
    }

    return result // Return intersection array
}

// Using Set for unsorted arrays
function findIntersectionUnsorted(arr1, arr2) {
    // Convert first array to Set for O(1) lookup
    const set1 = new Set(arr1)
    const result = []

    // Check each element in second array
    for (const num of arr2) {
        if (set1.has(num)) {
            result.push(num) // Add to result if found in first array
        }
    }

    return result
}

// Using filter and includes (less efficient)
function findIntersectionFilter(arr1, arr2) {
    // Filter elements from first array that exist in second array
    return arr1.filter((num) => arr2.includes(num))
}

// Using Map to count frequencies
function findIntersectionWithFrequency(arr1, arr2) {
    const frequency = new Map()
    const result = []

    // Count frequency of elements in first array
    for (const num of arr1) {
        frequency.set(num, (frequency.get(num) || 0) + 1)
    }

    // Check elements in second array
    for (const num of arr2) {
        if (frequency.get(num) > 0) {
            result.push(num) // Add to result
            frequency.set(num, frequency.get(num) - 1) // Decrease count
        }
    }

    return result
}
```

## How would you find the kth smallest element in an array?

```js
// Quick Select Algorithm - O(n) average time complexity
function findKthSmallest(arr, k) {
    if (k < 1 || k > arr.length) return null // Invalid k

    // Create copy to avoid modifying original array
    const nums = [...arr]

    function quickSelect(left, right, kSmallest) {
        // Base case: array has one element
        if (left === right) {
            return nums[left]
        }

        // Partition array and get pivot index
        const pivotIndex = partition(left, right)

        // If pivot is the kth smallest element
        if (kSmallest === pivotIndex) {
            return nums[pivotIndex]
        }
        // If kth smallest is in left partition
        else if (kSmallest < pivotIndex) {
            return quickSelect(left, pivotIndex - 1, kSmallest)
        }
        // If kth smallest is in right partition
        else {
            return quickSelect(pivotIndex + 1, right, kSmallest)
        }
    }

    // Partition function (similar to quicksort)
    function partition(left, right) {
        const pivot = nums[right] // Choose rightmost element as pivot
        let i = left // Index of smaller element

        for (let j = left; j < right; j++) {
            // If current element is smaller than or equal to pivot
            if (nums[j] <= pivot) {
                ;[nums[i], nums[j]] = [nums[j], nums[i]] // Swap elements
                i++ // Increment index of smaller element
            }
        }

        // Place pivot in correct position
        ;[nums[i], nums[right]] = [nums[right], nums[i]]
        return i // Return pivot index
    }

    return quickSelect(0, nums.length - 1, k - 1) // k-1 because array is 0-indexed
}

// Using sorting (simpler but O(n log n) time complexity)
function findKthSmallestSorted(arr, k) {
    if (k < 1 || k > arr.length) return null

    // Sort array and return kth element
    const sorted = [...arr].sort((a, b) => a - b)
    return sorted[k - 1] // k-1 because array is 0-indexed
}

// Using Min Heap (for multiple queries)
function findKthSmallestHeap(arr, k) {
    if (k < 1 || k > arr.length) return null

    // Build min heap
    const heap = [...arr]
    const n = heap.length

    // Heapify the array
    for (let i = Math.floor(n / 2) - 1; i >= 0; i--) {
        heapify(heap, n, i)
    }

    // Extract k-1 elements to get kth smallest
    for (let i = 0; i < k - 1; i++) {
        // Move largest element to end
        ;[heap[0], heap[n - 1 - i]] = [heap[n - 1 - i], heap[0]]
        // Heapify reduced heap
        heapify(heap, n - 1 - i, 0)
    }

    return heap[0] // kth smallest element is now at root
}

// Helper function to heapify
function heapify(arr, n, i) {
    let smallest = i // Initialize smallest as root
    const left = 2 * i + 1 // Left child
    const right = 2 * i + 2 // Right child

    // If left child is smaller than root
    if (left < n && arr[left] < arr[smallest]) {
        smallest = left
    }

    // If right child is smaller than smallest so far
    if (right < n && arr[right] < arr[smallest]) {
        smallest = right
    }

    // If smallest is not root
    if (smallest !== i) {
        ;[arr[i], arr[smallest]] = [arr[smallest], arr[i]] // Swap
        heapify(arr, n, smallest) // Recursively heapify affected sub-tree
    }
}
```

## How would you determine if a linked list has a cycle?

```js
// Floyd's Cycle Detection Algorithm (Tortoise and Hare)
function hasCycle(head) {
    // Handle empty list or single node
    if (head === null || head.next === null) {
        return false
    }

    let slow = head // Tortoise pointer (moves 1 step)
    let fast = head // Hare pointer (moves 2 steps)

    // Move pointers until they meet or fast reaches end
    while (fast !== null && fast.next !== null) {
        slow = slow.next // Move slow pointer one step
        fast = fast.next.next // Move fast pointer two steps

        // If pointers meet, there's a cycle
        if (slow === fast) {
            return true
        }
    }

    // If fast reached end, no cycle exists
    return false
}

// Find the starting node of the cycle
function findCycleStart(head) {
    if (head === null || head.next === null) {
        return null // No cycle possible
    }

    let slow = head
    let fast = head

    // First phase: detect if cycle exists
    while (fast !== null && fast.next !== null) {
        slow = slow.next
        fast = fast.next.next

        if (slow === fast) {
            break // Cycle detected
        }
    }

    // If no cycle found
    if (fast === null || fast.next === null) {
        return null
    }

    // Second phase: find cycle start
    slow = head // Reset slow to head

    // Move both pointers one step until they meet
    while (slow !== fast) {
        slow = slow.next
        fast = fast.next
    }

    return slow // This is the cycle start node
}

// Using Set to track visited nodes
function hasCycleSet(head) {
    const visited = new Set() // Set to track visited nodes

    let current = head

    // Traverse the linked list
    while (current !== null) {
        // If we've seen this node before, there's a cycle
        if (visited.has(current)) {
            return true
        }

        visited.add(current) // Mark current node as visited
        current = current.next // Move to next node
    }

    // If we reached end without finding cycle
    return false
}
```

## How would you determine if a binary tree is balanced?

```js
// Function to check if binary tree is height-balanced
function isBalanced(root) {
    // Helper function to calculate height and check balance
    function checkHeight(node) {
        if (node === null) {
            return 0 // Height of empty tree is 0
        }

        // Recursively check height of left and right subtrees
        const leftHeight = checkHeight(node.left)
        const rightHeight = checkHeight(node.right)

        // If any subtree is unbalanced, return -1
        if (leftHeight === -1 || rightHeight === -1) {
            return -1
        }

        // If height difference is more than 1, tree is unbalanced
        if (Math.abs(leftHeight - rightHeight) > 1) {
            return -1
        }

        // Return height of current node
        return Math.max(leftHeight, rightHeight) + 1
    }

    // If checkHeight returns -1, tree is unbalanced
    return checkHeight(root) !== -1
}

// Alternative approach with separate height calculation
function isBalancedSeparate(root) {
    // Function to calculate height of tree
    function height(node) {
        if (node === null) return 0
        return Math.max(height(node.left), height(node.right)) + 1
    }

    // Function to check if tree is balanced
    function checkBalance(node) {
        if (node === null) return true // Empty tree is balanced

        // Check if current node is balanced
        const leftHeight = height(node.left)
        const rightHeight = height(node.right)

        // Check if height difference is acceptable and subtrees are balanced
        return (
            Math.abs(leftHeight - rightHeight) <= 1 &&
            checkBalance(node.left) &&
            checkBalance(node.right)
        )
    }

    return checkBalance(root)
}
```

## How would you determine if a binary tree is symmetric?

```js
// Function to check if binary tree is symmetric
function isSymmetric(root) {
    if (root === null) return true // Empty tree is symmetric

    // Helper function to check if two subtrees are mirror images
    function isMirror(left, right) {
        // Both nodes are null
        if (left === null && right === null) {
            return true
        }

        // One node is null, other is not
        if (left === null || right === null) {
            return false
        }

        // Check if values are equal and subtrees are mirror images
        return (
            left.val === right.val &&
            isMirror(left.left, right.right) &&
            isMirror(left.right, right.left)
        )
    }

    // Check if left and right subtrees are mirror images
    return isMirror(root.left, root.right)
}

// Iterative approach using queue
function isSymmetricIterative(root) {
    if (root === null) return true

    const queue = [] // Queue to store node pairs for comparison

    // Add left and right children of root to queue
    queue.push(root.left)
    queue.push(root.right)

    // Process nodes in pairs
    while (queue.length > 0) {
        const left = queue.shift() // Get first node
        const right = queue.shift() // Get second node

        // Both nodes are null, continue
        if (left === null && right === null) {
            continue
        }

        // One node is null, other is not
        if (left === null || right === null) {
            return false
        }

        // Values don't match
        if (left.val !== right.val) {
            return false
        }

        // Add children in mirror order for next iteration
        queue.push(left.left) // Left child of left node
        queue.push(right.right) // Right child of right node
        queue.push(left.right) // Right child of left node
        queue.push(right.left) // Left child of right node
    }

    return true // All pairs matched
}
```

## How would you determine the maximum depth of a binary tree?

```js
// Recursive approach to find maximum depth
function maxDepth(root) {
    // Base case: empty tree has depth 0
    if (root === null) {
        return 0
    }

    // Recursively find depth of left and right subtrees
    const leftDepth = maxDepth(root.left)
    const rightDepth = maxDepth(root.right)

    // Return maximum depth plus 1 for current node
    return Math.max(leftDepth, rightDepth) + 1
}

// Iterative approach using level-order traversal
function maxDepthIterative(root) {
    if (root === null) return 0

    const queue = [root] // Queue for level-order traversal
    let depth = 0 // Track current depth

    // Process tree level by level
    while (queue.length > 0) {
        const levelSize = queue.length // Number of nodes in current level
        depth++ // Increment depth for each level

        // Process all nodes in current level
        for (let i = 0; i < levelSize; i++) {
            const node = queue.shift() // Remove node from queue

            // Add children to queue for next level
            if (node.left !== null) queue.push(node.left)
            if (node.right !== null) queue.push(node.right)
        }
    }

    return depth // Return maximum depth
}

// Using stack for iterative DFS
function maxDepthStack(root) {
    if (root === null) return 0

    const stack = [[root, 1]] // Stack: [node, depth]
    let maxDepth = 0

    // Process nodes using stack
    while (stack.length > 0) {
        const [node, depth] = stack.pop() // Get node and its depth

        // Update maximum depth
        maxDepth = Math.max(maxDepth, depth)

        // Add children to stack with incremented depth
        if (node.left !== null) {
            stack.push([node.left, depth + 1])
        }
        if (node.right !== null) {
            stack.push([node.right, depth + 1])
        }
    }

    return maxDepth
}
```

## How would you determine the maximum path sum in a binary tree?

```js
// Function to find maximum path sum in binary tree
function maxPathSum(root) {
    let maxSum = -Infinity // Track maximum path sum found

    // Helper function to calculate maximum path sum through a node
    function maxPathThroughNode(node) {
        if (node === null) {
            return 0 // Empty node contributes 0 to path sum
        }

        // Recursively find maximum path sum in left and right subtrees
        const leftMax = Math.max(0, maxPathThroughNode(node.left)) // Ignore negative sums
        const rightMax = Math.max(0, maxPathThroughNode(node.right)) // Ignore negative sums

        // Calculate maximum path sum through current node
        const currentMax = node.val + leftMax + rightMax

        // Update global maximum
        maxSum = Math.max(maxSum, currentMax)

        // Return maximum path sum that can be extended upward
        return node.val + Math.max(leftMax, rightMax)
    }

    maxPathThroughNode(root) // Start calculation from root
    return maxSum // Return maximum path sum found
}

// Alternative approach with explicit return values
function maxPathSumAlternative(root) {
    function dfs(node) {
        if (node === null) {
            return { maxPath: -Infinity, maxBranch: 0 }
        }

        // Get results from left and right subtrees
        const left = dfs(node.left)
        const right = dfs(node.right)

        // Calculate maximum path through current node
        const maxPath = Math.max(
            node.val, // Just current node
            node.val + left.maxBranch, // Current + left branch
            node.val + right.maxBranch, // Current + right branch
            node.val + left.maxBranch + right.maxBranch, // Current + both branches
            left.maxPath, // Best path in left subtree
            right.maxPath // Best path in right subtree
        )

        // Calculate maximum branch that can be extended upward
        const maxBranch = Math.max(
            node.val, // Just current node
            node.val + left.maxBranch, // Current + left branch
            node.val + right.maxBranch // Current + right branch
        )

        return { maxPath, maxBranch }
    }

    const result = dfs(root)
    return result.maxPath
}
```

## How would you find the longest common prefix of a set of strings?

```js
// Function to find longest common prefix
function longestCommonPrefix(strs) {
    if (strs.length === 0) return '' // Handle empty array
    if (strs.length === 1) return strs[0] // Single string is its own prefix

    // Start with first string as initial prefix
    let prefix = strs[0]

    // Compare prefix with each string in array
    for (let i = 1; i < strs.length; i++) {
        // While current string doesn't start with current prefix
        while (strs[i].indexOf(prefix) !== 0) {
            // Remove last character from prefix
            prefix = prefix.substring(0, prefix.length - 1)

            // If prefix becomes empty, no common prefix exists
            if (prefix === '') {
                return ''
            }
        }
    }

    return prefix // Return longest common prefix
}

// Vertical scanning approach
function longestCommonPrefixVertical(strs) {
    if (strs.length === 0) return ''

    // Compare characters column by column
    for (let i = 0; i < strs[0].length; i++) {
        const char = strs[0][i] // Character from first string

        // Check if this character matches in all other strings
        for (let j = 1; j < strs.length; j++) {
            // If we've reached end of string or character doesn't match
            if (i === strs[j].length || strs[j][i] !== char) {
                return strs[0].substring(0, i) // Return prefix up to this point
            }
        }
    }

    return strs[0] // All characters matched
}

// Divide and conquer approach
function longestCommonPrefixDivideConquer(strs) {
    if (strs.length === 0) return ''
    if (strs.length === 1) return strs[0]

    // Helper function to find common prefix of two strings
    function commonPrefix(str1, str2) {
        const minLength = Math.min(str1.length, str2.length)

        for (let i = 0; i < minLength; i++) {
            if (str1[i] !== str2[i]) {
                return str1.substring(0, i)
            }
        }

        return str1.substring(0, minLength)
    }

    // Recursive function to find common prefix
    function findPrefix(left, right) {
        if (left === right) {
            return strs[left] // Single string
        }

        const mid = Math.floor((left + right) / 2)
        const leftPrefix = findPrefix(left, mid) // Find prefix in left half
        const rightPrefix = findPrefix(mid + 1, right) // Find prefix in right half

        return commonPrefix(leftPrefix, rightPrefix) // Find common prefix of both halves
    }

    return findPrefix(0, strs.length - 1)
}
```

## How would you implement a hash table?

```js
// Hash table implementation with collision handling
function createHashTable(size = 16) {
    const table = new Array(size) // Array to store key-value pairs
    let count = 0 // Track number of elements

    // Hash function to convert key to index
    function hash(key) {
        let hashValue = 0
        const stringKey = String(key)

        // Simple hash function using character codes
        for (let i = 0; i < stringKey.length; i++) {
            hashValue = (hashValue * 31 + stringKey.charCodeAt(i)) % size
        }

        return Math.abs(hashValue) // Ensure positive index
    }

    // Resize table when load factor is too high
    function resize() {
        const oldTable = table
        const oldSize = size

        size *= 2 // Double the size
        table.length = size // Resize array
        count = 0 // Reset count

        // Rehash all existing elements
        for (let i = 0; i < oldSize; i++) {
            if (oldTable[i]) {
                for (const [key, value] of oldTable[i]) {
                    set(key, value) // Re-insert with new hash
                }
            }
        }
    }

    return {
        // Set key-value pair
        set: function (key, value) {
            const index = hash(key)

            // Initialize bucket if it doesn't exist
            if (!table[index]) {
                table[index] = []
            }

            // Check if key already exists
            const bucket = table[index]
            for (let i = 0; i < bucket.length; i++) {
                if (bucket[i][0] === key) {
                    bucket[i][1] = value // Update existing value
                    return
                }
            }

            // Add new key-value pair
            bucket.push([key, value])
            count++

            // Resize if load factor is too high
            if (count / size > 0.75) {
                resize()
            }
        },

        // Get value by key
        get: function (key) {
            const index = hash(key)
            const bucket = table[index]

            if (!bucket) return undefined

            // Search for key in bucket
            for (const [k, v] of bucket) {
                if (k === key) {
                    return v // Return value if found
                }
            }

            return undefined // Key not found
        },

        // Delete key-value pair
        delete: function (key) {
            const index = hash(key)
            const bucket = table[index]

            if (!bucket) return false

            // Find and remove key from bucket
            for (let i = 0; i < bucket.length; i++) {
                if (bucket[i][0] === key) {
                    bucket.splice(i, 1) // Remove key-value pair
                    count--
                    return true
                }
            }

            return false // Key not found
        },

        // Check if key exists
        has: function (key) {
            return this.get(key) !== undefined
        },

        // Get all keys
        keys: function () {
            const keys = []
            for (let i = 0; i < size; i++) {
                if (table[i]) {
                    for (const [key] of table[i]) {
                        keys.push(key)
                    }
                }
            }
            return keys
        },

        // Get all values
        values: function () {
            const values = []
            for (let i = 0; i < size; i++) {
                if (table[i]) {
                    for (const [, value] of table[i]) {
                        values.push(value)
                    }
                }
            }
            return values
        },

        // Get size
        size: function () {
            return count
        },

        // Clear all entries
        clear: function () {
            table.fill(null)
            count = 0
        },
    }
}
```

## How would you implement a binary heap?

```js
// Min Heap implementation
function createMinHeap() {
    const heap = [] // Array to store heap elements

    // Get parent index
    function getParentIndex(index) {
        return Math.floor((index - 1) / 2)
    }

    // Get left child index
    function getLeftChildIndex(index) {
        return 2 * index + 1
    }

    // Get right child index
    function getRightChildIndex(index) {
        return 2 * index + 2
    }

    // Swap two elements in heap
    function swap(index1, index2) {
        ;[heap[index1], heap[index2]] = [heap[index2], heap[index1]]
    }

    // Move element up to maintain heap property
    function heapifyUp(index) {
        if (index === 0) return // Already at root

        const parentIndex = getParentIndex(index)

        // If current element is smaller than parent, swap and continue
        if (heap[index] < heap[parentIndex]) {
            swap(index, parentIndex)
            heapifyUp(parentIndex) // Recursively check parent
        }
    }

    // Move element down to maintain heap property
    function heapifyDown(index) {
        const leftChildIndex = getLeftChildIndex(index)
        const rightChildIndex = getRightChildIndex(index)
        let smallestIndex = index

        // Find smallest among current node and its children
        if (
            leftChildIndex < heap.length &&
            heap[leftChildIndex] < heap[smallestIndex]
        ) {
            smallestIndex = leftChildIndex
        }

        if (
            rightChildIndex < heap.length &&
            heap[rightChildIndex] < heap[smallestIndex]
        ) {
            smallestIndex = rightChildIndex
        }

        // If smallest is not current node, swap and continue
        if (smallestIndex !== index) {
            swap(index, smallestIndex)
            heapifyDown(smallestIndex) // Recursively check child
        }
    }

    return {
        // Insert element into heap
        insert: function (value) {
            heap.push(value) // Add to end of array
            heapifyUp(heap.length - 1) // Move up to correct position
        },

        // Remove and return minimum element
        extractMin: function () {
            if (heap.length === 0) return null // Empty heap
            if (heap.length === 1) return heap.pop() // Single element

            const min = heap[0] // Store minimum element
            heap[0] = heap.pop() // Move last element to root
            heapifyDown(0) // Move down to correct position

            return min // Return minimum element
        },

        // Get minimum element without removing
        peek: function () {
            return heap.length > 0 ? heap[0] : null
        },

        // Get heap size
        size: function () {
            return heap.length
        },

        // Check if heap is empty
        isEmpty: function () {
            return heap.length === 0
        },

        // Build heap from array
        buildHeap: function (array) {
            heap.length = 0 // Clear existing heap
            heap.push(...array) // Add all elements

            // Heapify from bottom up
            for (let i = Math.floor(heap.length / 2) - 1; i >= 0; i--) {
                heapifyDown(i)
            }
        },

        // Get all elements
        toArray: function () {
            return [...heap]
        },
    }
}

// Max Heap implementation
function createMaxHeap() {
    const heap = []

    function getParentIndex(index) {
        return Math.floor((index - 1) / 2)
    }

    function getLeftChildIndex(index) {
        return 2 * index + 1
    }

    function getRightChildIndex(index) {
        return 2 * index + 2
    }

    function swap(index1, index2) {
        ;[heap[index1], heap[index2]] = [heap[index2], heap[index1]]
    }

    function heapifyUp(index) {
        if (index === 0) return

        const parentIndex = getParentIndex(index)

        // For max heap, parent should be larger
        if (heap[index] > heap[parentIndex]) {
            swap(index, parentIndex)
            heapifyUp(parentIndex)
        }
    }

    function heapifyDown(index) {
        const leftChildIndex = getLeftChildIndex(index)
        const rightChildIndex = getRightChildIndex(index)
        let largestIndex = index

        // Find largest among current node and its children
        if (
            leftChildIndex < heap.length &&
            heap[leftChildIndex] > heap[largestIndex]
        ) {
            largestIndex = leftChildIndex
        }

        if (
            rightChildIndex < heap.length &&
            heap[rightChildIndex] > heap[largestIndex]
        ) {
            largestIndex = rightChildIndex
        }

        if (largestIndex !== index) {
            swap(index, largestIndex)
            heapifyDown(largestIndex)
        }
    }

    return {
        insert: function (value) {
            heap.push(value)
            heapifyUp(heap.length - 1)
        },

        extractMax: function () {
            if (heap.length === 0) return null
            if (heap.length === 1) return heap.pop()

            const max = heap[0]
            heap[0] = heap.pop()
            heapifyDown(0)

            return max
        },

        peek: function () {
            return heap.length > 0 ? heap[0] : null
        },

        size: function () {
            return heap.length
        },

        isEmpty: function () {
            return heap.length === 0
        },
    }
}
```

## How would you implement a trie?

```js
// Trie (Prefix Tree) implementation
function createTrie() {
    const root = { children: {}, isEndOfWord: false } // Root node

    // Insert word into trie
    function insert(word) {
        let current = root // Start from root

        // Traverse through each character
        for (const char of word) {
            // Create new node if character doesn't exist
            if (!current.children[char]) {
                current.children[char] = { children: {}, isEndOfWord: false }
            }

            current = current.children[char] // Move to child node
        }

        current.isEndOfWord = true // Mark end of word
    }

    // Search for word in trie
    function search(word) {
        let current = root // Start from root

        // Traverse through each character
        for (const char of word) {
            // If character doesn't exist, word not found
            if (!current.children[char]) {
                return false
            }

            current = current.children[char] // Move to child node
        }

        // Return true only if we reached end of word
        return current.isEndOfWord
    }

    // Check if prefix exists in trie
    function startsWith(prefix) {
        let current = root // Start from root

        // Traverse through each character
        for (const char of prefix) {
            // If character doesn't exist, prefix not found
            if (!current.children[char]) {
                return false
            }

            current = current.children[char] // Move to child node
        }

        return true // Prefix exists
    }

    // Delete word from trie
    function deleteWord(word) {
        function deleteHelper(node, word, index) {
            // If we've processed all characters
            if (index === word.length) {
                // If this is not end of word, word doesn't exist
                if (!node.isEndOfWord) {
                    return false
                }

                node.isEndOfWord = false // Remove end of word marker

                // If node has no children, it can be deleted
                return Object.keys(node.children).length === 0
            }

            const char = word[index]
            const childNode = node.children[char]

            // If character doesn't exist, word not found
            if (!childNode) {
                return false
            }

            // Recursively delete from child
            const shouldDeleteChild = deleteHelper(childNode, word, index + 1)

            if (shouldDeleteChild) {
                delete node.children[char] // Delete child node

                // If current node has no children and is not end of word, it can be deleted
                return (
                    Object.keys(node.children).length === 0 && !node.isEndOfWord
                )
            }

            return false // Don't delete current node
        }

        return deleteHelper(root, word, 0)
    }

    // Get all words with given prefix
    function getWordsWithPrefix(prefix) {
        let current = root
        const words = []

        // Navigate to prefix node
        for (const char of prefix) {
            if (!current.children[char]) {
                return words // Prefix doesn't exist
            }
            current = current.children[char]
        }

        // Collect all words from prefix node
        function collectWords(node, currentWord) {
            if (node.isEndOfWord) {
                words.push(currentWord) // Add complete word
            }

            // Recursively collect from all children
            for (const [char, childNode] of Object.entries(node.children)) {
                collectWords(childNode, currentWord + char)
            }
        }

        collectWords(current, prefix)
        return words
    }

    return {
        insert,
        search,
        startsWith,
        delete: deleteWord,
        getWordsWithPrefix,

        // Get all words in trie
        getAllWords: function () {
            return this.getWordsWithPrefix('')
        },

        // Get trie size (number of words)
        size: function () {
            return this.getAllWords().length
        },
    }
}
```

## How would you implement a graph?

```js
// Graph implementation using adjacency list
function createGraph() {
    const adjacencyList = {} // Object to store adjacency lists

    // Add vertex to graph
    function addVertex(vertex) {
        if (!adjacencyList[vertex]) {
            adjacencyList[vertex] = [] // Initialize empty adjacency list
        }
    }

    // Add edge between two vertices
    function addEdge(vertex1, vertex2, weight = 1) {
        // Add vertices if they don't exist
        addVertex(vertex1)
        addVertex(vertex2)

        // Add edge in both directions (undirected graph)
        adjacencyList[vertex1].push({ vertex: vertex2, weight })
        adjacencyList[vertex2].push({ vertex: vertex1, weight })
    }

    // Add directed edge
    function addDirectedEdge(from, to, weight = 1) {
        addVertex(from)
        addVertex(to)
        adjacencyList[from].push({ vertex: to, weight })
    }

    // Remove edge between two vertices
    function removeEdge(vertex1, vertex2) {
        if (adjacencyList[vertex1]) {
            adjacencyList[vertex1] = adjacencyList[vertex1].filter(
                (edge) => edge.vertex !== vertex2
            )
        }

        if (adjacencyList[vertex2]) {
            adjacencyList[vertex2] = adjacencyList[vertex2].filter(
                (edge) => edge.vertex !== vertex1
            )
        }
    }

    // Remove vertex from graph
    function removeVertex(vertex) {
        // Remove all edges connected to this vertex
        if (adjacencyList[vertex]) {
            for (const neighbor of adjacencyList[vertex]) {
                removeEdge(vertex, neighbor.vertex)
            }

            delete adjacencyList[vertex] // Remove vertex
        }
    }

    // Get neighbors of a vertex
    function getNeighbors(vertex) {
        return adjacencyList[vertex] || []
    }

    // Check if edge exists between two vertices
    function hasEdge(vertex1, vertex2) {
        if (!adjacencyList[vertex1]) return false

        return adjacencyList[vertex1].some((edge) => edge.vertex === vertex2)
    }

    // Get all vertices
    function getVertices() {
        return Object.keys(adjacencyList)
    }

    // Get all edges
    function getEdges() {
        const edges = []
        const visited = new Set()

        for (const vertex of Object.keys(adjacencyList)) {
            for (const edge of adjacencyList[vertex]) {
                const edgeKey = `${vertex}-${edge.vertex}`
                const reverseEdgeKey = `${edge.vertex}-${vertex}`

                // Avoid duplicate edges in undirected graph
                if (!visited.has(edgeKey) && !visited.has(reverseEdgeKey)) {
                    edges.push({
                        from: vertex,
                        to: edge.vertex,
                        weight: edge.weight,
                    })
                    visited.add(edgeKey)
                }
            }
        }

        return edges
    }

    // Depth-First Search
    function dfs(startVertex, visited = new Set()) {
        const result = []

        function dfsHelper(vertex) {
            if (visited.has(vertex)) return

            visited.add(vertex)
            result.push(vertex)

            // Visit all unvisited neighbors
            for (const neighbor of adjacencyList[vertex] || []) {
                dfsHelper(neighbor.vertex)
            }
        }

        dfsHelper(startVertex)
        return result
    }

    // Breadth-First Search
    function bfs(startVertex) {
        const visited = new Set()
        const result = []
        const queue = [startVertex]

        visited.add(startVertex)

        while (queue.length > 0) {
            const vertex = queue.shift()
            result.push(vertex)

            // Add unvisited neighbors to queue
            for (const neighbor of adjacencyList[vertex] || []) {
                if (!visited.has(neighbor.vertex)) {
                    visited.add(neighbor.vertex)
                    queue.push(neighbor.vertex)
                }
            }
        }

        return result
    }

    // Check if graph is connected
    function isConnected() {
        const vertices = Object.keys(adjacencyList)
        if (vertices.length === 0) return true

        const visited = new Set()
        dfs(vertices[0], visited)

        // Graph is connected if all vertices are visited
        return visited.size === vertices.length
    }

    // Find shortest path using BFS (unweighted graph)
    function shortestPath(start, end) {
        if (start === end) return [start]

        const visited = new Set()
        const queue = [[start, [start]]] // [vertex, path]

        visited.add(start)

        while (queue.length > 0) {
            const [vertex, path] = queue.shift()

            for (const neighbor of adjacencyList[vertex] || []) {
                if (neighbor.vertex === end) {
                    return [...path, neighbor.vertex] // Found path
                }

                if (!visited.has(neighbor.vertex)) {
                    visited.add(neighbor.vertex)
                    queue.push([neighbor.vertex, [...path, neighbor.vertex]])
                }
            }
        }

        return null // No path found
    }

    return {
        addVertex,
        addEdge,
        addDirectedEdge,
        removeEdge,
        removeVertex,
        getNeighbors,
        hasEdge,
        getVertices,
        getEdges,
        dfs,
        bfs,
        isConnected,
        shortestPath,

        // Display graph structure
        display: function () {
            for (const vertex of Object.keys(adjacencyList)) {
                console.log(
                    `${vertex}: ${adjacencyList[vertex]
                        .map((edge) => edge.vertex)
                        .join(', ')}`
                )
            }
        },
    }
}
```

## How would you implement Dijkstra's algorithm?

```js
// Dijkstra's algorithm for shortest path in weighted graph
function dijkstra(graph, start) {
    const distances = {} // Store shortest distances from start
    const previous = {} // Store previous node in shortest path
    const unvisited = new Set() // Set of unvisited nodes
    const visited = new Set() // Set of visited nodes

    // Initialize distances
    for (const vertex of Object.keys(graph)) {
        distances[vertex] = vertex === start ? 0 : Infinity
        previous[vertex] = null
        unvisited.add(vertex)
    }

    // Main algorithm loop
    while (unvisited.size > 0) {
        // Find unvisited node with minimum distance
        let current = null
        let minDistance = Infinity

        for (const vertex of unvisited) {
            if (distances[vertex] < minDistance) {
                minDistance = distances[vertex]
                current = vertex
            }
        }

        // If no reachable unvisited nodes, break
        if (current === null) break

        unvisited.delete(current) // Mark as visited
        visited.add(current)

        // Update distances to neighbors
        if (graph[current]) {
            for (const neighbor of graph[current]) {
                const { vertex: neighborVertex, weight } = neighbor

                // Skip if already visited
                if (visited.has(neighborVertex)) continue

                // Calculate new distance
                const newDistance = distances[current] + weight

                // Update if new distance is shorter
                if (newDistance < distances[neighborVertex]) {
                    distances[neighborVertex] = newDistance
                    previous[neighborVertex] = current
                }
            }
        }
    }

    return { distances, previous }
}

// Get shortest path from start to end using Dijkstra's result
function getShortestPath(previous, start, end) {
    const path = []
    let current = end

    // Build path backwards from end to start
    while (current !== null) {
        path.unshift(current) // Add to beginning of path
        current = previous[current]
    }

    // Check if path exists (start should be first element)
    return path[0] === start ? path : null
}
```

## How would you implement the Floyd-Warshall algorithm?

```js
// Floyd-Warshall algorithm for all-pairs shortest paths
function floydWarshall(graph) {
    const vertices = Object.keys(graph)
    const n = vertices.length

    // Create vertex to index mapping
    const vertexToIndex = {}
    vertices.forEach((vertex, index) => {
        vertexToIndex[vertex] = index
    })

    // Initialize distance matrix
    const dist = Array(n)
        .fill()
        .map(() => Array(n).fill(Infinity))
    const next = Array(n)
        .fill()
        .map(() => Array(n).fill(null))

    // Initialize distances
    for (let i = 0; i < n; i++) {
        dist[i][i] = 0 // Distance from vertex to itself is 0
    }

    // Add edge weights to distance matrix
    for (const vertex of vertices) {
        const i = vertexToIndex[vertex]
        if (graph[vertex]) {
            for (const neighbor of graph[vertex]) {
                const j = vertexToIndex[neighbor.vertex]
                dist[i][j] = neighbor.weight
                next[i][j] = j // Next vertex in path
            }
        }
    }

    // Floyd-Warshall algorithm
    for (let k = 0; k < n; k++) {
        for (let i = 0; i < n; i++) {
            for (let j = 0; j < n; j++) {
                // If path through k is shorter, update distance
                if (dist[i][k] + dist[k][j] < dist[i][j]) {
                    dist[i][j] = dist[i][k] + dist[k][j]
                    next[i][j] = next[i][k] // Update next vertex
                }
            }
        }
    }

    // Convert back to vertex names
    const result = {}
    for (let i = 0; i < n; i++) {
        result[vertices[i]] = {}
        for (let j = 0; j < n; j++) {
            result[vertices[i]][vertices[j]] = dist[i][j]
        }
    }

    return { distances: result, next }
}
```

## How would you determine the maximum flow in a network?

```js
// Ford-Fulkerson algorithm for maximum flow
function maxFlow(graph, source, sink) {
    // Create residual graph (copy of original graph)
    const residualGraph = {}
    for (const vertex of Object.keys(graph)) {
        residualGraph[vertex] = [...(graph[vertex] || [])]
    }

    let maxFlow = 0
    const parent = {} // Store parent for path reconstruction

    // BFS to find augmenting path
    function bfs(start, end) {
        const visited = new Set()
        const queue = [start]

        visited.add(start)
        parent[start] = null

        while (queue.length > 0) {
            const current = queue.shift()

            if (current === end) {
                return true // Path found
            }

            // Check all neighbors
            if (residualGraph[current]) {
                for (const edge of residualGraph[current]) {
                    const { vertex: neighbor, weight } = edge

                    // If neighbor not visited and has capacity
                    if (!visited.has(neighbor) && weight > 0) {
                        visited.add(neighbor)
                        parent[neighbor] = current
                        queue.push(neighbor)
                    }
                }
            }
        }

        return false // No path found
    }

    // Find minimum capacity in augmenting path
    function getMinCapacity(start, end) {
        let minCapacity = Infinity
        let current = end

        while (current !== start) {
            const prev = parent[current]

            // Find edge from prev to current
            const edge = residualGraph[prev].find((e) => e.vertex === current)
            minCapacity = Math.min(minCapacity, edge.weight)

            current = prev
        }

        return minCapacity
    }

    // Update residual graph with flow
    function updateResidualGraph(start, end, flow) {
        let current = end

        while (current !== start) {
            const prev = parent[current]

            // Reduce capacity of forward edge
            const forwardEdge = residualGraph[prev].find(
                (e) => e.vertex === current
            )
            forwardEdge.weight -= flow

            // Add or increase capacity of backward edge
            let backwardEdge = residualGraph[current].find(
                (e) => e.vertex === prev
            )
            if (backwardEdge) {
                backwardEdge.weight += flow
            } else {
                residualGraph[current].push({ vertex: prev, weight: flow })
            }

            current = prev
        }
    }

    // Main algorithm
    while (bfs(source, sink)) {
        const flow = getMinCapacity(source, sink)
        maxFlow += flow
        updateResidualGraph(source, sink, flow)
    }

    return maxFlow
}
```

## How would you implement a minimum spanning tree algorithm?

```js
// Kruskal's algorithm for minimum spanning tree
function kruskalMST(graph) {
    const edges = []
    const vertices = Object.keys(graph)
    const mst = []

    // Collect all edges
    for (const vertex of vertices) {
        if (graph[vertex]) {
            for (const edge of graph[vertex]) {
                // Avoid duplicate edges in undirected graph
                if (vertex < edge.vertex) {
                    edges.push({
                        from: vertex,
                        to: edge.vertex,
                        weight: edge.weight,
                    })
                }
            }
        }
    }

    // Sort edges by weight
    edges.sort((a, b) => a.weight - b.weight)

    // Union-Find data structure
    const parent = {}
    const rank = {}

    // Initialize each vertex as its own parent
    for (const vertex of vertices) {
        parent[vertex] = vertex
        rank[vertex] = 0
    }

    // Find root of vertex
    function find(vertex) {
        if (parent[vertex] !== vertex) {
            parent[vertex] = find(parent[vertex]) // Path compression
        }
        return parent[vertex]
    }

    // Union two sets
    function union(vertex1, vertex2) {
        const root1 = find(vertex1)
        const root2 = find(vertex2)

        if (root1 === root2) return false // Already in same set

        // Union by rank
        if (rank[root1] < rank[root2]) {
            parent[root1] = root2
        } else if (rank[root1] > rank[root2]) {
            parent[root2] = root1
        } else {
            parent[root2] = root1
            rank[root1]++
        }

        return true // Successfully united
    }

    // Process edges in order of weight
    for (const edge of edges) {
        if (union(edge.from, edge.to)) {
            mst.push(edge) // Add edge to MST
        }

        // Stop when we have n-1 edges (MST is complete)
        if (mst.length === vertices.length - 1) {
            break
        }
    }

    return mst
}

// Prim's algorithm for minimum spanning tree
function primMST(graph, start) {
    const vertices = Object.keys(graph)
    const mst = []
    const visited = new Set()
    const minHeap = [] // Priority queue for edges

    // Add start vertex to visited set
    visited.add(start)

    // Add all edges from start vertex to heap
    if (graph[start]) {
        for (const edge of graph[start]) {
            minHeap.push({
                from: start,
                to: edge.vertex,
                weight: edge.weight,
            })
        }
    }

    // Sort heap by weight (simple implementation)
    minHeap.sort((a, b) => a.weight - b.weight)

    // Main algorithm
    while (visited.size < vertices.length && minHeap.length > 0) {
        // Get edge with minimum weight
        const edge = minHeap.shift()

        // If destination is not visited, add edge to MST
        if (!visited.has(edge.to)) {
            mst.push(edge)
            visited.add(edge.to)

            // Add new edges from newly visited vertex
            if (graph[edge.to]) {
                for (const neighbor of graph[edge.to]) {
                    if (!visited.has(neighbor.vertex)) {
                        minHeap.push({
                            from: edge.to,
                            to: neighbor.vertex,
                            weight: neighbor.weight,
                        })
                    }
                }
            }

            // Re-sort heap
            minHeap.sort((a, b) => a.weight - b.weight)
        }
    }

    return mst
}
```

## How would you implement a topological sort algorithm?

```js
// Topological sort using DFS
function topologicalSort(graph) {
    const visited = new Set()
    const stack = []
    const vertices = Object.keys(graph)

    // DFS helper function
    function dfs(vertex) {
        visited.add(vertex)

        // Visit all neighbors
        if (graph[vertex]) {
            for (const neighbor of graph[vertex]) {
                if (!visited.has(neighbor.vertex)) {
                    dfs(neighbor.vertex)
                }
            }
        }

        // Push vertex to stack after visiting all neighbors
        stack.push(vertex)
    }

    // Process all vertices
    for (const vertex of vertices) {
        if (!visited.has(vertex)) {
            dfs(vertex)
        }
    }

    // Return topological order (reverse of stack)
    return stack.reverse()
}

// Topological sort using Kahn's algorithm (BFS-based)
function topologicalSortKahn(graph) {
    const inDegree = {}
    const queue = []
    const result = []
    const vertices = Object.keys(graph)

    // Initialize in-degree for all vertices
    for (const vertex of vertices) {
        inDegree[vertex] = 0
    }

    // Calculate in-degrees
    for (const vertex of vertices) {
        if (graph[vertex]) {
            for (const neighbor of graph[vertex]) {
                inDegree[neighbor.vertex]++
            }
        }
    }

    // Add vertices with in-degree 0 to queue
    for (const vertex of vertices) {
        if (inDegree[vertex] === 0) {
            queue.push(vertex)
        }
    }

    // Process queue
    while (queue.length > 0) {
        const current = queue.shift()
        result.push(current)

        // Reduce in-degree of neighbors
        if (graph[current]) {
            for (const neighbor of graph[current]) {
                inDegree[neighbor.vertex]--

                // If in-degree becomes 0, add to queue
                if (inDegree[neighbor.vertex] === 0) {
                    queue.push(neighbor.vertex)
                }
            }
        }
    }

    // Check if all vertices were processed (no cycle)
    return result.length === vertices.length ? result : null
}
```

## How would you implement a maximum bipartite matching algorithm?

```js
// Maximum bipartite matching using DFS
function maxBipartiteMatching(graph, leftVertices, rightVertices) {
    const matchR = {} // Array to store right vertex assignments
    const visited = {} // Array to mark visited vertices

    // Initialize matchR with -1 (no match)
    for (const vertex of rightVertices) {
        matchR[vertex] = -1
    }

    // DFS function to find augmenting path
    function bpm(u) {
        // Try all right vertices one by one
        for (const v of rightVertices) {
            // If there's an edge from u to v and v is not visited
            if (graph[u] && graph[u].includes(v) && !visited[v]) {
                visited[v] = true // Mark v as visited

                // If v is not assigned or previously assigned vertex has alternate match
                if (matchR[v] === -1 || bpm(matchR[v])) {
                    matchR[v] = u // Assign v to u
                    return true
                }
            }
        }
        return false
    }

    let result = 0 // Count of maximum matching

    // For each left vertex, try to find a match
    for (const u of leftVertices) {
        // Mark all right vertices as not visited for next iteration
        for (const vertex of rightVertices) {
            visited[vertex] = false
        }

        // Find augmenting path starting from u
        if (bpm(u)) {
            result++
        }
    }

    return result
}

// Hopcroft-Karp algorithm for maximum bipartite matching (more efficient)
function hopcroftKarp(graph, leftVertices, rightVertices) {
    const pairU = {} // Pair of left vertices
    const pairV = {} // Pair of right vertices
    const dist = {} // Distance array for BFS

    // Initialize pairs
    for (const u of leftVertices) {
        pairU[u] = -1
    }
    for (const v of rightVertices) {
        pairV[v] = -1
    }

    // BFS to find shortest augmenting paths
    function bfs() {
        const queue = []

        // Initialize distances
        for (const u of leftVertices) {
            if (pairU[u] === -1) {
                dist[u] = 0
                queue.push(u)
            } else {
                dist[u] = Infinity
            }
        }

        dist[-1] = Infinity // Dummy vertex

        while (queue.length > 0) {
            const u = queue.shift()

            if (dist[u] < dist[-1]) {
                // Try all right vertices
                for (const v of rightVertices) {
                    if (graph[u] && graph[u].includes(v)) {
                        if (dist[pairV[v]] === Infinity) {
                            dist[pairV[v]] = dist[u] + 1
                            queue.push(pairV[v])
                        }
                    }
                }
            }
        }

        return dist[-1] !== Infinity
    }

    // DFS to find augmenting path
    function dfs(u) {
        if (u !== -1) {
            for (const v of rightVertices) {
                if (graph[u] && graph[u].includes(v)) {
                    if (dist[pairV[v]] === dist[u] + 1) {
                        if (dfs(pairV[v])) {
                            pairU[u] = v
                            pairV[v] = u
                            return true
                        }
                    }
                }
            }
            dist[u] = Infinity
            return false
        }
        return true
    }

    let result = 0

    // While there are augmenting paths
    while (bfs()) {
        for (const u of leftVertices) {
            if (pairU[u] === -1) {
                if (dfs(u)) {
                    result++
                }
            }
        }
    }

    return result
}
```

## How would you implement a knapsack problem algorithm?

```js
// 0-1 Knapsack using dynamic programming
function knapsack01(weights, values, capacity) {
    const n = weights.length
    const dp = Array(n + 1)
        .fill()
        .map(() => Array(capacity + 1).fill(0))

    // Build DP table
    for (let i = 1; i <= n; i++) {
        for (let w = 1; w <= capacity; w++) {
            // If current item's weight is less than or equal to current capacity
            if (weights[i - 1] <= w) {
                // Take maximum of including or excluding current item
                dp[i][w] = Math.max(
                    values[i - 1] + dp[i - 1][w - weights[i - 1]], // Include item
                    dp[i - 1][w] // Exclude item
                )
            } else {
                // Can't include current item
                dp[i][w] = dp[i - 1][w]
            }
        }
    }

    return dp[n][capacity] // Maximum value
}

// 0-1 Knapsack with items included
function knapsack01WithItems(weights, values, capacity) {
    const n = weights.length
    const dp = Array(n + 1)
        .fill()
        .map(() => Array(capacity + 1).fill(0))

    // Build DP table
    for (let i = 1; i <= n; i++) {
        for (let w = 1; w <= capacity; w++) {
            if (weights[i - 1] <= w) {
                dp[i][w] = Math.max(
                    values[i - 1] + dp[i - 1][w - weights[i - 1]],
                    dp[i - 1][w]
                )
            } else {
                dp[i][w] = dp[i - 1][w]
            }
        }
    }

    // Backtrack to find items included
    const items = []
    let i = n,
        w = capacity

    while (i > 0 && w > 0) {
        if (dp[i][w] !== dp[i - 1][w]) {
            items.push(i - 1) // Item i-1 is included
            w -= weights[i - 1]
        }
        i--
    }

    return { maxValue: dp[n][capacity], items: items.reverse() }
}

// Unbounded Knapsack (can use items multiple times)
function unboundedKnapsack(weights, values, capacity) {
    const dp = Array(capacity + 1).fill(0)

    // For each capacity from 1 to W
    for (let w = 1; w <= capacity; w++) {
        // Try each item
        for (let i = 0; i < weights.length; i++) {
            if (weights[i] <= w) {
                // Take maximum value by including item i
                dp[w] = Math.max(dp[w], values[i] + dp[w - weights[i]])
            }
        }
    }

    return dp[capacity]
}

// Fractional Knapsack (Greedy approach)
function fractionalKnapsack(weights, values, capacity) {
    const n = weights.length
    const items = []

    // Create array of items with value-to-weight ratio
    for (let i = 0; i < n; i++) {
        items.push({
            weight: weights[i],
            value: values[i],
            ratio: values[i] / weights[i],
            index: i,
        })
    }

    // Sort by value-to-weight ratio in descending order
    items.sort((a, b) => b.ratio - a.ratio)

    let totalValue = 0
    let remainingCapacity = capacity
    const selectedItems = []

    // Greedily select items
    for (const item of items) {
        if (remainingCapacity >= item.weight) {
            // Take whole item
            totalValue += item.value
            remainingCapacity -= item.weight
            selectedItems.push({ index: item.index, fraction: 1.0 })
        } else if (remainingCapacity > 0) {
            // Take fraction of item
            const fraction = remainingCapacity / item.weight
            totalValue += item.value * fraction
            selectedItems.push({ index: item.index, fraction })
            remainingCapacity = 0
            break
        }
    }

    return { maxValue: totalValue, items: selectedItems }
}
```

## How would you implement a dynamic programming solution to the longest increasing subsequence problem?

```js
// Longest Increasing Subsequence using DP
function longestIncreasingSubsequence(nums) {
    if (nums.length === 0) return 0

    const n = nums.length
    const dp = Array(n).fill(1) // dp[i] = length of LIS ending at index i

    // For each element, check all previous elements
    for (let i = 1; i < n; i++) {
        for (let j = 0; j < i; j++) {
            // If current element is greater than previous element
            if (nums[i] > nums[j]) {
                // Update LIS length if including current element gives longer sequence
                dp[i] = Math.max(dp[i], dp[j] + 1)
            }
        }
    }

    // Return maximum LIS length
    return Math.max(...dp)
}

// LIS with actual sequence
function longestIncreasingSubsequenceWithSequence(nums) {
    if (nums.length === 0) return { length: 0, sequence: [] }

    const n = nums.length
    const dp = Array(n).fill(1)
    const parent = Array(n).fill(-1) // Track parent for sequence reconstruction

    // Build DP table
    for (let i = 1; i < n; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[i] > nums[j] && dp[j] + 1 > dp[i]) {
                dp[i] = dp[j] + 1
                parent[i] = j // Set parent
            }
        }
    }

    // Find index with maximum LIS length
    let maxLength = Math.max(...dp)
    let maxIndex = dp.indexOf(maxLength)

    // Reconstruct sequence
    const sequence = []
    while (maxIndex !== -1) {
        sequence.unshift(nums[maxIndex])
        maxIndex = parent[maxIndex]
    }

    return { length: maxLength, sequence }
}

// LIS using Binary Search (O(n log n))
function longestIncreasingSubsequenceBinarySearch(nums) {
    if (nums.length === 0) return 0

    const tails = [] // tails[i] = smallest tail of all increasing subsequences of length i+1

    for (const num of nums) {
        // Binary search for the position to replace
        let left = 0,
            right = tails.length

        while (left < right) {
            const mid = Math.floor((left + right) / 2)
            if (tails[mid] < num) {
                left = mid + 1
            } else {
                right = mid
            }
        }

        // If num is larger than all elements in tails, append it
        if (left === tails.length) {
            tails.push(num)
        } else {
            // Otherwise, replace the first element >= num
            tails[left] = num
        }
    }

    return tails.length
}

// Longest Decreasing Subsequence
function longestDecreasingSubsequence(nums) {
    // Reverse the array and find LIS
    const reversed = [...nums].reverse()
    return longestIncreasingSubsequence(reversed)
}

// Longest Bitonic Subsequence (increases then decreases)
function longestBitonicSubsequence(nums) {
    const n = nums.length
    const inc = Array(n).fill(1) // LIS ending at i
    const dec = Array(n).fill(1) // LDS starting at i

    // Calculate LIS
    for (let i = 1; i < n; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                inc[i] = Math.max(inc[i], inc[j] + 1)
            }
        }
    }

    // Calculate LDS
    for (let i = n - 2; i >= 0; i--) {
        for (let j = n - 1; j > i; j--) {
            if (nums[i] > nums[j]) {
                dec[i] = Math.max(dec[i], dec[j] + 1)
            }
        }
    }

    // Find maximum bitonic sequence length
    let maxLength = 0
    for (let i = 0; i < n; i++) {
        maxLength = Math.max(maxLength, inc[i] + dec[i] - 1)
    }

    return maxLength
}
```

## How would you implement a dynamic programming solution to the coin change problem?

```js
// Coin Change - Minimum number of coins
function coinChange(coins, amount) {
    const dp = Array(amount + 1).fill(Infinity)
    dp[0] = 0 // Base case: 0 coins needed for amount 0

    // For each amount from 1 to target amount
    for (let i = 1; i <= amount; i++) {
        // Try each coin
        for (const coin of coins) {
            if (coin <= i) {
                // Take minimum of current value and 1 + dp[i - coin]
                dp[i] = Math.min(dp[i], 1 + dp[i - coin])
            }
        }
    }

    return dp[amount] === Infinity ? -1 : dp[amount]
}

// Coin Change - Number of ways to make amount
function coinChangeWays(coins, amount) {
    const dp = Array(amount + 1).fill(0)
    dp[0] = 1 // Base case: 1 way to make amount 0 (use no coins)

    // For each coin
    for (const coin of coins) {
        // For each amount from coin to target amount
        for (let i = coin; i <= amount; i++) {
            // Add ways to make (i - coin) to current ways
            dp[i] += dp[i - coin]
        }
    }

    return dp[amount]
}

// Coin Change with actual coins used
function coinChangeWithCoins(coins, amount) {
    const dp = Array(amount + 1).fill(Infinity)
    const parent = Array(amount + 1).fill(-1)
    dp[0] = 0

    for (let i = 1; i <= amount; i++) {
        for (const coin of coins) {
            if (coin <= i && dp[i - coin] + 1 < dp[i]) {
                dp[i] = dp[i - coin] + 1
                parent[i] = coin // Track which coin was used
            }
        }
    }

    if (dp[amount] === Infinity) {
        return { minCoins: -1, coins: [] }
    }

    // Reconstruct coins used
    const coinsUsed = []
    let current = amount
    while (current > 0) {
        const coin = parent[current]
        coinsUsed.push(coin)
        current -= coin
    }

    return { minCoins: dp[amount], coins: coinsUsed }
}

// Coin Change with limited supply
function coinChangeLimited(coins, amounts, target) {
    const dp = Array(target + 1).fill(Infinity)
    dp[0] = 0

    // For each coin type
    for (let i = 0; i < coins.length; i++) {
        const coin = coins[i]
        const maxUse = amounts[i]

        // Use each coin up to its limit
        for (let use = 1; use <= maxUse; use++) {
            const totalValue = coin * use

            // Update DP for all amounts >= totalValue
            for (let j = target; j >= totalValue; j--) {
                if (dp[j - totalValue] !== Infinity) {
                    dp[j] = Math.min(dp[j], dp[j - totalValue] + use)
                }
            }
        }
    }

    return dp[target] === Infinity ? -1 : dp[target]
}
```

## How would you implement a dynamic programming solution to the rod cutting problem?

```js
// Rod Cutting - Maximum profit
function rodCutting(prices, length) {
    const dp = Array(length + 1).fill(0)

    // For each possible rod length
    for (let i = 1; i <= length; i++) {
        let maxProfit = 0

        // Try all possible cuts
        for (let j = 1; j <= i; j++) {
            // Take maximum profit from cutting rod of length j and remaining length
            maxProfit = Math.max(maxProfit, prices[j - 1] + dp[i - j])
        }

        dp[i] = maxProfit
    }

    return dp[length]
}

// Rod Cutting with actual cuts
function rodCuttingWithCuts(prices, length) {
    const dp = Array(length + 1).fill(0)
    const cuts = Array(length + 1).fill(0) // Track optimal cut for each length

    for (let i = 1; i <= length; i++) {
        let maxProfit = 0
        let bestCut = 0

        for (let j = 1; j <= i; j++) {
            const profit = prices[j - 1] + dp[i - j]
            if (profit > maxProfit) {
                maxProfit = profit
                bestCut = j
            }
        }

        dp[i] = maxProfit
        cuts[i] = bestCut
    }

    // Reconstruct cuts
    const cutSequence = []
    let remaining = length
    while (remaining > 0) {
        const cut = cuts[remaining]
        cutSequence.push(cut)
        remaining -= cut
    }

    return { maxProfit: dp[length], cuts: cutSequence }
}

// Rod Cutting with cost per cut
function rodCuttingWithCost(prices, length, costPerCut) {
    const dp = Array(length + 1).fill(0)

    for (let i = 1; i <= length; i++) {
        let maxProfit = prices[i - 1] // Don't cut at all

        // Try all possible cuts
        for (let j = 1; j < i; j++) {
            // Profit from cutting at j minus cost of cut
            const profit = dp[j] + dp[i - j] - costPerCut
            maxProfit = Math.max(maxProfit, profit)
        }

        dp[i] = maxProfit
    }

    return dp[length]
}
```

## How would you implement a dynamic programming solution to the matrix chain multiplication problem?

```js
// Matrix Chain Multiplication - Minimum scalar multiplications
function matrixChainMultiplication(dimensions) {
    const n = dimensions.length - 1 // Number of matrices
    const dp = Array(n)
        .fill()
        .map(() => Array(n).fill(0))

    // For each chain length from 2 to n
    for (let length = 2; length <= n; length++) {
        // For each starting position
        for (let i = 0; i < n - length + 1; i++) {
            const j = i + length - 1 // Ending position
            dp[i][j] = Infinity

            // Try all possible split positions
            for (let k = i; k < j; k++) {
                // Cost of multiplying matrices from i to k and k+1 to j
                const cost =
                    dp[i][k] +
                    dp[k + 1][j] +
                    dimensions[i] * dimensions[k + 1] * dimensions[j + 1]

                // Take minimum cost
                dp[i][j] = Math.min(dp[i][j], cost)
            }
        }
    }

    return dp[0][n - 1] // Minimum cost to multiply all matrices
}

// Matrix Chain Multiplication with optimal parenthesization
function matrixChainMultiplicationWithParentheses(dimensions) {
    const n = dimensions.length - 1
    const dp = Array(n)
        .fill()
        .map(() => Array(n).fill(0))
    const split = Array(n)
        .fill()
        .map(() => Array(n).fill(0))

    for (let length = 2; length <= n; length++) {
        for (let i = 0; i < n - length + 1; i++) {
            const j = i + length - 1
            dp[i][j] = Infinity

            for (let k = i; k < j; k++) {
                const cost =
                    dp[i][k] +
                    dp[k + 1][j] +
                    dimensions[i] * dimensions[k + 1] * dimensions[j + 1]

                if (cost < dp[i][j]) {
                    dp[i][j] = cost
                    split[i][j] = k // Record optimal split position
                }
            }
        }
    }

    // Reconstruct optimal parenthesization
    function getParentheses(i, j) {
        if (i === j) {
            return `A${i}`
        }

        const k = split[i][j]
        return `(${getParentheses(i, k)} × ${getParentheses(k + 1, j)})`
    }

    return {
        minCost: dp[0][n - 1],
        parenthesization: getParentheses(0, n - 1),
    }
}
```

## How would you implement a dynamic programming solution to the longest common subsequence problem?

```js
// Longest Common Subsequence
function longestCommonSubsequence(text1, text2) {
    const m = text1.length
    const n = text2.length
    const dp = Array(m + 1)
        .fill()
        .map(() => Array(n + 1).fill(0))

    // Build DP table
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (text1[i - 1] === text2[j - 1]) {
                // Characters match, extend LCS
                dp[i][j] = dp[i - 1][j - 1] + 1
            } else {
                // Characters don't match, take maximum from previous
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1])
            }
        }
    }

    return dp[m][n] // Length of LCS
}

// LCS with actual subsequence
function longestCommonSubsequenceWithSequence(text1, text2) {
    const m = text1.length
    const n = text2.length
    const dp = Array(m + 1)
        .fill()
        .map(() => Array(n + 1).fill(0))

    // Build DP table
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (text1[i - 1] === text2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + 1
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1])
            }
        }
    }

    // Reconstruct LCS
    const lcs = []
    let i = m,
        j = n

    while (i > 0 && j > 0) {
        if (text1[i - 1] === text2[j - 1]) {
            lcs.unshift(text1[i - 1]) // Add character to beginning
            i--
            j--
        } else if (dp[i - 1][j] > dp[i][j - 1]) {
            i--
        } else {
            j--
        }
    }

    return {
        length: dp[m][n],
        sequence: lcs.join(''),
    }
}

// Space-optimized LCS (O(min(m,n)) space)
function longestCommonSubsequenceOptimized(text1, text2) {
    // Make text1 the shorter string
    if (text1.length > text2.length) {
        ;[text1, text2] = [text2, text1]
    }

    const m = text1.length
    const n = text2.length
    const prev = Array(m + 1).fill(0)
    const curr = Array(m + 1).fill(0)

    for (let j = 1; j <= n; j++) {
        for (let i = 1; i <= m; i++) {
            if (text1[i - 1] === text2[j - 1]) {
                curr[i] = prev[i - 1] + 1
            } else {
                curr[i] = Math.max(prev[i], curr[i - 1])
            }
        }

        // Swap arrays
        ;[prev, curr] = [curr, prev]
    }

    return prev[m]
}
```

## How would you implement a dynamic programming solution to the edit distance problem?

```js
// Edit Distance (Levenshtein Distance)
function editDistance(word1, word2) {
    const m = word1.length
    const n = word2.length
    const dp = Array(m + 1)
        .fill()
        .map(() => Array(n + 1).fill(0))

    // Initialize base cases
    for (let i = 0; i <= m; i++) {
        dp[i][0] = i // Delete all characters from word1
    }
    for (let j = 0; j <= n; j++) {
        dp[0][j] = j // Insert all characters to word1
    }

    // Fill DP table
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (word1[i - 1] === word2[j - 1]) {
                // Characters match, no operation needed
                dp[i][j] = dp[i - 1][j - 1]
            } else {
                // Take minimum of insert, delete, or replace
                dp[i][j] =
                    1 +
                    Math.min(
                        dp[i - 1][j], // Delete from word1
                        dp[i][j - 1], // Insert into word1
                        dp[i - 1][j - 1] // Replace in word1
                    )
            }
        }
    }

    return dp[m][n] // Minimum edit distance
}

// Edit Distance with operations
function editDistanceWithOperations(word1, word2) {
    const m = word1.length
    const n = word2.length
    const dp = Array(m + 1)
        .fill()
        .map(() => Array(n + 1).fill(0))
    const operations = Array(m + 1)
        .fill()
        .map(() => Array(n + 1).fill(''))

    // Initialize base cases
    for (let i = 0; i <= m; i++) {
        dp[i][0] = i
        operations[i][0] = 'D'.repeat(i) // Delete operations
    }
    for (let j = 0; j <= n; j++) {
        dp[0][j] = j
        operations[0][j] = 'I'.repeat(j) // Insert operations
    }

    // Fill DP table
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (word1[i - 1] === word2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1]
                operations[i][j] = operations[i - 1][j - 1] + 'M' // Match
            } else {
                const deleteCost = dp[i - 1][j] + 1
                const insertCost = dp[i][j - 1] + 1
                const replaceCost = dp[i - 1][j - 1] + 1

                if (deleteCost <= insertCost && deleteCost <= replaceCost) {
                    dp[i][j] = deleteCost
                    operations[i][j] = operations[i - 1][j] + 'D' // Delete
                } else if (insertCost <= replaceCost) {
                    dp[i][j] = insertCost
                    operations[i][j] = operations[i][j - 1] + 'I' // Insert
                } else {
                    dp[i][j] = replaceCost
                    operations[i][j] = operations[i - 1][j - 1] + 'R' // Replace
                }
            }
        }
    }

    return {
        distance: dp[m][n],
        operations: operations[m][n],
    }
}

// Space-optimized Edit Distance
function editDistanceOptimized(word1, word2) {
    const m = word1.length
    const n = word2.length

    // Make word1 the shorter string
    if (m > n) {
        ;[word1, word2] = [word2, word1]
        ;[m, n] = [n, m]
    }

    const prev = Array(m + 1).fill(0)
    const curr = Array(m + 1).fill(0)

    // Initialize first row
    for (let i = 0; i <= m; i++) {
        prev[i] = i
    }

    // Fill DP table
    for (let j = 1; j <= n; j++) {
        curr[0] = j

        for (let i = 1; i <= m; i++) {
            if (word1[i - 1] === word2[j - 1]) {
                curr[i] = prev[i - 1]
            } else {
                curr[i] = 1 + Math.min(prev[i], curr[i - 1], prev[i - 1])
            }
        }

        // Swap arrays
        ;[prev, curr] = [curr, prev]
    }

    return prev[m]
}
```

## How would you implement a dynamic programming solution to the longest palindromic subsequence problem?

```js
// Longest Palindromic Subsequence
function longestPalindromicSubsequence(s) {
    const n = s.length
    const dp = Array(n)
        .fill()
        .map(() => Array(n).fill(0))

    // Every single character is a palindrome of length 1
    for (let i = 0; i < n; i++) {
        dp[i][i] = 1
    }

    // For substrings of length 2 to n
    for (let length = 2; length <= n; length++) {
        for (let i = 0; i < n - length + 1; i++) {
            const j = i + length - 1

            if (s[i] === s[j]) {
                // Characters match, add 2 to inner subsequence
                dp[i][j] = dp[i + 1][j - 1] + 2
            } else {
                // Characters don't match, take maximum from excluding either end
                dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1])
            }
        }
    }

    return dp[0][n - 1] // Length of longest palindromic subsequence
}

// LPS with actual subsequence
function longestPalindromicSubsequenceWithSequence(s) {
    const n = s.length
    const dp = Array(n)
        .fill()
        .map(() => Array(n).fill(0))

    // Initialize base cases
    for (let i = 0; i < n; i++) {
        dp[i][i] = 1
    }

    // Fill DP table
    for (let length = 2; length <= n; length++) {
        for (let i = 0; i < n - length + 1; i++) {
            const j = i + length - 1

            if (s[i] === s[j]) {
                dp[i][j] = dp[i + 1][j - 1] + 2
            } else {
                dp[i][j] = Math.max(dp[i + 1][j], dp[i][j - 1])
            }
        }
    }

    // Reconstruct palindromic subsequence
    const left = []
    const right = []
    let i = 0,
        j = n - 1

    while (i <= j) {
        if (s[i] === s[j]) {
            left.push(s[i])
            if (i !== j) {
                right.unshift(s[j])
            }
            i++
            j--
        } else if (dp[i + 1][j] > dp[i][j - 1]) {
            i++
        } else {
            j--
        }
    }

    return {
        length: dp[0][n - 1],
        sequence: left.join('') + right.join(''),
    }
}

// Minimum deletions to make string palindrome
function minDeletionsToPalindrome(s) {
    const lpsLength = longestPalindromicSubsequence(s)
    return s.length - lpsLength
}

// Minimum insertions to make string palindrome
function minInsertionsToPalindrome(s) {
    return minDeletionsToPalindrome(s) // Same as deletions
}
```

## How would you implement a dynamic programming solution to the longest common substring problem?

```js
// Longest Common Substring
function longestCommonSubstring(text1, text2) {
    const m = text1.length
    const n = text2.length
    const dp = Array(m + 1)
        .fill()
        .map(() => Array(n + 1).fill(0))
    let maxLength = 0
    let endIndex = 0

    // Build DP table
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (text1[i - 1] === text2[j - 1]) {
                // Characters match, extend substring
                dp[i][j] = dp[i - 1][j - 1] + 1

                // Update maximum length and end index
                if (dp[i][j] > maxLength) {
                    maxLength = dp[i][j]
                    endIndex = i - 1
                }
            } else {
                // Characters don't match, reset to 0
                dp[i][j] = 0
            }
        }
    }

    return {
        length: maxLength,
        substring: text1.substring(endIndex - maxLength + 1, endIndex + 1),
    }
}

// Space-optimized Longest Common Substring
function longestCommonSubstringOptimized(text1, text2) {
    const m = text1.length
    const n = text2.length

    // Make text1 the shorter string
    if (m > n) {
        ;[text1, text2] = [text2, text1]
        ;[m, n] = [n, m]
    }

    const prev = Array(m + 1).fill(0)
    const curr = Array(m + 1).fill(0)
    let maxLength = 0
    let endIndex = 0

    for (let j = 1; j <= n; j++) {
        for (let i = 1; i <= m; i++) {
            if (text1[i - 1] === text2[j - 1]) {
                curr[i] = prev[i - 1] + 1

                if (curr[i] > maxLength) {
                    maxLength = curr[i]
                    endIndex = i - 1
                }
            } else {
                curr[i] = 0
            }
        }

        // Swap arrays
        ;[prev, curr] = [curr, prev]
    }

    return {
        length: maxLength,
        substring: text1.substring(endIndex - maxLength + 1, endIndex + 1),
    }
}
```

## How would you implement a dynamic programming solution to the longest zigzag subsequence problem?

```js
// Longest Zigzag Subsequence
function longestZigzagSubsequence(nums) {
    if (nums.length <= 2) return nums.length

    const n = nums.length
    const up = Array(n).fill(1) // Length of zigzag ending with up trend
    const down = Array(n).fill(1) // Length of zigzag ending with down trend

    for (let i = 1; i < n; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[i] > nums[j]) {
                // Current element is greater, extend down trend
                up[i] = Math.max(up[i], down[j] + 1)
            } else if (nums[i] < nums[j]) {
                // Current element is smaller, extend up trend
                down[i] = Math.max(down[i], up[j] + 1)
            }
        }
    }

    return Math.max(...up, ...down)
}

// Longest Zigzag Subsequence with actual sequence
function longestZigzagSubsequenceWithSequence(nums) {
    if (nums.length <= 2) return { length: nums.length, sequence: nums }

    const n = nums.length
    const up = Array(n).fill(1)
    const down = Array(n).fill(1)
    const upParent = Array(n).fill(-1)
    const downParent = Array(n).fill(-1)

    for (let i = 1; i < n; i++) {
        for (let j = 0; j < i; j++) {
            if (nums[i] > nums[j] && down[j] + 1 > up[i]) {
                up[i] = down[j] + 1
                upParent[i] = j
            } else if (nums[i] < nums[j] && up[j] + 1 > down[i]) {
                down[i] = up[j] + 1
                downParent[i] = j
            }
        }
    }

    // Find maximum length and starting index
    let maxLength = 0
    let startIndex = -1
    let isUp = false

    for (let i = 0; i < n; i++) {
        if (up[i] > maxLength) {
            maxLength = up[i]
            startIndex = i
            isUp = true
        }
        if (down[i] > maxLength) {
            maxLength = down[i]
            startIndex = i
            isUp = false
        }
    }

    // Reconstruct sequence
    const sequence = []
    let current = startIndex
    let currentIsUp = isUp

    while (current !== -1) {
        sequence.unshift(nums[current])

        if (currentIsUp) {
            current = upParent[current]
        } else {
            current = downParent[current]
        }
        currentIsUp = !currentIsUp
    }

    return { length: maxLength, sequence }
}
```

## How would you implement a dynamic programming solution to the longest even-odd subarray problem?

```js
// Longest Even-Odd Subarray
function longestEvenOddSubarray(nums) {
    if (nums.length === 0) return 0

    const n = nums.length
    let maxLength = 1
    let currentLength = 1

    for (let i = 1; i < n; i++) {
        // Check if current and previous elements have different parity
        if (nums[i] % 2 !== nums[i - 1] % 2) {
            currentLength++
            maxLength = Math.max(maxLength, currentLength)
        } else {
            currentLength = 1 // Reset current length
        }
    }

    return maxLength
}

// Longest Even-Odd Subarray with actual subarray
function longestEvenOddSubarrayWithSequence(nums) {
    if (nums.length === 0) return { length: 0, subarray: [] }

    const n = nums.length
    let maxLength = 1
    let currentLength = 1
    let startIndex = 0
    let maxStartIndex = 0

    for (let i = 1; i < n; i++) {
        if (nums[i] % 2 !== nums[i - 1] % 2) {
            currentLength++
            if (currentLength > maxLength) {
                maxLength = currentLength
                maxStartIndex = startIndex
            }
        } else {
            currentLength = 1
            startIndex = i
        }
    }

    return {
        length: maxLength,
        subarray: nums.slice(maxStartIndex, maxStartIndex + maxLength),
    }
}
```

## How would you implement a dynamic programming solution to the minimum deletion to make the sum of the array divisible by k problem?

```js
// Minimum deletions to make sum divisible by k
function minDeletionsForDivisibleSum(nums, k) {
    const n = nums.length
    const sum = nums.reduce((acc, num) => acc + num, 0)
    const remainder = sum % k

    if (remainder === 0) return 0 // Already divisible

    // dp[i][j] = minimum deletions to make sum of first i elements have remainder j
    const dp = Array(n + 1)
        .fill()
        .map(() => Array(k).fill(Infinity))
    dp[0][0] = 0 // Base case: 0 deletions for empty array with remainder 0

    for (let i = 1; i <= n; i++) {
        for (let j = 0; j < k; j++) {
            // Don't delete current element
            const newRemainder = (j - (nums[i - 1] % k) + k) % k
            if (dp[i - 1][newRemainder] !== Infinity) {
                dp[i][j] = Math.min(dp[i][j], dp[i - 1][newRemainder])
            }

            // Delete current element
            dp[i][j] = Math.min(dp[i][j], dp[i - 1][j] + 1)
        }
    }

    return dp[n][remainder] === Infinity ? -1 : dp[n][remainder]
}

// Alternative approach using subset sum
function minDeletionsForDivisibleSumAlternative(nums, k) {
    const sum = nums.reduce((acc, num) => acc + num, 0)
    const remainder = sum % k

    if (remainder === 0) return 0

    // Find minimum subset sum that has remainder equal to target remainder
    const target = remainder
    const dp = Array(k).fill(Infinity)
    dp[0] = 0

    for (const num of nums) {
        const newDp = [...dp]
        for (let i = 0; i < k; i++) {
            if (dp[i] !== Infinity) {
                const newRemainder = (i + num) % k
                newDp[newRemainder] = Math.min(newDp[newRemainder], dp[i] + 1)
            }
        }
        dp = newDp
    }

    return dp[target] === Infinity ? -1 : dp[target]
}
```
