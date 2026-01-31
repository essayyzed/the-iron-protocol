# 📌 Semester 3 – Core CS I (Data Structures & Algorithms)

> **The Deep Dive Phase**: Master problem-solving patterns, understand memory layout, and build algorithmic thinking

---

## 🎯 Goals

- Build **strong DSA fundamentals** from first principles
- Develop **pattern recognition** for problem-solving
- Understand **how data structures map to memory**
- Analyze **time and space complexity** confidently
- Ship **1-2 DSA-centric projects**

---

## 📚 University Courses (Typical)

- Data Structures & Algorithms
- Computer Organization / Architecture
- Discrete Mathematics II / Logic & Proofs
- Elective / General Education

---

## 🛠 Parallel Track (Self-Directed)

Dedicate **3-5 hours per week** to deliberate DSA practice alongside coursework.

### Daily Practice (30-45 minutes/day)

#### 📝 **Problem-Solving Routine**

**Monday, Wednesday, Friday:**
- Solve **1 curated problem** (focus on one pattern)
- Implement from scratch (no copy-paste)
- Write complexity analysis in comments
- Add to your pattern notebook

**Tuesday, Thursday:**
- **Review** previous solutions
- **Optimize** one old solution
- **Explain** the pattern to someone (or rubber duck)

**Weekend:**
- **Deep dive** into one data structure
- Implement it from scratch in your language
- Write tests
- Document how it works

#### 📖 **Pattern Notebook**

Keep a digital or physical notebook with:

```markdown
## Pattern: Two Pointers

**When to use:** 
- Sorted array problems
- Finding pairs with target sum
- Removing duplicates

**Template:**
```python
left, right = 0, len(arr) - 1
while left < right:
    # check condition
    # move pointers
```

**Problems solved:**
- Two Sum II
- Container With Most Water
- Remove Duplicates

**Key insight:** Reduces O(n²) to O(n)
```

---

### Weekly Deep Dives

#### Week 1-2: Arrays & Strings

**Core Concepts:**
- Array indexing and traversal
- String manipulation techniques
- In-place modifications
- Sliding window pattern
- Two pointers technique

**Implementation Tasks:**

```python
# 1. Reverse array in-place
def reverse_array(arr):
    """Two pointers: O(n) time, O(1) space"""
    left, right = 0, len(arr) - 1
    while left < right:
        arr[left], arr[right] = arr[right], arr[left]
        left += 1
        right -= 1

# 2. Check palindrome
def is_palindrome(s):
    """Ignore non-alphanumeric, case-insensitive"""
    cleaned = ''.join(c.lower() for c in s if c.isalnum())
    return cleaned == cleaned[::-1]

# 3. Sliding window max sum
def max_subarray_sum(arr, k):
    """Maximum sum of k consecutive elements"""
    if len(arr) < k:
        return None
    
    # Initialize first window
    window_sum = sum(arr[:k])
    max_sum = window_sum
    
    # Slide window
    for i in range(k, len(arr)):
        window_sum += arr[i] - arr[i - k]
        max_sum = max(max_sum, window_sum)
    
    return max_sum
```

**Curated Problems:**
1. Two Sum (hash map pattern)
2. Best Time to Buy/Sell Stock
3. Valid Anagram
4. Longest Substring Without Repeating Characters

**Memory Insight:** Arrays are contiguous memory blocks. Index access is O(1) because `address = base + (index * element_size)`.

---

#### Week 3-4: Linked Lists

**Core Concepts:**
- Node structure (data + pointer)
- Traversal patterns
- Two-pointer techniques (fast/slow)
- Dummy node trick
- In-place reversal

**Complete Implementation:**

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None
    
    def insert_at_beginning(self, data):
        """O(1) insertion"""
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node
    
    def insert_at_end(self, data):
        """O(n) traversal required"""
        new_node = Node(data)
        if not self.head:
            self.head = new_node
            return
        
        current = self.head
        while current.next:
            current = current.next
        current.next = new_node
    
    def reverse(self):
        """Reverse in-place: O(n) time, O(1) space"""
        prev = None
        current = self.head
        
        while current:
            next_node = current.next
            current.next = prev
            prev = current
            current = next_node
        
        self.head = prev
    
    def detect_cycle(self):
        """Floyd's algorithm: fast/slow pointers"""
        if not self.head:
            return False
        
        slow = fast = self.head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True
        return False
    
    def find_middle(self):
        """Slow/fast pointers to find middle"""
        slow = fast = self.head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        return slow
