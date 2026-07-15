---
tags:
- algorithms
- data-structures
- discrete-math
- graphs
- programming
---

# 01 Graphs

A graph G = (V, E) is a set of vertices connected by edges. Graphs model networks, dependencies, recommendations, maps — and they're the most common interview topic after arrays and trees.

---

## Representations

| | Adjacency List | Adjacency Matrix |
|---|:---:|:---:|
| **Space** | O(V + E) | O(V²) |
| **Edge lookup** | O(degree) | O(1) |
| **Iterate neighbors** | O(degree) | O(V) |
| **Best for** | Sparse graphs (most real-world) | Dense graphs, small V |

```java
// Adjacency list
Map<Integer, List<Integer>> graph = new HashMap<>();
graph.computeIfAbsent(0, k -> new ArrayList<>()).add(1);
graph.computeIfAbsent(0, k -> new ArrayList<>()).add(2);
graph.computeIfAbsent(1, k -> new ArrayList<>()).add(2);
// 0 → [1, 2], 1 → [2]
```

---

## BFS (Breadth-First Search)

Queue-based. Shortest path in **unweighted** graph. Level-order.

```java
void bfs(Map<Integer, List<Integer>> graph, int start) {
    Set<Integer> visited = new HashSet<>();
    Queue<Integer> q = new LinkedList<>();
    q.offer(start);
    visited.add(start);
    
    while (!q.isEmpty()) {
        int node = q.poll();
        System.out.println(node);
        for (int neighbor : graph.getOrDefault(node, List.of())) {
            if (!visited.contains(neighbor)) {
                visited.add(neighbor);
                q.offer(neighbor);
            }
        }
    }
}
```

---

## DFS (Depth-First Search)

Stack-based (or recursive). Explores paths to completion. Used for cycle detection, topological sort, connected components.

```java
void dfs(Map<Integer, List<Integer>> graph, int node, Set<Integer> visited) {
    visited.add(node);
    System.out.println(node);
    for (int neighbor : graph.getOrDefault(node, List.of())) {
        if (!visited.contains(neighbor)) {
            dfs(graph, neighbor, visited);
        }
    }
}
```

---

## Key Algorithms

| Algorithm | What It Solves | Time |
|-----------|---------------|:----:|
| **BFS** | Shortest path (unweighted) | O(V + E) |
| **DFS** | Connectivity, cycles, topological sort | O(V + E) |
| **Dijkstra** | Shortest path (weighted, non-negative) | O((V+E) log V) |
| **Bellman-Ford** | Shortest path (negative edges OK) | O(VE) |
| **Floyd-Warshall** | All-pairs shortest path | O(V³) |
| **Kruskal / Prim** | Minimum Spanning Tree | O(E log V) |
| **Topological Sort** | Ordering of DAG | O(V + E) |

### Dijkstra's Algorithm

```java
int[] dijkstra(Map<Integer, List<Edge>> graph, int start, int n) {
    int[] dist = new int[n];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[start] = 0;
    
    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[1] - b[1]);
    pq.offer(new int[]{start, 0});
    
    while (!pq.isEmpty()) {
        int[] curr = pq.poll();
        int node = curr[0], d = curr[1];
        if (d > dist[node]) continue;  // Stale entry
        
        for (Edge e : graph.getOrDefault(node, List.of())) {
            if (dist[node] + e.weight < dist[e.to]) {
                dist[e.to] = dist[node] + e.weight;
                pq.offer(new int[]{e.to, dist[e.to]});
            }
        }
    }
    return dist;
}
```

---

## Common Graph Patterns

| Pattern | How |
|---------|-----|
| **Number of islands** | DFS/BFS on grid. Count connected components. |
| **Course schedule** | Topological sort. Detect cycle. |
| **Clone graph** | BFS/DFS + HashMap(original → clone). |
| **Word ladder** | BFS on implicit graph (words = nodes, edit distance = edges). |

---

## Sources

- CLRS — Chapters 22–25
- LeetCode — Graph problem sets


---

## Hands-On Exercises

### Exercise 1: BFS — Shortest Path in Unweighted Graph
Implement BFS to find the shortest distance from a source node to all other nodes in an unweighted graph.

```java
int[] bfsShortestPath(Map<Integer, List<Integer>> graph, int start, int n) {
    int[] dist = new int[n];
    Arrays.fill(dist, -1);  // -1 = unreachable
    Deque<Integer> queue = new ArrayDeque<>();
    // TODO: BFS from start, track distance at each level
}
```

---

### Exercise 2: DFS — Number of Islands
Given a 2D grid of `'1'`s (land) and `'0'`s (water), count the number of islands. An island is formed by connecting adjacent lands horizontally or vertically.

```java
int numIslands(char[][] grid) {
    int count = 0;
    // TODO: For each unvisited '1', increment count and DFS to mark all connected '1's
}
```

**Hint:** Mark visited cells by setting them to `'0'` (or use a `visited` boolean grid).

---

### Exercise 3: Topological Sort — Course Schedule
Given `numCourses` and prerequisites `[a, b]` meaning "to take `a`, you must first take `b`", determine if you can finish all courses (detect cycle in directed graph).

```java
boolean canFinish(int numCourses, int[][] prerequisites) {
    // TODO: Build adjacency list, then either:
    //   - DFS with 3 colors (white/gray/black) for cycle detection
    //   - BFS (Kahn's algorithm) — count processed nodes
}
```

---

## Assignments

| # | Problem | Difficulty | Key Technique |
|---|---------|:----------:|---------------|
| 1 | [Flood Fill](https://leetcode.com/problems/flood-fill/) (LC 733) | 🟢 Easy | DFS/BFS on Grid |
| 2 | [Island Perimeter](https://leetcode.com/problems/island-perimeter/) (LC 463) | 🟢 Easy | Grid Traversal |
| 3 | [Number of Islands](https://leetcode.com/problems/number-of-islands/) (LC 200) | 🟡 Medium | DFS/BFS Connected Components |
| 4 | [Clone Graph](https://leetcode.com/problems/clone-graph/) (LC 133) | 🟡 Medium | BFS/DFS + HashMap |
| 5 | [Course Schedule](https://leetcode.com/problems/course-schedule/) (LC 207) | 🟡 Medium | Topological Sort / Cycle Detection |
| 6 | [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/) (LC 210) | 🟡 Medium | Topological Sort (Kahn's) |
| 7 | [Word Ladder](https://leetcode.com/problems/word-ladder/) (LC 127) | 🔴 Hard | BFS on Implicit Graph |
| 8 | [Network Delay Time](https://leetcode.com/problems/network-delay-time/) (LC 743) | 🟡 Medium | Dijkstra's |

### Assignment Guidelines
- **Start** with 1–2 (Easy) — grid-based DFS/BFS.
- **Then** 3–6 (Medium) — graph traversal, cycle detection, topological sort.
- **Problem 7** (Word Ladder) is a classic BFS interview problem. Treat each word as a node.
- **Problem 8** (Network Delay Time) is a direct application of Dijkstra's from this note.
- **Target time:** 10 min per Easy, 25 min per Medium, 35 min per Hard.
