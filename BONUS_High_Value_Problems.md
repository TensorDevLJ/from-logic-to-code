# 🎯 BONUS: 30 High-Value Interview Problems

These problems appear frequently in FAANG/MAANG interviews.
Master these AFTER completing the A2Z sheet.

---
## 🟡 ARRAYS / PREFIX / GREEDY

### B1. Container With Most Water (LeetCode 11)
https://leetcode.com/problems/container-with-most-water/
```java
public int maxArea(int[] height) {
    int left = 0, right = height.length - 1, maxWater = 0;
    while (left < right) {
        maxWater = Math.max(maxWater, Math.min(height[left], height[right]) * (right - left));
        if (height[left] < height[right]) left++;  // Move shorter side
        else right--;
    }
    return maxWater;
}
// Time: O(N) | Two pointer — always move the shorter wall
```

### B2. Product of Array Except Self (LeetCode 238)
https://leetcode.com/problems/product-of-array-except-self/
```java
public int[] productExceptSelf(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    result[0] = 1;
    // Left pass: result[i] = product of all elements to the LEFT
    for (int i = 1; i < n; i++) result[i] = result[i-1] * nums[i-1];
    // Right pass: multiply by product of all elements to the RIGHT
    int right = 1;
    for (int i = n-1; i >= 0; i--) { result[i] *= right; right *= nums[i]; }
    return result;
}
// Time: O(N), Space: O(1) extra — no division used!
```

### B3. Gas Station (LeetCode 134)
https://leetcode.com/problems/gas-station/
```java
public int canCompleteCircuit(int[] gas, int[] cost) {
    int totalGas = 0, currGas = 0, start = 0;
    for (int i = 0; i < gas.length; i++) {
        totalGas += gas[i] - cost[i];
        currGas  += gas[i] - cost[i];
        if (currGas < 0) { start = i + 1; currGas = 0; }  // Reset start
    }
    return totalGas >= 0 ? start : -1;
}
// Key: if total gas >= total cost, solution exists. Greedy finds the start.
```

---
## 🟣 SLIDING WINDOW

### B4. Minimum Size Subarray Sum (LeetCode 209)
https://leetcode.com/problems/minimum-size-subarray-sum/
```java
public int minSubArrayLen(int target, int[] nums) {
    int left = 0, sum = 0, minLen = Integer.MAX_VALUE;
    for (int right = 0; right < nums.length; right++) {
        sum += nums[right];
        while (sum >= target) {
            minLen = Math.min(minLen, right - left + 1);
            sum -= nums[left++];
        }
    }
    return minLen == Integer.MAX_VALUE ? 0 : minLen;
}
// Time: O(N) | Shrink window when sum >= target
```

### B5. Permutation in String (LeetCode 567)
https://leetcode.com/problems/permutation-in-string/
```java
public boolean checkInclusion(String s1, String s2) {
    if (s1.length() > s2.length()) return false;
    int[] count = new int[26];
    for (char c : s1.toCharArray()) count[c-'a']++;
    int[] window = new int[26];
    int k = s1.length();
    for (int i = 0; i < s2.length(); i++) {
        window[s2.charAt(i)-'a']++;
        if (i >= k) window[s2.charAt(i-k)-'a']--;
        if (Arrays.equals(count, window)) return true;
    }
    return false;
}
// Fixed window of size s1.length(), compare frequency arrays
```

---
## 🔵 GRAPH INTERVIEW FAVORITES

### B6. Clone Graph (LeetCode 133)
https://leetcode.com/problems/clone-graph/
```java
Map<Node, Node> visited = new HashMap<>();
public Node cloneGraph(Node node) {
    if (node == null) return null;
    if (visited.containsKey(node)) return visited.get(node);
    Node clone = new Node(node.val);
    visited.put(node, clone);
    for (Node neighbor : node.neighbors)
        clone.neighbors.add(cloneGraph(neighbor));
    return clone;
}
// HashMap: original → clone. DFS to copy all nodes and edges.
```

