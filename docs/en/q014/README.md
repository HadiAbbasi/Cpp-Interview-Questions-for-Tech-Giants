<div align="center">

[🇺🇸 English](./README.md) | [🇮🇷 فارسی](../../fa/q014/README.md)

</div>

---

# Question014: Finding a Specific Pattern in a Text

<div align="center">
  <img src="../../../assets/img014-001.png" alt="Image" />
</div>

## Problem Statement

**Question:** If we want to find a specific string, called `Pattern`, inside a larger string, called `Text`, what approaches are available, and which one is more efficient?

## Problem Definition

Assume that we have a function that receives two strings, `Pattern` and `Text`, as input.

Our goal is to **find every position at which `Pattern` appears completely inside `Text`**. The output is an array of integers, where each integer represents the starting index of a match in `Text`.

Throughout this document, indices are zero-based, just like array and string indexing in C++.

For example, consider the following inputs:

```cpp
Text = "Hello How Are You? I wish All the best for you!"
Pattern = "You"
```

In this example, if the search is **case-insensitive**, the result will be:

```cpp
Result = [14, 43]
```

Note that the text contains `You` once and `you` once. Therefore, the result `[14, 43]` is correct only for a case-insensitive search. For a case-sensitive search with `Pattern = "You"`, only index `14` is found.

Also, spaces and punctuation marks each count as a character and therefore affect the calculated indices.

## Case Sensitivity

Should the algorithm distinguish between uppercase and lowercase letters?

We can introduce a `Boolean` parameter to specify whether the search is case-sensitive. If the search should be case-insensitive, both strings must be normalized in the same way; for example, by converting both to lowercase.

This operation must take the language and `Unicode` rules into account. A simple lowercase conversion is not necessarily sufficient for every language or environment. However, for simple examples and English text, a method such as `ToLower` can be used, provided that both strings are processed in exactly the same way.

For now, this is not our main focus. We want to explore the different ways to find `Pattern` inside `Text` and how to improve the algorithm's execution speed.

---

## Method 1: Naive String Search

In the simplest approach, we start at the beginning of `Text` and consider each position, one at a time, as a possible starting position for `Pattern`. We then compare the characters of `Pattern` with the corresponding characters in `Text`, one by one.

If the length of `Pattern` is greater than the length of `Text`, a match is obviously impossible. Otherwise, the last index in `Text` from which we can start a search is:

```cpp
int endIndex = text.length() - pattern.length();
```

Why? If we start searching at `endIndex`, the last character of `Pattern` will align exactly with the last character of `Text`. Starting at any larger index would cause part of `Pattern` to extend beyond the end of `Text`.

Therefore, we must examine every position from index `0` through `endIndex` as a possible starting position. At each position, we first compare `Text[i]` with `Pattern[0]`. If they are equal, we move on to the next characters:

```text
Text[i]     == Pattern[0]
Text[i + 1] == Pattern[1]
Text[i + 2] == Pattern[2]
...
```

Two situations are possible:

1. **Full Match:** If every character in `Pattern` matches the corresponding character in `Text`, we add the starting index, `i`, to `Result`. We then continue searching from the next starting position, `i + 1`. This ensures that overlapping matches are not missed.
2. **Mismatch:** If we encounter a character that does not match its corresponding character in `Text` before reaching the end of `Pattern`, we discard the current position, `i`, and continue searching from `i + 1`.

The following code is a simple C++ implementation of this approach. Here, we assume that `Pattern` is not empty. In production code, we must also handle an empty `Pattern` and the case where its length is greater than the length of `Text`.

```cpp
vector<int> result;

int tlen = static_cast<int>(text.length());
int plen = static_cast<int>(pattern.length());

if (plen == 0 || plen > tlen) {
    return result;
}

for (int ti = 0; ti <= tlen - plen; ti++) {
    int pi = 0;

    while (pi < plen && text[ti + pi] == pattern[pi]) {
        pi++;
    }

    if (pi == plen) {
        result.push_back(ti);
    }
}
```

This method is simple and easy to understand, but for some inputs it repeats many comparisons.

For example, suppose `Pattern` contains many `A` characters followed by a `B` at the end. If `Text` also contains a long sequence of `A` characters, the algorithm will recheck many of those same `A` characters at every position. Eventually, it reaches `B`, and the comparison fails.

