# MODULE I: Introduction, Complexity, Arrays

## Definition and Types (Data Structures)

A **data structure** is a way of organizing and storing data in a computer so it can be accessed and modified efficiently. It's not just about storage — it's about the relationships between data elements and the operations that can be performed on them.

**Types of Data Structures:**

1. **Primitive Data Structures**: Basic types directly operated on by machine instructions — int, float, char, pointer, boolean.

2. **Non-Primitive Data Structures**: Built from primitive types, divided into:
   - **Linear Data Structures**: Elements arranged sequentially, each element connected to its previous and next element. E.g., Arrays, Linked Lists, Stacks, Queues.
   - **Non-Linear Data Structures**: Elements are not arranged sequentially; each element can connect to multiple other elements. E.g., Trees, Graphs.

You might also be asked to classify by **static vs dynamic**:
- **Static**: Size fixed at compile time (e.g., Arrays).
- **Dynamic**: Size can grow/shrink at runtime (e.g., Linked Lists).

## Algorithm Design

An **algorithm** is a finite, well-defined, step-by-step procedure for solving a problem, taking some input and producing some output in a finite amount of time.

**Characteristics of a good algorithm** (commonly asked list):
- **Finiteness**: Must terminate after a finite number of steps.
- **Definiteness**: Each step must be precisely and unambiguously defined.
- **Input**: Zero or more well-defined inputs.
- **Output**: One or more well-defined outputs.
- **Effectiveness**: Every operation must be basic enough to be carried out, in principle, by a person using pencil and paper.

**Steps in Algorithm Design:**
1. Problem definition — understand what needs to be solved.
2. Design — choose an approach/strategy (e.g., divide and conquer, greedy, dynamic programming).
3. Validate correctness — prove it works for all valid inputs (often via induction).
4. Analyze — determine time and space complexity.
5. Implement — code it in a programming language.
6. Test and debug.

## Complexity (Time and Space)

This is a **very heavily tested numerical topic**. You need to be able to compute Big-O for given code snippets.

**Time Complexity**: A measure of the amount of time an algorithm takes to run, as a function of the size of the input (n). It doesn't measure actual clock time (which varies by hardware) — it measures the growth rate of operations as n increases.

**Space Complexity**: A measure of the amount of memory an algorithm needs to run, as a function of input size n. Includes both the space to store the input and any extra (auxiliary) space used during execution.

**Asymptotic Notations** (must-know, always tested):

1. **Big-O (O)** — Upper bound (worst case). f(n) = O(g(n)) if there exist constants c > 0, n₀ such that f(n) ≤ c·g(n) for all n ≥ n₀.

2. **Omega (Ω)** — Lower bound (best case). f(n) = Ω(g(n)) if f(n) ≥ c·g(n) for all n ≥ n₀.

3. **Theta (Θ)** — Tight bound (average/exact case). f(n) = Θ(g(n)) if c₁·g(n) ≤ f(n) ≤ c₂·g(n).

**Common complexity classes (memorize this order, smallest to largest):**
$$O(1) < O(\log n) < O(n) < O(n \log n) < O(n^2) < O(n^3) < O(2^n) < O(n!)$$

| Complexity | Name | Example |
|---|---|---|
| O(1) | Constant | Accessing array element by index |
| O(log n) | Logarithmic | Binary search |
| O(n) | Linear | Linear search, single loop |
| O(n log n) | Linearithmic | Merge sort, Quick sort (avg) |
| O(n²) | Quadratic | Bubble sort, Selection sort, Insertion sort |
| O(n³) | Cubic | Triple nested loops (e.g., matrix multiplication naive) |
| O(2ⁿ) | Exponential | Recursive Fibonacci (naive), subsets generation |

**Worked complexity example (very common exam question):**

```c
for (i = 0; i < n; i++) {          // runs n times
    for (j = 0; j < n; j++) {      // runs n times for each i
        printf("%d", i*j);
    }
}
```
Total operations = n × n = n² → **Time Complexity = O(n²)**