```

**Curated Problems:**
1. Reverse Linked List
2. Detect Cycle
3. Merge Two Sorted Lists
4. Remove Nth Node From End

**Memory Insight:** Linked lists use scattered memory locations. Each node stores data + pointer. Trade-off: flexible insertion O(1) vs. no random access.

---

#### Week 5-6: Stacks & Queues

**Core Concepts:**
- LIFO (Stack) vs FIFO (Queue)
- Applications: function calls, expression evaluation, BFS
- Monotonic stack/queue patterns

**Stack Implementation:**

```python
class Stack:
    def __init__(self):
        self.items = []
    
    def push(self, item):
        self.items.append(item)
    
    def pop(self):
        if not self.is_empty():
            return self.items.pop()
    
    def peek(self):
        if not self.is_empty():
            return self.items[-1]
    
    def is_empty(self):
        return len(self.items) == 0

# Application: Balanced parentheses
def is_balanced(s):
    """Check if parentheses are balanced"""
    stack = []
    pairs = {')': '(', '}': '{', ']': '['}
    
    for char in s:
        if char in '({[':
            stack.append(char)
        elif char in ')}]':
            if not stack or stack.pop() != pairs[char]:
                return False
    
    return len(stack) == 0
```

**Queue Implementation:**

```python
from collections import deque

class Queue:
    def __init__(self):
        self.items = deque()
    
    def enqueue(self, item):
        self.items.append(item)
    
    def dequeue(self):
        if not self.is_empty():
            return self.items.popleft()
    
    def is_empty(self):
        return len(self.items) == 0
```

**Curated Problems:**
1. Valid Parentheses
2. Min Stack (O(1) min operation)
3. Implement Queue using Stacks
4. Sliding Window Maximum (monotonic deque)

---

#### Week 7-8: Recursion & Backtracking

**Core Concepts:**
- Base case + recursive case
- Call stack visualization
- Backtracking template

**Recursion Patterns:**

```python
# Pattern 1: Simple recursion
def factorial(n):
    if n <= 1:
        return 1
    return n * factorial(n - 1)

# Pattern 2: Binary recursion
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

# Pattern 3: Binary search
def binary_search(arr, target, left, right):
    if left > right:
        return -1
    
    mid = (left + right) // 2
    if arr[mid] == target:
        return mid
    elif arr[mid] < target:
        return binary_search(arr, target, mid + 1, right)
    else:
        return binary_search(arr, target, left, mid - 1)
```

**Backtracking Template:**

```python
def backtrack(path, choices):
    """Universal template"""
    # Base case
    if is_solution(path):
        result.append(path[:])
        return
    
    # Try each choice
    for choice in choices:
        # Make choice
        path.append(choice)
        # Recurse
        backtrack(path, updated_choices)
        # Undo choice
        path.pop()

# Application: Generate all subsets
def subsets(nums):
    result = []
    
    def backtrack(start, path):
        result.append(path[:])
        for i in range(start, len(nums)):
            path.append(nums[i])
            backtrack(i + 1, path)
            path.pop()
    
    backtrack(0, [])
    return result
```

**Curated Problems:**
1. Power(x, n)
2. Subsets
3. Permutations
4. N-Queens

---

#### Week 9-10: Trees

**Core Concepts:**
- Tree traversals (inorder, preorder, postorder, level-order)
- Binary Search Tree properties
- Tree height and balance

**BST Implementation:**

```python
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

