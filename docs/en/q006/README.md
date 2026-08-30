<div align="center">

[🇺🇸 English](./README.md) | [🇮🇷 فارسی](../../fa/q006/README.md)

</div>

---

# Question 006: Airbnb Math Question Single Number

------

Suppose you are given an array of integers. You are told that all the numbers in this array are unordered, and every number appears exactly twice, except for one number that appears only once.
What is the best algorithm to find that single number?

A Possible Solution
Like many people, the first idea that might come to mind is using std::unordered_set.
You can iterate through the array one element at a time:
If the current number is not in the std::unordered_set, insert it.
If it is already present, remove it from the set.
At the end, the only remaining element in the set will be the number that appears exactly once.

A Better Solution: The Magic of XOR
The XOR operator (denoted by ^) is a very interesting operator.
When you XOR two numbers, the machine performs a bit-by-bit XOR operation.
The XOR operator works as follows:
If two bits are the same (both 0 or both 1), the result is 0.
If two bits are different (one 0 and one 1), the result is 1


<div align="center">

| A | Operator | B | Result |
|:---:|:---:|:---:|:---:|
| 0 | ^ | 0 | 0 |
| 0 | ^ | 1 | 1 |
| 1 | ^ | 0 | 1 |
| 1 | ^ | 1 | 0 |

</div>

Any number XOR itself equals 0, Because all corresponding bits are identical.

Any number XOR 0 equals the number itself! Because if there we a zero bit, 0 ^ 0 will be 0 and if there were a 1, the 1 ^ 0 will be 1!

Very Important: XOR is commutative and associative

```cpp
A ^ B ^ C ^ D ^ E ^ F  ==  E ^ F ^ A ^ C ^ B ^ D   ==   F ^ E ^ D ^ C ^ B ^ A
```
Final Insight
Since every number appears twice except one, if we XOR all elements of the array together:
Each duplicated number cancels itself out (A ^ A = 0)
Zero does not affect the result (A ^ 0 = A)
The only remaining value will be the number that appears once.

This gives us:

<div align="center">

✅ Time Complexity: O(n)
✅ Space Complexity: O(1)
🚀 Extremely efficient

</div>

```cpp
#include <iostream>
#include <vector>

int findSingleNumber(const std::vector<int>& nums)
{
    int result = 0;

    for (int num : nums)
    {
        result ^= num;
    }

    return result;
}

int main()
{
    std::vector<int> nums = {4, 1, 2, 1, 7 , 2, 4};

    int single = findSingleNumber(nums);

    std::cout << "The number that appears once is: "
              << single << std::endl;

    return 0;
}

```

---

## 🤝 Contributors

<div align="center">

| GitHub | LinkedIn | Email | Site | Telegram |
|--------|----------|-------|------|----------|
| [HadiAbbasi](https://github.com/HadiAbbasi) | [Hadi Abbasi](https://www.linkedin.com/in/hadi-abbasi-programmer/) | [Hadi Abbasi](hadi.abbasi.programmer@gmail.com) | [Hiens.org](https://hiens.org) | [Hadi Abbasi](@Hadi_Abbasi_Programmer) |

</div>