Another example:
```c
for (i = 1; i < n; i = i*2) {      // i: 1,2,4,8,...,n → runs log₂n times
    printf("%d", i);
}
```
**Time Complexity = O(log n)** — because i doubles each time, it takes log₂n iterations to reach n.

## Time-Space Tradeoffs

This is a very conceptual topic — the idea that you can often **reduce time complexity by using more memory (space)**, or **reduce memory usage at the cost of more time**. Neither is universally "better" — the right choice depends on constraints.

**Examples:**
- **Hashing**: uses extra space (hash table) to achieve O(1) average lookup time instead of O(n) linear search — trading space for time.
- **Memoization / Dynamic Programming**: storing previously computed results (extra space) to avoid recomputation (saves time). E.g., naive recursive Fibonacci is O(2ⁿ) time but O(n) space (call stack); memoized Fibonacci is O(n) time but uses O(n) extra space for the memo table.
- **Precomputed lookup tables**: e.g., storing precomputed factorial values instead of recalculating each time — more memory, faster access.
- **Compression**: reduces space used but requires more time to compress/decompress data.

**Exam-style point to remember**: There's rarely a "free lunch" — optimizing one (time) often costs you the other (space), and a good engineer/algorithm designer must choose the right tradeoff based on the problem's constraints (Is memory limited? Is speed critical?).

## Use of Pointers in Data Structures

A **pointer** is a variable that stores the memory address of another variable, rather than storing a data value directly.

**Why pointers matter for data structures:**
- They allow **dynamic memory allocation** — creating data structures whose size can grow/shrink at runtime (e.g., linked lists, trees), unlike arrays which have fixed size.
- They enable **efficient passing of large data structures** to functions (pass by reference instead of copying entire structures — pass by address, saving both time and memory).
- They form the backbone of **linked data structures** — each node in a linked list, tree, or graph contains a pointer to the next/child node(s), allowing structures to be linked together without requiring contiguous memory (unlike arrays).

**Basic pointer syntax (C):**
```c
int x = 10;
int *p = &x;    // p stores the address of x
printf("%d", *p);  // *p dereferences p, prints value at that address: 10
```

**In the context of linked lists** (Module III), a node is typically defined as:
```c
struct Node {
    int data;
    struct Node *next;   // pointer to the next node
};
```
This `next` pointer is what allows nodes to be scattered anywhere in memory yet still form a connected sequence — this is the fundamental difference from arrays (which require contiguous memory).

---

## Array Definition and Analysis

An **array** is a collection of elements of the **same data type**, stored in **contiguous (sequential) memory locations**, accessed using an index.

**Key properties:**
- Fixed size (in most languages like C — size must be declared at creation, though dynamic arrays exist in some languages).
- Random access — any element can be accessed directly in O(1) time using its index, because the address can be calculated directly (unlike linked lists, which require traversal).
- Homogeneous — all elements must be of the same type.

## Representation of Linear Arrays in Memory

Since array elements are stored in contiguous memory, the address of any element can be calculated directly using a formula — this is a **very common numerical exam question**.

**For a 1D array**, if the base address (address of the first element) is `Base`, and each element takes `w` bytes (size of the data type):

$$\text{Address}(A[i]) = Base + (i - LB) \times w$$

where LB = lower bound (starting index, usually 0).

**Worked numerical example:**
Array A[100] starts at Base address 1000, each integer takes 4 bytes. Find the address of A[25].

Address(A[25]) = 1000 + (25 - 0) × 4 = 1000 + 100 = **1100**

## Traversing Linear Arrays

**Traversal** means visiting/accessing each element of the array exactly once, typically to display or process it.

```c
for (i = 0; i < n; i++) {
    printf("%d ", A[i]);
}
```
**Time Complexity: O(n)** — since every element must be visited once.

## Insertion and Deletion in Arrays

**Insertion**: To insert an element at position `k` in an array of `n` elements, all elements from position `k` to `n-1` must be **shifted one position to the right** first, to make room.

