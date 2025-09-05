// Design

// ============================================================================
// 146. LRU Cache
// Problem: Design an LRU cache with get/put in O(1), evicting least-recently-used.
// Approach: Use HashMap (key -> node) for O(1) lookups and a Doubly Linked List
//           to maintain recency order. 
// - Head = most recently used
// - Tail = least recently used (evicted when over capacity)
class LRUCache(private val capacity: Int) {

    // Node structure for doubly linked list
    private data class Node(
        var key: Int,
        var value: Int,
        var prev: Node? = null,
        var next: Node? = null
    )

    private val map = mutableMapOf<Int, Node>()  // Key -> Node mapping
    private val head = Node(0, 0) // Dummy head (sentinel node)
    private val tail = Node(0, 0) // Dummy tail (sentinel node)

    init {
        // Link head and tail initially
        head.next = tail
        tail.prev = head
    }

    fun get(key: Int): Int {
        val node = map[key] ?: return -1          // Return -1 if not found
        moveToHead(node)                          // Mark as most recently used
        return node.value
    }

    fun put(key: Int, value: Int) {
        val node = map[key]
        if (node != null) {
            // Update existing node and move it to head
            node.value = value
            moveToHead(node)
        } else {
            // Create new node
            val n = Node(key, value)
            map[key] = n
            addAfterHead(n)
            if (map.size > capacity) removeLRU()  // Evict least recently used
        }
    }

    // Insert new node right after dummy head
    private fun addAfterHead(n: Node) {
        n.prev = head
        n.next = head.next
        head.next!!.prev = n
        head.next = n
    }

    // Remove node from list
    private fun remove(n: Node) {
        n.prev!!.next = n.next
        n.next!!.prev = n.prev
    }

    // Move existing node to head (most recent)
    private fun moveToHead(n: Node) {
        remove(n)
        addAfterHead(n)
    }

    // Remove least recently used node (node before tail)
    private fun removeLRU() {
        val lru = tail.prev!!
        remove(lru)
        map.remove(lru.key)
    }
}
// Time Complexity: O(1) per get/put → HashMap + LinkedList operations are constant time.
// Space Complexity: O(capacity) → Store up to `capacity` nodes in map and list.
