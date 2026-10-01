<div align="center">

[🇺🇸 English](./README.md) · [🇮🇷 فارسی](../../fa/q012/README.md)

</div>

---

# 🔎 Finding Common Substrings Between Two Strings in C++

<div align="center">
  <img src="../../../assets/img012-001.jpg" alt="Image" />
</div>

## 📌 Problem Statement

We have two strings, `str01` and `str02`, and we want to find **all distinct common substrings** between them.

> The term **Substring** refers to a contiguous sequence of characters within a string. Simply put, it is extracting a range from a string!

Now, regarding the main problem, suppose we have two strings! For example:

```text
str01 = "abcde"
str02 = "xbcde"
```

The set of common substrings includes items such as:

```text
c
d
e
cd
de
cde
```

However:

```text
ace
```

is not considered a substring because its characters are not contiguous in the original string.

---

## 🧠 General Idea

To solve this problem, we have three main approaches:

1. **Generating all substrings and using `unordered_set`**
2. **Using Dynamic Programming and an `n × m` matrix**
3. **Using Dynamic Programming and a 1D array replacing `n × m`**

The first method is simple and easy to understand, but it is expensive for large strings.

The second method utilizes the relationship between characters of two strings and is very suitable for finding the **Longest Common Substring**.

The third method is an optimized version of the second method that uses a 1D array of the length of one of the strings instead of an n × m matrix! It does not matter which string you choose for the array length, because the length of the longest common substring or all common substrings is shorter than or equal to both strings!

---

# Method 1: `unordered_set`

## 💡 Idea

In the naive approach, we first generate all substrings of the first string and insert them into an `unordered_set`.

Then, we generate all substrings of the second string and check whether they exist in the `unordered_set` of the first string.

Overall structure of the algorithm:

```text
str01
  │
  ├── all of substrings
  │
  ▼
unordered_set
  ▲
  │
  ├── substrings of str2
  │
str02
```

---

## 1️⃣ Generating Substrings of the First String

Using two nested loops, we generate all possible range intervals of the string:

```cpp
unordered_set<string> str01_substrings;

for (int i = 0; i < str01.size(); i++)
{
    for (int j = i + 1; j <= str01.size(); j++)
    {
        str01_substrings.insert(
            str01.substr(i, j - i)
        );
    }
}
```

Here:

* Variable `i` is the starting point of the substring.
* Variable `j` is the ending point of the interval.
* The value `j - i` is the length of the substring.
* `substr(i, j - i)` generates the substring itself.
* The `unordered_set` structure ensures duplicate substrings are kept only once.

---

## 2️⃣ Finding Common Substrings

Now we check all substrings of the second string:

```cpp
unordered_set<string> results;

for (int i = 0; i < str02.size(); i++)
{
    for (int j = i + 1; j <= str02.size(); j++)
    {
        string sub = str02.substr(i, j - i);

        if (str01_substrings.find(sub) != str01_substrings.end())
        {
            results.insert(sub);
        }
    }
}
```

If the substring exists in the set of the first string, we insert it into `results`.

---

## ⏱️ Complexity of Method 1

A string of length `n` has approximately:

```text
n × (n + 1) / 2
```

non-empty substrings.

Therefore, the number of substrings is on the order of:

```text
O(n²)
```

However, an important point is that `substr()` itself requires creating a new `string`, incurring the cost of copying characters. To solve this, `string_view` can also be used! `string_view` allows you to hold a pointer to a character and a length inside a lightweight object, replacing heavy copies with an object pointing to a memory position, avoiding extra copies, extra memory, and extra processing overhead!

Thus, overall, this method can be very slow and memory-intensive for large strings in practice.

### Summary

```text
all of substrings of str1
        ↓
   unordered_set
        ↑
all of substrings of str2
        ↓
common substrings
```

Pros:

* Simple
* Easy to understand
* Suitable for initial implementation

Cons:

* Generates a large number of `string` objects
* High memory consumption
* Cost of `substr`
* Not suitable for large inputs

---

# Method 2: Dynamic Programming with an `n × m` Matrix

Here, instead of directly generating all substrings, we compare characters of the two strings with each other.

Assume:

```text
str01 → length n
str02 → length m
```

We create a matrix with dimensions:

```text
n × m
```

Each cell in the matrix specifies **the length of the longest common substring that ends at `str01[i]` and `str02[j]`.**

---

## 🔑 Core Relation

If the character at index i of the first string equals the character at index j of the second string:

```cpp
str01[i] == str02[j]
```

Then:

```cpp
dp[i][j] = dp[i - 1][j - 1] + 1;
```

Why?

Because if the two current characters are equal, we can extend the previous common substring located at the top-left diagonal by one character.