**Worked example:**
Array: [10, 20, 30, 40, 50], insert 25 at index 2.
- Shift elements from index 4 down to index 2 rightward: [10, 20, 30, 30, 40, 50] → wait, correctly:
- Shift 50→index5, 40→index4, 30→index3
- Result: [10, 20, 25, 30, 40, 50]

**Time Complexity**: O(n) in the worst case (inserting at the beginning requires shifting all n elements). Best case O(1) (inserting at the end, if space available).

**Deletion**: To delete an element at position `k`, all elements after position `k` must be **shifted one position to the left** to fill the gap.

**Worked example:**
Array: [10, 20, 30, 40, 50], delete element at index 1 (value 20).
- Shift 30→index1, 40→index2, 50→index3
- Result: [10, 30, 40, 50]

**Time Complexity**: O(n) worst case (deleting the first element), O(1) best case (deleting the last element).

## Single, Two-Dimensional, and Multidimensional Arrays

**1D Array**: A simple list, e.g., `int A[5]`.

**2D Array**: A "matrix" — array of arrays, e.g., `int A[3][4]` (3 rows, 4 columns). Used for representing tables, grids, matrices.

**Address calculation for 2D arrays** — another very common numerical exam question. Two storage schemes:

**Row-Major Order** (used in C): elements stored row by row.
$$\text{Address}(A[i][j]) = Base + [(i - LB_1) \times N + (j - LB_2)] \times w$$
where N = number of columns.

**Column-Major Order** (used in Fortran): elements stored column by column.
$$\text{Address}(A[i][j]) = Base + [(j - LB_2) \times M + (i - LB_1)] \times w$$
where M = number of rows.

**Worked numerical example:**
Array A[4][5] (4 rows, 5 columns), Base = 1000, w = 4 bytes, row-major order. Find address of A[2][3].

Address = 1000 + [(2-0)×5 + (3-0)] × 4 = 1000 + [10+3]×4 = 1000 + 52 = **1052**

Same array, but column-major order:
Address = 1000 + [(3-0)×4 + (2-0)] × 4 = 1000 + [12+2]×4 = 1000 + 56 = **1056**

**Multidimensional Arrays**: Generalization to 3D, 4D, etc. arrays — e.g., `int A[2][3][4]`. Address formula generalizes similarly, using nested offsets based on the size of each dimension.

## Functions Associated with Arrays

Common operations you should be able to describe with their complexity:
- **Traverse**: Visit each element — O(n)
- **Search**: Find an element — O(n) linear, O(log n) binary (if sorted)
- **Insert**: Add an element — O(n) worst case
- **Delete**: Remove an element — O(n) worst case
- **Sort**: Arrange in order — O(n log n) to O(n²) depending on algorithm
- **Merge**: Combine two arrays — O(m+n)
- **Reverse**: Reverse the order of elements — O(n)

## Character Strings in C

A **string** in C is essentially a **1D array of characters**, terminated by a special **null character `\0`** which marks the end of the string. This is different from many other languages where strings are a distinct built-in type.

```c
char str[6] = "Hello";  // stored as {'H','e','l','l','o','\0'} — 6 bytes, not 5
```

## Character String Operations

Key operations you should know (all commonly implemented as C library functions in `<string.h>`):

- **strlen(str)**: Returns length of string (excluding `\0`). Time: O(n)
- **strcpy(dest, src)**: Copies src string into dest. Time: O(n)
- **strcat(dest, src)**: Concatenates (appends) src to the end of dest. Time: O(n+m)
- **strcmp(str1, str2)**: Compares two strings lexicographically; returns 0 if equal, negative if str1<str2, positive if str1>str2. Time: O(n)
- **strrev(str)**: Reverses a string (not standard, but often implemented manually). Time: O(n)

**Worked example (strcmp)**: strcmp("apple", "banana") → compares 'a' (97) vs 'b' (98) → returns negative (since 'a' < 'b' in ASCII).

## Sparse Matrix

