

// ============================================================================
// 200. Number of Islands
// Problem: Count number of islands in a grid, where '1' is land and '0' is water.
//          An island is formed by connected '1's (connected in 4 directions).
// Approach: DFS from each land cell ('1'), sinking the island by marking cells as '0'.
//           Each DFS marks an entire island as visited.
fun numIslands(grid: Array<CharArray>): Int {
    if (grid.isEmpty()) return 0
    val rows = grid.size
    val cols = grid[0].size
    var count = 0

    fun dfs(r: Int, c: Int) {
        // Stop if out of bounds or at water
        if (r !in 0 until rows || c !in 0 until cols || grid[r][c] == '0') return

        grid[r][c] = '0' // Mark visited (sink land into water)

        // Explore all 4 directions
        dfs(r + 1, c)
        dfs(r - 1, c)
        dfs(r, c + 1)
        dfs(r, c - 1)
    }

    // Iterate over entire grid
    for (r in 0 until rows) {
        for (c in 0 until cols) {
            if (grid[r][c] == '1') {
                count++   // Found new island
                dfs(r, c) // Sink entire island
            }
        }
    }
    return count
}
// Time Complexity: O(m * n) → Every cell is visited once in worst case.
// Space Complexity: O(m * n) → Worst case recursion depth if grid is all land (stack usage).

// ============================================================================
// 101. Symmetric Tree
// Problem: Check whether a binary tree is a mirror of itself.
// Approach: Recursively compare left subtree with mirrored right subtree.
class SymmetricSolution {
    fun isSymmetric(root: TreeNode): Boolean = mirror(root.left, root.right)

    private fun mirror(a: TreeNode?, b: TreeNode?): Boolean =
        when {
            a == null && b == null -> true      // Both null -> symmetric
            a == null || b == null -> false     // One null, one not -> not symmetric
            a.`val` != b.`val` -> false         // Values differ -> not symmetric
            else -> mirror(a.left, b.right)     // Compare outer children
                 && mirror(a.right, b.left)     // Compare inner children
        }
}
// Time Complexity: O(n) → Every node is visited once.
// Space Complexity: O(h) → Recursion stack depth (h = height of tree).


// ============================================================================
// 226. Invert Binary Tree
// Problem: Invert (mirror) a binary tree and return its root.
// Approach: Recursively swap left and right children.
fun invertTree(root: TreeNode?): TreeNode? {
    if (root == null) return null

    val left = invertTree(root.left)     // Invert left subtree
    root.left = invertTree(root.right)   // Invert right subtree and assign to left
    root.right = left                    // Assign saved left subtree to right

    return root
}
// Time Complexity: O(n) → Each node is visited once.
// Space Complexity: O(h) → Recursion depth (h = height of tree).


// ============================================================================
// 543. Diameter of Binary Tree
// Problem: Find length (in edges) of the longest path between any two nodes.
// Approach: DFS returns height; update global diameter as leftHeight + rightHeight.
class DiameterSolution {
    private var best = 0  // Tracks the maximum diameter found

    fun diameterOfBinaryTree(root: TreeNode?): Int {
        best = 0
        depth(root)
        return best
    }

    private fun depth(node: TreeNode?): Int {
        if (node == null) return 0
        val L = depth(node.left)        // Height of left subtree
        val R = depth(node.right)       // Height of right subtree
        best = maxOf(best, L + R)       // Update diameter (longest path through this node)
        return maxOf(L, R) + 1          // Height of current node
    }
}
// Time Complexity: O(n) → Each node visited once.
// Space Complexity: O(h) → Recursion depth (stack).


// ============================================================================
// 938. Range Sum of BST
// Problem: Sum values of all nodes with value in [low, high] in a BST.
// Approach: DFS with pruning using BST property.
fun rangeSumBST(root: TreeNode?, low: Int, high: Int): Int {
    if (root == null) return 0

    var sum = 0
    if (root.`val` in low..high) sum += root.`val`  // Count if within range

    // Only recurse left if node value could have smaller values >= low
    if (root.`val` > low) sum += rangeSumBST(root.left, low, high)

    // Only recurse right if node value could have larger values <= high
    if (root.`val` < high) sum += rangeSumBST(root.right, low, high)

    return sum
}
// Time Complexity: O(n) worst (skewed tree with no pruning), 
//                  but better on average due to BST pruning.
// Space Complexity: O(h) recursion depth (h = height of tree).

// ============================================================================


