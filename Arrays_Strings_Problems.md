

## 167. Two Sum II - Input Array Is Sorted

**Problem Statement:**
- Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

```kotlin
fun twoSum(nums: IntArray, target: Int): IntArray {
    val map = HashMap<Int, Int>()   // Stores number -> index mapping

    for (i in nums.indices) {
        val curr = nums[i]
        val complement = target - curr

        // If complement exists in map, return the pair of indices
        if (map.containsKey(complement)) {
            return intArrayOf(map[complement]!!, i)
        }

        // Otherwise, store current number with its index
        map[curr] = i
    }

    return intArrayOf(0, 0) // If no solution found
}
```
- Time Complexity: O(n) → each element is processed once, map lookups are O(1) on average.
- Space Complexity: O(n) → in the worst case, we store all elements in the HashMap.

---

## 14. Longest Common Prefix

**Problem Statement:**
- Write a function to find the longest common prefix string amongst an array of strings.
- If there is no common prefix, return an empty string "".

```kotlin
fun longestCommonPrefix(strs: Array<String>): String {
    if (strs.isEmpty()) return ""

    var prefix = strs[0]

    // Compare prefix with each string in the array
    for (i in 1 until strs.size) {
        // Keep reducing prefix until it matches the start of current string
        while (strs[i].indexOf(prefix) != 0) {
            prefix = prefix.dropLast(1) // drop last char
            if (prefix.isEmpty()) return ""
        }
    }

    return prefix
}
```
-- Time Complexity: O(N * M) → N = number of strings, M = average length of strings (in worst case we compare character by character).
-- Space Complexity: O(1) → constant extra space.

---

## 88. Merge Sorted Array

**Problem Statement:**
- You are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.
- Merge nums1 and nums2 into a single array sorted in non-decreasing order.

```kotlin
fun merge(nums1: IntArray, m: Int, nums2: IntArray, n: Int): Unit {
    var p1 = m - 1   // pointer for nums1
    var p2 = n - 1   // pointer for nums2

    // Start filling from the back of nums1
    for (i in m + n - 1 downTo 0) {
        if (p2 < 0) break   // no elements left in nums2

        if (p1 >= 0 && nums1[p1] > nums2[p2]) {
            nums1[i] = nums1[p1--]
        } else {
            nums1[i] = nums2[p2--]
        }
    }
}
```
- Time Complexity: O(m + n) → Each element compared and placed once.
- Space Complexity: O(1) → In-place merge, no extra arrays.

---

## 118. Pascal's Triangle

**Problem Statement:**
Given an integer numRows, return the first numRows of Pascal's triangle.

```kotlin
fun generate(numRows: Int): List<List<Int>> {
    val triangle = mutableListOf<List<Int>>()

    for (i in 0 until numRows) {
        val row = MutableList(i + 1) { 1 } // fill with 1s

        // Fill the middle using values from previous row
        for (j in 1 until i) {
            row[j] = triangle[i - 1][j - 1] + triangle[i - 1][j]
        }

        triangle.add(row)
    }

    return triangle
}
````
- Time Complexity: O(numRows²) → Filling each row requires iteration.
- Space Complexity: O(numRows²) → Stores all rows of the triangle.

---

## 121. Best Time to Buy and Sell Stock

**Problem Statement:**
- You are given an array prices where prices[i] is the price of a given stock on the ith day.
- You want to maximize your profit by choosing a single day to buy one stock and choosing a different day in the future to sell that stock.
- Return the maximum profit you can achieve from this transaction. If you cannot achieve any profit, return 0.

```kotlin
fun maxProfit(prices: IntArray): Int {
    var profit = 0                       // Maximum profit found so far
    var buy = Int.MAX_VALUE              // Track the lowest price seen so far (best buy option)

    prices.forEach { price ->
        if (price < buy) {
            // Found a lower price → update buy
            buy = price
        } else {
            // Calculate profit if we sell at current price
            profit = maxOf(profit, price - buy)
        }
    }

    return profit
}
````
- Time Complexity: O(n) > We iterate over the prices array once, checking each price exactly one time.
- Space Complexity: O(1) > Only a few variables (profit, buy) are used regardless of input size.

---


## 20. Valid Parentheses

**Problem Statement:**
- Given a string containing only '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

A string is valid if:
1. Open brackets are closed by the same type of bracket.
2. Open brackets are closed in the correct order.