A **sparse matrix** is a matrix in which the **majority of elements are zero**. Storing such a matrix as a full 2D array wastes a lot of memory, since we'd be storing mostly zeros.

**Why it matters**: If a matrix is, say, 1000×1000 but only 50 elements are non-zero, storing it as a normal 2D array wastes memory storing ~999,950 zeros unnecessarily.

**Sparse Matrix Representation** — the standard technique used is the **triplet (3-tuple) representation**: Store only the non-zero elements, each represented as (row, column, value).

**Worked example:**

Given a sparse matrix:
```
0  0  3  0
0  0  0  4
0  5  0  0
```

This is a 3×4 matrix. Non-zero elements: 3 at (0,2), 4 at (1,3), 5 at (2,1).

**Sparse representation (triplet form)**, typically stored as:

| Row | Col | Value |
|---|---|---|
| 3 | 4 | 3 (this row stores: total rows, total cols, total non-zero elements) |
| 0 | 2 | 3 |
| 1 | 3 | 4 |
| 2 | 1 | 5 |

This drastically reduces storage: instead of storing 3×4=12 elements, we only store the 3 non-zero elements (plus a header row) = 4 rows of 3 values each = 12 values total in this small case, but for large sparse matrices the savings are massive.

**Advantages**: Saves memory, faster processing (only non-zero elements are processed in operations like addition/multiplication).

---

# MODULE V: Sorting, Searching, Hashing

This module is **the most numerically-heavy** — expect full step-by-step trace questions ("sort this array using X sort, show each pass").

## Insertion Sort

**Idea**: Build the sorted array one element at a time, by taking each element and inserting it into its correct position among the already-sorted elements to its left.

```c
for (i = 1; i < n; i++) {
    key = A[i];
    j = i - 1;
    while (j >= 0 && A[j] > key) {
        A[j+1] = A[j];
        j--;
    }
    A[j+1] = key;
}
```

**Time Complexity**: Best case O(n) (already sorted), Worst/Average case O(n²).
**Space Complexity**: O(1) — in-place sort.
**Stable**: Yes (equal elements retain relative order).

**Full worked trace:**
Array: [5, 2, 4, 6, 1, 3]

- Pass 1 (i=1, key=2): Compare with 5 → 5>2, shift → [5,5,4,6,1,3] → insert 2 at index 0 → **[2,5,4,6,1,3]**
- Pass 2 (i=2, key=4): Compare with 5 → shift → [2,5,5,6,1,3] → compare with 2 → 2<4, stop → insert at index 1 → **[2,4,5,6,1,3]**
- Pass 3 (i=3, key=6): 5<6, no shift needed → **[2,4,5,6,1,3]**
- Pass 4 (i=4, key=1): shift 6,5,4,2 all right → **[1,2,4,5,6,3]**
- Pass 5 (i=5, key=3): shift 6,5,4 right, 2<3 stop → **[1,2,3,4,5,6]**

Final sorted array: **[1,2,3,4,5,6]**

## Bubble Sort

**Idea**: Repeatedly step through the array, compare adjacent elements, and swap them if they're in the wrong order. After each full pass, the largest unsorted element "bubbles up" to its correct position at the end.

```c
for (i = 0; i < n-1; i++) {
    for (j = 0; j < n-i-1; j++) {
        if (A[j] > A[j+1]) {
            swap(A[j], A[j+1]);
        }
    }
}
```

**Time Complexity**: Best case O(n) (if optimized with a "no swaps" flag to detect already-sorted array), Worst/Average O(n²).
**Space Complexity**: O(1).
**Stable**: Yes.

**Full worked trace:**
Array: [5, 1, 4, 2, 8]

Pass 1: (5,1)→swap→[1,5,4,2,8]; (5,4)→swap→[1,4,5,2,8]; (5,2)→swap→[1,4,2,5,8]; (5,8)→no swap → **[1,4,2,5,8]**
Pass 2: (1,4)→no; (4,2)→swap→[1,2,4,5,8]; (4,5)→no; → **[1,2,4,5,8]**
Pass 3: (1,2)→no; (2,4)→no; (4,5)→no → **[1,2,4,5,8]** (no swaps → could terminate early)

