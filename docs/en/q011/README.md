<div align="center">

[🇺🇸 English](./README.md) | [🇮🇷 فارسی](../../fa/q011/README.md)

</div>

---

# Question 011: Facebook(Meta) question: Move Zeroes to the End of an Array

> **Facebook / Meta Interview Question**

<div align="center">
  <img src="../../../assets/img011-001.png" alt="Move Zeroes Problem" />
</div>

## A Practical Guide to Solving the Move Zeroes Problem

**Move Zeroes** is a classic Easy-level array problem frequently encountered on platforms such as LeetCode and commonly used as an interview question by major technology companies, including Meta (Facebook).

Although the problem itself is simple, it tests several important concepts:

* Array traversal
* The Two-Pointer technique
* In-place modification
* Preserving the relative order of elements
* Time and space complexity analysis

---

## 1. Problem Statement

Given an integer array `nums`, move all `0` values to the end of the array.

The following requirements must be satisfied:

* The **relative order of non-zero elements must remain unchanged**.
* The operation must be performed **in-place**.
* No additional array should be created.

### Examples

| Input              | Output             |
| ------------------ | ------------------ |
| `[0, 1, 0, 3, 12]` | `[1, 3, 12, 0, 0]` |
| `[0]`              | `[0]`              |

For example:

```text
[0, 1, 0, 3]
```

must become:

```text
[1, 3, 0, 0]
```

Notice that the relative order of the non-zero elements (`1` and `3`) is preserved.

---

## 2. The Two-Pointer Approach

One straightforward approach is to create another array, copy all non-zero elements into it, and then append the required number of zeroes.

However, this approach violates the **in-place** requirement because it requires additional memory proportional to the size of the input array.

A better solution is to use the **Two-Pointer technique** and process the array in a single pass.

### The Two Pointers

We use two indexes:

1. **`i` — the Explorer**

   Traverses the entire array from left to right and examines every element.

2. **`lastNonZeroFoundAt` — the Collector**

   Keeps track of the position where the next non-zero element should be placed.

The key idea is simple:

> `lastNonZeroFoundAt` always points to the first position that has not yet received its correct non-zero value.

Whenever `i` finds a non-zero element, that element is moved to the Collector's position.

---

## 3. C++ Implementation

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        // Position where the next non-zero element should be placed
        int lastNonZeroFoundAt = 0;

        // Traverse the array once
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != 0) {
                // Move the non-zero element to the next available position
                swap(nums[lastNonZeroFoundAt], nums[i]);

                // Advance the position for the next non-zero element
                lastNonZeroFoundAt++;
            }
        }
    }
};
```

### Why Does This Work?

Suppose we have:

```text
[0, 1, 0, 3]
```

Initially:

```text
lastNonZeroFoundAt = 0
```

This means:

> "The next non-zero element should be placed at index `0`."

The `i` pointer then scans the array.

* If `nums[i] == 0`, there is nothing to do.
* If `nums[i] != 0`, the element is swapped into the position tracked by `lastNonZeroFoundAt`.

After placing the non-zero element, `lastNonZeroFoundAt` moves forward.

This naturally pushes all zeroes toward the end of the array.

---

## 4. An Intuitive Way to Understand the Algorithm

If the pointer-based explanation feels too abstract, imagine a row of boxes.

Some boxes contain **gifts** and some boxes are **empty**.

In this analogy:

* A **gift** represents a non-zero value.
* An **empty box** represents a zero.
* The **Explorer** searches through all boxes.
* The **Collector** marks the first position where the next gift should be placed.

### The Explorer

The Explorer moves from left to right and examines every box.

### The Collector

The Collector does not move every time the Explorer moves.

Instead, it waits at the first position that still needs a non-zero value.

This distinction is the key to understanding the algorithm.

---

## 5. How the Two Pointers Behave

### When the Explorer Finds a Zero

Suppose the Explorer encounters:

```text
0
```

There is nothing to move.

The Explorer simply continues to the next position.

The Collector **does not move**, because its current position is still waiting for the next non-zero element.

---

### When the Explorer Finds a Non-Zero Value

Suppose the Explorer finds:

```text
3
```

The Collector's position represents the first available position for a non-zero value.

So we swap the two elements:

```text
Collector position <-> Explorer position
```

Then the Collector moves one position forward because that position has now been filled correctly.

---

## 6. Step-by-Step Example

Consider the following array:

```text
[0, 1, 0, 3]
```

Initially:

```text
Explorer   -> index 0
Collector  -> index 0
```

### Step 1

The Explorer finds:

```text
0
```

Nothing happens.

```text
[0, 1, 0, 3]
 ↑