```kotlin
fun isValid(s: String): Boolean {
    val stack = ArrayDeque<Char>()                // Stack to keep track of opening brackets
    val match = mapOf(')' to '(', '}' to '{', ']' to '[') // Closing -> Opening mapping

    for (ch in s) {
        if (ch in match.values) {
            // If it's an opening bracket, push to stack
            stack.addLast(ch)
        } else if (ch in match.keys) {
            // If it's a closing bracket:
            // 1. Stack must not be empty
            // 2. Last opening bracket must match the expected one
            if (stack.isEmpty() || stack.removeLast() != match[ch]) return false
        }
        // Ignore any other characters (not required in this problem)
    }

    // At the end, stack should be empty if all brackets were matched
    return stack.isEmpty()
}
```
- Time Complexity: O(n) > Each character in s is processed once. Push/pop operations on ArrayDeque are O(1).
- Space Complexity: O(n) > In the worst case (e.g., "(((((((("), all characters are stored in the stack. The mapping match is constant size, so negligible.

---

## 56. Merge Intervals

- Problem: Merge all overlapping intervals and return non-overlapping result.
- Approach: Sort by start; iterate and merge when overlap occurs.

```kotlin
fun merge(intervals: Array<IntArray>): Array<IntArray> {
    if (intervals.isEmpty()) return arrayOf()
    intervals.sortBy { it[0] }
    val out = mutableListOf<IntArray>()
    var cur = intervals[0]
    for (i in 1 until intervals.size) {
        val nxt = intervals[i]
        if (nxt[0] <= cur[1]) {
            cur[1] = maxOf(cur[1], nxt[1]) // merge
        } else {
            out.add(cur)
            cur = nxt
        }
    }
    out.add(cur)
    return out.toTypedArray()
}
````
- Time: O(n log n) for sort
- Space: O(n) for output

---

## 242. Valid Anagram

**Problem Statement:**
Given two strings s and t, return true if t is an anagram of s, and false otherwise.

```kotlin
fun isAnagram(s: String, t: String): Boolean {
    return if (s.length != t.length) false
    else {
        // If problem guarantees only lowercase a-z, use fixed array of size 26
        val freq = IntArray(26) // freq[i] tracks count of ('a' + i)

        // Count s and subtract with t in one pass
        for (i in s.indices) {
            freq[s[i] - 'a']++
            freq[t[i] - 'a']--
        }

        // All zeros => anagram
        freq.all { it == 0 }
    }
}
```
- Time: O(n) → single pass over strings.
- Space: O(1) → 26-sized array regardless of input length.

---

## 26. Remove Duplicates from Sorted Array

**Problem Statement:**
- Given an integer array nums sorted in non-decreasing order, remove the duplicates in-place such that each unique element appears only once. The relative order of the elements should be kept the same. Then return the number of unique elements in nums.

```kotlin
fun removeDuplicates(nums: IntArray): Int {
    var p = 1                // pointer for placing the next unique element
    var prev = nums[0]       // track the last unique element
    // iterate from the 2nd element (index = 1) to the end
    for (i in 1 until nums.size) {
        if (nums[i] != prev) {   // found a new unique element
            nums[p++] = nums[i]  // place it at index p, then increment p
            prev = nums[i]       // update last unique element
        }
    }

    return p   // length of the unique portion of the array
}
```
- Time: O(n) → single pass over the array.
- Space: O(1) → in-place modification, no extra storage.

---

## 1249. Minimum Remove to Make Valid Parentheses

**Problem Statement:**
- Given a string s of '(' , ')' and lowercase English characters.
- Your task is to remove the minimum number of parentheses ( '(' or ')', in any positions ) so that the resulting parentheses string is valid and return any valid string.

```kotlin
fun minRemoveToMakeValid(s: String): String {
    val indexesToRemove = mutableSetOf<Int>() // indices of invalid parentheses
    val stack = ArrayDeque<Int>() // store indices of '(' that need matching

    // First pass: identify indices to remove
    for (i in s.indices) {
        when (s[i]) {
            '(' -> stack.push(i) // unmatched '(' → push index
            ')' -> {
                if (stack.isEmpty()) {
                    indexesToRemove.add(i) // unmatched ')' → mark for removal
                } else {
                    stack.pop() // matched with earlier '('
                }
            }
        }
    }

    // Any '(' left in stack are unmatched → mark them for removal
    while (stack.isNotEmpty()) {
        indexesToRemove.add(stack.pop())
    }

    // Build final string skipping removed indices
    val sb = StringBuilder()
    for (i in s.indices) {
        if (i !in indexesToRemove) {
            sb.append(s[i])
        }
    }
    return sb.toString()
}
```
- Time: O(n) (two passes through the string).
- Space: O(n) (because we’re building a new string).

---

## 1762. Buildings With an Ocean View

**Problem Statement:**
- here are n buildings in a line. You are given an integer array heights of size n that represents the heights of the buildings in the line.
- The ocean is to the right of the buildings. A building has an ocean view if the building can see the ocean without obstructions. Formally, a building has an ocean view if all the buildings to its right have a smaller height.
- Return a list of indices (0-indexed) of buildings that have an ocean view, sorted in increasing order.

```kotlin