Final: **[1,2,4,5,8]**

## Selection Sort

**Idea**: Repeatedly find the minimum element from the unsorted portion of the array and swap it into its correct position at the beginning of the unsorted portion.

```c
for (i = 0; i < n-1; i++) {
    min_idx = i;
    for (j = i+1; j < n; j++) {
        if (A[j] < A[min_idx]) min_idx = j;
    }
    swap(A[i], A[min_idx]);
}
```

**Time Complexity**: O(n²) in all cases (best, worst, average) — it always scans the remaining array regardless of order.
**Space Complexity**: O(1).
**Stable**: No (can swap equal elements out of original relative order).

**Full worked trace:**
Array: [29, 10, 14, 37, 13]

Pass 1: min in [29,10,14,37,13] = 10 (index 1) → swap with index 0 → **[10,29,14,37,13]**
Pass 2: min in [29,14,37,13] = 13 (index 4) → swap with index 1 → **[10,13,14,37,29]**
Pass 3: min in [14,37,29] = 14 (already at index 2) → no swap → **[10,13,14,37,29]**
Pass 4: min in [37,29] = 29 (index 4) → swap with index 3 → **[10,13,14,29,37]**

Final: **[10,13,14,29,37]**

## Quick Sort / Partition Exchange Sort

("Quick sort" and "Partition exchange sort" refer to the same algorithm — quicksort works by partitioning.)

**Idea**: Choose a **pivot** element, then partition the array so all elements smaller than the pivot go to its left, and all elements larger go to its right. Recursively apply the same process to the left and right sub-arrays.

**Partition process (Lomuto scheme, pivot = last element):**
```c
int partition(A, low, high) {
    pivot = A[high];
    i = low - 1;
    for (j = low; j < high; j++) {
        if (A[j] < pivot) {
            i++;
            swap(A[i], A[j]);
        }
    }
    swap(A[i+1], A[high]);
    return i+1;   // final position of pivot
}
```

**Time Complexity**: 
- Best/Average case: **O(n log n)** (when pivot divides array roughly in half each time)
- Worst case: **O(n²)** (when pivot is always the smallest/largest element, e.g., already sorted array with poor pivot choice — leads to unbalanced partitions)

**Space Complexity**: O(log n) average (recursion stack), O(n) worst case.
**Stable**: No.

**Full worked trace (pivot = last element):**
Array: [10, 80, 30, 90, 40], low=0, high=4, pivot=40

- i=-1
- j=0: A[0]=10 < 40 → i=0, swap(A[0],A[0]) → [10,80,30,90,40]
- j=1: A[1]=80, not < 40 → skip
- j=2: A[2]=30 < 40 → i=1, swap(A[1],A[2]) → [10,30,80,90,40]
- j=3: A[3]=90, not < 40 → skip
- After loop: swap(A[i+1], A[high]) = swap(A[2], A[4]) → **[10,30,40,90,80]**
- Pivot 40 is now at index 2 (correct sorted position). Recursively sort left [10,30] and right [90,80].

Left [10,30]: already sorted (or trivially partitioned) → [10,30]
Right [90,80]: pivot=80, 90 not<80 → swap(A[i+1],A[high]) → **[80,90]**

Final merged: **[10,30,40,80,90]**

## Merge Sort

**Idea**: A classic **divide and conquer** algorithm. Recursively divide the array into two halves until each sub-array has one element (trivially sorted), then **merge** the sorted halves back together in sorted order.

```c
void mergeSort(A, l, r) {
    if (l < r) {
        m = (l+r)/2;
        mergeSort(A, l, m);
        mergeSort(A, m+1, r);
        merge(A, l, m, r);
    }
}
```

**Time Complexity**: **O(n log n)** in ALL cases (best, worst, average) — very consistent, unlike quicksort.
**Space Complexity**: **O(n)** — requires extra array space for merging (not in-place).
**Stable**: Yes.

