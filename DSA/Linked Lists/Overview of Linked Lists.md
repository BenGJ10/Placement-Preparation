# Linked Lists

A **Linked List** is a linear data structure where elements (nodes) are stored **non-contiguously** in memory.
Each node contains:

* **data**
* **pointer(s)** to the next (and possibly previous) node

Unlike arrays, linked lists do **not support random access**, but they excel at **dynamic insertion and deletion**.

---

## Why Linked Lists Exist

Arrays have limitations:

* fixed size (static arrays)
* costly insertions/deletions (O(n))
* memory wastage or overflow

Linked lists solve these by:

* dynamic size
* O(1) insertion/deletion (if pointer known)
* flexible memory usage

---

## Basic Terminology

* **Node**: individual element
* **Head**: first node
* **Tail**: last node
* **NULL**: indicates end of list

---

## Types of Linked Lists


## 1. Singly Linked List (SLL)

Each node points to the **next node only**.

```
[data | next] → [data | next] → NULL
```

### Structure

```cpp
struct Node {
    int data;
    Node* next;
};
```

---

## 2. Doubly Linked List (DLL)

Each node has **two pointers**:

* next
* previous

```
NULL ← [prev | data | next] ↔ [prev | data | next] → NULL
```

### Structure

```cpp
struct Node {
    int data;
    Node* prev;
    Node* next;
};
```

---

## 3. Circular Linked List

Last node points back to the **head** instead of NULL.

* Circular Singly Linked List
* Circular Doubly Linked List

Used in:

* round-robin scheduling
* buffer management

---

## Basic Operations on Singly Linked List

### Traversal

```cpp
void printList(Node* head) {
    while (head != NULL) {
        cout << head->data << " ";
        head = head->next;
    }
}
```

Time: **O(n)**

---

### Insertion Operations

### Insert at Head

```cpp
Node* insertAtHead(Node* head, int val) {
    Node* newNode = new Node();
    newNode->data = val;
    newNode->next = head;
    return newNode;
}
```

Time: **O(1)**

---

### Insert at Tail

```cpp
Node* insertAtTail(Node* head, int val) {
    Node* newNode = new Node();
    newNode->data = val;
    newNode->next = NULL;

    if (!head) return newNode;

    Node* temp = head;
    while (temp->next)
        temp = temp->next;

    temp->next = newNode;
    return head;
}
```

Time: **O(n)** (O(1) if tail pointer exists)

---

### Insert at Position

```cpp
Node* insertAtPos(Node* head, int pos, int val) {
    if (pos == 1) return insertAtHead(head, val);

    Node* temp = head;
    for (int i = 1; i < pos - 1 && temp; i++)
        temp = temp->next;

    if (!temp) return head;

    Node* newNode = new Node();
    newNode->data = val;
    newNode->next = temp->next;
    temp->next = newNode;

    return head;
}
```

---

### Deletion Operations

### Delete Head

```cpp
Node* deleteHead(Node* head) {
    if (!head) return NULL;
    Node* temp = head;
    head = head->next;
    delete temp;
    return head;
}
```

---

### Delete by Value

