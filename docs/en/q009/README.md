<div align="right">

[🇺🇸 English](./README.md) | [🇮🇷 فارسی](../../fa/q009/README.md)

</div>

---

# Question 009: Microsoft Array Question Container with most water Medium


<div align="center">
  <img src="../../../assets/img009-001.png" alt="Image" />
</div>

Suppose we have a series of vertical lines of different heights placed on a horizontal line. for example:

```
        |
        |       |
        |       |       |
        |       |       |
    |   |       |       |
    |   |       |       |
    |   |   |   |       |
-----------------------------
```

Whichever two lines we choose, if we pour water between them, they form a container.

What is the goal of the question?

We need to find two lines that can hold the maximum amount of water between them.

The important point is that:

The distance between the two lines is important. The greater the distance, the more water they can hold!
The shorter height of the two lines determines how high the water can rise.

For example, if we have the following lines:

```
|           |
|           |
|           |
|           |
|           |
|     |     |
|     |     |
------------
```

If the height of the first line is 5 and the second is 3, the water cannot rise above a height of 3, because any water higher than the shorter line will overflow!

So the amount of water is approximately:

Distance between the two lines × Height of the shorter line

Which means:

```
Area = width × min(height1, height2)
```

## Summary in one sentence:

Among all pairs of lines, find the two lines that can hold the maximum amount of water between them.

## Optimal Solution:

We start from both ends of the lines and calculate the amount of water that can be trapped between them. We compare this with a temporary variable that stores the maximum amount of water found so far. If the newly calculated amount is greater, we update (override) the variable with the new value and then narrow the search range by moving inward from the side of the shorter line!

Why do we narrow the range from the shorter line? Because, as you can see, the shorter line limits the volume of water that can be held between the two lines. Therefore, we narrow the range from that side in the hope of finding a taller line that would increase the water capacity! In other words, we try to improve the situation by addressing the side that is the limiting factor. In this case, finding a line taller than the current shorter line might lead us to the maximum possible water volume between two lines!

But what about the decreasing distance between the lines? Yes! Even if two short lines with a height of 1 are 20 units apart, the area of the water they can hold is 1 * 20 = 20!

Whereas two lines with a height of 40, even if they are only 1 unit apart, will hold 40 * 1 = 40 units of water between them!

Algorithm to solve this problem:

```
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
public:
    int maxArea(vector<int>& height) {
        int left = 0;
        int right = height.size() - 1;
        int maxArea = 0;

        while (left < right) {
            // عرض ظرف
            int width = right - left;

            // ارتفاع ظرف = ارتفاع کوتاه‌تر
            int h = min(height[left], height[right]);

            // مساحت
            int area = width * h;

            // بیشترین مساحت
            maxArea = max(maxArea, area);

            // حرکت دادن اشاره‌گر خط کوتاه‌تر
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }

        return maxArea;
    }
};

int main() {
    Solution solution;

    vector<int> height = {1, 8, 6, 2, 5, 4, 8, 3, 7};

    cout << solution.maxArea(height) << endl;

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