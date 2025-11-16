# Data Structures

Data structures are ways of organizing and storing data for efficient access and modification.

## 📚 Linear Data Structures

### Arrays
- **Access**: O(1)
- **Search**: O(n)
- **Insertion**: O(n)
- **Deletion**: O(n)
- **Use Case**: Fixed-size collections, random access

### Linked Lists

#### Singly Linked List
```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None
    
    def append(self, data):
        new_node = Node(data)
        if not self.head:
            self.head = new_node
            return
        current = self.head
        while current.next:
            current = current.next
        current.next = new_node
```

- **Access**: O(n)
- **Search**: O(n)
- **Insertion**: O(1) at head, O(n) elsewhere
- **Deletion**: O(1) at head, O(n) elsewhere

#### Doubly Linked List
- Allows traversal in both directions
- Extra memory for previous pointer

### Stacks
- **Principle**: LIFO (Last In, First Out)
- **Operations**: Push, Pop, Peek
- **Time Complexity**: O(1) for all operations
- **Use Cases**: Function calls, undo operations, expression evaluation

```python
class Stack:
    def __init__(self):
        self.items = []
    
    def push(self, item):
        self.items.append(item)
    
    def pop(self):
        return self.items.pop() if not self.is_empty() else None
    
    def peek(self):
        return self.items[-1] if not self.is_empty() else None
    
    def is_empty(self):
        return len(self.items) == 0
```

### Queues
- **Principle**: FIFO (First In, First Out)
- **Operations**: Enqueue, Dequeue
- **Time Complexity**: O(1) for all operations
- **Use Cases**: Task scheduling, BFS

## 🌲 Tree Data Structures

### Binary Tree
- Each node has at most 2 children
- **Height**: log(n) to n

### Binary Search Tree (BST)
- Left subtree < Node < Right subtree
- **Average Time Complexity**: O(log n)
- **Worst Case**: O(n)

```python
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

def insert_bst(root, val):
    if not root:
        return TreeNode(val)
    if val < root.val:
        root.left = insert_bst(root.left, val)
    else:
        root.right = insert_bst(root.right, val)
    return root
```

### AVL Tree
- Self-balancing BST
- **Balance Factor**: -1, 0, or 1
- **Time Complexity**: O(log n) guaranteed

### Red-Black Tree
- Self-balancing BST with color properties
- Used in many standard libraries

### B-Tree
- Generalized BST for disk-based storage
- Used in databases and file systems

### Heap
- **Min-Heap**: Parent ≤ Children
- **Max-Heap**: Parent ≥ Children
- **Time Complexity**: O(log n) insert/delete, O(1) find min/max
- **Use Cases**: Priority queues, heap sort

## 🔗 Graph Data Structures

### Adjacency Matrix
- 2D array representation
- **Space**: O(V²)
- **Edge Check**: O(1)

### Adjacency List
- Array of linked lists
- **Space**: O(V + E)
- **Edge Check**: O(degree)

## 🗂️ Hash-based Structures

### Hash Table
- **Average Time Complexity**: O(1) for insert, delete, search
- **Collision Resolution**: Chaining, Open Addressing
- **Use Cases**: Dictionaries, caching, deduplication

```python
class HashTable:
    def __init__(self, size=10):
        self.size = size
        self.table = [[] for _ in range(size)]
    
    def hash_function(self, key):
        return hash(key) % self.size
    
    def insert(self, key, value):
        hash_index = self.hash_function(key)
        for i, (k, v) in enumerate(self.table[hash_index]):
            if k == key:
                self.table[hash_index][i] = (key, value)
                return
        self.table[hash_index].append((key, value))
```

### Hash Set
- Stores unique elements
- Fast membership testing

## 🎯 Advanced Data Structures

### Trie (Prefix Tree)
- **Use Cases**: Autocomplete, spell checking, IP routing
- **Space**: O(ALPHABET_SIZE * key_length * N)

### Segment Tree
- **Use Case**: Range queries
- **Time Complexity**: O(log n) query/update

### Fenwick Tree (Binary Indexed Tree)
- **Use Case**: Prefix sums, range queries
- **Space**: O(n)

### Disjoint Set Union (Union-Find)
- **Use Cases**: Network connectivity, Kruskal's algorithm
- **Time Complexity**: Nearly O(1) with path compression

## 📊 Comparison Table

| Data Structure | Access | Search | Insert | Delete | Space |
|----------------|--------|--------|--------|--------|-------|
| Array          | O(1)   | O(n)   | O(n)   | O(n)   | O(n)  |
| Linked List    | O(n)   | O(n)   | O(1)   | O(1)   | O(n)  |
| Stack          | O(n)   | O(n)   | O(1)   | O(1)   | O(n)  |
| Queue          | O(n)   | O(n)   | O(1)   | O(1)   | O(n)  |
| Hash Table     | -      | O(1)   | O(1)   | O(1)   | O(n)  |
| BST (balanced) | O(log n)| O(log n)| O(log n)| O(log n)| O(n)  |
| Heap           | O(n)   | O(n)   | O(log n)| O(log n)| O(n)  |

## Related Topics

- [[Algorithms]]
- [[Complexity Analysis]]
- [[Design Patterns]]

---

#datastructures #fundamentals #computerscience
