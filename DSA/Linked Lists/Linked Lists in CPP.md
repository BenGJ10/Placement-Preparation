# Linked Lists in C++

## Pointers, Memory Model, and Internal Mechanics

Linked Lists are not just a data structure problem — they are a **pointer + memory management problem**.
To truly master linked lists in C++, you must understand:

* pointers vs references
* stack vs heap memory
* dynamic allocation (`new` / `delete`)
* pointer manipulation safety
* why bugs like segmentation fault happen

---

## Why Linked Lists Are Pointer-Based Structures

Unlike arrays:

* arrays use **contiguous memory**
* linked lists use **nodes allocated dynamically**
* nodes are connected using **memory addresses**

Every linked list operation is fundamentally a **pointer update problem**.

---

## Memory Model in C++ (Very Important)

Before linked lists, understand memory regions.

### C++ Memory Layout

```
---------------------
| Stack            |  ← function calls, local variables
---------------------
| Heap             |  ← dynamic memory (new / delete)
---------------------
| Global / Static  |
---------------------
| Code Segment     |
---------------------
```

---

### Stack vs Heap

| Stack               | Heap                   |
| ------------------- | ---------------------- |
| Automatic memory    | Manual memory          |
| Fast                | Slower                 |
| Limited size        | Large                  |
| Local variables     | new / delete           |
| Freed automatically | Must be freed manually |

---

### Why Linked Lists Use Heap

Nodes must:

* persist after function exits
* be dynamically created
* grow/shrink at runtime

Hence: **heap allocation using `new`**

---

## Pointer Fundamentals


### What Is a Pointer?

A pointer stores the **address of a variable**, not the value.

```cpp
int x = 10;
int* p = &x;
```

* `x` → value = 10
* `&x` → address
* `p` → stores address of x
* `*p` → dereferences pointer → 10

---

### Pointer to Pointer (Why It Matters in LL)

```cpp
Node** head;
```

Used when:

* function must modify the **actual head pointer**
* especially insertion/deletion at head

---

### NULL vs nullptr

```cpp
Node* p = nullptr;   // modern C++
```

* `nullptr` is type-safe
* preferred over `NULL`

---

## Linked List Node in C++


### Basic Node Structure

```cpp
struct Node {
    int data;
    Node* next;
};
```

Each node contains:

* actual data
* pointer to next node’s address

---

### Memory Allocation of a Node

```cpp
Node* newNode = new Node();
```

What happens internally:

1. memory allocated on heap
2. constructor (if any) runs
3. address returned
4. stored in pointer

---

### Initializing Node Properly

```cpp
Node* newNode = new Node();
newNode->data = 10;
newNode->next = nullptr;
```

Failing to initialize `next` causes **undefined behavior**.

---

## How Linked List Looks in Memory

Example list:

```
10 → 20 → 30 → NULL
```

Memory view:

```
Address 1000: [10 | 2000]
Address 2000: [20 | 3000]
Address 3000: [30 | NULL]
```

Pointers store **addresses**, not indices.

---

## Creating a Linked List in C++

### Insert at Head (Pointer Reassignment)

```cpp
Node* insertAtHead(Node* head, int val) {
    Node* newNode = new Node();
    newNode->data = val;
    newNode->next = head;
    return newNode;
}
```

Why return head?
Because:

* head pointer changes
* caller must update its reference

---

### Insert at Tail

```cpp
Node* insertAtTail(Node* head, int val) {
    Node* newNode = new Node();
    newNode->data = val;
    newNode->next = nullptr;

    if (!head) return newNode;

    Node* temp = head;
    while (temp->next)
        temp = temp->next;

    temp->next = newNode;
    return head;
}
```

---

## Deletion and Memory Deallocation

### Why `delete` Is Mandatory

Using `new` without `delete` causes **memory leak**.

Bad:

```cpp
Node* n = new Node();
// no delete → memory leak
```

Correct:

```cpp
delete n;
```

---

### Delete Head Node Safely

```cpp
Node* deleteHead(Node* head) {
    if (!head) return nullptr;

    Node* temp = head;
    head = head->next;
    delete temp;

    return head;
}
```