**Full worked trace:**
Array: [38, 27, 43, 3, 9, 82, 10]

Divide: [38,27,43,3] and [9,82,10]
  Further divide [38,27,43,3] → [38,27] and [43,3]
    → [38] [27] → merge → [27,38]
    → [43] [3] → merge → [3,43]
  → merge [27,38] and [3,43] → **[3,27,38,43]**
  
  Further divide [9,82,10] → [9,82] and [10]
    → [9] [82] → merge → [9,82]
  → merge [9,82] and [10] → **[9,10,82]**

Final merge: merge [3,27,38,43] and [9,10,82]
Compare 3 vs 9 → 3; 27 vs 9 → 9; 27 vs 10 → 10; 27 vs 82 → 27; 38 vs 82 → 38; 43 vs 82 → 43; remaining → 82

Final sorted array: **[3,9,10,27,38,43,82]**

**Merge sort vs Quick sort — common comparison question:**

| Aspect | Merge Sort | Quick Sort |
|---|---|---|
| Worst case | O(n log n) | O(n²) |
| Space | O(n) — not in-place | O(log n) avg — in-place |
| Stable | Yes | No |
| Good for | Linked lists, external sorting, guaranteed performance | Arrays, general-purpose, faster in practice (small constants) |

---

## Linear Search

**Idea**: Sequentially check every element in the array, one by one, until the target is found or the array is exhausted.

```c
for (i = 0; i < n; i++) {
    if (A[i] == key) return i;
}
return -1;
```

**Time Complexity**: Best case O(1) (found at first position), Worst/Average case **O(n)**.
**Works on**: Both sorted and unsorted arrays.

## Binary Search

**Idea**: Only works on a **sorted array**. Repeatedly divide the search interval in half — compare the target with the middle element; if equal, found; if target is smaller, search the left half; if larger, search the right half.

```c
low = 0; high = n-1;
while (low <= high) {
    mid = (low+high)/2;
    if (A[mid] == key) return mid;
    else if (A[mid] < key) low = mid+1;
    else high = mid-1;
}
return -1;
```

**Time Complexity**: **O(log n)** — because the search space halves each iteration.
**Requires**: Array must be sorted.

**Full worked trace:**
Sorted array: [2, 5, 8, 12, 16, 23, 38, 45, 56, 72, 91], search for key = 23

- low=0, high=10, mid=5, A[5]=23 → **Found at index 5!** (in this lucky case, just 1 comparison)

Let's do one that takes multiple steps — search for key = 56:
- low=0, high=10, mid=5, A[5]=23 < 56 → low=6
- low=6, high=10, mid=8, A[8]=56 → **Found at index 8** (2 comparisons)

Search for key = 100 (not present):
- low=0, high=10, mid=5, A[5]=23<100 → low=6
- low=6, high=10, mid=8, A[8]=56<100 → low=9
- low=9, high=10, mid=9, A[9]=72<100 → low=10
- low=10, high=10, mid=10, A[10]=91<100 → low=11
- low(11) > high(10) → **Not found**, return -1

## Hashing

**Idea**: A technique that maps data (keys) to specific positions (called **hash indices**) in a table (called a **hash table**) using a **hash function**, enabling near-instant O(1) average-case insertion, deletion, and search — much faster than linear O(n) search.

## Hash Functions

A **hash function** h(key) takes a key as input and produces an index within the bounds of the hash table.

**Common hash functions:**

1. **Division Method**: h(key) = key mod m, where m = table size (usually chosen as a prime number to reduce collisions).

2. **Multiplication Method**: h(key) = floor(m × (key × A mod 1)), where A is a constant between 0 and 1 (often A ≈ 0.618, the golden ratio).

3. **Mid-Square Method**: Square the key, then extract middle digits.

4. **Folding Method**: Split the key into parts, add the parts together, then take mod m.

**Worked example (Division method):**
Table size m = 7. Insert keys: 18, 27, 33, 12

