# 🗺️ DSA MASTER CHEAT SHEET — All Patterns (Q1–468 + Bonus)

## ⚡ SORTING CHEAT SHEET
| Algorithm | Best | Avg | Worst | Space | Stable |
|-----------|------|-----|-------|-------|--------|
| Selection | O(N²) | O(N²) | O(N²) | O(1) | ❌ |
| Bubble | O(N) | O(N²) | O(N²) | O(1) | ✅ |
| Insertion | O(N) | O(N²) | O(N²) | O(1) | ✅ |
| Merge | O(N logN) | O(N logN) | O(N logN) | O(N) | ✅ |
| Quick | O(N logN) | O(N logN) | O(N²) | O(logN) | ❌ |
| Heap | O(N logN) | O(N logN) | O(N logN) | O(1) | ❌ |

## ⚡ BINARY SEARCH TEMPLATES

### Standard BS
```java
while (low <= high) {
    int mid = low + (high - low) / 2;
    if (arr[mid] == target) return mid;
    else if (arr[mid] < target) low = mid + 1;
    else high = mid - 1;
}
```

### Lower Bound (first index where arr[i] >= target)
```java
int result = arr.length;
while (low <= high) {
    int mid = low + (high - low) / 2;
    if (arr[mid] >= target) { result = mid; high = mid - 1; }
    else low = mid + 1;
}
```

### BS on Answer Space (minimize X such that condition holds)
```java
while (low <= high) {
    int mid = low + (high - low) / 2;
    if (isValid(mid)) { result = mid; high = mid - 1; }  // Try better
    else low = mid + 1;
}
```

## ⚡ BACKTRACKING TEMPLATE
```java
void backtrack(state, choices) {
    if (goal_reached) { record_result; return; }
    for (choice : choices) {
        if (!isValid(choice)) continue;   // Pruning
        make(choice);                      // Choose
        backtrack(new_state, new_choices); // Explore
        undo(choice);                      // Unchoose (backtrack!)
    }
}
```

## ⚡ SLIDING WINDOW TEMPLATES

### Fixed Window
```java
// Initialize window with first k elements
for (int i = k; i < n; i++) {
    add(arr[i]);         // Expand right
    remove(arr[i-k]);    // Shrink left
    updateAnswer();
}
```

### Variable Window
```java
int left = 0;
for (int right = 0; right < n; right++) {
    add(arr[right]);           // Expand right
    while (invalid_window) {
        remove(arr[left++]);   // Shrink left
    }
    updateAnswer(right - left + 1);
}
```

## ⚡ GRAPH ALGORITHMS COMPLEXITY
| Algorithm | Time | Space | Use Case |
|-----------|------|-------|---------|
| BFS | O(V+E) | O(V) | Shortest path (unweighted) |
| DFS | O(V+E) | O(V) | Cycle detect, topo sort |
| Dijkstra | O((V+E)logV) | O(V) | Shortest path (non-neg weights) |
| Bellman-Ford | O(VE) | O(V) | Shortest path (neg weights) |
| Floyd-Warshall | O(V³) | O(V²) | All-pairs shortest path |
| Kruskal | O(E logE) | O(V) | MST (sparse graph) |
| Prim | O((V+E)logV) | O(V) | MST (dense graph) |
| Topo Sort (Kahn) | O(V+E) | O(V) | DAG ordering |
| Kosaraju SCC | O(V+E) | O(V) | Strongly connected components |
| Tarjan Bridges | O(V+E) | O(V) | Bridge/Articulation detection |
| DSU (with opt) | O(α(N)) | O(N) | Union-Find |

## ⚡ DP PATTERNS SUMMARY
| Pattern | Example Problems | Recurrence Style |
|---------|-----------------|-----------------|
| 1D Fibonacci | Climbing Stairs, House Robber | dp[i] = dp[i-1] + dp[i-2] |
| 0/1 Knapsack | Subset Sum, Partition | dp[j] = max(dp[j], dp[j-w]+v) R→L |
| Unbounded | Coin Change, Rod Cut | dp[j] = min/max(dp[j], dp[j-w]+v) L→R |
| 2D String | LCS, Edit Distance | dp[i][j] based on match/mismatch |
| Grid DP | Unique Paths, Min Path | dp[i][j] = dp[i-1][j] + dp[i][j-1] |
| Partition (MCM) | Matrix Chain, Burst Balloons | dp[i][j] = try all k in [i,j] |
| LIS | LIS, Longest Bitonic | dp[i] = 1 + max(dp[j]) for j<i |
| Stock DP | All Stock variants | State machine (buy/sell/hold) |
| DP on Trees | Max path sum, Diameter | Post-order, return pair |

## ⚡ TIME COMPLEXITY CHEAT SHEET
| Operation | Array | LinkedList | Stack | Queue | HashSet | TreeSet |
|-----------|-------|------------|-------|-------|---------|---------|
| Access | O(1) | O(N) | O(N) | O(N) | O(1)* | O(logN) |
| Search | O(N) | O(N) | O(N) | O(N) | O(1)* | O(logN) |
| Insert | O(N) | O(1) | O(1) | O(1) | O(1)* | O(logN) |
| Delete | O(N) | O(1)** | O(1) | O(1) | O(1)* | O(logN) |
*Average case (amortized). **With pointer to node.

## ⚡ INTERVIEW PROBLEM IDENTIFICATION GUIDE

**"Maximum/minimum of something"** → DP or Greedy
**"Count ways"** → DP
**"All possibilities/combinations"** → Backtracking
**"Sorted array + search"** → Binary Search
**"Consecutive elements"** → Sliding Window
**"Next greater/smaller"** → Monotonic Stack
**"Shortest path"** → BFS (unweighted), Dijkstra (weighted)
**"Connected components"** → BFS/DFS/DSU
**"Detect cycle"** → DFS coloring / Floyd's (LL)
**"LRU / LFU"** → HashMap + DLL
**"Prefix matching"** → Trie
**"String matching"** → KMP / Z-Function
**"K-th largest/smallest"** → Heap / QuickSelect
**"Interval problems"** → Sort by start, Greedy
**"Matrix problems"** → BFS/DFS with (row, col) state
**"Tree + ancestor"** → LCA / DFS with depth tracking
**"Parentheses"** → Stack / Greedy counter

## ⚡ COMMON EDGE CASES CHECKLIST
- [ ] Empty input (null, empty array/string)
- [ ] Single element
- [ ] All same elements
- [ ] Already sorted / reverse sorted
- [ ] Negative numbers
- [ ] Integer overflow (use long)
- [ ] Off-by-one in loops/indices
- [ ] Cycle in linked list / graph
- [ ] Disconnected graph (outer loop needed)
- [ ] 0 as target value