However, if:

```cpp
str01[i] != str02[j]
```

Then:

```cpp
dp[i][j] = 0;
```

Because substrings must be **contiguous**.

---

## 🔍 A Simple Example

Suppose:

```text
str01 = "abc"
str02 = "xbc"
```

If we compare characters `b` and `b`, and the top-left diagonal has a value of `1`:

```text
dp[i][j] = dp[i-1][j-1] + 1
         = 1 + 1
         = 2
```

Meaning up to this point, we have a common substring of length `2`.

---

# 📊 Why the Top-Left Diagonal?

Suppose:

```text
str01 = "abcd"
str02 = "xbcd"
```

When:

```text
c == c
```

To know whether `c` is a continuation of a previous common substring, we must inspect the top-left diagonal cell.

```text
        x   b   c   d
      ┌────────────────
a     │ 0   0   0   0
b     │ 0   1   0   0
c     │ 0   0   2   0
d     │ 0   0   0   3
```

Values:

```text
1 → length 1
2 → length 2
3 → length 3
```

indicate that a contiguous common substring is growing.

---

# 🏆 Finding the Longest Common Substring

One of the main applications of this matrix is finding the **Longest Common Substring**.

While filling the matrix, we keep track of the maximum value:

```cpp
maxLength = max(maxLength, dp[i][j]);
```

Finally:

```text
maxLength
```

will be the length of the longest common substring.

To extract the substring itself, we only need to store the position of the maximum value.

If the maximum value is located at:

```text
dp[i][j]
```

The target substring will be:

```cpp
str01.substr(
    i - maxLength + 1,
    maxLength
);
```

---

## ⚠️️ A Very Important Point

Normally, this algorithm is designed to find the:

> **Longest Common Substring**

However, if our goal is to find:

> **All distinct common substrings**

Simply storing one substring for each cell is not enough.

For instance, if the value of a cell is:

```text
dp[i][j] = 3
```

Substrings of the following lengths also end at that position:

```text
length = 1
length = 2
length = 3
```

Therefore, to extract **all common substrings**, we must process all these lengths as well.

---

# 🚀 Dynamic Programming Version for All Common Substrings

In this version, whenever a cell's value equals `length`, we extract all substrings ending at that point:

```cpp
unordered_set<string> method2(
    const string& str01,
    const string& str02
)
{
    int n = str01.size();
    int m = str02.size();

    vector<vector<int>> dp(
        n,
        vector<int>(m, 0)
    );

    unordered_set<string> results;

    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < m; j++)
        {
            if (str01[i] == str02[j])
            {
                if (i == 0 || j == 0)
                    dp[i][j] = 1;
                else
                    dp[i][j] = dp[i - 1][j - 1] + 1;

                int length = dp[i][j];

                // All substrings ending at str01[i]
                for (int len = 1; len <= length; len++)
                {
                    string sub = str01.substr(
                        i - len + 1,
                        len
                    );

                    results.insert(sub);
                }
            }
        }
    }

    return results;
}
```

Here, an `unordered_set` is still necessary because a common substring might appear at multiple different positions.

---

# 🧮 Complexity of the Dynamic Programming Method

Constructing the matrix is:

```text
O(n × m)
```

However, if we want to extract **all common substrings**, we might generate multiple substrings for each `dp[i][j]` value.

Therefore, the actual time complexity for extracting results can be greater than:

```text
O(n × m)
```

This is an important distinction:

> `O(n × m)` is the DP construction complexity for computing the Longest Common Substring; not necessarily the complexity for extracting all common substrings.

---

# 🧠 Method 3: Reducing Memory Consumption

In the previous method, we stored the full matrix:

```cpp
vector<vector<int>> dp;
```

However, to compute common substring lengths, each cell only needs the **top-left diagonal cell**.

Thus, we can reduce memory consumption.

Idea:

```text
n × m Matrix
     ↓
A 1D Array
```

For this to work, we must carefully choose the iteration order.

---

## ⚠️ Why Iteration Must Be Right-to-Left?

If we update the array from left to right, we might overwrite the value of:

```text
dp[i - 1]
```

before using it.

However, by moving from right to left (end to start), the required value from the previous step is preserved.

---

## Implementation

```cpp
unordered_set<string> method3(
    const string& str01,
    const string& str02
)
{
    int n = str01.size();

    vector<int> dp(n, 0);

    unordered_set<string> results;

    for (int j = 0; j < str02.size(); j++)
    {
        for (int i = n - 1; i >= 0; i--)
        {
            if (str01[i] == str02[j])
            {
                if (i == 0)
                    dp[i] = 1;
                else
                    dp[i] = dp[i - 1] + 1;

                int length = dp[i];

                for (int len = 1; len <= length; len++)
                {
                    string sub = str01.substr(
                        i - len + 1,
                        len
                    );

                    results.insert(sub);
                }
            }
            else
            {
                dp[i] = 0;
            }
        }
    }

    return results;
}
```