- h(18) = 18 mod 7 = 4
- h(27) = 27 mod 7 = 6
- h(33) = 33 mod 7 = 5
- h(12) = 12 mod 7 = 5 → **Collision!** (33 and 12 both map to index 5)

## Collision Resolution Techniques

A **collision** occurs when two different keys hash to the same index. This is unavoidable in general (by the pigeonhole principle) and must be resolved.

**1. Chaining (Open Hashing/Separate Chaining)**
Each slot in the hash table holds a **linked list** of all elements that hash to that index. On collision, simply append the new element to the list at that index.

Using the example above (m=7): Index 5 would have a linked list: 33 → 12

**Advantages**: Simple, handles unlimited collisions (list grows), never "full."
**Disadvantages**: Extra memory for pointers, worst case O(n) if all keys hash to the same slot.

**2. Open Addressing (Closed Hashing)**
All elements are stored **within the hash table itself** — on collision, we probe (search) for the next available empty slot according to some sequence.

- **Linear Probing**: If h(key) is occupied, try (h(key)+1) mod m, then (h(key)+2) mod m, etc. — check the next slots sequentially.
  
  **Worked example**: Table size 7, insert 18(→4), 27(→6), 33(→5), 12(→5, collision → try 5+1=6, occupied by 27 → try 5+2=7mod7=0, empty → place 12 at index 0)
  
  **Problem**: Leads to **"clustering"** — consecutive occupied slots build up, making future insertions slower.

- **Quadratic Probing**: If h(key) is occupied, try (h(key) + 1²) mod m, then (h(key) + 2²) mod m, then (h(key)+3²) mod m, etc. — reduces clustering compared to linear probing.

- **Double Hashing**: Use a **second hash function** h2(key) to determine the probe sequence: index = (h1(key) + i×h2(key)) mod m, for i=0,1,2,... This distributes collisions more uniformly, minimizing clustering almost entirely.

**Load Factor**: α = n/m (number of elements / table size). As α approaches 1, the chance of collisions increases dramatically, and performance degrades — so hash tables are often resized ("rehashed") once load factor crosses a threshold (commonly 0.7).

---

# MODULE III: Singly Linked Lists

## Introduction to Singly Linked Lists

A **linked list** is a linear data structure where elements (called **nodes**) are NOT stored in contiguous memory locations. Instead, each node contains data plus a **pointer/reference to the next node**, forming a chain.

