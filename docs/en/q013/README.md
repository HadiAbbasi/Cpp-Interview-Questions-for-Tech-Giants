<div align="center">

[🇺🇸 English](./README.md) | [🇮🇷 فارسی](../../fa/q013/README.md)

</div>

---
# Question 013: Finding the Longest Palindromic Substring

<div align="center">
  <img src="../../../assets/img013-001.png" alt="Image" />
</div>

## Problem Statement

In this problem, we are asked to find the longest palindromic substring within a given input string as efficiently as possible.

Before diving into the algorithm, however, we need to understand what a palindrome is and how to recognize one.

## 1. What Is a Palindrome?

A **palindrome** is a string that reads the same from left to right as it does from right to left.

For example, the following strings are palindromes:

```text
BACDCAB
ZDDZ
ABBA
AHA
```

However, `ABCD` is not a palindrome because its reverse is `DCBA`.

### What Is a Substring?

A **substring** is a contiguous sequence of characters within a string.

For example, `ANA` is a substring of `BANANA`, but `BNA` is not, because its characters do not appear consecutively in the original string.

Our goal is to find the longest contiguous part of the input string that is also a palindrome. If multiple palindromic substrings share the maximum length, we may return any one of them.

## 2. How Can We Check Whether a String Is a Palindrome?

Do we need to create a reversed copy of a string and compare it with the original to determine whether it is a palindrome?

No! We can do this without creating a reversed copy.

Simply compare characters from opposite ends of the string:

- Compare the first character with the last character.
- Compare the second character with the second-to-last character.
- Compare the third character with the third-to-last character.
- Continue until we reach the middle of the string.

If all corresponding characters match, the string is a palindrome.

For example:

```text
B A C D C A B
|           |
```

First, we compare `B` with `B`. Then, we compare `A` with `A`, followed by `C` with `C`. Finally, we reach the middle character, `D`.

Since every comparison succeeds, the string is a palindrome.

### C++ Implementation

We can use `std::equal` to implement this check:

```cpp
#include <algorithm>
#include <string>

bool isPalindrome(const std::string& str)
{
    return std::equal(
        str.begin(),
        str.begin() + str.size() / 2,
        str.rbegin()
    );
}
```

Here, `str.begin()` points to the beginning of the string, while `str.rbegin()` starts traversing it in reverse.

Since we only need to compare the first half of the string with the corresponding characters in the reversed string, the number of comparisons is approximately half the string's length.

We can also implement the same logic more explicitly:

```cpp
#include <string>

bool isPalindrome(const std::string& str)
{
    std::size_t left = 0;
    std::size_t right = str.size();

    while (left < right)
    {
        --right;

        if (str[left] != str[right])
            return false;

        ++left;
    }

    return true;
}
```

In this version, `left` moves to the right while `right` moves to the left. As soon as two corresponding characters differ, we can immediately conclude that the string is not a palindrome.

An important detail is that an empty string or a single-character string is also considered a palindrome, since there are no mismatching characters.

The worst-case time complexity of this function is \(O(n)\), and its auxiliary space complexity is \(O(1)\).

This approach is suitable for checking whether an entire string is a palindrome, but our actual problem is slightly different.

We do not merely want to determine whether the entire string is a palindrome. We want to find its **longest palindromic substring**.

---

## 3. Odd-Length and Even-Length Palindromes

Palindromes fall into two categories based on their length.

### Odd-Length Palindromes

Consider the following example:

```text
BACDCAB
```

This string contains seven characters, with a single character, `D`, at its center:

```text
B A C D C A B
      ^
    Center
```

### Even-Length Palindromes

Now consider:

```text
ABBA
```

This string contains four characters, and its center lies between the two `B` characters:

```text
A B | B A
    ^
  Center
```

If we consider only the characters themselves as potential centers, we can find odd-length palindromes. However, we must also consider the gaps between characters to find even-length palindromes.

This distinction is important when designing the algorithm.

Manacher's algorithm uses a simple transformation to handle both cases within a unified structure.

## 4. Transforming the String to Unify Palindrome Centers

The main idea is to insert a separator between every pair of characters. We also insert separators at the beginning and end of the string to make the structure consistent.

For example, consider the string:

```text
BANANA
```

After transformation, it becomes:

```text
#B#A#N#A#N#A#
```