To better understand the third algorithm, consider the following two strings:

```cpp
str1 = "rstABCxyz"
str2 = "ijABCk"
```

We build a numerical array equal to the length of the first string (9 cells) with an initial value of `0`. Then, a loop iterates from `0` to the length of the second string, evaluating characters of the second string one by one. Inside this loop, another loop checks all cells of the first string to compare characters between the two strings. The key point is that the inner loop runs **from end to start**.

In the example strings, the longest common substring is `ABC`, and the other characters are not continuations of a common substring. Thus, all substrings within `ABC` like `A`, `B`, `C`, `AB`, `BC`, and `ABC` must be added to `results`.

In this structure, `j` runs from `0` to `5`, providing the reference character from the second string to compare against the first string. Inside this loop, another loop runs for the length of the first string, but from right to left.

At first, `j = 0`! The outer loop reaches character `A` in the second string using `j`. The inner loop moves from the end of the first string to the beginning until it reaches character `A`. Here, both loops match on character `A`, satisfying the equality condition! When the inner loop reaches character `A` in the first string, since `A` matches the current character of the second string and the previous cell's value is `0`, this cell's value becomes `0 + 1` which is `1`. Then, `A` is added to `results`. The inner loop continues, and since other characters do not equal `A`, their cell values become `0`.

Then, the outer loop moves forward by one unit and reaches `B`. Again, the inner loop moves from back to front until it reaches `B` in the first string. Since `B` equals the current character of the second string, its cell value becomes the previous cell's value plus `1`:

```text
dp[4] = dp[3] + 1
      = 1 + 1
      = 2
```

The number `2` means up to this point, two consecutive characters (`AB`) are shared between both strings. Therefore, substrings `B` and `AB` are placed in `results`. During this same pass, since character `A` does not equal `B`, the value of cell `A` (which was previously 1) resets back to `0`.

In essence, this method does not store the full matrix of Method 2; instead, it retains only the necessary info from the previous step in a 1D array and calculates the new step on the same array.

Next, the outer loop advances to `C`. The inner loop moves right-to-left again to reach `C` in the first string. Since `C` matches the current character of the second string, its cell value becomes the previous cell's value plus `1`:

```text
dp[5] = dp[4] + 1
      = 2 + 1
      = 3
```

The number `3` indicates three consecutive characters (`ABC`) are common at this point. Thus, all substrings derived at this step (`C`, `BC`, and `ABC`) are saved into `results`. Meanwhile, the cell for character `B` (previously 2) resets to `0`!

So in this example, the progression of common substring formation looks like this:

```text
A       → 1
AB      → 2
ABC     → 3
```

And from each step, all possible substrings are extracted:

```text
A
B
AB
C
BC
ABC
```

In the remainder of the algorithm, no new common substrings are found.

Therefore, we can say that at each step of iterating through the second string, all matching characters in the first string are identified. If the current character equals the second string's character, its cell value is calculated as the previous cell's value plus `1`. Based on the new value, all common substrings ending at that point are extracted and saved into `results`.

If the common substring is `ABC`, the main loop automatically discovers the following substrings in different steps:

```text
A
B
AB
```

And the task of continuing the loops at step `C` is to discover and register the following substrings:

```text
C
BC
ABC
```

---

# 💾 Memory Consumption Comparison

Matrix method:

```text
O(n × m)
```

Array method:

```text
O(n)
```

Or if reversing traversal direction:

```text
O(min(n, m))
```

Thus, when string lengths are large, memory reduction becomes highly significant.

# 🔬 Complete Implementation with `string_view` for Higher Speed:

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <string_view>
#include <unordered_set>

using namespace std;


// ============================================================
// Convert string_views to strings
// ============================================================

unordered_set<string> toStrings(
    const unordered_set<string_view>& views
)
{
    unordered_set<string> results;

    for (string_view view : views)
        results.emplace(view);

    return results;
}


// ============================================================
// Method 1
// Generate all substrings + unordered_set
// ============================================================

