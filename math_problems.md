Math Problems

/*
9. Palindrome Number
Given an integer x, return true if x is a palindrome, and false otherwise.

A palindrome number reads the same backward as forward.
Examples:
121 -> true
-121 -> false (negative sign breaks palindrome)
10 -> false
*/

fun isPalindrome(x: Int): Boolean {
    // Negative numbers are not palindromes
    // Numbers ending with 0 (except 0 itself) cannot be palindromes
    if (x < 0 || (x % 10 == 0 && x != 0)) return false

    var num = x
    var reversedHalf = 0

    // Reverse half of the number
    while (num > reversedHalf) {
        val digit = num % 10
        reversedHalf = reversedHalf * 10 + digit
        num /= 10
    }

    // For even length: num == reversedHalf
    // For odd length: num == reversedHalf/10 (middle digit ignored)
    return num == reversedHalf || num == reversedHalf / 10
}

/*Time Complexity: O(log₁₀(n))
We process half the digits of x, hence proportional to the number of digits.
Space Complexity: O(1)
Only integer variables are used, no extra data structures.*/

//------------------------------------------------------------------------------------------

// 509. Fibonacci Number (both versions)
// Problem: Return the nth Fibonacci number.
// Approaches: (1) Simple recursion; (2) Iterative DP with rolling vars.
// Recursive: Time O(2^n) | Space O(n)
fun fibRecursive(n: Int): Int = if (n < 2) n else fibRecursive(n - 1) + fibRecursive(n - 2)
// Iterative: Time O(n) | Space O(1)
fun fibIterative(n: Int): Int {
    if (n < 2) return n
    var a = 0; var b = 1
    for (i in 2..n) { val c = a + b; a = b; b = c }
    return b
}

//------------------------------------------------------------------------------------------

// 70. Climbing Stairs
// Problem: You can climb 1 or 2 steps; how many ways to reach the nth step?
// Insight: Same recurrence as Fibonacci: ways(n) = ways(n-1) + ways(n-2).
// Approach: Iterative DP with rolling variables.
// Time: O(n) | Space: O(1)
fun climbStairs(n: Int): Int {
    if (n <= 2) return n
    var a = 1 // ways to reach (i-2)
    var b = 2 // ways to reach (i-1)
    for (i in 3..n) {
        val c = a + b
        a = b
        b = c
    }
    return b
}

//------------------------------------------------------------------------------------------