Now, let's see how this transformation handles both even-length and odd-length palindromes.

### Even-Length Palindrome

Original string:

```text
ABBA
```

Transformed string:

```text
#A#B#B#A#
```

The center of the transformed palindrome is the separator between the two `B` characters.

### Odd-Length Palindrome

Original string:

```text
ABA
```

Transformed string:

```text
#A#B#A#
```

The center of the transformed palindrome is the character `B`.

Therefore, we no longer need to handle two different types of centers separately. Every palindrome in the transformed string has a well-defined center.

### Why Does This Transformation Work?

Suppose an original palindrome contains \(k\) characters.

In the transformed string, we have those \(k\) characters plus \(k-1\) separators between consecutive characters.

Therefore, the corresponding portion of the transformed string has length:

\[
k+(k-1)=2k-1
\]

The value \(2k-1\) is always odd.

If we also insert separators at the beginning and end of the string, two additional separators are added. The total length remains odd:

\[
(2k-1)+2=2k+1=2(k+1)-1
\]

As a result, we no longer need to worry about whether the original palindrome has an odd or even length. All centers are represented consistently, either by original characters or by separators.

**Implementation note:** Using `#` is convenient for illustrative examples, but if the input string itself contains `#`, the separator could be confused with an actual character.

In the final implementation, we represent the transformed string as an array of integers and use `-1` as the separator. Since `unsigned char` values range from 0 to 255, `-1` cannot collide with any of them.

## 5. The Simple Approach: Expanding Around Every Center

After transforming the string, we can treat every position as a potential palindrome center.

For each center:

1. Move one position to the left and one position to the right.
2. If the two values match, increase the palindrome radius by one.
3. Move one position farther in each direction.
4. Continue until the values differ or we reach either end of the array.

For example, consider the following transformed string:

```text
#A#B#B#A#
```

The center of the longest palindrome is the separator between the two `B` characters.

Expanding around this center produces the following comparisons:

```text
B == B
# == #
A == A
# == #
```

Every comparison succeeds, so the entire string `ABBA` is a palindrome.

### Implementing the Simple Approach

```cpp
#include <string>
#include <vector>

std::vector<int> palindromeRadiiSimple(
    const std::string& str)
{
    std::vector<int> transformed;
    // Character codes interleaved with separators (-1).
    transformed.reserve(str.size() * 2 + 1);

    for (unsigned char c : str)
    {
        transformed.push_back(-1);
        transformed.push_back(static_cast<int>(c));
    }

    transformed.push_back(-1);

    const int n = static_cast<int>(transformed.size());
    std::vector<int> p(n, 0);
    // p[i] stores the palindrome radius around position i.

    for (int i = 1; i < n - 1; ++i)
    {
        int left = i - 1;
        int right = i + 1;

        while (left >= 0 &&
               right < n &&
               transformed[left] == transformed[right])
        {
            ++p[i];
            --left;
            ++right;
        }
    }

    return p;
}
```

In this implementation, the array `p` stores the palindrome radius for every position.

The value `p[i]` tells us how many positions we can expand from center `i` in both directions while the corresponding values continue to match.

With this representation, the palindrome radius in the transformed string is exactly the length of the corresponding palindrome in the original string.

Finally, we can inspect the radius array, find the position with the largest radius, and extract the corresponding substring from the original string.

### What Is the Problem with This Approach?

Suppose the input string contains 1,000 characters.

In the worst case, we may need to examine a large portion of the string again for every center.

The problem is that we repeatedly examine parts of the string even when we have already obtained information that could help us determine whether those parts are palindromes.

The worst-case time complexity of this approach is:

\[
O(n^2)
\]

To solve the problem more efficiently, we need to reuse information about palindromes we have already discovered.

This is exactly the idea behind Manacher's algorithm.

---

## 6. The Core Idea Behind Manacher's Algorithm: The Mirror Property

Imagine a large palindrome whose center is at position `center` and whose radius is `p[center]`.

In this article, we define `right` as the **exclusive right boundary**: the first position immediately outside the known palindrome.

Therefore, if the palindrome has center `center` and radius `p[center]`, its range is:

```text
[center - p[center], center + p[center] + 1)
```

The right boundary is exclusive, meaning that the position equal to `right` is not included in the palindrome.