fun findBuildings(heights: IntArray): IntArray {
    var max = Int.MIN_VALUE  // Tracks tallest building seen so far from the right
    val res = mutableListOf<Int>()

    // Traverse from right to left
    for (i in heights.lastIndex downTo 0) {
        val curr = heights[i]

        // If current building is taller than all to its right
        if (curr > max) {
            res.add(i)   // Add index to result
            max = curr   // Update the max
        }
    }

    // The result was built in reverse order, so reverse it before returning
    res.reverse()
    return res.toIntArray()
}
```
- Time Complexity: O(n) - We traverse the array once (right to left) → O(n), Reverse the list at the end → O(n), Total still O(n)
- Space Complexity: O(n) worst case

---

## 11. Container With Most Water

**Problem Statement:**
- You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).
- Find two lines that together with the x-axis form a container, such that the container contains the most water.
- Return the maximum amount of water a container can store.

```kotlin

fun maxArea(height: IntArray): Int {
    var left = 0
    var right = height.lastIndex      // start at the actual last index
    var best = 0

    while (left < right) {
        val width = right - left
        val h = minOf(height[left], height[right])  // limiting height is the shorter side
        best = maxOf(best, h * width)               // update best area

        // Move the pointer at the shorter bar inward:
        // Only moving the shorter side can possibly increase area,
        // because width shrinks each step.
        if (height[left] <= height[right]) {
            left++
        } else {
            right--                                  // <-- decrement, not increment
        }
    }
    return best
}
```
- Time: O(n) — each pointer moves at most n steps total.
- Space: O(1) — just a few integers.

---

## 3. Longest Substring Without Repeating Characters

**Problem Statement:**
- Given a string s, find the length of the longest substring without duplicate characters.

```kotlin
fun lengthOfLongestSubstring(s: String): Int {
    val map = mutableMapOf<Char, Int>() // Stores char -> last seen index
    var prev = -1  // Marks position before the start of current valid window
    var res = 0    // Max length of substring found

    for (i in s.indices) {
        val ch = s[i]

        // If char seen before, move 'prev' to max of its last seen index
        if (map.containsKey(ch)) {
            prev = maxOf(prev, map[ch]!!)
        }

        // Update the last seen index of current char
        map[ch] = i

        // Window size is (i - prev)
        res = maxOf(res, i - prev)
    }

    return res
}
```
- Time: O(n) — Each character is processed once, and map operations are O(1).
- Space: O(min(n, charset)) — Map stores at most one entry per character (e.g., 26 lowercase letters or full ASCII).

---

## 53. Maximum Subarray

**Problem Statement:**
- Given an integer array nums, find the subarray with the largest sum, and return its sum.

```kotlin
fun maxSubArray(nums: IntArray): Int {
    var res = nums[0]            // Start with the first element as initial max
    var curr = nums[0]           // Current running sum

    // Start loop from index 1 because we already used index 0
    for (i in 1 until nums.size) {
        val num = nums[i]
        // Either take the current number alone OR extend the previous subarray
        curr = maxOf(num, curr + num)
        // Track the global maximum
        res = maxOf(res, curr)
    }

    return res
}
```
- Time: O(n) — single pass through the array.
- Space: O(1) — uses only two variables (curr and res).

---

## 560. Subarray Sum Equals K

**Problem Statement:**
- Given an array of integers nums and an integer k, return the total number of subarrays whose sum equals to k.

```kotlin
fun subarraySum(nums: IntArray, k: Int): Int {
    var sum = 0
    var ans = 0
    val prefixMap = mutableMapOf<Int, Int>()

    // base case: subarray starting at index 0
    prefixMap[0] = 1

    for (num in nums) {
        sum += num

        // Check if there is a prefix sum that satisfies sum(i) - prefixSum = k
        // Add count of those prefix sums
        val prefix = sum - k
        ans += prefixMap[prefix] ?: 0

        // Update frequency of current prefix sum
        prefixMap[sum] = (prefixMap[sum] ?: 0) + 1
    }

    return ans
}
```
- Time: O(n)
- Space: O(n) (for the prefix map)

---

## 49. Group Anagrams

**Problem Statement:**
- Given an array of strings strs, group the anagrams together. You can return the answer in any order.

```kotlin
fun groupAnagrams(strs: Array<String>): List<List<String>> {
    if (strs.isEmpty()) return emptyList()

    val anagramMap = mutableMapOf<String, MutableList<String>>()

    for (str in strs) {
        // Sort the string to create a unique key for its anagram group
        val sorted = str.toCharArray().sorted().joinToString("")

        // If key not present, create a new list, then add the string
        anagramMap.getOrPut(sorted) { mutableListOf() }.add(str)
    }

    return anagramMap.values.toList()
}
```
- Time Complexity: Sorting each string: O(m log m) where m = length of each string. => For n strings → O(n * m log m).
- Space Complexity: Map storing all strings grouped → O(n * m) (in worst case, all unique).

---