### B7. Pacific Atlantic Water Flow (LeetCode 417)
https://leetcode.com/problems/pacific-atlantic-water-flow/
```java
// Reverse thinking: BFS/DFS from ocean boundaries inward
// Pacific touches top-left, Atlantic touches bottom-right
// A cell can flow to both if reachable from both oceans (reverse direction)
public List<List<Integer>> pacificAtlantic(int[][] heights) {
    int m = heights.length, n = heights[0].length;
    boolean[][] pac = new boolean[m][n], atl = new boolean[m][n];
    Queue<int[]> pQ = new LinkedList<>(), aQ = new LinkedList<>();

    for (int i = 0; i < m; i++) {
        pQ.offer(new int[]{i, 0}); pac[i][0] = true;
        aQ.offer(new int[]{i, n-1}); atl[i][n-1] = true;
    }
    for (int j = 0; j < n; j++) {
        pQ.offer(new int[]{0, j}); pac[0][j] = true;
        aQ.offer(new int[]{m-1, j}); atl[m-1][j] = true;
    }

    bfs(pQ, pac, heights); bfs(aQ, atl, heights);

    List<List<Integer>> res = new ArrayList<>();
    for (int i = 0; i < m; i++)
        for (int j = 0; j < n; j++)
            if (pac[i][j] && atl[i][j]) res.add(Arrays.asList(i, j));
    return res;
}

void bfs(Queue<int[]> q, boolean[][] visited, int[][] h) {
    int[][] dirs = {{0,1},{0,-1},{1,0},{-1,0}};
    int m = h.length, n = h[0].length;
    while (!q.isEmpty()) {
        int[] cell = q.poll();
        for (int[] d : dirs) {
            int r = cell[0]+d[0], c = cell[1]+d[1];
            if (r<0||r>=m||c<0||c>=n||visited[r][c]||h[r][c]<h[cell[0]][cell[1]]) continue;
            visited[r][c] = true; q.offer(new int[]{r, c});
        }
    }
}
```

---
## 🟢 DP MUST-KNOW

### B8. Decode Ways (LeetCode 91)
https://leetcode.com/problems/decode-ways/
```java
public int numDecodings(String s) {
    int n = s.length();
    int[] dp = new int[n+1];
    dp[0] = 1;
    dp[1] = s.charAt(0) != '0' ? 1 : 0;

    for (int i = 2; i <= n; i++) {
        int oneDigit = s.charAt(i-1) - '0';           // Last 1 digit
        int twoDigit = Integer.parseInt(s.substring(i-2, i)); // Last 2 digits

        if (oneDigit >= 1) dp[i] += dp[i-1];  // Valid single decode
        if (twoDigit >= 10 && twoDigit <= 26) dp[i] += dp[i-2]; // Valid double decode
    }
    return dp[n];
}
// Pattern: Fibonacci-like DP with validity checks
```

### B9. Interleaving String (LeetCode 97)
https://leetcode.com/problems/interleaving-string/
```java
public boolean isInterleave(String s1, String s2, String s3) {
    int m = s1.length(), n = s2.length();
    if (m + n != s3.length()) return false;
    boolean[][] dp = new boolean[m+1][n+1];
    dp[0][0] = true;
    for (int i = 1; i <= m; i++) dp[i][0] = dp[i-1][0] && s1.charAt(i-1) == s3.charAt(i-1);
    for (int j = 1; j <= n; j++) dp[0][j] = dp[0][j-1] && s2.charAt(j-1) == s3.charAt(j-1);
    for (int i = 1; i <= m; i++)
        for (int j = 1; j <= n; j++)
            dp[i][j] = (dp[i-1][j] && s1.charAt(i-1) == s3.charAt(i+j-1)) ||
                       (dp[i][j-1] && s2.charAt(j-1) == s3.charAt(i+j-1));
    return dp[m][n];
}
```

---
## 🔴 STACK / DESIGN

### B10. Evaluate Reverse Polish Notation (LeetCode 150)
https://leetcode.com/problems/evaluate-reverse-polish-notation/
```java
public int evalRPN(String[] tokens) {
    Deque<Integer> stack = new ArrayDeque<>();
    for (String token : tokens) {
        if ("+-*/".contains(token)) {
            int b = stack.pop(), a = stack.pop();
            switch (token) {
                case "+": stack.push(a+b); break;
                case "-": stack.push(a-b); break;
                case "*": stack.push(a*b); break;
                case "/": stack.push(a/b); break;
            }
        } else {
            stack.push(Integer.parseInt(token));
        }
    }
    return stack.pop();
}
// Classic stack problem: numbers → push, operators → pop 2, compute, push result
```

### B11. Daily Temperatures (LeetCode 739)
https://leetcode.com/problems/daily-temperatures/
```java
public int[] dailyTemperatures(int[] temperatures) {
    int n = temperatures.length;
    int[] result = new int[n];
    Deque<Integer> stack = new ArrayDeque<>();  // Monotonic decreasing stack of indices

    for (int i = 0; i < n; i++) {
        while (!stack.isEmpty() && temperatures[stack.peek()] < temperatures[i]) {
            int idx = stack.pop();
            result[idx] = i - idx;  // Days to wait
        }
        stack.push(i);
    }
    return result;
}
// Next Greater Element variant: answer = index difference
```