Collector
```

The Explorer moves forward.

The Collector stays at index `0`.

---

### Step 2

The Explorer finds:

```text
1
```

This is a non-zero value.

The Collector is currently at index `0`, so we swap:

```text
[0, 1, 0, 3]
```

becomes:

```text
[1, 0, 0, 3]
```

The Collector moves to index `1`.

---

### Step 3

The Explorer finds:

```text
0
```

Nothing happens.

```text
[1, 0, 0, 3]
    ↑
Collector
```

The Collector stays at index `1`.

---

### Step 4

The Explorer finds:

```text
3
```

The Collector is at index `1`.

We swap:

```text
[1, 0, 0, 3]
```

and get:

```text
[1, 3, 0, 0]
```

The Collector then moves to index `2`.

The final result is:

```text
[1, 3, 0, 0]
```

---

## 7. The Key Invariant

The most important concept behind this solution is the following invariant:

> **Before the Collector's position, all elements are non-zero and already in their correct relative order.**

Meanwhile:

> **From the Collector's position onward, the remaining elements have not yet been fully processed.**

This invariant explains why the algorithm can safely place every discovered non-zero element at the Collector's position.

For example, during the execution:

```text
[1, 3, ?, ?, ...]
       ↑
   Collector
```

Everything before the Collector is already correct.

The Collector marks the boundary between:

```text
Correctly placed non-zero elements
                |
                v
[1, 3, | remaining elements ...]
```

This is the core idea of the Two-Pointer solution.

---

## 8. Why the Relative Order Is Preserved

Consider:

```text
[0, 1, 0, 3, 12]
```

The non-zero elements appear in this order:

```text
1 → 3 → 12
```

The Explorer encounters them in exactly this order.

Each one is placed at the next available Collector position:

```text
1 → index 0
3 → index 1
12 → index 2
```

Therefore, their relative order never changes.

The result is:

```text
[1, 3, 12, 0, 0]
```

---

## 9. Complexity Analysis

### Time Complexity

```text
O(n)
```

The array is traversed exactly once.

Where `n` is the number of elements in the array.

### Space Complexity

```text
O(1)
```

No additional array or data structure is created.

The elements are rearranged directly inside the original array.

Therefore, this is an **in-place** solution.

---

## 10. Why This Solution Is Interview-Friendly

This problem is a good example of how a seemingly simple array manipulation can test several important programming skills.

A strong solution should demonstrate that the candidate understands:

* How to traverse an array efficiently.
* How to use two indexes to maintain different responsibilities.
* How to modify an array in-place.
* How to preserve the relative order of elements.
* How to achieve `O(n)` time complexity.
* How to achieve `O(1)` extra space complexity.
* How to explain the invariant behind the algorithm.

The important insight is not simply knowing how to use `swap`.

The important insight is recognizing that one pointer can track the **next position for a non-zero element**, while another pointer scans the array.

---

## 🤝 Contributors

<div align="center">

| GitHub                                      | LinkedIn                                                           | Email                                           | Site                           | Telegram                               |
| ------------------------------------------- | ------------------------------------------------------------------ | ----------------------------------------------- | ------------------------------ | -------------------------------------- |
| [HadiAbbasi](https://github.com/HadiAbbasi) | [Hadi Abbasi](https://www.linkedin.com/in/hadi-abbasi-programmer/) | [Hadi Abbasi](hadi.abbasi.programmer@gmail.com) | [Hiens.org](https://hiens.org) | [Hadi Abbasi](@Hadi_Abbasi_Programmer) |

</div>