For example, suppose the palindrome's center is at position 20 and its radius is 8.

The palindrome extends from position 12 through position 28, inclusive. Its exclusive right boundary is therefore 29:

```text
        12                 20                  29
         |------------------|-------------------)
       Left               Center             Right
```

Since this segment is a palindrome, every position to the left of the center has a corresponding mirrored position to its right.

For example:

- Position 19 is the mirror of position 21.
- Position 18 is the mirror of position 22.
- Position 17 is the mirror of position 23.
- Position 16 is the mirror of position 24.

This symmetry allows us to reuse information previously computed for one position when processing its mirror.

### A Concrete Example

Suppose a large palindrome has center 20 and exclusive right boundary 29.

Now, assume we are processing position 23.

The mirror of position 23 with respect to center 20 is calculated using:

```cpp
mirror = 2 * center - i;
```

The formula comes from the following reasoning: calculate the distance from `i` to the center, then move the same distance in the opposite direction to find its mirror.

\[
\begin{aligned}
mirror &= center-(i-center)\\
       &= 2\times center-i
\end{aligned}
\]

Therefore:

```text
mirror = 2 * 20 - 23 = 17
```

<div align="center">
  <img src="../../../assets/img013-002.png" alt="Image" />
</div>

Suppose we previously calculated the palindrome radius at position 17 as 6.

Can we immediately conclude that the palindrome radius at position 23 is also 6?

Not necessarily!

The known palindrome extends only as far as its exclusive right boundary, 29. The distance from position 23 to that boundary is:

```text
right - i = 29 - 23 = 6
```

The mirror information alone can guarantee a radius of at most 6. If the mirror's radius is larger than 6, we cannot assume that the current position also has that larger radius without additional comparisons.

The initial radius is therefore bounded by both the mirror's radius and the distance to the known boundary.

Using our exclusive-boundary convention, the implementation uses:

```cpp
p[i] = std::min(right - i - 1, p[mirror]);
```

The `-1` is important: the known palindrome's last included position is `right - 1`, and the algorithm must not assume anything about positions beyond that boundary.

### Why Do We Sometimes Need Additional Comparisons?

As we have seen, the mirror property lets us initialize the radius at position 23 using the information from position 17.

In our example, the initial radius is 5 if the exclusive boundary is 28, because:

```text
right - i - 1 = 28 - 23 - 1 = 4
```

If the exclusive boundary is 29, the corresponding value is 5:

```text
right - i - 1 = 29 - 23 - 1 = 5
```

This means the algorithm can skip comparisons for the portion already covered by the known palindrome and continue expanding from the boundary of that known region.

But why do we still need the `while` loop after this jump?

Because the known palindrome only provides information up to its boundary. We do not yet know whether the characters beyond that boundary will continue to match.

After initializing the radius, we must resume comparing characters one pair at a time until either:

- We reach the end of the transformed string, or
- We encounter the first pair of mismatching values.

If these additional comparisons allow the palindrome centered at position 23 to extend beyond the previously known boundary, we update `center` and `right` so that subsequent positions can benefit from the newly discovered palindrome.

**In short:** The mirror property lets us skip redundant work, but we must still perform actual comparisons to discover new territory beyond the known boundary.

## 7. Two Important Variables in Manacher's Algorithm

To use the mirror property, we maintain two variables:

- `center`: the center of the palindrome that currently reaches the farthest to the right.
- `right`: the first position outside that palindrome, i.e., its exclusive right boundary.

If the current position `i` is smaller than `right`, it lies inside the known rightmost palindrome, so we can use the mirror property.

If `i` is equal to or greater than `right`, we initialize its radius to zero because no previously known palindrome provides useful information for that position.

### When Should We Update `center` and `right`?

After calculating the radius at the current center, we compute the new exclusive right boundary:

```cpp
newRight = i + p[i] + 1;
```

If this value is greater than the current `right`, the newly discovered palindrome extends farther to the right.

In that case, we update both variables:

```cpp
center = i;
right = i + p[i] + 1;
```

This ensures that the algorithm always keeps track of the palindrome with the farthest known right boundary.

An important detail is that `right` is exclusive throughout this implementation. We must use this convention consistently in all formulas and conditions. Confusing the exclusive boundary with the last included position can introduce off-by-one errors.

