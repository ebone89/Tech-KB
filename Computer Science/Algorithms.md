# Algorithms

Algorithms are step-by-step procedures for solving problems or performing computations.

## 📖 Common Algorithm Types

### Sorting Algorithms

#### Quick Sort
- **Time Complexity**: O(n log n) average, O(n²) worst case
- **Space Complexity**: O(log n)
- **Approach**: Divide and conquer
- **Use Case**: General-purpose sorting

```python
def quicksort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    return quicksort(left) + middle + quicksort(right)
```

#### Merge Sort
- **Time Complexity**: O(n log n)
- **Space Complexity**: O(n)
- **Approach**: Divide and conquer
- **Use Case**: Stable sorting, external sorting

#### Bubble Sort
- **Time Complexity**: O(n²)
- **Space Complexity**: O(1)
- **Use Case**: Educational purposes, small datasets

### Searching Algorithms

#### Binary Search
- **Time Complexity**: O(log n)
- **Space Complexity**: O(1)
- **Requirement**: Sorted array

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

#### Linear Search
- **Time Complexity**: O(n)
- **Use Case**: Unsorted data

### Graph Algorithms

#### Depth-First Search (DFS)
- **Time Complexity**: O(V + E)
- **Space Complexity**: O(V)
- **Use Case**: Path finding, cycle detection

#### Breadth-First Search (BFS)
- **Time Complexity**: O(V + E)
- **Space Complexity**: O(V)
- **Use Case**: Shortest path in unweighted graphs

#### Dijkstra's Algorithm
- **Time Complexity**: O((V + E) log V)
- **Use Case**: Shortest path in weighted graphs

### Dynamic Programming

#### Fibonacci Sequence
```python
def fibonacci(n, memo={}):
    if n in memo:
        return memo[n]
    if n <= 2:
        return 1
    memo[n] = fibonacci(n-1, memo) + fibonacci(n-2, memo)
    return memo[n]
```

#### Knapsack Problem
- **Use Case**: Optimization problems

## 🎯 Algorithm Design Strategies

1. **Divide and Conquer**: Break problem into smaller subproblems
2. **Greedy**: Make locally optimal choices
3. **Dynamic Programming**: Store and reuse solutions to subproblems
4. **Backtracking**: Try all possibilities, backtrack when needed
5. **Branch and Bound**: Systematically search solution space

## 📊 Complexity Classes

- **P**: Polynomial time solvable
- **NP**: Non-deterministic polynomial time verifiable
- **NP-Complete**: Hardest problems in NP
- **NP-Hard**: At least as hard as NP-Complete

## Related Topics

- [[Data Structures]]
- [[Complexity Analysis]]
- [[Graph Theory]]

---

#algorithms #fundamentals #computerscience