class BST:
    def __init__(self):
        self.root = None
    
    def insert(self, val):
        if not self.root:
            self.root = TreeNode(val)
        else:
            self._insert_recursive(self.root, val)
    
    def _insert_recursive(self, node, val):
        if val < node.val:
            if node.left is None:
                node.left = TreeNode(val)
            else:
                self._insert_recursive(node.left, val)
        else:
            if node.right is None:
                node.right = TreeNode(val)
            else:
                self._insert_recursive(node.right, val)
    
    def search(self, val):
        return self._search_recursive(self.root, val)
    
    def _search_recursive(self, node, val):
        if node is None or node.val == val:
            return node
        if val < node.val:
            return self._search_recursive(node.left, val)
        return self._search_recursive(node.right, val)
    
    def inorder_traversal(self):
        """Returns sorted order for BST"""
        result = []
        self._inorder(self.root, result)
        return result
    
    def _inorder(self, node, result):
        if node:
            self._inorder(node.left, result)
            result.append(node.val)
            self._inorder(node.right, result)
```

**Level-Order Traversal (BFS):**

```python
from collections import deque

def level_order(root):
    if not root:
        return []
    
    result = []
    queue = deque([root])
    
    while queue:
        level_size = len(queue)
        level = []
        
        for _ in range(level_size):
            node = queue.popleft()
            level.append(node.val)
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        result.append(level)
    
    return result
```

**Curated Problems:**
1. Validate BST
2. Lowest Common Ancestor
3. Maximum Depth
4. Serialize/Deserialize Binary Tree

---

#### Week 11-12: Hashing

**Core Concepts:**
- Hash function design
- Collision handling
- When to use hash map vs array vs set

**Hash Map Implementation:**

```python
class HashMap:
    def __init__(self, capacity=16):
        self.capacity = capacity
        self.size = 0
        self.buckets = [[] for _ in range(capacity)]
    
    def _hash(self, key):
        return hash(key) % self.capacity
    
    def put(self, key, value):
        index = self._hash(key)
        bucket = self.buckets[index]
        
        # Update if exists
        for i, (k, v) in enumerate(bucket):
            if k == key:
                bucket[i] = (key, value)
                return
        
        # Insert new
        bucket.append((key, value))
        self.size += 1
        
        # Resize if needed
        if self.size / self.capacity > 0.75:
            self._resize()
    
    def get(self, key):
        index = self._hash(key)
        bucket = self.buckets[index]
        
        for k, v in bucket:
            if k == key:
                return v
        
        raise KeyError(key)
```

**Common Patterns:**

```python
# Pattern 1: Two Sum
def two_sum(nums, target):
    seen = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i

# Pattern 2: Group Anagrams
def group_anagrams(strs):
    groups = {}
    for s in strs:
        key = tuple(sorted(s))
        if key not in groups:
            groups[key] = []
        groups[key].append(s)
    return list(groups.values())
```

**Curated Problems:**
1. Two Sum
2. Group Anagrams
3. Longest Consecutive Sequence
4. Subarray Sum Equals K

---

### Complexity Analysis Mastery

#### Time Complexity Rules

1. Drop constants: O(2n) → O(n)
2. Drop non-dominant terms: O(n² + n) → O(n²)
3. Different inputs = different variables: O(a + b)

**Common Complexities:**

| Complexity | Name | Example |
|------------|------|---------|
| O(1) | Constant | Array access, hash map get |
| O(log n) | Logarithmic | Binary search, balanced BST |
| O(n) | Linear | Single loop, linear search |
| O(n log n) | Linearithmic | Merge sort, heap sort |
| O(n²) | Quadratic | Nested loops, bubble sort |
| O(2ⁿ) | Exponential | Recursive fibonacci |
| O(n!) | Factorial | Permutations |

**Analysis Examples:**

```python
# Example 1: O(n)
def sum_array(arr):
    total = 0
    for num in arr:  # n iterations
        total += num  # O(1)
    return total
# Total: O(n)

# Example 2: O(n²)
def has_duplicate(arr):
    for i in range(len(arr)):  # n times
        for j in range(i + 1, len(arr)):  # n times
            if arr[i] == arr[j]:
                return True
    return False
# Total: O(n²)

# Example 3: O(log n)
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:  # Halves each time
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
# Total: O(log n)
```

---

## 📦 Projects (Choose 1-2)

### Project 1: Data Structure Library

**Difficulty:** Intermediate  
**Time:** 3-4 weeks

**Requirements:**
- Implement from scratch:
  - Dynamic Array
  - Linked List (singly & doubly)
  - Stack & Queue
  - Binary Search Tree
  - Min/Max Heap
  - Hash Map
- Full CRUD operations
- Unit tests
- Complexity documentation

**Repository Structure:**
```
ds-library/
├── README.md
├── src/
│   ├── array.py
│   ├── linked_list.py
│   ├── stack.py
│   ├── tree.py
│   └── hashmap.py
├── tests/
│   └── test_*.py
└── docs/
    └── complexity.md