---
## 🟠 TREES

### B12. Path Sum II (LeetCode 113)
https://leetcode.com/problems/path-sum-ii/
```java
public List<List<Integer>> pathSum(TreeNode root, int target) {
    List<List<Integer>> result = new ArrayList<>();
    dfs(root, target, new ArrayList<>(), result);
    return result;
}
void dfs(TreeNode node, int remain, List<Integer> path, List<List<Integer>> result) {
    if (node == null) return;
    path.add(node.val);
    if (node.left == null && node.right == null && remain == node.val)
        result.add(new ArrayList<>(path));  // Deep copy!
    dfs(node.left, remain - node.val, path, result);
    dfs(node.right, remain - node.val, path, result);
    path.remove(path.size() - 1);  // Backtrack
}
```

### B13. Construct Binary Tree from Level Order (common interview question)
```java
// BFS approach: use queue of nodes, assign children from levelOrder array
public TreeNode buildFromLevelOrder(int[] levelOrder) {
    if (levelOrder == null || levelOrder.length == 0) return null;
    TreeNode root = new TreeNode(levelOrder[0]);
    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);
    int i = 1;
    while (!q.isEmpty() && i < levelOrder.length) {
        TreeNode node = q.poll();
        node.left = new TreeNode(levelOrder[i++]);
        q.offer(node.left);
        if (i < levelOrder.length) {
            node.right = new TreeNode(levelOrder[i++]);
            q.offer(node.right);
        }
    }
    return root;
}
```

---
## 🟤 DESIGN

### B14. Design HashMap (LeetCode 706)
https://leetcode.com/problems/design-hashmap/
```java
class MyHashMap {
    private static final int SIZE = 1000;
    private List<int[]>[] buckets;

    public MyHashMap() {
        buckets = new List[SIZE];
        for (int i = 0; i < SIZE; i++) buckets[i] = new ArrayList<>();
    }
    private int hash(int key) { return key % SIZE; }

    public void put(int key, int value) {
        List<int[]> bucket = buckets[hash(key)];
        for (int[] pair : bucket) if (pair[0] == key) { pair[1] = value; return; }
        bucket.add(new int[]{key, value});
    }
    public int get(int key) {
        for (int[] pair : buckets[hash(key)]) if (pair[0] == key) return pair[1];
        return -1;
    }
    public void remove(int key) {
        buckets[hash(key)].removeIf(pair -> pair[0] == key);
    }
}
// Chaining collision resolution, SIZE=1000 buckets
```

### B15. Design Hit Counter (common interview)
```java
class HitCounter {
    // Circular buffer approach — O(1) time, O(300) space
    private int[] times = new int[300];
    private int[] hits  = new int[300];

    public void hit(int timestamp) {
        int idx = timestamp % 300;
        if (times[idx] != timestamp) { times[idx] = timestamp; hits[idx] = 0; }
        hits[idx]++;
    }
    public int getHits(int timestamp) {
        int total = 0;
        for (int i = 0; i < 300; i++)
            if (timestamp - times[i] < 300) total += hits[i];
        return total;
    }
}
// Key: circular buffer of last 300 seconds, timestamp to invalidate old data
```

---
## 📊 QUICK REFERENCE: Pattern → Problem Mapping

| Problem Type | Pattern | Key DS |
|---|---|---|
| Shortest path (unweighted) | BFS | Queue |
| Shortest path (weighted) | Dijkstra | PriorityQueue |
| Shortest path (neg weights) | Bellman-Ford | Array |
| Detect cycle (undirected) | BFS/DFS | visited[] |
| Detect cycle (directed) | DFS with color | color[] |
| Topological order | Kahn's/DFS | inDegree[]/stack |
| Connected components | BFS/DFS/DSU | visited[]/DSU |
| MST | Prim's/Kruskal's | PQ/DSU |
| SCC | Kosaraju's | 2 DFS + transpose |
| Bridges | Tarjan's | disc/low arrays |
| Max flow | Ford-Fulkerson | BFS/DFS |
| String matching | KMP/Z/RK | LPS array/Z array |
| Next greater element | Mono Stack | Deque |
| Sliding window | Two pointer | HashMap/count |
| Subsequence/LCS | 2D DP | dp[m+1][n+1] |
| Knapsack | 0/1 DP | dp[W+1] R→L |
| Coin change | Unbounded DP | dp[amount] L→R |
