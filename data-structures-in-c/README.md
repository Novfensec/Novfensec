# 📘 Data Structures using C — Complete Viva Preparation Guide

*Diagrams are loaded from Wikimedia Commons. If one doesn't load offline, just read the text – it's self-contained.*

---

## 📑 Table of Contents
1. [Module I – Introduction, Arrays, Strings, Sparse Matrix](#module-i--introduction-to-data-structures)
2. [Module II – Stacks and Queues](#module-ii--stacks-and-queues)
3. [Module III – Linked Lists](#module-iii--programming-with-linked-lists)
4. [Module IV – Trees](#module-iv--trees)
5. [Module V – Searching and Sorting](#module-v--searching-and-sorting)
6. [Module VI – Graphs](#module-vi--graphs-and-their-applications)
7. [Master Complexity Cheat Sheet](#-master-complexity-cheat-sheet)
8. [Rapid-Fire Viva Questions](#-rapid-fire-viva-questions)
9. [Last-Night Checklist](#-last-night-checklist)

---

# Module I – Introduction to Data Structures

## 1.1 Definition
A **data structure** is a particular way of **organizing, storing and managing data in memory** so that it can be accessed and modified **efficiently**.

> Data structure = Data + Relationships between data + Operations on data

### Classification

```
                    Data Structures
                   /               \
            Primitive           Non-Primitive
        (int, char, float,      /           \
         double, pointer)   Linear        Non-Linear
                         (Array, Stack,   (Tree, Graph)
                          Queue, Linked List)
```

| Basis | Types |
|---|---|
| Primitive vs Non-primitive | `int`, `char`, `float` vs arrays, lists, trees |
| Linear vs Non-linear | Elements in a sequence (array, stack, queue, list) vs hierarchical/network (tree, graph) |
| Static vs Dynamic | Fixed size at compile time (array) vs size changes at runtime (linked list) |
| Homogeneous vs Heterogeneous | Same type (array) vs mixed types (structure) |

**Abstract Data Type (ADT):** a mathematical model that defines *what* operations are done (interface) without saying *how* (implementation). Stack, Queue, List are ADTs.

## 1.2 Algorithm Design
An **algorithm** is a finite, step-by-step set of instructions to solve a problem.

**Properties:** Input, Output, Definiteness, Finiteness, Effectiveness.

**Ways to write:** Natural language, flowchart, pseudocode, program.

**Design techniques:** Brute force, Divide & Conquer (merge/quick sort), Greedy (Prim/Kruskal), Dynamic Programming, Backtracking, Recursion.

## 1.3 Complexity Analysis

![Growth of functions](https://commons.wikimedia.org/wiki/Special:FilePath/Comparison_computational_complexity.svg)

- **Time complexity** – amount of time an algorithm takes as a function of input size `n`.
- **Space complexity** – amount of memory it needs = fixed part (code, constants) + variable part (input, stack, dynamic memory).

### Asymptotic Notations
| Notation | Meaning | Remember as |
|---|---|---|
| **O (Big-Oh)** | Upper bound | Worst case |
| **Ω (Omega)** | Lower bound | Best case |
| **Θ (Theta)** | Tight bound (both) | Average/exact |

Order of growth: `O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ) < O(n!)`

**Example – counting steps**
```c
for (i = 0; i < n; i++)          // runs n times
    for (j = 0; j < n; j++)      // runs n times each
        sum++;                    // → O(n²)

for (i = 1; i < n; i *= 2)       // doubles → O(log n)
    sum++;
```

### Best / Average / Worst case
Linear search: best O(1) (first element), worst O(n) (last/absent), average O(n/2) ≈ O(n).

## 1.4 Time–Space Tradeoff
You can often **save time by using more memory**, or **save memory by spending more time**.

Examples:
- **Lookup table / hashing** – more memory, O(1) search.
- **Memoization in recursion** (Fibonacci) – stores results, saves time.
- **Compressed data** – less space, more time to decompress.
- **Linked list vs Array** – list uses extra pointer memory but insertion is faster.

## 1.5 Pointers in Data Structures
A **pointer** stores the **address** of another variable.

```c
int a = 10;
int *p = &a;       // p holds address of a
printf("%d", *p);  // dereference → 10
```
| Operator | Meaning |
|---|---|
| `&` | address-of |
| `*` | value-at (dereference) |

**Why pointers matter in DS:** dynamic memory allocation, linked lists, trees, graphs (nodes linked by addresses), passing by reference.

```c
int *arr = (int*) malloc(5 * sizeof(int));  // allocate
arr = (int*) realloc(arr, 10 * sizeof(int));// resize
free(arr);                                  // release
```
- `malloc` – allocates uninitialized block; `calloc` – allocates and zero-initializes; `realloc` – resizes; `free` – deallocates.
- **Dangling pointer** – points to freed memory. **Null pointer** – points to nothing. **Memory leak** – allocated memory never freed.
- **Self-referential structure:**
```c
struct node {
    int data;
    struct node *next;   // pointer to same type
};
```

## 1.6 Arrays

An **array** is a collection of **homogeneous elements stored in contiguous memory locations**, accessed by index.

![Array memory layout](https://commons.wikimedia.org/wiki/Special:FilePath/Row_and_column_major_order.svg)

### Representation of Linear Array in Memory
If `LB` = lower bound, `UB` = upper bound, `Base` = address of first element, `w` = size of each element:

```
Length   = UB − LB + 1
LOC(A[k]) = Base + w × (k − LB)
```
Example: `int A[10]` at base 1000, `w=4`: `LOC(A[5]) = 1000 + 4×5 = 1020`.

### Traversing
```c
for (i = 0; i < n; i++)
    printf("%d ", a[i]);       // O(n)
```

### Insertion at position `pos`
```c
void insert(int a[], int *n, int pos, int val) {
    for (int i = *n - 1; i >= pos; i--)
        a[i + 1] = a[i];       // shift right
    a[pos] = val;
    (*n)++;
}
```
### Deletion at position `pos`
```c
void delete(int a[], int *n, int pos) {
    for (int i = pos; i < *n - 1; i++)
        a[i] = a[i + 1];       // shift left
    (*n)--;
}
```
Insertion/deletion: **O(n)** (shifting). At end: O(1).

### Two-Dimensional Arrays
`int a[3][4];` – 3 rows, 4 columns.

**Row-major** (C uses this): `LOC(A[i][j]) = Base + w × (n × (i − LBr) + (j − LBc))` where `n` = number of columns.
**Column-major** (Fortran): `LOC(A[i][j]) = Base + w × (m × (j − LBc) + (i − LBr))` where `m` = number of rows.

Example: `int A[3][4]`, base 2000, w=2, find `A[2][1]` (row-major): `2000 + 2×(4×2 + 1) = 2018`.

### Multidimensional Arrays
`int a[2][3][4];` – 3-D array = 2 tables of 3 rows × 4 columns. Row-major address generalizes by nested multiplication.

### Functions Associated with Arrays
Traversal, insertion, deletion, searching, sorting, merging, reversing, finding max/min.

### Arrays as Parameters
Arrays are **always passed by address** (name decays to pointer to first element), so changes inside the function affect the original.
```c
void print(int a[], int n);      // same as int *a
void matrix(int a[][4], int r);  // 2D: column size required
```
`sizeof(a)` inside the function gives pointer size, not array size – so pass `n` separately.

### Implementing a One-Dimensional Array (full program)
```c
#include <stdio.h>
int main() {
    int a[50], n, i, ch, pos, val;
    printf("Enter n: "); scanf("%d", &n);
    for (i = 0; i < n; i++) scanf("%d", &a[i]);
    // call insert/delete/traverse as above
    return 0;
}
```

## 1.7 Character Strings in C
A string is a **char array terminated by the null character `'\0'`**.
```c
char s[] = "HELLO";      // size 6 (5 + '\0')
char *p = "HELLO";       // string literal (read-only)
```
### String Operations (`<string.h>`)
| Function | Purpose |
|---|---|
| `strlen(s)` | length (excludes `\0`) |
| `strcpy(d, s)` | copy |
| `strcat(d, s)` | concatenate |
| `strcmp(a, b)` | compare (0 if equal) |
| `strrev(s)` | reverse (non-standard) |
| `strstr(s, sub)` | substring search |
| `strchr(s, c)` | find char |

**Manual length & reverse:**
```c
int len(char s[]) { int i = 0; while (s[i] != '\0') i++; return i; }

void reverse(char s[]) {
    int i = 0, j = len(s) - 1;
    while (i < j) { char t = s[i]; s[i++] = s[j]; s[j--] = t; }
}
```
`gets()` is unsafe (buffer overflow) – use `fgets()`. `scanf("%s")` stops at whitespace.

## 1.8 Sparse Matrix
A matrix in which **most elements are zero** (usually > 2/3 zeros). Storing all zeros wastes memory.

**Triplet (3-tuple) representation:** store only non-zero as `(row, col, value)`; first row stores `(rows, cols, non-zero count)`.

```
Original 4×4:           Triplet form:
0 0 3 0                 Row Col Val
0 0 0 4                  4   4   3     ← header
5 0 0 0                  0   2   3
0 0 0 0                  1   3   4
                         2   0   5
```
```c
struct term { int row, col, val; };
struct term sp[100];     // sp[0] = header
```
**Transpose** – swap row & col of every triplet and re-sort by row (simple: O(cols × terms); fast transpose: O(cols + terms)).

**Other representations:** linked list (each node: row, col, value, next).
**Memory saving:** 100×100 int matrix = 10,000 ints; with 50 nonzero → 150 ints.

## ❓ Module I – Viva Questions
1. **What is a data structure? Give types.** – See 1.1.
2. **Difference between array and linked list?** – Array: fixed size, contiguous, random access O(1), insert O(n). List: dynamic, non-contiguous, sequential access O(n), insert O(1) once position known.
3. **What is an ADT?** – Model of data + operations without implementation details.
4. **Define time and space complexity. What is Big-O?** – Upper-bound growth rate.
5. **Time-space tradeoff example?** – Hash table, memoization.
6. **Why is array index starting from 0?** – Index = offset from base address; `a[i]` = `*(a+i)`.
7. **What happens if you access beyond array bounds?** – C doesn't check; undefined behaviour / segmentation fault.
8. **What is `a[i]` equivalent to?** – `*(a + i)`, even `i[a]` works.
9. **Difference between `malloc` and `calloc`?** – calloc zero-initialises and takes (count, size).
10. **What is a dangling pointer? Memory leak?** – See 1.5.
11. **Why `'\0'` in strings?** – Marks end; functions need it to know where string stops.
12. **What is sparse matrix and why triplet?** – Saves memory.
13. **Row-major vs column-major?** – C stores rows one after another.
14. **How are arrays passed to functions?** – By reference (address of first element).
15. **Complexity of insertion in an array?** – O(n).

---

# Module II – Stacks and Queues

# 2A. STACK

## 2.1 Definition
A **stack** is a linear data structure in which insertion and deletion happen only at **one end called TOP**. It follows **LIFO (Last In First Out)**.

![Stack LIFO](https://commons.wikimedia.org/wiki/Special:FilePath/Lifo_stack.png)

**Real-life:** pile of plates, undo in editors, browser back button, function call stack.

## 2.2 Array Representation & Operations
```c
#define MAX 5
int stack[MAX], top = -1;

void push(int x) {
    if (top == MAX - 1) { printf("Stack Overflow\n"); return; }
    stack[++top] = x;
}
int pop() {
    if (top == -1) { printf("Stack Underflow\n"); return -1; }
    return stack[top--];
}
int peek()    { return stack[top]; }
int isEmpty() { return top == -1; }
int isFull()  { return top == MAX - 1; }
```
- **Overflow:** push on full stack. **Underflow:** pop on empty stack.
- Push, pop, peek: **O(1)**.

## 2.3 Polish Notations (Expressions)
| Notation | Operator position | Example for A+B |
|---|---|---|
| **Infix** | between operands | `A + B` |
| **Prefix (Polish)** | before operands | `+ A B` |
| **Postfix (Reverse Polish)** | after operands | `A B +` |

Postfix/prefix need **no parentheses** and no precedence rules at evaluation time – compilers love them.

**Precedence (high→low):** `^` (right-assoc) > `* / %` > `+ -` (left-assoc).

### Infix → Postfix (using stack)
**Algorithm:**
1. Scan left to right.
2. Operand → output.
3. `(` → push.
4. `)` → pop to output until `(`; discard `(`.
5. Operator → pop operators of **higher or equal** precedence (for `^`: only higher) to output, then push current.
6. At end pop everything.

**Example:** `A + B * C - D`

| Symbol | Stack | Output |
|---|---|---|
| A | | A |
| + | + | A |
| B | + | A B |
| * | + * | A B |
| C | + * | A B C |
| − | − (pop `*`, `+`) | A B C * + |
| D | − | A B C * + D |
| end | | **A B C * + D −** |

**Example 2:** `(A + B) * (C − D)` → `A B + C D − *`
**Example 3:** `A ^ B ^ C` → `A B C ^ ^` (right associative)

```c
int prec(char c) {
    if (c == '^') return 3;
    if (c == '*' || c == '/') return 2;
    if (c == '+' || c == '-') return 1;
    return 0;
}
void infixToPostfix(char *in, char *out) {
    char st[100]; int top = -1, k = 0;
    for (int i = 0; in[i]; i++) {
        char c = in[i];
        if (isalnum(c)) out[k++] = c;
        else if (c == '(') st[++top] = c;
        else if (c == ')') {
            while (top >= 0 && st[top] != '(') out[k++] = st[top--];
            top--;                       // remove '('
        } else {
            while (top >= 0 && prec(st[top]) >= prec(c)) out[k++] = st[top--];
            st[++top] = c;
        }
    }
    while (top >= 0) out[k++] = st[top--];
    out[k] = '\0';
}
```

### Infix → Prefix
**Method:** (1) Reverse the infix string, swapping `(` ↔ `)`. (2) Convert to postfix (for equal precedence operators, **pop only if higher** precedence). (3) Reverse the result.

Example: `A + B * C` → reverse `C * B + A` → postfix `C B * A +` → reverse **`+ A * B C`**.

### Postfix → Infix / Prefix (vice versa)
- **Postfix → Infix:** scan; operand → push; operator → pop two (`b`, then `a`), push `(a op b)`.
  `A B + C *` → `((A+B)*C)`
- **Prefix → Infix:** scan **right to left**; operator → pop `a`, then `b`, push `(a op b)`.
- **Postfix → Prefix:** pop `b`, `a`, push `op a b` as string.
- **Prefix → Postfix:** scan right to left; pop `a`, `b`, push `a b op`.

### Evaluation of Postfix
Scan left→right: operand → push; operator → pop `b` then `a`, push `a op b`.

Evaluate `5 6 2 + * 12 4 / -`:
`5 | 5 6 | 5 6 2 | 5 8 | 40 | 40 12 | 40 12 4 | 40 3 | 37` → **37**

```c
int evalPostfix(char *e) {
    int st[100], top = -1;
    for (int i = 0; e[i]; i++) {
        if (isdigit(e[i])) st[++top] = e[i] - '0';
        else {
            int b = st[top--], a = st[top--];
            switch (e[i]) {
                case '+': st[++top] = a + b; break;
                case '-': st[++top] = a - b; break;
                case '*': st[++top] = a * b; break;
                case '/': st[++top] = a / b; break;
            }
        }
    }
    return st[top];
}
```
**Prefix evaluation:** scan right→left; pop `a` then `b`, compute `a op b`.

## 2.4 Applications of Stack
1. Expression conversion & evaluation (compilers)
2. **Recursion** (system stack stores activation records)
3. Parenthesis/bracket matching
4. Undo/redo, backtracking (maze, DFS)
5. Function calls, reversing a string
6. Compilation: syntax parsing

### Recursion & the stack
Recursion = function calling itself, needs a **base case**. Each call is pushed as a **stack frame** (parameters, locals, return address).
```c
int fact(int n) { return (n <= 1) ? 1 : n * fact(n - 1); }
int fib(int n)  { return (n <= 1) ? n : fib(n-1) + fib(n-2); }   // O(2^n)
```
Recursion vs iteration: recursion is shorter/elegant, uses more memory (stack), risk of stack overflow.

### Tower of Hanoi

![Tower of Hanoi](https://commons.wikimedia.org/wiki/Special:FilePath/Tower_of_Hanoi_4.gif)

Move `n` discs from source (A) to destination (C) using auxiliary (B). Rules: move one disc at a time; never place a bigger disc on a smaller one.

```c
void hanoi(int n, char from, char to, char aux) {
    if (n == 0) return;
    hanoi(n - 1, from, aux, to);
    printf("Move disc %d from %c to %c\n", n, from, to);
    hanoi(n - 1, aux, to, from);
}
```
- **Minimum moves = 2ⁿ − 1** (n=3 → 7 moves). Time complexity **O(2ⁿ)**.
- Recurrence: `T(n) = 2T(n−1) + 1`.

---

# 2B. QUEUE

## 2.5 Definition
A **queue** is a linear structure where insertion happens at **REAR** and deletion at **FRONT**. It follows **FIFO (First In First Out)**.

![Queue](https://commons.wikimedia.org/wiki/Special:FilePath/Data_Queue.svg)

**Real-life:** ticket counter, printer queue, CPU scheduling, BFS.

## 2.6 Array Representation – Insert & Delete
```c
#define MAX 5
int q[MAX], front = -1, rear = -1;

void enqueue(int x) {
    if (rear == MAX - 1) { printf("Queue Overflow\n"); return; }
    if (front == -1) front = 0;
    q[++rear] = x;
}
int dequeue() {
    if (front == -1 || front > rear) { printf("Queue Underflow\n"); return -1; }
    return q[front++];
}
```
**Problem with linear queue:** after some dequeues, the front space is wasted even if the queue shows "full" → solved by circular queue.

## 2.7 Circular Queue
Last position connects back to the first (wrap-around using modulo).

![Circular queue / buffer](https://commons.wikimedia.org/wiki/Special:FilePath/Circular_Buffer_Animation.gif)

```c
void enqueue(int x) {
    if ((rear + 1) % MAX == front) { printf("Full\n"); return; }
    if (front == -1) front = 0;
    rear = (rear + 1) % MAX;
    q[rear] = x;
}
int dequeue() {
    if (front == -1) { printf("Empty\n"); return -1; }
    int x = q[front];
    if (front == rear) front = rear = -1;      // last element
    else front = (front + 1) % MAX;
    return x;
}
```
- **Full:** `(rear+1) % MAX == front`; **Empty:** `front == -1`.

## 2.8 Priority Queue
Each element has a **priority**; element with highest priority is served first (ties → FIFO).
- **Ascending** (smallest first) / **Descending** (largest first).
- Implementations: unsorted array/list (insert O(1), delete O(n)), sorted list (insert O(n), delete O(1)), **heap** (both O(log n)).
- Uses: CPU scheduling, Dijkstra, Huffman coding, OS interrupts.

## 2.9 Deque (Double-Ended Queue)
Insertion and deletion allowed at **both ends**.
- **Input-restricted deque:** insert only at one end, delete at both.
- **Output-restricted deque:** delete only at one end, insert at both.
- Operations: `insertFront`, `insertRear`, `deleteFront`, `deleteRear`.
- Can act as both stack and queue.

## ❓ Module II – Viva Questions
1. **Stack vs Queue?** – LIFO vs FIFO; one end vs two ends.
2. **What is stack overflow/underflow?**
3. **Why is postfix preferred by compilers?** – No parentheses, no precedence handling, evaluate in a single pass with a stack.
4. **Convert `A*(B+C)/D` to postfix.** – `A B C + * D /`
5. **Convert `(A+B)*C` to prefix.** – `* + A B C`
6. **Evaluate `2 3 4 * +`.** – 14
7. **How is recursion implemented internally?** – Using system call stack.
8. **Tower of Hanoi: moves for 4 discs?** – 15.
9. **Drawback of linear queue? How to fix?** – Wasted space → circular queue.
10. **How to distinguish full and empty in circular queue?** – Use `(rear+1)%MAX==front` for full (one slot sacrificed) or maintain a counter.
11. **Applications of queue?** – Scheduling, buffering, BFS.
12. **Priority queue implementation?** – Heap.
13. **What is a deque? Types?**
14. **Can we implement stack using queues?** – Yes, with two queues.
15. **Which data structure for balanced-parentheses check?** – Stack.
16. **Associativity of `^`?** – Right to left.

---

# Module III – Programming with Linked Lists

## 3.1 Singly Linked List
A **linked list** is a dynamic linear collection of **nodes**; each node has **data** and a **pointer to the next node**. The last node points to `NULL`. Accessed through a `head` pointer.

![Singly linked list](https://commons.wikimedia.org/wiki/Special:FilePath/Singly-linked-list.svg)

### Representation in memory
Nodes are scattered in memory (non-contiguous); the `next` field stores the address of the following node. Two parallel arrays (INFO[], LINK[]) with a `START` pointer and an **AVAIL list** of free nodes is the textbook model.

```c
struct node {
    int data;
    struct node *next;
};
struct node *head = NULL;
```

### Traversal – O(n)
```c
void display() {
    struct node *t = head;
    while (t != NULL) { printf("%d -> ", t->data); t = t->next; }
    printf("NULL\n");
}
```
### Searching – O(n)
```c
int search(int key) {
    struct node *t = head; int pos = 1;
    while (t) { if (t->data == key) return pos; t = t->next; pos++; }
    return -1;
}
```
### Insertion
```c
struct node* newNode(int x) {
    struct node *n = (struct node*) malloc(sizeof(struct node));
    n->data = x; n->next = NULL;
    return n;
}
void insertBegin(int x) {                // O(1)
    struct node *n = newNode(x);
    n->next = head; head = n;
}
void insertEnd(int x) {                  // O(n)
    struct node *n = newNode(x), *t = head;
    if (!head) { head = n; return; }
    while (t->next) t = t->next;
    t->next = n;
}
void insertAfter(int key, int x) {       // after node containing key
    struct node *t = head;
    while (t && t->data != key) t = t->next;
    if (!t) return;
    struct node *n = newNode(x);
    n->next = t->next; t->next = n;
}
```
### Deletion
```c
void deleteBegin() {                     // O(1)
    if (!head) return;
    struct node *t = head; head = head->next; free(t);
}
void deleteKey(int key) {
    struct node *t = head, *prev = NULL;
    while (t && t->data != key) { prev = t; t = t->next; }
    if (!t) return;                      // not found
    if (!prev) head = t->next; else prev->next = t->next;
    free(t);
}
```
### Reverse a list (very common viva question)
```c
void reverse() {
    struct node *prev = NULL, *cur = head, *next;
    while (cur) { next = cur->next; cur->next = prev; prev = cur; cur = next; }
    head = prev;
}
```

## 3.2 Garbage Collection and Compaction
- **Garbage:** memory cells that are allocated but no longer reachable by any pointer.
- **Garbage collection:** system process that finds such cells and returns them to the free (AVAIL) list. Two phases: **Mark** (mark all reachable nodes from pointers) and **Sweep/Collect** (scan memory, free unmarked cells). Triggered when free list is empty (overflow).
- **Compaction:** moving all used blocks together to one end of memory so that free space becomes one big contiguous block (removes **fragmentation**).
- C has **no automatic GC** – programmer must call `free()`; Java/Python have one.

## 3.3 Doubly Linked List

![Doubly linked list](https://commons.wikimedia.org/wiki/Special:FilePath/Doubly-linked-list.svg)

Each node has **prev**, **data**, **next**. Can traverse both directions.
```c
struct dnode {
    int data;
    struct dnode *prev, *next;
};
```
**Insert at beginning:**
```c
void insertBegin(int x) {
    struct dnode *n = malloc(sizeof(struct dnode));
    n->data = x; n->prev = NULL; n->next = head;
    if (head) head->prev = n;
    head = n;
}
```
**Delete a node `t`:**
```c
if (t->prev) t->prev->next = t->next; else head = t->next;
if (t->next) t->next->prev = t->prev;
free(t);
```
| Pros | Cons |
|---|---|
| Backward traversal; delete given a node pointer in O(1) | Extra memory for `prev`; more pointer changes |

## 3.4 Circular Linked List

![Circular linked list](https://commons.wikimedia.org/wiki/Special:FilePath/Circularly-linked-list.svg)

Last node's `next` points back to the **first node** (no NULL). Can be singly or doubly circular.
```c
void insertEnd(int x) {
    struct node *n = newNode(x);
    if (!head) { head = n; n->next = head; return; }
    struct node *t = head;
    while (t->next != head) t = t->next;
    t->next = n; n->next = head;
}
void display() {
    if (!head) return;
    struct node *t = head;
    do { printf("%d ", t->data); t = t->next; } while (t != head);
}
```
Traversal stops when `t->next == head` (not NULL). **Uses:** round-robin scheduling, circular buffers, multiplayer games. Keeping a pointer to the **last** node gives O(1) insertion at both ends.

## 3.5 Generalized List
A list whose elements can be **atoms or other lists (sublists)**. Example: `L = (a, (b, c), d, (e, (f)))`.
Node has a **tag** field: `0` = atom (data), `1` = sublist (down-pointer).
```c
struct gnode {
    int tag;                    // 0 = atom, 1 = list
    union { char data; struct gnode *down; } u;
    struct gnode *next;
};
```
Used in Lisp, representing polynomials in several variables, sparse structures. **Head** = first element, **tail** = rest of list.

## 3.6 Applications

### Polynomial Representation
Each node: **coefficient, exponent, next**; terms stored in decreasing exponent.
`5x³ + 4x² + 2` → `[5|3] → [4|2] → [2|0] → NULL`
```c
struct poly { int coef, exp; struct poly *next; };
```
**Addition:** compare exponents of the two lists:
- equal → add coefficients, advance both
- larger exponent term is copied to result, advance that list
- copy remaining terms
Time **O(m + n)**. Multiplication: multiply each term pair, combine like terms – O(m·n).

### Stack using linked list
Push = insert at beginning, pop = delete from beginning (top = head). No overflow (only heap exhaustion).
```c
void push(int x) { struct node *n = newNode(x); n->next = top; top = n; }
int pop() { struct node *t = top; int x = t->data; top = top->next; free(t); return x; }
```
### Queue using linked list
Maintain `front` and `rear`; enqueue at rear, dequeue at front – both O(1).
```c
void enqueue(int x) {
    struct node *n = newNode(x);
    if (!rear) front = rear = n;
    else { rear->next = n; rear = n; }
}
int dequeue() {
    struct node *t = front; int x = t->data;
    front = front->next; if (!front) rear = NULL;
    free(t); return x;
}
```

## ❓ Module III – Viva Questions
1. **Array vs linked list?** – See Module I.
2. **Why use linked list?** – Dynamic size, easy insert/delete, no shifting.
3. **Why can't we do binary search on a linked list?** – No random access to middle element.
4. **What does `head->next` mean? What if head is NULL?** – Dereferencing NULL → crash; always check.
5. **Singly vs doubly vs circular?**
6. **How to detect a loop in a linked list?** – Floyd's tortoise-and-hare (slow & fast pointers).
7. **How to find the middle node?** – Slow/fast pointer.
8. **Time to insert at the end of singly list without tail pointer?** – O(n).
9. **What is garbage collection? Compaction?**
10. **What is generalized list?**
11. **Polynomial addition steps?**
12. **Which is better for stack: array or list?** – List if size unknown; array if fixed and speed matters.
13. **Why `struct node *next` not `struct node next`?** – Infinite size otherwise.
14. **Memory required per node in doubly list?** – data + 2 pointers.
15. **Reverse a linked list – explain.** – Three pointers prev/cur/next.

---

# Module IV – Trees

## 4.1 Basic Terminology

![Binary tree](https://commons.wikimedia.org/wiki/Special:FilePath/Binary_tree.svg)

| Term | Meaning |
|---|---|
| **Root** | Topmost node (no parent) |
| **Parent / Child** | Node above / below connected by edge |
| **Siblings** | Same parent |
| **Leaf (terminal)** | Node with no children |
| **Internal node** | Node with at least one child |
| **Degree** | Number of children of a node (tree degree = max degree) |
| **Level** | Root at level 0 (or 1 – state your convention) |
| **Height/Depth** | Longest path root→leaf (number of edges) |
| **Subtree** | Tree formed by a node and its descendants |
| **Forest** | Collection of disjoint trees |
| **Path** | Sequence of nodes connected by edges |

A tree with `n` nodes has **n − 1 edges**.

## 4.2 Binary Trees
Every node has **at most 2 children** (left, right).
```c
struct tnode {
    int data;
    struct tnode *left, *right;
};
```
**Properties:**
- Max nodes at level `i` = 2ⁱ (root level 0).
- Max nodes in tree of height `h` = 2^(h+1) − 1.
- Min height with `n` nodes = ⌈log₂(n+1)⌉ − 1.
- Leaves = (nodes with 2 children) + 1.

**Types:**
- **Strictly/Full binary tree:** each node has 0 or 2 children.
- **Complete binary tree:** all levels full except possibly the last, which is filled left to right (used in heaps).
- **Perfect binary tree:** all internal nodes have 2 children and all leaves at same level.
- **Skewed tree:** all nodes only left or only right (becomes a linked list; worst case).
- **Extended binary tree (2-tree):** replace every NULL pointer by a special **external node**, so every internal node has exactly 2 children. Number of external nodes = internal nodes + 1. Used for expression trees, Huffman coding, path-length analysis.

### Representation
1. **Array (sequential):** root at index 1; for node at `i`: left = `2i`, right = `2i+1`, parent = `⌊i/2⌋`. Good for complete trees; wasteful for skewed.
2. **Linked:** structure with left/right pointers (above).

### Expression Tree and Evaluation
Operands are leaves, operators are internal nodes. `(A + B) * C`:
```
      *
     / \
    +   C
   / \
  A   B
```
- Inorder → infix, Postorder → postfix, Preorder → prefix.
- Evaluate by postorder: evaluate left, right, then apply operator.

## 4.3 Traversing Binary Trees

| Traversal | Order |
|---|---|
| **Preorder** | Root → Left → Right |
| **Inorder** | Left → Root → Right |
| **Postorder** | Left → Right → Root |
| **Level order** | Level by level (uses queue) |

```c
void inorder(struct tnode *r)   { if (r) { inorder(r->left); printf("%d ", r->data); inorder(r->right); } }
void preorder(struct tnode *r)  { if (r) { printf("%d ", r->data); preorder(r->left); preorder(r->right); } }
void postorder(struct tnode *r) { if (r) { postorder(r->left); postorder(r->right); printf("%d ", r->data); } }
```
**Example tree:** root 1, left 2 (children 4, 5), right 3.
Preorder: `1 2 4 5 3` | Inorder: `4 2 5 1 3` | Postorder: `4 5 2 3 1`.

**Rebuilding a tree:** you can uniquely construct a tree from Inorder + Preorder, or Inorder + Postorder (not from Pre + Post alone).
Time of traversal: **O(n)**; recursion stack space O(h).

## 4.4 Binary Search Tree (BST)

![Binary search tree](https://commons.wikimedia.org/wiki/Special:FilePath/Binary_search_tree.svg)

**Property:** for every node, all keys in **left subtree < node < all keys in right subtree**. **Inorder traversal of BST gives sorted order.**

### Searching
```c
struct tnode* search(struct tnode *r, int key) {
    if (!r || r->data == key) return r;
    return (key < r->data) ? search(r->left, key) : search(r->right, key);
}
```
### Insertion
```c
struct tnode* insert(struct tnode *r, int x) {
    if (!r) { struct tnode *n = malloc(sizeof(*n)); n->data = x; n->left = n->right = NULL; return n; }
    if (x < r->data) r->left  = insert(r->left, x);
    else if (x > r->data) r->right = insert(r->right, x);
    return r;
}
```
### Deletion – 3 cases
1. **Leaf:** just remove.
2. **One child:** replace node by its child.
3. **Two children:** replace with **inorder successor** (smallest in right subtree) or **inorder predecessor** (largest in left), then delete that node.
```c
struct tnode* minNode(struct tnode *r) { while (r->left) r = r->left; return r; }

struct tnode* delete(struct tnode *r, int key) {
    if (!r) return r;
    if (key < r->data) r->left = delete(r->left, key);
    else if (key > r->data) r->right = delete(r->right, key);
    else {
        if (!r->left)  { struct tnode *t = r->right; free(r); return t; }
        if (!r->right) { struct tnode *t = r->left;  free(r); return t; }
        struct tnode *s = minNode(r->right);
        r->data = s->data;
        r->right = delete(r->right, s->data);
    }
    return r;
}
```
**Complexity:** average O(log n), **worst O(n)** (skewed, e.g., inserting sorted data). Fix → balanced trees (AVL).

## 4.5 General Trees
Nodes can have **any number of children**. Stored as:
- **Linked list of children**, or
- **Left-child right-sibling representation** – converts any general tree into a binary tree: `left` = first child, `right` = next sibling.
```c
struct gtnode { int data; struct gtnode *firstChild, *nextSibling; };
```
Examples: file system directories, organization charts.

## 4.6 AVL Trees

![AVL tree](https://commons.wikimedia.org/wiki/Special:FilePath/AVL-tree-wBalance_K.svg)

A **self-balancing BST** (Adelson-Velsky & Landis, 1962) where for every node:
**Balance Factor = height(left) − height(right) ∈ {−1, 0, +1}.**
Guarantees height O(log n) → search/insert/delete **O(log n)**.

### Rotations (after insertion causes |BF| = 2)
| Case | Imbalance | Fix |
|---|---|---|
| **LL** | Inserted in left of left child | **Single right rotation** |
| **RR** | Inserted in right of right child | **Single left rotation** |
| **LR** | Inserted in right of left child | **Left rotation on child, then right rotation** |
| **RL** | Inserted in left of right child | **Right rotation on child, then left rotation** |

```c
struct tnode* rotateRight(struct tnode *y) {
    struct tnode *x = y->left;
    y->left = x->right;
    x->right = y;
    // update heights of y then x
    return x;
}
struct tnode* rotateLeft(struct tnode *x) {
    struct tnode *y = x->right;
    x->right = y->left;
    y->left = x;
    // update heights of x then y
    return y;
}
```
**Example:** Insert 10, 20, 30 → RR imbalance at 10 → left rotate → 20 becomes root with 10 and 30.
Minimum nodes in AVL tree of height h: `N(h) = N(h−1) + N(h−2) + 1` (Fibonacci-like).

## 4.7 Threaded Binary Trees
In a normal binary tree with `n` nodes, **n + 1 pointers are NULL** (wasted). In a **threaded tree**, NULL pointers are replaced by **threads** pointing to the **inorder predecessor/successor**.

![Threaded binary tree](https://commons.wikimedia.org/wiki/Special:FilePath/Threaded_tree.svg)

- **Right-threaded:** null right → inorder successor.
- **Left-threaded:** null left → inorder predecessor.
- **Double-threaded:** both.
- Need a **flag/tag bit** (`lthread`, `rthread`) to distinguish a real child from a thread.
- **Advantage:** inorder traversal **without recursion or stack**, faster; **Disadvantage:** insertion/deletion more complex.
```c
struct tnode { int data; struct tnode *left, *right; int lthread, rthread; };
```

## 4.8 B-Trees

![B-tree](https://commons.wikimedia.org/wiki/Special:FilePath/B-tree.svg)

A **multiway (m-way) balanced search tree** designed for **disks/databases**, where each node can hold many keys, reducing disk accesses.

**Properties of B-tree of order m:**
1. Each node has **at most m children** and `m − 1` keys.
2. Every non-root internal node has **at least ⌈m/2⌉ children**.
3. Root has at least 2 children (if not a leaf).
4. A node with `k` children has `k − 1` keys, stored in sorted order.
5. **All leaves are at the same level** (perfectly balanced).

**Insertion:** insert in the proper leaf; if the node overflows (m keys), **split** it around the median key and push the median up to the parent. Tree grows at the **root** (upwards).
**Deletion:** remove key; if node has too few keys → **borrow** from sibling or **merge** with sibling.
**Complexity:** search/insert/delete **O(log n)**.
**Use:** database indexing, file systems (NTFS, ext4). **B+ tree:** all data in leaves, leaves linked.

## ❓ Module IV – Viva Questions
1. **Define tree, binary tree, BST.**
2. **Max nodes in a binary tree of height h?** – 2^(h+1) − 1.
3. **Difference between full, complete, perfect tree?**
4. **Which traversal gives sorted output for BST?** – Inorder.
5. **Preorder: 50 30 20 40 70 60 80. Draw the BST and give inorder & postorder.** – Inorder: 20 30 40 50 60 70 80; Postorder: 20 40 30 60 80 70 50.
6. **Worst case of BST? Why?** – Skewed, O(n).
7. **Deletion of node with two children?** – Replace by inorder successor/predecessor.
8. **What is balance factor? Which rotations exist?**
9. **Why AVL over BST?** – Guaranteed O(log n).
10. **Why threaded tree?** – Use NULL pointers, non-recursive traversal.
11. **Why B-tree?** – Reduces disk I/O; used in DBMS.
12. **Height of complete binary tree with n nodes?** – ⌊log₂ n⌋.
13. **What is an extended binary tree?**
14. **How are expressions evaluated using trees?**
15. **Array representation formulas for a binary tree?** – 2i, 2i+1, ⌊i/2⌋.
16. **Applications of trees?** – File systems, parsing, databases, Huffman coding, routing, heaps.
17. **Can you construct a tree from preorder + postorder?** – Not uniquely.

---

# Module V – Searching and Sorting

## 5.1 Basics of Sorting
**Sorting** arranges data in ascending/descending order.
- **Internal sort:** all data fits in RAM (bubble, quick, heap…). **External sort:** data too large, resides on disk (external merge sort).
- **Stable sort:** equal keys keep original relative order (bubble, insertion, merge). **Unstable:** selection, quick, heap.
- **In-place:** uses O(1) extra memory.

## 5.2 Sorting Algorithms

### Bubble Sort
![Bubble sort](https://commons.wikimedia.org/wiki/Special:FilePath/Bubble-sort-example-300px.gif)

Repeatedly compare adjacent elements and swap if out of order; largest "bubbles" to the end each pass.
```c
void bubble(int a[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int swapped = 0;
        for (int j = 0; j < n - i - 1; j++)
            if (a[j] > a[j + 1]) { int t = a[j]; a[j] = a[j+1]; a[j+1] = t; swapped = 1; }
        if (!swapped) break;             // optimization → best case O(n)
    }
}
```
Worst/avg **O(n²)**, best O(n) (optimized), space O(1), **stable**.

### Selection Sort
![Selection sort](https://commons.wikimedia.org/wiki/Special:FilePath/Selection-Sort-Animation.gif)

Find the minimum of the unsorted part and place it at the beginning.
```c
void selection(int a[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int min = i;
        for (int j = i + 1; j < n; j++) if (a[j] < a[min]) min = j;
        int t = a[i]; a[i] = a[min]; a[min] = t;
    }
}
```
**O(n²)** in all cases, only **n−1 swaps**, unstable, in-place.

### Insertion Sort
![Insertion sort](https://commons.wikimedia.org/wiki/Special:FilePath/Insertion-sort-example-300px.gif)

Take each element and insert it into its correct place in the sorted left part (like sorting playing cards).
```c
void insertion(int a[], int n) {
    for (int i = 1; i < n; i++) {
        int key = a[i], j = i - 1;
        while (j >= 0 && a[j] > key) { a[j + 1] = a[j]; j--; }
        a[j + 1] = key;
    }
}
```
Worst/avg O(n²), **best O(n)** (already sorted), stable, in-place. Great for small/nearly sorted data.

### Quick Sort (Partition-Exchange Sort)
![Quick sort](https://commons.wikimedia.org/wiki/Special:FilePath/Sorting_quicksort_anim.gif)

**Divide & conquer:** choose a **pivot**, partition so that smaller elements go left and larger go right, then recursively sort both parts. Also called **partition exchange sort**.
```c
int partition(int a[], int low, int high) {
    int pivot = a[high], i = low - 1;
    for (int j = low; j < high; j++)
        if (a[j] < pivot) { i++; int t = a[i]; a[i] = a[j]; a[j] = t; }
    int t = a[i + 1]; a[i + 1] = a[high]; a[high] = t;
    return i + 1;
}
void quick(int a[], int low, int high) {
    if (low < high) {
        int p = partition(a, low, high);
        quick(a, low, p - 1);
        quick(a, p + 1, high);
    }
}
```
Best/avg **O(n log n)**, **worst O(n²)** (already sorted with bad pivot), space O(log n) stack, **unstable**, fastest in practice. Worst case avoided by random or median-of-three pivot.

### Merge Sort
![Merge sort](https://commons.wikimedia.org/wiki/Special:FilePath/Merge-sort-example-300px.gif)

Divide array into halves, sort each recursively, **merge** two sorted halves.
```c
void merge(int a[], int l, int m, int r) {
    int n1 = m - l + 1, n2 = r - m, L[n1], R[n2];
    for (int i = 0; i < n1; i++) L[i] = a[l + i];
    for (int j = 0; j < n2; j++) R[j] = a[m + 1 + j];
    int i = 0, j = 0, k = l;
    while (i < n1 && j < n2) a[k++] = (L[i] <= R[j]) ? L[i++] : R[j++];
    while (i < n1) a[k++] = L[i++];
    while (j < n2) a[k++] = R[j++];
}
void mergeSort(int a[], int l, int r) {
    if (l < r) {
        int m = (l + r) / 2;
        mergeSort(a, l, m); mergeSort(a, m + 1, r);
        merge(a, l, m, r);
    }
}
```
**O(n log n)** in all cases, **space O(n)**, **stable**. Recurrence `T(n) = 2T(n/2) + n`. Basis of external sorting.

### Heap Sort
![Heap sort](https://commons.wikimedia.org/wiki/Special:FilePath/Sorting_heapsort_anim.gif)

A **heap** is a complete binary tree with **heap property**: Max-heap → parent ≥ children; Min-heap → parent ≤ children.

![Max heap](https://commons.wikimedia.org/wiki/Special:FilePath/Max-Heap.svg)

Steps: (1) Build max-heap (O(n)). (2) Swap root with last element, reduce heap size, **heapify** root. Repeat.
```c
void heapify(int a[], int n, int i) {
    int largest = i, l = 2*i + 1, r = 2*i + 2;
    if (l < n && a[l] > a[largest]) largest = l;
    if (r < n && a[r] > a[largest]) largest = r;
    if (largest != i) { int t = a[i]; a[i] = a[largest]; a[largest] = t; heapify(a, n, largest); }
}
void heapSort(int a[], int n) {
    for (int i = n/2 - 1; i >= 0; i--) heapify(a, n, i);
    for (int i = n - 1; i > 0; i--) {
        int t = a[0]; a[0] = a[i]; a[i] = t;
        heapify(a, i, 0);
    }
}
```
**O(n log n)** all cases, O(1) space, **unstable**.

### Shell Sort
![Shell sort](https://commons.wikimedia.org/wiki/Special:FilePath/Sorting_shellsort_anim.gif)

Generalized insertion sort: sorts elements far apart using a **gap sequence** (n/2, n/4, … 1), so elements move quickly toward correct position.
```c
void shell(int a[], int n) {
    for (int gap = n / 2; gap > 0; gap /= 2)
        for (int i = gap; i < n; i++) {
            int t = a[i], j;
            for (j = i; j >= gap && a[j - gap] > t; j -= gap) a[j] = a[j - gap];
            a[j] = t;
        }
}
```
Worst O(n²) (with n/2 gaps), better with other sequences (~O(n^1.5) or O(n log² n)); in-place, unstable.

### Sorting on Different Keys
Sorting records (structures) by one or more **fields (keys)**. **Primary key** decides first; ties broken by **secondary key**. Example: sort students by `class`, and within class by `marks`. Implemented by comparison on multiple fields, or by `qsort` with a custom comparator, or radix-style (sort by least significant key first with a **stable** sort).
```c
int cmp(const void *a, const void *b) {
    struct student *x = (struct student*)a, *y = (struct student*)b;
    if (x->cls != y->cls) return x->cls - y->cls;
    return y->marks - x->marks;     // marks descending
}
qsort(s, n, sizeof(struct student), cmp);
```
Also: **Radix sort** (digit by digit), **Bucket sort**, **Counting sort** – non-comparison sorts, O(n + k).

### External Sorting
Used when data **doesn't fit in main memory** (stored on disk/tape). Standard method: **External merge sort**:
1. **Run generation:** read chunks that fit in RAM, sort them internally, write sorted **runs** to disk.
2. **Merge phase:** K-way merge of runs using buffers (and a min-heap/tournament tree).
Variants: balanced multiway merge, polyphase merge, replacement selection (makes longer runs). Cost measured in **disk I/O passes**.

## 5.3 Searching

### Linear (Sequential) Search
```c
int linear(int a[], int n, int key) {
    for (int i = 0; i < n; i++) if (a[i] == key) return i;
    return -1;
}
```
O(n); works on unsorted data; sentinel technique improves slightly.

### Binary Search
**Requires sorted array.** Compare with the middle; discard half each step.
```c
int binary(int a[], int n, int key) {
    int low = 0, high = n - 1;
    while (low <= high) {
        int mid = low + (high - low) / 2;
        if (a[mid] == key) return mid;
        else if (a[mid] < key) low = mid + 1;
        else high = mid - 1;
    }
    return -1;
}
```
**O(log n)**, best O(1). Recursive version uses O(log n) stack. Example: search 23 in `[2,5,8,12,16,23,38,56,72,91]` → mid=16 → right → mid=56 → left → 23 found (3 steps).
Max comparisons ≈ ⌊log₂ n⌋ + 1.

## 5.4 Hashing

![Hash table](https://commons.wikimedia.org/wiki/Special:FilePath/Hash_table_3_1_1_0_1_0_0_SP.svg)

**Hashing** maps a key to a table index using a **hash function** so search/insert/delete are **O(1) on average**.
`index = h(key)`

### Hash Functions
| Method | Idea | Example (table size 10) |
|---|---|---|
| **Division (modulo)** | `h(k) = k mod m` (m prime is best) | h(125) = 5 |
| **Mid-square** | Square key, take middle digits | 123² = 15129 → 51 |
| **Folding** | Split key into parts and add | 123456 → 12+34+56 = 102 → 02 |
| **Digit extraction** | Use selected digits | |
| **Multiplication** | `floor(m × frac(k × A))`, A≈0.618 | |

A good hash function: fast, uniform distribution, minimizes collisions.

### Collision
When two different keys map to the **same index**. Load factor α = n / m.

### Collision Resolution Techniques
**1. Open Hashing – Separate Chaining:** each slot holds a linked list of all keys hashing there.

![Separate chaining](https://commons.wikimedia.org/wiki/Special:FilePath/Hash_table_5_0_1_1_1_1_0_LL.svg)

**2. Closed Hashing – Open Addressing:** find another empty slot in the table itself.
| Technique | Probe sequence | Issue |
|---|---|---|
| **Linear probing** | `(h(k) + i) mod m` | **Primary clustering** |
| **Quadratic probing** | `(h(k) + i²) mod m` | Secondary clustering |
| **Double hashing** | `(h1(k) + i·h2(k)) mod m` | Best distribution, more computation |

**3. Rehashing** – build a bigger table and reinsert when load factor too high.

**Example – Linear probing:** m=10, keys 25, 35, 45 → h = 5 for all → placed at 5, 6, 7.
**Deletion** in open addressing requires a *tombstone/deleted marker* so searches don't break.

```c
#define M 10
int table[M];                       // init with -1
void insert(int k) {
    int i = k % M;
    while (table[i] != -1) i = (i + 1) % M;     // linear probing
    table[i] = k;
}
```

## ❓ Module V – Viva Questions
1. **Which sorting is best?** – Depends: quick (average), merge (stable, guaranteed), insertion (small/nearly sorted).
2. **Stable vs unstable sort? Examples.**
3. **Why quick sort is called partition-exchange sort?**
4. **Worst case of quick sort?** – Sorted/reverse sorted input with first/last pivot → O(n²).
5. **Why merge sort needs extra space?** – Temporary arrays for merging.
6. **Which sorts are in-place?** – Bubble, selection, insertion, quick, heap, shell.
7. **Best case of bubble & insertion?** – O(n).
8. **Which sort makes the minimum number of swaps?** – Selection sort.
9. **What is a heap? Max-heap vs min-heap?**
10. **Complexity of building a heap?** – O(n).
11. **Binary search prerequisites?** – Sorted data, random access.
12. **Linear vs binary search?**
13. **What is hashing? Collision? Load factor?**
14. **Chaining vs open addressing?**
15. **Primary clustering?** – Consecutive occupied slots in linear probing.
16. **Why is table size taken prime?** – Better distribution.
17. **What is external sorting?**
18. **Sort an array of 1 million numbers that doesn't fit in RAM?** – External merge sort.
19. **Sort 5 2 9 1 using bubble sort – show passes.** → `2 5 1 9` → `2 1 5 9` → `1 2 5 9`.

---

# Module VI – Graphs and Their Applications

## 6.1 Introduction & Terminology

![Graph](https://commons.wikimedia.org/wiki/Special:FilePath/6n-graf.svg)

A **graph G = (V, E)** is a set of **vertices (nodes)** and **edges** connecting pairs of vertices.

| Term | Meaning |
|---|---|
| **Directed graph (digraph)** | Edges have direction (u → v) |
| **Undirected graph** | Edges are two-way |
| **Weighted graph** | Edges carry a cost/weight |
| **Adjacent vertices** | Connected by an edge |
| **Degree** | Number of edges at a vertex (in-degree / out-degree for digraphs) |
| **Path** | Sequence of vertices connected by edges |
| **Cycle** | Path that starts and ends at same vertex |
| **Acyclic / DAG** | Graph without cycles / directed acyclic graph |
| **Connected graph** | Path exists between every pair of vertices |
| **Strongly connected** | In digraph, path both ways between every pair |
| **Complete graph** | Every pair of vertices has an edge; edges = n(n−1)/2 |
| **Loop / Multigraph** | Edge from a vertex to itself / multiple edges between same pair |
| **Subgraph** | Graph formed from subset of V and E |
| **Isolated vertex** | Degree 0 |

**Handshaking lemma:** sum of degrees = 2 × |E|.
**Tree** = connected acyclic graph with n − 1 edges.

## 6.2 Sequential Representation

### Adjacency Matrix
`n × n` matrix `A`; `A[i][j] = 1` if edge from i to j (else 0); for weighted graphs store the weight (∞/0 for no edge).
```
Graph: 0–1, 0–2, 1–2, 2–3
     0 1 2 3
  0 [0 1 1 0]
  1 [1 0 1 0]
  2 [1 1 0 1]
  3 [0 0 1 0]
```
- Undirected → symmetric matrix.
- **Space O(V²)**; check edge in **O(1)**; finding neighbours O(V). Good for dense graphs.
- Sum of row i = degree (out-degree) of i.

### Path Matrix (Reachability Matrix)
`P[i][j] = 1` if there is **a path (of any length)** from vertex i to j. Computed from the adjacency matrix:
- **Powers of A:** `Bᵣ = A + A² + … + Aⁿ`; `P[i][j] = 1` if `Bᵣ[i][j] > 0`.
- **Warshall's algorithm** (more efficient).
- `Aᵏ[i][j]` = number of paths of length k from i to j.

## 6.3 Warshall's Algorithm (Transitive Closure)
Finds the path matrix by checking, for each intermediate vertex `k`, whether a path i → k → j exists.

**Idea:** `P[i][j] = P[i][j] OR (P[i][k] AND P[k][j])`

```c
void warshall(int P[][N], int n) {
    // P initially = adjacency matrix
    for (int k = 0; k < n; k++)
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                P[i][j] = P[i][j] || (P[i][k] && P[k][j]);
}
```
**Time O(n³)**, space O(n²). **Floyd–Warshall** is the weighted variant for all-pairs shortest paths: `D[i][j] = min(D[i][j], D[i][k] + D[k][j])`.

## 6.4 Linked Representation – Adjacency List
Array of linked lists; list `i` contains all vertices adjacent to `i`.
```
0 → 1 → 2
1 → 0 → 2
2 → 0 → 1 → 3
3 → 2
```
```c
struct anode { int vertex; struct anode *next; };
struct anode *adj[MAXV];

void addEdge(int u, int v) {
    struct anode *n = malloc(sizeof(*n));
    n->vertex = v; n->next = adj[u]; adj[u] = n;
    // for undirected also add v→u
}
```
- **Space O(V + E)**; good for **sparse** graphs; edge check O(degree).

| | Adjacency Matrix | Adjacency List |
|---|---|---|
| Space | O(V²) | O(V + E) |
| Edge exists? | O(1) | O(deg) |
| Iterate neighbours | O(V) | O(deg) |
| Best for | Dense graphs | Sparse graphs |

## 6.5 Operations on Graphs
Insert vertex, delete vertex (remove all incident edges), insert edge, delete edge, search/traverse, find degree, check connectivity, find shortest path, find spanning tree.
```c
// Using adjacency matrix
void addEdge(int u, int v)    { adj[u][v] = adj[v][u] = 1; }
void removeEdge(int u, int v) { adj[u][v] = adj[v][u] = 0; }
```

## 6.6 Graph Traversal

### Breadth First Search (BFS)

![BFS](https://commons.wikimedia.org/wiki/Special:FilePath/Breadth-first-tree.svg)

Visits vertices **level by level** using a **QUEUE**. Finds the **shortest path** in unweighted graphs.
```c
void bfs(int start, int n) {
    int visited[N] = {0}, q[N], front = 0, rear = 0;
    visited[start] = 1; q[rear++] = start;
    while (front < rear) {
        int u = q[front++];
        printf("%d ", u);
        for (int v = 0; v < n; v++)
            if (adj[u][v] && !visited[v]) { visited[v] = 1; q[rear++] = v; }
    }
}
```
### Depth First Search (DFS)

![DFS](https://commons.wikimedia.org/wiki/Special:FilePath/Depth-first-tree.svg)

Goes **as deep as possible**, then backtracks, using a **STACK** (or recursion).
```c
int visited[N];
void dfs(int u, int n) {
    visited[u] = 1;
    printf("%d ", u);
    for (int v = 0; v < n; v++)
        if (adj[u][v] && !visited[v]) dfs(v, n);
}
```
**Complexity (both):** O(V²) with matrix, **O(V + E)** with adjacency list.

| | BFS | DFS |
|---|---|---|
| Data structure | Queue | Stack / recursion |
| Strategy | Level-wise | Depth-wise |
| Shortest path (unweighted) | Yes | No |
| Uses | Shortest path, social networks, level order | Cycle detection, topological sort, connected components, maze |

**Example** (graph above, start 0): BFS → `0 1 2 3`; DFS → `0 1 2 3`.

## 6.7 Spanning Trees

![Minimum spanning tree](https://commons.wikimedia.org/wiki/Special:FilePath/Minimum_spanning_tree.svg)

**Spanning tree:** a subgraph of a connected graph that includes **all vertices**, is **connected and has no cycles**, hence has exactly **V − 1 edges**.
- A graph can have **many** spanning trees; a complete graph Kₙ has **nⁿ⁻²** (Cayley's formula).
- Obtained by **DFS tree** or **BFS tree**, or by removing edges that form cycles.
- **Minimum Spanning Tree (MST):** spanning tree of a weighted graph with **minimum total weight**.

### Representation of Spanning Tree
- As a set of **V − 1 edges** (edge list),
- As an **adjacency matrix / adjacency list** of the tree,
- As a **parent array** (`parent[i]` = parent of vertex i in rooted spanning tree).

### MST Algorithms (be ready for a follow-up)
| Algorithm | Approach | Complexity |
|---|---|---|
| **Kruskal's** | Sort edges by weight, add smallest edge if it doesn't form a cycle (uses Union-Find) | O(E log E) |
| **Prim's** | Start from a vertex, repeatedly add the cheapest edge connecting tree to a new vertex | O(V²) / O(E log V) with heap |

Both are **greedy** algorithms.

## ❓ Module VI – Viva Questions
1. **Graph vs tree?** – Tree is a connected acyclic graph; graph may have cycles, multiple components.
2. **Ways to represent a graph?** – Adjacency matrix, adjacency list (also incidence matrix, edge list).
3. **What is a path matrix? How is it found?** – Warshall.
4. **Time complexity of Warshall?** – O(n³).
5. **BFS vs DFS?**
6. **Which data structures do BFS and DFS use?**
7. **What is a spanning tree? How many edges?** – V − 1.
8. **What is MST? Name algorithms.**
9. **Degree of a vertex? Sum of degrees?** – 2E.
10. **Maximum edges in an undirected simple graph?** – n(n−1)/2; directed: n(n−1).
11. **Adjacency matrix vs list?**
12. **How to detect a cycle in a graph?** – DFS with back-edge detection.
13. **What is a DAG? Application?** – Topological sort, task scheduling.
14. **Applications of graphs?** – Maps (shortest path), social networks, networks/routing, web crawling, dependency resolution.

---

# 📊 Master Complexity Cheat Sheet

### Data structure operations
| Structure | Access | Search | Insert | Delete | Space |
|---|---|---|---|---|---|
| Array | O(1) | O(n) | O(n) | O(n) | O(n) |
| Stack | O(n) | O(n) | O(1) | O(1) | O(n) |
| Queue | O(n) | O(n) | O(1) | O(1) | O(n) |
| Singly linked list | O(n) | O(n) | O(1)* | O(1)* | O(n) |
| BST (avg) | – | O(log n) | O(log n) | O(log n) | O(n) |
| BST (worst) | – | O(n) | O(n) | O(n) | O(n) |
| AVL / B-tree | – | O(log n) | O(log n) | O(log n) | O(n) |
| Hash table (avg) | – | O(1) | O(1) | O(1) | O(n) |
| Heap | – | O(n) | O(log n) | O(log n) | O(n) |

\* at known position / at head.

### Sorting
| Algorithm | Best | Average | Worst | Space | Stable |
|---|---|---|---|---|---|
| Bubble | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Selection | O(n²) | O(n²) | O(n²) | O(1) | No |
| Insertion | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Shell | O(n log n) | ~O(n^1.5) | O(n²) | O(1) | No |
| Merge | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Quick | O(n log n) | O(n log n) | O(n²) | O(log n) | No |
| Heap | O(n log n) | O(n log n) | O(n log n) | O(1) | No |

### Searching
| Algorithm | Best | Worst | Needs sorted? |
|---|---|---|---|
| Linear | O(1) | O(n) | No |
| Binary | O(1) | O(log n) | **Yes** |
| Hashing | O(1) | O(n) | No |

### Graph algorithms
| Algorithm | Complexity |
|---|---|
| BFS / DFS | O(V + E) list, O(V²) matrix |
| Warshall | O(V³) |
| Prim | O(V²) or O(E log V) |
| Kruskal | O(E log E) |

---

# 🔥 Rapid-Fire Viva Questions

**General**
- What is the difference between **data** and **information**? *(raw facts vs processed facts)*
- Why do we study data structures? *(efficient storage, retrieval, and processing)*
- Static vs dynamic memory allocation? *(compile-time vs runtime; stack/data vs heap)*
- What is a **structure** vs **union** in C? *(separate memory for each member vs shared memory)*
- What does `sizeof(struct node)` return for `{int; struct node*}`? *(typically 16 on 64-bit due to padding; 8 on 32-bit)*
- What is `NULL`? *(a macro for a null pointer, `((void*)0)`)*
- Pass by value vs pass by reference in C? *(C is always pass-by-value; references are simulated using pointers)*
- What is a **void pointer**? *(generic pointer, must be cast before dereference)*
- What is **typedef**? *(alias for a type)*
- Difference between `struct node *p` and `struct node **p`? *(pointer to node vs pointer to pointer – used to modify head inside a function)*

**Choose the right structure**
| Need | Use |
|---|---|
| Undo feature | Stack |
| Print jobs | Queue |
| Fast lookup by key | Hash table |
| Sorted data + dynamic updates | BST / AVL |
| Disk-based index | B-tree |
| Shortest path unweighted | BFS |
| Schedule by priority | Priority queue (heap) |
| Network / maps | Graph |
| Matching brackets | Stack |
| Round-robin CPU | Circular queue/list |

**Tricky ones**
- *Can a stack be implemented using a queue?* Yes, two queues.
- *Can binary search work on a linked list?* Not efficiently.
- *Which traversal copies a tree?* Preorder. *Which deletes a tree?* Postorder.
- *Is quick sort better than merge sort?* Usually faster in practice due to cache; merge guarantees O(n log n) and stability.
- *Maximum number of nodes at level L in a binary tree?* 2^L.
- *Number of null pointers in a binary tree with n nodes?* n + 1.
- *Number of binary trees possible with n nodes?* Catalan number `C(2n, n)/(n+1)`.
- *Which data structure does recursion use?* Stack.
- *What happens to `top` when stack is empty?* −1 (array implementation).

---

# ✅ Last-Night Checklist

- [ ] Can I write **push/pop**, **enqueue/dequeue** from memory?
- [ ] Can I do **infix → postfix** and **evaluate postfix** by hand?
- [ ] Can I write **linked list insert/delete/reverse**?
- [ ] Can I **draw a BST** from a sequence and give all 3 traversals?
- [ ] Do I know **4 AVL rotations** with an example?
- [ ] Can I write **bubble, selection, insertion, quick, merge** sort code?
- [ ] Do I know the **complexity table** by heart?
- [ ] Can I explain **hash collision & 3 resolutions**?
- [ ] Can I do **BFS & DFS** on a drawn graph?
- [ ] Do I understand **Warshall** & **spanning tree**?
- [ ] Do I know **why** each structure is used (applications)?

### 🎯 Viva Tips
1. **Start with the definition**, then give an example, then the complexity.
2. Draw a small diagram on paper – examiners love it.
3. If you don't know, say what you *do* know about the topic rather than staying silent.
4. Mention **trade-offs** (time vs space, array vs list) – it shows understanding.
5. Be ready to **dry-run** your code with 3–4 inputs.

**Good luck for your viva! 🚀**