```cpp
Node* deleteValue(Node* head, int val) {
    if (!head) return NULL;

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

## Key Linked List Patterns (Interview Critical)

### 1. Fast and Slow Pointer (Floyd’s Algorithm)

Used for:

* cycle detection
* finding middle
* cycle start

---

### Detect Cycle

```cpp
bool hasCycle(Node* head) {
    Node *slow = head, *fast = head;

    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) return true;
    }
    return false;
}
```

---

### Find Middle of Linked List

```cpp
Node* middleNode(Node* head) {
    Node *slow = head, *fast = head;

    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
}
```

---

### 2. Reversal of Linked List


### Iterative Reversal

```cpp
Node* reverseList(Node* head) {
    Node *prev = NULL, *cur = head;

    while (cur) {
        Node* nextNode = cur->next;
        cur->next = prev;
        prev = cur;
        cur = nextNode;
    }
    return prev;
}
```

---

### Recursive Reversal

```cpp
Node* reverseRec(Node* head) {
    if (!head || !head->next) return head;

    Node* newHead = reverseRec(head->next);
    head->next->next = head;
    head->next = NULL;

    return newHead;
}
```

---

## 3. Merge Two Sorted Linked Lists

```cpp
Node* mergeLists(Node* l1, Node* l2) {
    Node dummy;
    Node* tail = &dummy;
    dummy.next = NULL;

    while (l1 && l2) {
        if (l1->data < l2->data) {
            tail->next = l1;
            l1 = l1->next;
        } else {
            tail->next = l2;
            l2 = l2->next;
        }
        tail = tail->next;
    }

    tail->next = l1 ? l1 : l2;
    return dummy.next;
}
```

---

### 4. Remove Nth Node from End

```cpp
Node* removeNthFromEnd(Node* head, int n) {
    Node dummy;
    dummy.next = head;

    Node *fast = &dummy, *slow = &dummy;

    for (int i = 0; i <= n; i++)
        fast = fast->next;

    while (fast) {
        fast = fast->next;
        slow = slow->next;
    }

    Node* del = slow->next;
    slow->next = del->next;
    delete del;

    return dummy.next;
}
```

---

### 5. Palindrome Linked List

Key steps:

1. find middle
2. reverse second half
3. compare halves

---

## Doubly Linked List (Key Differences)

| Feature   | Singly              | Doubly          |
| --------- | ------------------- | --------------- |
| Memory    | Less                | More            |
| Traversal | One direction       | Both directions |
| Deletion  | Needs prev tracking | Easier          |
| Use case  | Simple problems     | LRU cache       |

---

### Doubly Linked List Node

```cpp
struct Node {
    int data;
    Node* prev;
    Node* next;
};
```

---

## Time Complexity Summary

| Operation           | Time |
| ------------------- | ---- |
| Access              | O(n) |
| Insert at head      | O(1) |
| Insert at tail      | O(n) |
| Delete (known node) | O(1) |
| Search              | O(n) |

---

## Common Mistakes

* forgetting to update pointers
* memory leaks (missing delete)
* losing head reference
* infinite loops (cycle)
* incorrect base cases in recursion
* not handling empty list

---

## Interview Questions

### Q1. Why is linked list traversal O(n)?

Because nodes are not stored contiguously; random access is not possible.

---

### Q2. Why is merge sort preferred for linked lists?

Because:

* no random access required
* splitting and merging are pointer-based
* O(n log n) guaranteed

---

### Q3. How do you find the starting node of a cycle?

After slow == fast:

* move one pointer to head
* move both one step at a time
* meeting point is cycle start

---

### Q4. Difference between array and linked list?

| Array            | Linked List    |
| ---------------- | -------------- |
| contiguous       | non-contiguous |
| O(1) access      | O(n) access    |
| fixed size       | dynamic        |
| costly insertion | easy insertion |

---

### Q5. Why dummy node is useful?

It simplifies edge cases like:

* deletion at head
* empty list handling

---

## LeetCode Problems to Practice (Linked Lists)

* [https://leetcode.com/problems/reverse-linked-list/](https://leetcode.com/problems/reverse-linked-list/)
* [https://leetcode.com/problems/linked-list-cycle/](https://leetcode.com/problems/linked-list-cycle/)
* [https://leetcode.com/problems/middle-of-the-linked-list/](https://leetcode.com/problems/middle-of-the-linked-list/)
* [https://leetcode.com/problems/merge-two-sorted-lists/](https://leetcode.com/problems/merge-two-sorted-lists/)
* [https://leetcode.com/problems/remove-nth-node-from-end-of-list/](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)
* [https://leetcode.com/problems/palindrome-linked-list/](https://leetcode.com/problems/palindrome-linked-list/)
* [https://leetcode.com/problems/add-two-numbers/](https://leetcode.com/problems/add-two-numbers/)
* [https://leetcode.com/problems/sort-list/](https://leetcode.com/problems/sort-list/)
* [https://leetcode.com/problems/lru-cache/](https://leetcode.com/problems/lru-cache/)
* [https://leetcode.com/problems/copy-list-with-random-pointer/](https://leetcode.com/problems/copy-list-with-random-pointer/)

---

## Summary

* linked list is dynamic linear structure
* main types: singly, doubly, circular
* key patterns:

  * fast–slow pointers
  * reversal
  * merging
  * dummy node usage
* very common in interviews
* mastering pointer manipulation is crucial

---