## 8. The Complete Manacher Algorithm

We now have all the necessary components:

1. Transform the string to unify even-length and odd-length palindromes.
2. Create an array `p` to store the palindrome radius at each position.
3. Maintain the center and exclusive right boundary of the known rightmost palindrome.
4. Use the mirrored center to initialize the radius.
5. Perform additional comparisons only when necessary.
6. Keep track of the best center and the largest radius found so far.

In the implementation below, the function returns the longest palindromic substring itself.

### Complete C++ Implementation

```cpp
#include <algorithm>
#include <string>
#include <vector>

std::string longestPalindrome(const std::string& str)
{
    // Transform the string by inserting separators.
    // -1 cannot collide with any unsigned char value.
    std::vector<int> s;
    s.reserve(str.size() * 2 + 1);

    for (unsigned char c : str)
    {
        s.push_back(-1);
        s.push_back(static_cast<int>(c));
    }

    s.push_back(-1);

    const int n = static_cast<int>(s.size());

    // p[i] stores the palindrome radius around i.
    std::vector<int> p(n, 0);

    // The right boundary is exclusive.
    int center = 0;
    int right = 0;

    // Best palindrome found so far.
    int bestCenter = 0;
    int bestRadius = 0;

    for (int i = 1; i < n - 1; ++i)
    {
        // Initialize the radius using the mirrored position.
        if (i < right)
        {
            const int mirror = 2 * center - i;

            p[i] = std::min(
                right - i - 1,
                p[mirror]
            );
        }

        // Expand while the values on both sides match.
        while (i - p[i] - 1 >= 0 &&
               i + p[i] + 1 < n &&
               s[i - p[i] - 1] == s[i + p[i] + 1])
        {
            ++p[i];
        }

        // Update the rightmost known palindrome.
        const int newRight = i + p[i] + 1;

        if (newRight > right)
        {
            center = i;
            right = newRight;
        }

        // Keep the first maximum-length answer.
        if (p[i] > bestRadius)
        {
            bestRadius = p[i];
            bestCenter = i;
        }
    }

    // Convert transformed-string indices to original indices.
    const std::size_t start =
        static_cast<std::size_t>(
            (bestCenter - bestRadius) / 2
        );

    return str.substr(
        start,
        static_cast<std::size_t>(bestRadius)
    );
}
```

### Example Usage

```cpp
#include <iostream>
#include <string>

std::string longestPalindrome(const std::string& str);

int main()
{
    std::cout << longestPalindrome("babad") << '\n';
    std::cout << longestPalindrome("abba") << '\n';
    std::cout << longestPalindrome("cbbd") << '\n';
    std::cout << longestPalindrome("") << '\n';
}
```

Example output:

```text
bab
abba
bb

```

For the input `babad`, returning `aba` would also be correct because both `bab` and `aba` are palindromic substrings of the same maximum length.

For `abba`, the result is `abba`.

For `cbbd`, the result is `bb`.

For an empty input string, the result is also an empty string.

## 9. Why Does the Substring Extraction Formula Work?

After the algorithm finishes, `bestCenter` contains the center of the longest palindrome in the transformed string, and `bestRadius` contains its radius.

In the transformed string, separators and original characters alternate.

With this structure:

- The length of the corresponding palindrome in the original string is `bestRadius`.
- The starting position of the substring in the original string is calculated using:

```cpp
start = (bestCenter - bestRadius) / 2;
```

Therefore, the following statement extracts the desired substring:

```cpp
return str.substr(start, bestRadius);
```

### Walking Through the Extraction with an Example

Suppose the input string is:

```text
BAAB
```

After inserting separators, it becomes:

```text
#B#A#A#B#
```

The largest palindrome radius is 4, centered at the separator between the two `A` characters.

Thus:

```text
bestCenter = 4
bestRadius = 4
```

The starting index in the original string is:

```text
(bestCenter - bestRadius) / 2
= (4 - 4) / 2
= 0
```

The palindrome's length is 4, so we take four characters from index 0 of the original string:

```text
BAAB
```

Now suppose we prepend three characters, `EWZ`, to the original string:

```text
EWZBAAB
```

The transformed string becomes:

```text
#E#W#Z#B#A#A#B#
```

The same palindrome, `BAAB`, is now centered at transformed index 10, with a radius of 4.