---

### Delete Node by Value

```cpp
Node* deleteValue(Node* head, int val) {
    if (!head) return nullptr;

    if (head->data == val)
        return deleteHead(head);

    Node* cur = head;
    while (cur->next && cur->next->data != val)
        cur = cur->next;

    if (cur->next) {
        Node* del = cur->next;
        cur->next = del->next;
        delete del;
    }
    return head;
}
```

---

## Dangling Pointer (Very Dangerous)

```cpp
Node* p = new Node();
delete p;
// p is now dangling
```

Accessing `p->data` after delete = **undefined behavior**.

Correct:

```cpp
delete p;
p = nullptr;
```

---

## Why Segmentation Faults Happen in Linked Lists

Common reasons:

1. accessing `nullptr->next`
2. using uninitialized pointer
3. deleting same node twice
4. losing head pointer
5. incorrect loop conditions

---

### Example Bug

```cpp
Node* temp;
temp->next = head;   // temp not initialized
```

Fix:

```cpp
Node* temp = new Node();
```

---

## References vs Pointers in Linked Lists

### Using Pointer

```cpp
void insert(Node* head) { }
```

* cannot modify actual head

---

### Using Reference to Pointer

```cpp
void insert(Node*& head) {
    head = new Node();
}
```

This **directly modifies original head**.

Used heavily in clean C++ linked list APIs.

---

## Dummy Node Technique

Dummy node simplifies edge cases.

```cpp
Node dummy;
dummy.next = head;
Node* cur = &dummy;
```

Benefits:

* no special case for head deletion
* cleaner logic
* used in professional codebases

---

## Doubly Linked List Memory Structure

```cpp
struct Node {
    int data;
    Node* prev;
    Node* next;
};
```

Memory per node increases, but:

* backward traversal allowed
* deletion becomes O(1) if node known

Used in:

* LRU cache
* browser history
* undo/redo systems

---

## Complexity Reality

| Operation   | Time | Memory          |
| ----------- | ---- | --------------- |
| Access      | O(n) | O(1)            |
| Insert head | O(1) | heap allocation |
| Insert tail | O(n) | heap allocation |
| Delete      | O(1) | free heap       |

---

## Common Linked List Interview Questions

### Q1. Why linked list access is O(n)?

Because nodes are not contiguous and require pointer traversal.

---

### Q2. Why merge sort is best for linked list?

Because:

* no random access required
* pointer-based merging
* O(1) extra space for links

---

### Q3. Why quick sort is bad for linked lists?

Because:

* partitioning needs random access
* pointer manipulation becomes expensive

---

### Q4. Why memory leaks occur in linked lists?

Because `new` nodes are not properly `delete`d.

---

### Q5. Why dummy node is used?

To simplify pointer updates and avoid head edge cases.

---

## Best Practices for Linked Lists in C++

* always initialize pointers
* set deleted pointers to `nullptr`
* use dummy nodes
* prefer `nullptr` over `NULL`
* avoid raw pointers in production (use smart pointers if allowed)
* draw memory diagram when debugging

---

## Practice Problems (Pointer-Heavy)

* [https://leetcode.com/problems/reverse-linked-list/](https://leetcode.com/problems/reverse-linked-list/)
* [https://leetcode.com/problems/linked-list-cycle/](https://leetcode.com/problems/linked-list-cycle/)
* [https://leetcode.com/problems/remove-nth-node-from-end-of-list/](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
* [https://leetcode.com/problems/add-two-numbers/](https://leetcode.com/problems/add-two-numbers/)
* [https://leetcode.com/problems/lru-cache/](https://leetcode.com/problems/lru-cache/)
* [https://leetcode.com/problems/copy-list-with-random-pointer/](https://leetcode.com/problems/copy-list-with-random-pointer/)

---

## Summary

* Linked lists are pointer + heap memory structures

* Every operation is pointer reassignment

* Understanding memory layout prevents bugs

* `new` without `delete` causes leaks

* Dummy nodes simplify complex logic

* Master pointers to master linked lists

---
