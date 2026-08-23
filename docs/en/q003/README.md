<div align="right">

[🇺🇸 English](./README.md) | [🇮🇷 فارسی](../../fa/q003/README.md)

</div>

---

# Question 003: (Microsoft Math Question Missing Number (Easy)) find the one lost number from not sorted array of numbers between 1-N 

------

Microsoft Math Question Missing Number (Easy):

You are given an array of integers. These integers are supposed to be the consecutive numbers from 1 to N, placed in an unsorted order. However, one number from this range is missing from the array. Your task is to find the missing number as efficiently as possible.
In other words, for example, the consecutive numbers from 1 to 50 should appear in an array in an unsorted order, but exactly one of these numbers has been removed. You must determine which number is missing using the fastest possible approach.

Answer:
The sum of the numbers from 1 to N is equal to:

```cpp
SumOf1ToN = (N*(N+1))/2
```

You know the value of N in this problem. Using this formula, you first calculate the total sum of the numbers from 1 to N (SumOf1ToN). Then, you iterate through the given array and compute the sum of its elements in a variable called Array_SUM. Finally, you subtract Array_SUM from SumOf1ToN, and the result will be the number that is missing from the array.

```cpp
Int SumOf1ToN = (N*(N+1))/2
Int Array_SUM = 0;
For (int I = 0; I < Len(Array);i++)
         Array_SUM += Array[i];
Int Lost_Num = SumOf1ToN  - Array_SUM ;
```

---

## 🤝 Contributors

<div align="center">

| GitHub | LinkedIn | Email | Site | Telegram |
|--------|----------|-------|------|----------|
| [HadiAbbasi](https://github.com/HadiAbbasi) | [Hadi Abbasi](https://www.linkedin.com/in/hadi-abbasi-programmer/) | [Hadi Abbasi](hadi.abbasi.programmer@gmail.com) | [Hiens.org](https://hiens.org) | [Hadi Abbasi](@Hadi_Abbasi_Programmer) |

</div>