**Why use linked lists instead of arrays?**
- **Dynamic size**: Can grow/shrink at runtime, no need to declare size upfront.
- **Efficient insertion/deletion**: O(1) if you already have a pointer to the position (no shifting required, unlike arrays).
- **Disadvantage**: No random access — to reach the k-th element, you must traverse from the head, taking O(n) time (unlike array's O(1) indexed access). Also uses extra memory for storing pointers.

**Structure of a node (singly linked list)**:
```c
struct Node {
    int data;
    struct Node *next;
};
```

Each node has exactly one link — pointing forward to the next node. The last node's `next` points to `NULL`, marking the end of the list. A separate pointer called `head` (or `start`) points to the first node of the list.

## Representation of Linked Lists in Memory

Unlike arrays where you can calculate any element's address directly with a formula, in a linked list, nodes can be **scattered anywhere in memory**. The only way to know where the next node is, is by following the `next` pointer stored in the current node.

This means: **Address(Node[i]) cannot be calculated directly — you must traverse from head, following i pointers.**

## Traversing a Linked List

Visiting each node exactly once, starting from `head` and following `next` pointers until reaching `NULL`.

```c
struct Node *temp = head;
while (temp != NULL) {
    printf("%d ", temp->data);
    temp = temp->next;
}
```

**Time Complexity: O(n)** — must visit every node.

## Searching in a Linked List

Similar to traversal, but stop early if the target is found.

```c
struct Node *temp = head;
while (temp != NULL) {
    if (temp->data == key) return temp;  // found
    temp = temp->next;
}
return NULL;  // not found
```

**Time Complexity: O(n)** worst case — since there's no random access, binary search is NOT possible on a linked list (even if sorted), because you can't jump to the middle directly without traversing.

## Insertion into a Linked List

There are three cases, each with different logic:

**1. Insertion at the Beginning:**
```c
struct Node *newNode = malloc(sizeof(struct Node));
newNode->data = value;
newNode->next = head;
head = newNode;
```
**Time Complexity: O(1)** — no traversal needed.

**2. Insertion at the End:**
```c
struct Node *newNode = malloc(sizeof(struct Node));
newNode->data = value;
newNode->next = NULL;
if (head == NULL) { head = newNode; return; }
struct Node *temp = head;
while (temp->next != NULL) temp = temp->next;
temp->next = newNode;
```
**Time Complexity: O(n)** — must traverse to the last node first (unless you maintain a separate `tail` pointer, which brings this down to O(1)).

**3. Insertion at a Given Position (say after node with value `x`):**
```c
struct Node *temp = head;
while (temp != NULL && temp->data != x) temp = temp->next;
if (temp != NULL) {
    struct Node *newNode = malloc(sizeof(struct Node));
    newNode->data = value;
    newNode->next = temp->next;
    temp->next = newNode;
}
```
**Time Complexity: O(n)** — need to traverse/find the position first, but the actual insertion itself is O(1) once the position is found.

**Worked example (Insertion at beginning):**
List: 10 → 20 → 30 → NULL. Insert 5 at the beginning.
New node(5)->next = head(10) → head = new node(5)
Result: **5 → 10 → 20 → 30 → NULL**

## Deletion from a Linked List

Similarly, three cases:

**1. Deletion at the Beginning:**
```c
struct Node *temp = head;
head = head->next;
free(temp);
```
**Time Complexity: O(1)**

**2. Deletion at the End:**
```c
struct Node *temp = head;
while (temp->next->next != NULL) temp = temp->next;
free(temp->next);
temp->next = NULL;
```
**Time Complexity: O(n)** — must traverse to the second-last node.

**3. Deletion of a Node with Given Value:**
```c
struct Node *temp = head, *prev = NULL;
while (temp != NULL && temp->data != key) {
    prev = temp;
    temp = temp->next;
}
if (temp == NULL) return;  // not found
if (prev == NULL) head = temp->next;  // deleting head node
else prev->next = temp->next;
free(temp);
```
**Time Complexity: O(n)**

**Worked example:**
List: 10 → 20 → 30 → 40 → NULL. Delete node with value 30.
- prev=20's node, temp=30's node
- prev->next = temp->next → 20's next becomes 40's node
Result: **10 → 20 → 40 → NULL**

## Reversing a Linked List

**Idea**: Reverse the direction of all `next` pointers, so the last node becomes the new head. This is one of the **most frequently asked coding/tracing questions** in exams.

```c
struct Node *prev = NULL, *curr = head, *next = NULL;
while (curr != NULL) {
    next = curr->next;   // save next node before we overwrite curr->next
    curr->next = prev;   // reverse the pointer
    prev = curr;         // move prev forward
    curr = next;         // move curr forward
}
head = prev;              // prev is now the new head
```

**Time Complexity: O(n)**, **Space Complexity: O(1)** (iterative version — no extra data structure needed, just pointer manipulation).

**Full worked trace** (this exact trace, step by step, is a classic exam question):
List: 10 → 20 → 30 → NULL

| Step | prev | curr | next | Action |
|---|---|---|---|---|
| Start | NULL | 10 | — | |
| 1 | NULL | 10 | 20 | curr(10)->next = NULL; prev=10; curr=20 |
| 2 | 10 | 20 | 30 | curr(20)->next = 10; prev=20; curr=30 |
| 3 | 20 | 30 | NULL | curr(30)->next = 20; prev=30; curr=NULL |
| End | curr=NULL, loop stops | | | head = prev = 30 |

Result: **30 → 20 → 10 → NULL** ✓ (list successfully reversed)
