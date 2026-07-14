---
tags:
- algorithms
- data-structures
- programming
---

# 01 Trees & BSTs

Trees are hierarchical data structures. Every node has a value and references to children. The most important tree variants: binary trees, BSTs, and tries.

---

## Binary Tree

Each node has at most 2 children.

```java
class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int val) { this.val = val; }
}
```

### Traversals

```
        1
       / \
      2   3
     / \
    4   5

Pre-order:  [1, 2, 4, 5, 3]   (node → left → right)
In-order:   [4, 2, 5, 1, 3]   (left → node → right)  ← BST sorted order
Post-order: [4, 5, 2, 3, 1]   (left → right → node)
Level-order:[1, 2, 3, 4, 5]   (BFS)
```

```java
// In-order (recursive)
void inorder(TreeNode root) {
    if (root == null) return;
    inorder(root.left);
    System.out.println(root.val);
    inorder(root.right);
}

// Level-order (BFS)
void levelOrder(TreeNode root) {
    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);
    while (!q.isEmpty()) {
        TreeNode node = q.poll();
        System.out.println(node.val);
        if (node.left != null) q.offer(node.left);
        if (node.right != null) q.offer(node.right);
    }
}
```

---

## Binary Search Tree (BST)

Left subtree < node < right subtree. Enables O(log n) search, insert, delete (when balanced).

```java
TreeNode search(TreeNode root, int target) {
    if (root == null || root.val == target) return root;
    return target < root.val 
        ? search(root.left, target) 
        : search(root.right, target);
}
```

| Operation | Average | Worst (skewed) |
|-----------|:-------:|:-------------:|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |

### Self-Balancing BSTs

| Tree | Balancing Rule | Use |
|------|---------------|-----|
| **AVL** | Height difference ≤ 1 | Fast lookups, frequent reads |
| **Red-Black** | Color rules + rotations | Balanced insert/delete, Java `TreeMap` |

---

## Trie (Prefix Tree)

Tree for strings. Each node represents a character. Root is empty.

```
Words: ["cat", "car", "dog"]

        root
       /    \
      c      d
      |      |
      a      o
     / \     |
    t   r    g
```

```java
class TrieNode {
    Map<Character, TrieNode> children = new HashMap<>();
    boolean isEndOfWord;
}

void insert(TrieNode root, String word) {
    TrieNode curr = root;
    for (char c : word.toCharArray()) {
        curr = curr.children.computeIfAbsent(c, k -> new TrieNode());
    }
    curr.isEndOfWord = true;
}
```

| Use Case | How |
|----------|-----|
| **Autocomplete** | Walk trie from prefix, collect all words |
| **Spell checker** | Edit distance + trie traversal |
| **IP routing** | Longest prefix match |

---

## Sources

- CLRS — Chapters 12, 13
- Sedgewick — Chapter 3 (Trees)


---

## Hands-On Exercises

### Exercise 1: Tree Traversal — Inorder, Preorder, Postorder
Implement all three recursive traversals for the tree in the note:
```
        1
       / \
      2   3
     / \
    4   5
```

```java
void inorder(TreeNode root)   { /* TODO */ }  // [4,2,5,1,3]
void preorder(TreeNode root)  { /* TODO */ }  // [1,2,4,5,3]
void postorder(TreeNode root) { /* TODO */ }  // [4,5,2,3,1]
```

---

### Exercise 2: BST Search — Validate BST
Implement the search algorithm from the note, then write a function to check if a binary tree is a valid BST.

```java
boolean isValidBST(TreeNode root) {
    // TODO: Use inorder traversal — values must be strictly increasing
    // Or use min/max bounds recursively
}
```

**Hint:** In a valid BST, inorder traversal produces a sorted sequence.

---

### Exercise 3: Trie — Implement Insert and Search
Implement the Trie `insert` and `search` methods from the note.

```java
class Trie {
    TrieNode root = new TrieNode();

    void insert(String word) { /* TODO */ }
    boolean search(String word) { /* TODO */ }  // Exact match
    boolean startsWith(String prefix) { /* TODO */ }  // Prefix exists?
}
```

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 1 | [Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/) (LC 226) | 🟢 Easy | Recursion |
| 2 | [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/) (LC 104) | 🟢 Easy | DFS |
| 3 | [Same Tree](https://leetcode.com/problems/same-tree/) (LC 100) | 🟢 Easy | Recursion |
| 4 | [Validate BST](https://leetcode.com/problems/validate-binary-search-tree/) (LC 98) | 🟡 Medium | Inorder / Bounds |
| 5 | [Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/) (LC 102) | 🟡 Medium | BFS (Queue) |
| 6 | [Lowest Common Ancestor of BST](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/) (LC 235) | 🟡 Medium | BST Property |
| 7 | [Lowest Common Ancestor of Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/) (LC 236) | 🟡 Medium | Recursion |
| 8 | [Implement Trie](https://leetcode.com/problems/implement-trie-prefix-tree/) (LC 208) | 🟡 Medium | Trie |

### Assignment Guidelines
- **Start** with 1–3 (Easy) — basic tree recursion.
- **Then** 4–8 (Medium) — BST properties, BFS, LCA, Trie.
- **Problem 4** (Validate BST) is a very common interview trap. Don't just check `left < node < right` locally — check with bounds.
- **Target time:** 10 min per Easy, 20 min per Medium.