This is where the **KMP algorithm** comes in. It was designed to avoid precisely these repeated comparisons.

## The KMP Algorithm (Knuth–Morris–Pratt)

The KMP algorithm is named after the surnames of the three researchers who introduced it:

1. Donald Ervin Knuth
2. James Hiram Morris
3. Vaughan Ronald Pratt

The algorithm's name is formed from the initial letters of their surnames:

`Knuth – Morris – Pratt`

The algorithm was introduced in 1977. Let `N` be the length of `Text` and `M` the length of `Pattern`. Building the `LPS` array takes `O(M)` time, and searching takes `O(N)` time. Therefore, the overall time complexity is `O(N + M)`.

## The Main Idea Behind KMP

As we saw in the naive search method, when part of `Pattern` matches part of `Text` but we then encounter the first mismatching character, we move to the next starting position in `Text` and begin comparing again from the first character of `Pattern`.

The problem with this approach is that, at every step, we may repeat many comparisons that we have already performed.

This is where KMP comes into play.

The main idea behind KMP is very clever:

**When a mismatch occurs, why should we throw away the information we gained from previous comparisons and start all over again?**

In fact, KMP determines how far we can move `Pattern` to the right along `Text` after a mismatch without missing any valid possible starting position. This shift is based on information we already have about the characters that matched.

To do this, before searching begins, we build an array called `LPS` from `Pattern` itself. For each prefix of `Pattern`, this array tells us the length of the longest substring that is both a `Prefix` and a `Suffix` of that prefix.

Therefore, unlike the naive approach, after every mismatch we do not need to restart the comparison from the first character of `Pattern` or re-examine previously processed characters in `Text`.

In other words, KMP uses information from previous matches to avoid repeated comparisons and scan `Text` more efficiently.

In the standard KMP implementation, the `Text` index only moves forward; it never moves backward. When a mismatch occurs, it is usually only the `Pattern` index that is moved to a more appropriate position using the `LPS` array.

### Time Complexity Comparison

In the naive approach, in the worst case, we may examine almost every character in `Pattern` for each possible starting position. Therefore, the worst-case time complexity is:

`O((N - M + 1) × M)`

This is also commonly expressed as `O(N × M)`.

For KMP:

- Time to build `LPS`: `O(M)`
- Time to search `Text`: `O(N)`
- Total time: `O(N + M)`
- Auxiliary space used by the `LPS` array: `O(M)`

The main advantage of KMP, then, is that it avoids unnecessary repeated comparisons by using `O(M)` auxiliary space.

---

## Understanding LPS in the KMP Algorithm

The full name of this concept is **Longest Proper Prefix which is also a Suffix**.

For each index `i`, `LPS[i]` is the length of the longest `Proper Prefix` of the substring `Pattern[0..i]` that is also a `Suffix` of that same substring.

The name may seem a little complicated at first, but the idea itself is quite simple. Let's examine it step by step.

### Step 1: What Do Prefix and Suffix Mean?

Suppose we have the following string:

```cpp
Pattern = "ABABC"
```

Any string that starts at the beginning of another string is called a `Prefix`.

For example, the prefixes of the string above are:

```text
A
AB
ABA
ABAB
ABABC
```

On the other hand, any string that ends at the end of the original string is called a `Suffix`.

Therefore, the suffixes of this string are:

```text
C
BC
ABC
BABC
ABABC
```

### Step 2: What Is a Proper Prefix?

The word `Proper` adds an important condition:

A `Prefix` must not be equal to the entire original string.

Therefore, in the example above, we remove `ABABC` from the set of prefixes.

Similarly, a `Proper Suffix` is a suffix that is not equal to the entire original string.

**Note:** In the definition of LPS, both the prefix and the suffix must be proper; neither can be the entire substring. Also, we are looking for a non-empty common prefix and suffix. If no such string exists, the LPS value is zero.

### Step 3: Finding a Common Prefix and Suffix

Now let's check whether the string `ABABC` has a common prefix and suffix. We consider only proper prefixes and proper suffixes, leaving out the entire string itself.

Proper prefixes:

```text
A
AB
ABA
ABAB
```

Proper suffixes:

```text
C
BC
ABC
BABC
```

We cannot find any non-empty prefix that is also a suffix. Therefore, the length of the longest common one is zero:

```text
LPS("ABABC") = 0
```

Now consider another example:

```cpp
Pattern = "ABAB"
```

Proper prefixes:

```text
A
AB
ABA
```

Proper suffixes:

```text
B
AB
BAB
```

The string `AB` appears in both the prefix and suffix sets. So we have found a common prefix and suffix.

Is there another common string with a greater length? No.

Therefore, the length of the longest proper prefix that is also a suffix is 2:

```text
LPS("ABAB") = 2
```

The value 2 means that, in the substring `ABAB`, the longest `Proper Prefix` that is also a `Suffix` is `AB`, which has two characters.

### Step 4: Calculating LPS for Every Index

So far, we have examined LPS for an entire string. However, the `LPS` array calculates the same concept at every index, using the substring that starts at the beginning of `Pattern` and ends at that index.

In other words:

`LPS[i]` answers the following question:

What is the length of the longest `Proper Prefix` of `Pattern[0..i]` that is also a `Proper Suffix` of that substring?

For example, suppose `Pattern = "AAAA"`:

- For the substring `A`, there is no non-empty common proper prefix and suffix, so `LPS[0] = 0`.
- For the substring `AA`, the common prefix and suffix is `A`, so `LPS[1] = 1`.
- For the substring `AAA`, the longest common prefix and suffix is `AA`, so `LPS[2] = 2`.
- For the substring `AAAA`, the longest common prefix and suffix is `AAA`, so `LPS[3] = 3`.

Therefore:

```text
Pattern: A A A A
LPS:     0 1 2 3
```

In the same way, for a string consisting of consecutive `A` characters, the LPS value increases by one at each step, as long as the following characters allow it.

### How Does LPS Help KMP?

Suppose part of `Pattern` has matched part of `Text`, but we then encounter a mismatching character.

With the naive method, we would have to start searching from the next position in `Text` and compare again from the beginning of `Pattern`.

But KMP says:

Wait! We have already compared some characters of `Pattern` with `Text`, and we know that they were equal. Why not use that information?