unordered_set<string> method1(
    string_view str01,
    string_view str02
)
{
    unordered_set<string_view> str01_substrings;
    unordered_set<string_view> results;

    // Generate all substrings of the first string
    for (size_t i = 0; i < str01.size(); i++)
    {
        for (size_t j = i + 1; j <= str01.size(); j++)
        {
            str01_substrings.emplace(
                str01.data() + i,
                j - i
            );
        }
    }

    // Check all substrings of the second string
    for (size_t i = 0; i < str02.size(); i++)
    {
        for (size_t j = i + 1; j <= str02.size(); j++)
        {
            string_view sub(
                str02.data() + i,
                j - i
            );

            if (str01_substrings.find(sub)
                != str01_substrings.end())
            {
                results.emplace(sub);
            }
        }
    }

    return toStrings(results);
}


// ============================================================
// Method 2
// Dynamic Programming + Matrix
// ============================================================

unordered_set<string> method2(
    string_view str01,
    string_view str02
)
{
    int n = str01.size();
    int m = str02.size();

    vector<vector<int>> dp(
        n,
        vector<int>(m, 0)
    );

    unordered_set<string_view> results;

    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < m; j++)
        {
            if (str01[i] == str02[j])
            {
                if (i == 0 || j == 0)
                    dp[i][j] = 1;
                else
                    dp[i][j] =
                        dp[i - 1][j - 1] + 1;

                int length = dp[i][j];

                // Extract all substrings
                // ending at this position
                for (int len = 1;
                     len <= length;
                     len++)
                {
                    results.emplace(
                        str01.data() + i - len + 1,
                        len
                    );
                }
            }
        }
    }

    return toStrings(results);
}


// ============================================================
// Method 3
// Dynamic Programming with a One-Dimensional Array
// ============================================================

unordered_set<string> method3(
    string_view str01,
    string_view str02
)
{
    int n = str01.size();

    vector<int> dp(n, 0);

    unordered_set<string_view> results;

    for (size_t j = 0; j < str02.size(); j++)
    {
        // Moving from right to left is essential
        for (int i = n - 1; i >= 0; i--)
        {
            if (str01[i] == str02[j])
            {
                if (i == 0)
                    dp[i] = 1;
                else
                    dp[i] = dp[i - 1] + 1;

                int length = dp[i];

                // Extract all substrings
                // ending at this position
                for (int len = 1;
                     len <= length;
                     len++)
                {
                    results.emplace(
                        str01.data() + i - len + 1,
                        len
                    );
                }
            }
            else
            {
                dp[i] = 0;
            }
        }
    }

    return toStrings(results);
}


// ============================================================
// Print Results
// ============================================================

void printResults(
    const unordered_set<string>& results
)
{
    for (const string& sub : results)
    {
        cout << sub << '\n';
    }
}


// ============================================================
// MAIN
// ============================================================

int main()
{
    string str01 = "abcde";
    string str02 = "xbcde";


    // --------------------------------------------------------
    // Method 1
    // --------------------------------------------------------

    // auto result1 = method1(str01, str02);
    // printResults(result1);


    // --------------------------------------------------------
    // Method 2
    // --------------------------------------------------------

    // auto result2 = method2(str01, str02);
    // printResults(result2);


    // --------------------------------------------------------
    // Method 3
    // --------------------------------------------------------

    auto result3 = method3(
        str01,
        str02
    );

    printResults(result3);

    return 0;
}
```

---

# 🎯 Summary

The three core ideas for this problem:

### 1. Naive Method

```text
Generate → Hash Set → Search
```

We generate all substrings and search for matches using an `unordered_set`.

### 2. Dynamic Programming

```text
Character Comparison
        ↓
    DP Matrix
        ↓
Common Substrings
```

By comparing characters and referencing the top-left diagonal value, we compute the lengths of common substrings.

### 3. Space Optimization

```text
O(n × m)
     ↓
  O(n)
```

By using a 1D array, we eliminate the need to store the entire matrix.

---

## ⭐ Key Takeaway

Always keep the difference between these two problems in mind:

```text
Common Substring
        ≠
Longest Common Substring
```

Standard DP with the recurrence relation:

```cpp
dp[i][j] =
    dp[i - 1][j - 1] + 1
```

is directly suited for **Longest Common Substring**.

However, if the goal is **all distinct common substrings**, we must extract all possible lengths from every `dp[i][j]` value in addition to running DP.

---

## 📌 Animation Showing Method 3
<div align="center">
  <img src="../../../assets/img012-002.gif" alt="Animation" />
</div>

---

## 🤝 Contributions

<div align="center">

| GitHub | LinkedIn | Email | Site | Telegram |
|--------|----------|-------|------|----------|
| [HadiAbbasi](https://github.com/HadiAbbasi) | [Hadi Abbasi](https://www.linkedin.com/in/hadi-abbasi-programmer/) | [Hadi Abbasi](hadi.abbasi.programmer@gmail.com) | [Hiens.org](https://hiens.org) | [Hadi Abbasi](@Hadi_Abbasi_Programmer) |

</div>