The starting position in the original string is:

```text
(10 - 4) / 2 = 3
```

Taking four characters from index 3 gives us:

```text
EWZ{BAAB}  =>  BAAB
```

The formula therefore works even when the palindrome does not start at the beginning of the original string.

### Does `bestCenter` Always Point to an Original Character?

No.

If the longest palindromic substring has an even length, its center lies on a separator in the transformed string.

If it has an odd length, its center lies on an original character.

By inserting separators between characters, Manacher's algorithm allows even-length palindromes to have explicit centers rather than centers that fall between two characters. Consequently, the `p` array handles both cases uniformly, and `bestCenter` may refer to either a separator or an original character.

## 10. Implementation Details and Limitations

### Empty Strings

If the input is empty, the algorithm creates a transformed array containing a single separator.

The best radius remains zero, and the function returns an empty string.

### Repeated Characters

The algorithm also works correctly for strings such as:

```text
aaaaaa
```

In this case, the entire string is the longest palindrome.

### Multiple Valid Answers

There may be multiple palindromic substrings with the same maximum length.

In the implementation above, we update the best answer only when the new radius is strictly greater than the previous best:

```cpp
if (p[i] > bestRadius)
```

Therefore, if multiple maximum-length answers exist, the algorithm retains the first one it encounters.

### Persian Text and UTF-8

In C++, `std::string` stores a sequence of bytes. Therefore, the implementation above is suitable for ASCII and data in which each character of interest is represented by a single byte.

However, Persian text is usually encoded in UTF-8, where a single Unicode character may occupy multiple bytes.

If we want to compare Persian characters as Unicode characters rather than individual bytes, we must first convert the input into an appropriate sequence of character units and then run the algorithm on that sequence.

Real-world text processing may also require Unicode normalization before processing, because two visually identical characters can have different byte representations.

### Data Types and Input Size

This implementation uses `int` for indices and radii. For inputs that may exceed the range of `int`, a more appropriate index type should be used.

Also, in a general-purpose implementation, we should check for possible overflow before calculating:

```cpp
2 * str.size() + 1
```

## 11. Time and Space Complexity

If the original string has length \(n\), the transformed string has length:

\[
2n+1
\]

This is still \(O(n)\).

### Time Complexity

In the simple approach, each center may revisit portions of the string that have already been examined. Therefore, its worst-case time complexity is:

\[
O(n^2)
\]

Manacher's algorithm, however, uses previously calculated palindrome radii and the mirror property to avoid many redundant comparisons.

Whenever an expansion moves beyond the known right boundary, that boundary advances. Consequently, the total number of successful expansions across the entire algorithm remains linear.

The time complexity of Manacher's algorithm is therefore:

\[
O(n)
\]

### Space Complexity

We need additional memory to store the transformed string and the palindrome-radius array.

Both arrays are proportional to the length of the input string, so the auxiliary space complexity is:

\[
O(n)
\]

## Conclusion

In this problem, we first learned what a palindrome is and how to check one using a simple approach. We then saw that palindromes can have either odd or even lengths, making it more complicated to handle their centers separately.

By inserting separators between characters, we transformed both types of palindromes into a unified structure. We then implemented the straightforward center-expansion approach and saw how it can perform many redundant comparisons.

The key idea behind Manacher's algorithm is the **mirror property**: once we know a large palindrome, information about its mirrored positions can help us calculate the radii of new centers.

This information is reliable only within the boundary of the known palindrome. To expand beyond that boundary, we still need to perform actual character comparisons.

Ultimately, Manacher's algorithm finds the longest palindromic substring in linear time.

The most important takeaway is that Manacher's strength lies not merely in finding palindromes, but in reusing previously computed information to avoid unnecessary repeated work.

------

## 🤝 Contributors

<div align="center">

| GitHub | LinkedIn | Email | Site | Telegram |
|--------|----------|-------|------|----------|
| [HadiAbbasi](https://github.com/HadiAbbasi) | [Hadi Abbasi](https://www.linkedin.com/in/hadi-abbasi-programmer/) | [Hadi Abbasi](hadi.abbasi.programmer@gmail.com) | [Hiens.org](https://hiens.org) | [Hadi Abbasi](@Hadi_Abbasi_Programmer) |

</div>