If the matched portion of `Pattern` contains a common prefix and suffix, we can align the beginning of `Pattern` with the end of that matched portion. Since we already know these two parts are equal, we do not need to compare them with `Text again.

Suppose `j` characters of `Pattern` have matched `Text` before a mismatch occurs. In this situation, the last matched character is at index `j - 1` in `Pattern`. Therefore, we examine:

```cpp
lps[j - 1]
```

This value tells us the length of the longest prefix that is equal to a suffix of the matched portion. For this reason, we can set the `Pattern` index to this value:

```cpp
j = lps[j - 1];
```

**Why `j - 1`?** Because `j` is the number of matched characters, not the index of the last matched character. If the first `j` characters of `Pattern` have matched, their last character is at index `j - 1`. Therefore, to access the LPS value for that matched portion, we must use `lps[j - 1]`. This is not simply subtracting one to compensate for zero-based indexing; the actual reason is that the last matched character is located at index `j - 1`.

Also note that the LPS value is **not the distance by which `Pattern` is shifted**. It is the length of the prefix that we can preserve as an already-matched portion. If we want to calculate the actual shift distance, it is:

```cpp
int shift = j - lps[j - 1];
```

However, the KMP implementation does not need to calculate this value. We can simply set the `Pattern` index directly to the LPS value. The `Text` index does not change at this point, because the current character in `Text` has not yet been compared with the new position in `Pattern`.

### An Example to Understand the Shift

Suppose `Pattern` is:

```text
Pattern: A B A B C
Index:   0 1 2 3 4
LPS:     0 0 1 2 0
```

Assume that the first four characters of `Pattern` have matched `Text`, but a mismatch occurs at the fifth character.

At this moment:

```cpp
j = 4;
```

The last matched character is at index `j - 1`, which is index 3. Its LPS value is 2:

```cpp
lps[j - 1] = lps[3] = 2;
```

This tells us that the last two characters of the matched portion are equal to the first two characters of `Pattern`:

```text
Matched portion:  A B A B
Common prefix:    A B
Common suffix:        A B
```

Therefore, we do not need to compare the first two characters of `Pattern` with `Text` again. We set the `Pattern` index to 2:

```cpp
j = lps[j - 1]; // j = 2
```

The comparison now continues between `Pattern[2]` and the same current character in `Text`.

If we imagine physically shifting `Pattern` over `Text`, this is equivalent to moving `Pattern` two positions to the right, because `4 - 2 = 2`. In the code, however, instead of physically moving the string or changing the `Text` index, we simply update the `Pattern` index.

### A Larger Example of Shifting in KMP

In the following example, we will look at a `Pattern` string and its corresponding LPS array. The `Text` string is also shown below. Note that `Text` does not end at `W`; it continues beyond that point.

**Pattern and its LPS array:**

<!-- Insert the first table image here. -->

The `Pattern` string in this example is:

```text
AAAAABCDEAAAAAAAAAXT
```

And `Text` begins as follows:

```text
AAAAABCDEAAAAAAAAAZW...
```

We use two variables to build LPS:

- `i`: The index in `Pattern` for which we want to calculate the LPS value.
- `len`: The length of the best common prefix/suffix found so far.

The value of `lps[0]` is always zero, because a single-character string has no non-empty proper prefix.

For example, if `lps[i]` is 3, it means that the first three characters of `Pattern` are equal to the last three characters of the substring `Pattern[0..i]`. This number is the length of that section, not an index and not the number of characters that should be removed from `Text`.

Now let's return to the search. In this example, the first 18 characters of `Pattern` match `Text`, but the next character in `Pattern`, `X`, does not match the character `Z` in `Text`.

Since `lps[17]` is 5, we use the LPS information at this point instead of restarting the search at the next position in `Text` and comparing from the first character of `Pattern`.

So far, indices 0 through 17 in `Text` and `Pattern` have matched. Therefore, the current `Text` index is 18, and the `Pattern` index has also reached 18 after 18 matches. The last matched character is at index 17, and `lps[17]` is 5.

This means that the first five characters of `Pattern` are equal to the last five characters of the matched portion. Therefore, we can shift `Pattern` to the right so that its beginning aligns with those same five trailing characters.

The new starting position of `Pattern` is:

```text
18 - 5 = 13
```

Thus, instead of restarting the search at starting position 1 in `Text`, we move directly to the next possible starting position, 13.

**Key question:** How can we be sure that we have not missed any possible starting position between position 0 and position 13?

**Answer:** Because `LPS` gives us the length of the longest common prefix and suffix. If there were another common prefix and suffix with a length greater than 5, the LPS value for that portion would be greater than 5. If a possible match existed at a closer starting position, there would have to be a longer border in the matched portion. But LPS has already given us the longest border. Therefore, shifting by `18 - 5` does not skip any valid starting position.

<!-- Insert the second table image here. -->

In this way, when a match fails, we shift `Pattern` by the amount indicated by LPS and then check the next comparison.

### Comparing KMP's Performance in This Example

Without KMP, after the match failed at starting position zero, we would have to move the starting position forward by one and begin comparing again from the start of `Pattern`. If that attempt failed, we would repeat the process at the next starting position.

However, when KMP encounters a mismatch at index 18, it uses `lps[17] = 5` to move the next possible starting position directly to 13. In other words, it skips positions 1 through 12 instead of checking them again.

---

## C++ Implementation of the KMP Algorithm

The following code first builds the LPS array and then uses it to find every starting position of `Pattern` in `Text`.

```cpp
#include <iostream>
#include <string>
#include <vector>

using namespace std;

// lps[pi]: Length of the longest proper prefix of pat[0..pi]
// that is also a suffix of pat[0..pi].
vector<int> buildLPS(const string& pat) {
    int plen = static_cast<int>(pat.size());
    vector<int> lps(plen, 0);

    int len = 0; // Length of the current matching border.
    int pi = 1;  // lps[0] is already initialized to zero.

    while (pi < plen) {
        if (pat[pi] == pat[len]) {
            // Extend the current matching prefix and suffix.
            len++;
            lps[pi] = len;
            pi++;
        } else {
            if (len != 0) {
                // Try a shorter border without advancing pi.
                len = lps[len - 1];
            } else {
                // No non-empty border exists.
                lps[pi] = 0;
                pi++;
            }
        }
    }

    return lps;
}

