<div align="right">

[🇺🇸 English](./README.md) | [🇮🇷 فارسی](../../fa/q005/README.md)

</div>

---

# Question 005:  Google Array question first bad version Easy

------

Suppose we are developing a piece of software. Its first committed version in the company’s source control system is version 1, the second is version 2, the third is version 3, and so on.
Suddenly, a problem is reported in one of the versions. We roll back to the previous version to investigate, but unfortunately we realize that the bug also exists there. However, no one had noticed it before and no bug reports had been submitted by users.
We continue checking earlier versions and discover that the problem still exists in those versions as well

On the other hand, the number of software versions is very large, and checking each version one by one would be inefficient. Therefore, we need to efficiently find the first version that contains the bug, such that the previous version does not have the bug.
Please suggest an algorithm that can quickly find the first bad version.
Assume that you are given a predefined function:
bool IsBadVersion(int version);
By passing a version number to this function, it returns:
•	true if the version is bad
•	false if the version is good
Your task is to find the first bad version in the most efficient way possible
App Versions (Greens are good version, Yellows are Bad versions)

<div align="center">
  <img src="../../../assets/img005-001.png" alt="Image" />
</div>

---

The solution to this problem is similar to Binary Search.
We know that the versions are ordered sequentially. The key observation is that once a version becomes bad, all versions after it are also bad. Therefore, the versions can be considered as sorted in this form:
Good, Good, Good, ..., Bad, Bad, Bad
Assume the software has N versions, and starting from version N - 7, all versions are bad. Our goal is to efficiently find that first bad version.
The best approach is to use Binary Search:
1.	We check the middle version: mid = N / 2.
2.	If mid is bad, then the first bad version must be in the range [1, mid].
3.	If mid is good, then the first bad version must be in the range [mid + 1, N].
Each time, we reduce the search space by half.
We continue this process until left == right.
At that point, we have found the first bad version — meaning that:
•	The current version is bad.
•	The previous version is good (if it exists).
Since we divide the search space in half at each step, the time complexity of this algorithm is:
O(log N)

---

```cpp
class Solution {
public:
    int firstBadVersion(int n) {
        int left = 1;
        int right = n;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (isBadVersion(mid)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
};

```

---

## 🤝 Contributors

<div align="center">

| GitHub | LinkedIn | Email | Site | Telegram |
|--------|----------|-------|------|----------|
| [HadiAbbasi](https://github.com/HadiAbbasi) | [Hadi Abbasi](https://www.linkedin.com/in/hadi-abbasi-programmer/) | [Hadi Abbasi](hadi.abbasi.programmer@gmail.com) | [Hiens.org](https://hiens.org) | [Hadi Abbasi](@Hadi_Abbasi_Programmer) |

</div>