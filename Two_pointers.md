// Two Pointers

/*
167. Two Sum II - Input Array Is Sorted
Return indices (1-indexed) of the two numbers that add up to target.
*/
fun twoSum(numbers: IntArray, target: Int): IntArray {
    var left = 0                     // Left pointer starts at the beginning
    var right = numbers.size - 1     // Right pointer starts at the end

    while (left < right) {
        val sum = numbers[left] + numbers[right]
        when {
            sum == target -> return intArrayOf(left + 1, right + 1) // Found the pair (1-indexed)
            sum < target -> left++     // If sum is too small, move left pointer to increase sum
            else -> right--            // If sum is too large, move right pointer to decrease sum
        }
    }
    return intArrayOf() // No valid pair found
}
// Time Complexity: O(n) → Each element is checked at most once as we move left/right pointers inward.
// Space Complexity: O(1) → Constant space used for pointers, no extra data structures.

//------------------------------------------------------------------------------------------

/*
125. Valid Palindrome
Check if the string is a palindrome considering only alphanumeric characters.
*/
fun isPalindrome(s: String): Boolean {
    var left = 0                    // Left pointer at start
    var right = s.length - 1        // Right pointer at end

    while (left < right) {
        // Skip non-alphanumeric characters from left
        while (left < right && !s[left].isLetterOrDigit()) left++
        // Skip non-alphanumeric characters from right
        while (left < right && !s[right].isLetterOrDigit()) right--

        // Compare characters (ignoring case)
        if (s[left].lowercaseChar() != s[right].lowercaseChar()) return false

        left++
        right--
    }
    return true // String is a palindrome
}
// Time Complexity: O(n) → Each character is checked at most once.
// Space Complexity: O(1) → No extra data structures used.

//------------------------------------------------------------------------------------------

/*
11. Container With Most Water
Find two lines that together with the x-axis form a container, such that it contains the most water.
*/
fun maxArea(height: IntArray): Int {
    var left = 0                    // Left pointer at index 0
    var right = height.size - 1     // Right pointer at last index
    var maxArea = 0                 // Track maximum water area

    while (left < right) {
        val minHeight = minOf(height[left], height[right]) // Water limited by shorter line
        val width = right - left                          // Distance between pointers
        maxArea = maxOf(maxArea, minHeight * width)       // Update max area if larger

        // Move pointer pointing to smaller line
        if (height[left] < height[right]) left++ else right--
    }
    return maxArea
}
// Time Complexity: O(n) → Each element is visited at most once.
// Space Complexity: O(1) → Only variables used, no extra storage.

//------------------------------------------------------------------------------------------

/*
1768. Merge Strings Alternately
You are given two strings word1 and word2.
Merge the strings by adding letters in alternating order, starting with word1.
If one string is longer, append the remaining characters at the end.
*/

fun mergeAlternately(word1: String, word2: String): String {
    val res = StringBuilder()        // Efficient string building
    val c1 = word1.length            // Length of word1
    val c2 = word2.length            // Length of word2

    // Loop over word1 and alternately append chars from word2 if available
    word1.forEachIndexed { i, c ->
        res.append(c)                // Always add current char from word1
        if (i < c2) {                // If word2 still has chars left
            res.append(word2[i])     // Add char from word2
        }
    }

    // If word2 is longer, append the remaining part
    if (c2 > c1) {
        res.append(word2.substring(c1, c2))
    }

    return res.toString()
}

/*Time Complexity: O(n + m)
n = word1.length, m = word2.length
Each character from both strings is processed once.
The substring operation at the end is also linear in the leftover length.
Space Complexity: O(n + m)
The StringBuilder stores the merged result, which in the worst case contains all characters from both strings.
Aside from that, only a few integer variables are used.*/

//------------------------------------------------------------------------------------------