vector<int> KMPsearch(const string& txt, const string& pat) {
    vector<int> result;

    int tlen = static_cast<int>(txt.size());
    int plen = static_cast<int>(pat.size());

    // An empty pattern is treated as having no matches.
    if (plen == 0 || plen > tlen) {
        return result;
    }

    vector<int> lps = buildLPS(pat);

    int ti = 0; // Current index in the text.
    int pi = 0; // Current index in the pattern.

    while (ti < tlen) {
        if (txt[ti] == pat[pi]) {
            ti++;
            pi++;

            if (pi == plen) {
                // The entire pattern has been found.
                result.push_back(ti - pi);

                // Continue searching for overlapping matches.
                pi = lps[pi - 1];
            }
        } else {
            if (pi != 0) {
                // Keep the text index unchanged and shift the pattern
                // using the LPS array.
                pi = lps[pi - 1];
            } else {
                // No prefix match is possible, so advance the text index.
                ti++;
            }
        }
    }

    return result;
}

int main() {
    string txt = "aabaacaadaabaaba";
    string pat = "aaba";

    vector<int> res = KMPsearch(txt, pat);

    for (int index : res) {
        cout << index << " ";
    }

    // Expected output: 0 9 12

    return 0;
}
```

## Code Explanation

### The `main` Function

In `main`, we create a `Text` and a `Pattern`, then pass both to `KMPsearch`. The function returns a list of all starting indices where `Pattern` matches in `Text`.

In the following loop, we iterate over the elements of the returned array and print them.

### The `KMPsearch` Function

Inside `KMPsearch`, we first obtain the lengths of `Text` and `Pattern`:

```cpp
int tlen = static_cast<int>(txt.size());
int plen = static_cast<int>(pat.size());
```

If `Pattern` is empty or its length is greater than the length of `Text`, no match can be found, so we return the empty result array:

```cpp
if (plen == 0 || plen > tlen) {
    return result;
}
```

Next, we build the LPS array for `Pattern`:

```cpp
vector<int> lps = buildLPS(pat);
```

We now have two indices:

- `ti`: The current index in `Text`.
- `pi`: The current index in `Pattern`.

The main loop continues until the `Text` index reaches the end of the string:

```cpp
while (ti < tlen)
```

At each step, we compare the current character in `Text` with the current character in `Pattern`.

**Case 1: The characters match**

If the two characters are equal, we advance both indices by one:

```cpp
ti++;
pi++;
```

We then check whether the number of matched characters has become equal to the length of `Pattern`:

```cpp
if (pi == plen)
```

If so, the entire `Pattern` has been found. The starting index of this match is `ti - pi`, because after the match `ti` points to the position immediately after the last matched character, while `pi` represents the number of matched characters in `Pattern`.

We add this starting index to the result array:

```cpp
result.push_back(ti - pi);
```

But we are not finished yet. There may be another match that overlaps with the current one. For example, repeated consecutive characters may allow another match to be found by moving backward from the current `ti` position. We must not miss such matches. To handle them, we use LPS:

```cpp
pi = lps[pi - 1];
```

In other words, we use the length of the longest prefix that is equal to a suffix of the matched portion as the new number of already-matched characters. However, we do not move the `Text` index backward.

**Case 2: The characters do not match**

If the two characters are different, there are two possibilities:

1. If `pi` is not zero, some characters of `Pattern` matched `Text` before this mismatch. Therefore, we do not need to change the `Text` index. We use LPS to move the `Pattern` index to an appropriate position:

   ```cpp
   pi = lps[pi - 1];
   ```

   Pay attention to what happens here: when we encounter a mismatch at some position in the original text, we keep `ti` at that exact mismatch position and move the comparison point within `Pattern`. It is as though we are moving the `Pattern` template backward over the mismatch position by an amount that allows us to consider whether the beginning of `Pattern` might match the characters that appeared before the current `ti` position. Then, in the next loop iteration, the comparison is performed again. We either continue matching from the new position in `Pattern`, or eventually determine that no match is possible at this alignment and advance the main `Text` index.

   In this case, the current character in `Text` has not yet been examined against the new `Pattern` position. Therefore, we compare that same `Text` character again, this time with the new position in `Pattern`.

2. If `pi` is zero, even the first character of `Pattern` does not match the current character in `Text`. There is no matched prefix that we can use to continue with LPS. Therefore, we advance the `Text` index by one:

   ```cpp
   ti++;
   ```

Finally, after reaching the end of `Text`, we return the array containing all the positions that were found.

### Why Does the Main Loop Continue to the End of Text Instead of Stopping at `tlen - plen`?

In the naive search, we needed to examine starting positions from index 0 through `tlen - plen`. But KMP works differently.

Consider the following example:

```text
txt = "xxxxABCD";
pat = "ABCD";
```

The lengths of the text and pattern are:

```text
tlen = 8
plen = 4
tlen - plen = 4
```

Therefore, the last possible position at which a **complete match can start** is index `4`.

However, remember that to find a match starting at index 4, we must examine the characters at indices 4, 5, 6, and 7.

**Index-by-index view**

```text
Character: x x x x A B C D
Index:     0 1 2 3 4 5 6 7
```

Index 4 is the last possible starting position, but we must continue through index 7 to complete the match.

If we wrote the loop condition like this:

```cpp
while (ti <= tlen - plen)
```

in this example, the loop would continue only while `ti` is equal to or less than 4. As soon as we read the character `A`, `ti` becomes 5 and the loop terminates!

As a result, we would never get the chance to examine `B`, `C`, and `D`, and we would miss the complete match.

### The Key Difference

In naive search:

```cpp
for (int i = 0; i <= tlen - plen; ++i)
```

The variable `i` represents the **possible starting position of Pattern**.

But in KMP:

```cpp
while (ti < tlen)
```

The variable `ti` represents the **current character in Text that must be processed**.

These two variables do not serve the same purpose.

### The `buildLPS` Function

The purpose of this function is to build the LPS array for `Pattern`.

First, we create an array whose length is equal to the length of `Pattern` and initialize all its elements to zero:

```cpp
vector<int> lps(plen, 0);
```

We then use two variables:

- `pi`: The index in `Pattern` for which we are currently calculating the LPS value.
- `len`: The length of the common prefix and suffix currently known for the substring being examined.

Since `lps[0]` is always zero, we start the calculation at index 1:

```cpp
int len = 0;
int pi = 1;
```

At each step, we compare `pat[pi]` with `pat[len]`.

**Case 1: The two characters are equal**

If the characters are equal, we can extend the common prefix and suffix by one character. Therefore, we increment `len` and store the new value in the corresponding LPS entry:

```cpp
len++;
lps[pi] = len;
pi++;
```

This calculates the LPS value for the current index, and we move on to the next index.

**Case 2: The two characters are not equal**

There are two possibilities:

1. If `len` is greater than zero, we already have a non-empty common prefix and suffix. Therefore, we do not need to reset `len` to zero immediately. Instead, we first try a shorter border:

   ```cpp
   len = lps[len - 1];
   ```

   This instruction reduces the length of the current border to the length of its longest shorter border. Notice that we do not advance `pi` at this point, because we still need to compare the current character in `Pattern` with the shorter prefix.

2. If `len` is zero, there is no non-empty common prefix and suffix that we can continue with. Therefore, the LPS value at the current index is zero, and we move to the next index:

   ```cpp
   lps[pi] = 0;
   pi++;
   ```

Finally, after all indices in `Pattern` have been processed, we return the LPS array.

---

## Summary

The KMP algorithm uses the information stored in the LPS array to avoid unnecessary repeated comparisons.

The most important point is that LPS gives us the length of the longest prefix that is also a suffix of the portion examined. With this information, after a mismatch we do not need to restart the search from the beginning of `Pattern`.

In the KMP implementation:

- The `Text` index (`ti`) never moves backward.
- The `Pattern` index (`pi`) is moved to an appropriate position using the LPS array.
- Overlapping matches can also be detected by resetting `pi` after a complete match.
- The overall time complexity is `O(N + M)`, and the auxiliary space complexity is `O(M)`.
 

------

## 🤝 Contributors

<div align="center">

| GitHub | LinkedIn | Email | Site | Telegram |
|--------|----------|-------|------|----------|
| [HadiAbbasi](https://github.com/HadiAbbasi) | [Hadi Abbasi](https://www.linkedin.com/in/hadi-abbasi-programmer/) | [Hadi Abbasi](hadi.abbasi.programmer@gmail.com) | [Hiens.org](https://hiens.org) | [Hadi Abbasi](@Hadi_Abbasi_Programmer) |

</div>