// Linked List

/*206. Reverse Linked List
Given the head of a singly linked list, reverse the list, and return the reversed list.*/

fun reverseList(head: ListNode?): ListNode? {
    var prev: ListNode? = null       // previous node (initially none)
    var curr = head                  // current node
    var next: ListNode?              // temp pointer to save next node

    while (curr != null) {
        next = curr.next             // 1. store next node
        curr.next = prev             // 2. reverse pointer (curr -> prev)
        prev = curr                  // 3. move prev forward
        curr = next                  // 4. move curr forward
    }

    return prev  // prev is now the new head of the reversed list
}

// Time Complexity: O(n) → each node visited once.
// Space Complexity: O(1) → only three pointers used (prev, curr, next).

// ============================================================================

/*21. Merge Two Sorted Lists
You are given the heads of two sorted linked lists list1 and list2.
Merge the two lists into one sorted list. The list should be made by splicing together the nodes of the first two lists.
Return the head of the merged linked list.*/

fun mergeTwoLists(list1: ListNode?, list2: ListNode?): ListNode? {
    val prehead = ListNode(-1) // dummy head to simplify pointer logic
    var prev = prehead // pointer to build the new list

    var one = list1
    var two = list2

    while (one != null && two != null) {
        if (one.`val` <= two.`val`) {
            prev.next = one // attach smaller node
            one = one.next // move list1 forward
        } else {
            prev.next = two
            two = two.next
        }
        prev = prev.next	// move merged list forward
    }

    // Attach remaining nodes (only one list can have leftovers)
    prev.next = if (one == null) two else one 

    // skip dummy head
    return prehead.next
}

// Time Complexity: O(n + m) → must process all nodes of both lists.
// Space Complexity: → O(1) (only pointers).

// ============================================================================

// 2. Add Two Numbers (LeetCode)
// Problem: Two non-empty linked lists represent two non-negative integers in reverse order.
// Each node contains a single digit. Add the numbers and return the sum as a linked list.

// Time Complexity: O(max(m, n))
//   - We traverse both lists once (where m = length of l1, n = length of l2).
//   - Each step does constant work (addition, carry, node creation).
//   - So total operations scale with the longer list.
//
// Space Complexity: O(max(m, n))
//   - Output linked list has at most max(m, n) + 1 nodes (extra for final carry).
//   - Aside from result, only O(1) extra space is used (pointers + carry).

fun addTwoNumbers(l1: ListNode, l2: ListNode): ListNode {
    // Dummy head simplifies list construction (avoids edge cases for head node).
    val dummy = ListNode(-1)
    var curr = dummy

    // Carry holds overflow when sum of digits >= 10
    var carry = 0

    // Pointers to traverse both input lists
    var p = l1
    var q = l2

    // Continue until both lists are exhausted AND no carry left
    while (p != null || q != null || carry != 0) {
        // Extract current digit or 0 if list ended
        val x = p?.`val` ?: 0
        val y = q?.`val` ?: 0

        // Add digits + carry
        val sum = x + y + carry
        carry = sum / 10 // new carry (either 0 or 1)

        // Create new node with the digit value (sum % 10)
        curr.next = ListNode(sum % 10)
        curr = curr.next

        // Advance pointers if nodes remain
        p?.let { p = it.next }
        q?.let { q = it.next }
    }

    // Return the result list (skipping dummy head)
    return dummy.next
}
