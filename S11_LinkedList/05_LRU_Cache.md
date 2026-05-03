# 05. LRU Cache

**LeetCode:** https://leetcode.com/problems/lru-cache/
**Difficulty:** 🟡 Medium | **Pattern:** Doubly Linked List + HashMap

---

## 🧠 Intuition

**In Simple Words:**
Design a cache with capacity. When full and a new item comes:
- Remove the LEAST RECENTLY USED item
- Add new item
Both get() and put() should be O(1)!

**Real-Life Analogy:**
Browser tabs. When you have too many open, the browser closes the one you haven't used in the longest time.

**Core Idea:**
- HashMap: key → node (for O(1) access)
- Doubly Linked List: maintain ORDER of use (most recent ↔ least recent)
- Most recently used: near HEAD
- Least recently used: near TAIL
- On get/put: move accessed node to HEAD
- On capacity exceeded: remove TAIL node

---

## 🚀 Solution

```java
import java.util.*;

public class LRUCache {
    // Doubly Linked List Node
    class Node {
        int key, value;
        Node prev, next;
        Node(int k, int v) { key = k; value = v; }
    }

    private int capacity;
    private Map<Integer, Node> map;  // key → node
    private Node head, tail;          // Dummy head and tail

    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.map = new HashMap<>();

        // Dummy nodes (simplify edge cases)
        head = new Node(0, 0);  // Most recent end
        tail = new Node(0, 0);  // Least recent end
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        if (!map.containsKey(key)) return -1;

        Node node = map.get(key);
        moveToFront(node);  // Most recently used
        return node.value;
    }

    public void put(int key, int value) {
        if (map.containsKey(key)) {
            Node node = map.get(key);
            node.value = value;
            moveToFront(node);
        } else {
            if (map.size() == capacity) {
                // Remove LRU (node before tail)
                Node lru = tail.prev;
                removeNode(lru);
                map.remove(lru.key);
            }
            Node newNode = new Node(key, value);
            addToFront(newNode);
            map.put(key, newNode);
        }
    }

    private void moveToFront(Node node) {
        removeNode(node);
        addToFront(node);
    }

    private void removeNode(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    private void addToFront(Node node) {
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
    }

    public static void main(String[] args) {
        LRUCache cache = new LRUCache(2);
        cache.put(1, 1);   // cache: {1=1}
        cache.put(2, 2);   // cache: {2=2, 1=1}
        System.out.println(cache.get(1)); // 1, cache: {1=1, 2=2}
        cache.put(3, 3);   // evict 2, cache: {3=3, 1=1}
        System.out.println(cache.get(2)); // -1 (not found)
        cache.put(4, 4);   // evict 1, cache: {4=4, 3=3}
        System.out.println(cache.get(1)); // -1
        System.out.println(cache.get(3)); // 3
        System.out.println(cache.get(4)); // 4
    }
}
```

**Time:** O(1) for both get and put  
**Space:** O(capacity)

**Visual — DLL structure:**
```
HEAD ↔ [most recent] ↔ ... ↔ [least recent] ↔ TAIL
        ↑ add here                    ↑ evict from here
```

---

## 🔁 Pattern Recognition

**Pattern:** HashMap + Doubly Linked List (DLL)
- HashMap gives O(1) lookup
- DLL gives O(1) insertion and deletion at any position

---

## ⚠️ Common Mistakes

1. ❌ Using singly LL (can't remove node in O(1) without knowing prev)
2. ❌ Not using dummy head/tail (messy edge case code)
3. ❌ Forgetting to remove from map when evicting
4. ❌ Not updating node on put if key already exists

---

## 🎯 Final Summary

- HashMap maps key→Node for O(1) access
- DLL maintains usage order (front=recent, back=old)
- get: if found, move to front, return value
- put: if exists, update+move to front. If new: if full evict tail.prev, add to front
- Always update both map AND DLL together