```

---

### Project 2: Algorithm Visualizer

**Difficulty:** Intermediate-Advanced  
**Time:** 4-5 weeks

**Goal:** Build a tool that visualizes algorithms step-by-step.

**Core Features:**
- Visualize sorting algorithms:
  - Bubble, Selection, Insertion Sort
  - Merge Sort, Quick Sort
- Visualize search algorithms:
  - Linear Search, Binary Search
  - BFS, DFS
- Show:
  - Step-by-step execution
  - Array/tree state at each step
  - Comparisons and swaps count

**Tech Stack:**
- **Web**: HTML/CSS/JavaScript + Canvas
- **Python**: Pygame or Matplotlib animations
- **CLI**: Rich library for terminal graphics

---

## ✅ Semester 3 Checklist

- [ ] I can implement stack, queue, linked list, BST, heap from scratch
- [ ] I can analyze and explain O(1), O(n), O(log n), O(n²), O(2ⁿ)
- [ ] I understand how data structures map to memory
- [ ] I can recognize and apply 5-7 problem-solving patterns
- [ ] I solved 30-50 curated problems with understanding
- [ ] I maintain a pattern notebook with templates
- [ ] I can explain trade-offs between different data structures
- [ ] I shipped at least one DSA project
- [ ] I can perform complexity analysis on my own code
- [ ] I can trace recursion and understand the call stack

---

## ⚠️ Common Mistakes

### ❌ **LeetCode grinding without understanding**
Solving 500 problems randomly won't help. Focus on **patterns**, not quantity.

### ❌ **Skipping implementation**
Reading solutions ≠ understanding. You must **implement from scratch**.

### ❌ **Ignoring complexity analysis**
Always analyze time/space complexity. It's not optional.

### ❌ **Memorizing solutions**
Train pattern recognition, not memory. Understand **why** a solution works.

### ❌ **Not connecting to memory**
Understand **why** arrays are O(1) access but linked lists aren't.

---

## 📖 Recommended Resources

### Books
- *Grokking Algorithms* by Aditya Bhargava
- *Introduction to Algorithms (CLRS)*
- *Cracking the Coding Interview* by Gayle McDowell

### Courses
- [Data Structures and Algorithms Specialization](https://www.coursera.org/specializations/data-structures-algorithms) - UC San Diego
- [Algorithms, Part I & II](https://www.coursera.org/learn/algorithms-part1) - Princeton
- [MIT 6.006 Introduction to Algorithms](https://ocw.mit.edu/courses/6-006-introduction-to-algorithms-fall-2011/)

### Problem Sets
- [NeetCode 150](https://neetcode.io/) - curated by pattern
- [Blind 75](https://leetcode.com/discuss/general-discussion/460599/blind-75-leetcode-questions)
- [LeetCode Patterns](https://seanprashad.com/leetcode-patterns/)

### Visualizations
- [VisuAlgo](https://visualgo.net/)
- [Algorithm Visualizer](https://algorithm-visualizer.org/)

---

## 🔗 Reinforcement (Optional)

- [LeetCode, HackerRank, TopCoder are Bad for Programming Newbies](https://www.youtube.com/watch?v=Zb8zsbtgmSI) — focus on patterns before grinding.
- [Teach Yourself Programming in 10 Years](https://www.youtube.com/watch?v=p7Oi74K9p18) — slow, deliberate practice and long-term skill building.

---

## 🎯 Success Metric

**You know you're ready for Semester 4 when:**
- You can implement any basic data structure from memory
- You can identify the right data structure for a given problem
- You can analyze complexity without looking it up
- You recognize common patterns in new problems

---

**Next:** [Semester 4 – Core CS II (Operating Systems, Networks, Databases)](./semester_4.md)

---

*"Don't aim to solve 500 problems. Aim to understand 50 patterns deeply."*  
*— The Protocol*
