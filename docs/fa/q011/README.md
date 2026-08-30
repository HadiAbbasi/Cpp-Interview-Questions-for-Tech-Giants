<div align="center">

[🇺🇸 English](../../en/q011/README.md) | [🇮🇷 فارسی](./README.md)

</div>

---
# سوال 011:  (سوال فیسبوک) انتقال صفرهای آرایه به انتها

<div align="center">
  <img src="../../../assets/img011-001.png" alt="Image" />
</div>

## راهنمای جامع حل مسئله Move Zeroes (سوال مصاحبه فیس‌بوک/متا)

مسئله **Move Zeroes** یکی از پرکاربردترین سوالات سطح Easy در پلتفرم LeetCode و الگوی تکرارشونده در مصاحبه‌های شرکت‌های بزرگ تکنولوژی نظیر Meta (Facebook) است. این مسئله مفهوم پیمایش آرایه‌ها و مدیریت حافظه درجا (In-place) را ارزیابی می‌کند.

---

### ۱. صورت مسئله

یک آرایه از اعداد صحیح به نام `nums` به شما داده شده است. هدف این است که تمام مقادیر `0` را به انتهای آرایه منتقل کنید، به طوری که:

* ترتیب نسبی عناصر غیرصفر حفظ شود.
* تغییرات حتماً **درجا (In-place)** انجام شود (بدون کپی کردن آرایه یا ساخت آرایه جدید).

**مثال ورودی و خروجی:**

| ورودی | خروجی |
| --- | --- |
| `[0, 1, 0, 3, 12]` | `[1, 3, 12, 0, 0]` |
| `[0]` | `[0]` |

---

### ۲. استراتژی حل (روش دو اشاره‌گر - Two Pointers)

روش ساده اما نادرست این است که یک آرایه جدید بپذیریم و غیرصفرها را درون آن بریزیم؛ اما این کار شرط حافظه درجا را نقض می‌کند.

بهترین راهکار استفاده از الگوریتم **دو اشاره‌گر** در یک پیمایش (Single Pass) است:

1. **اشاره‌گر اول (`lastNonZeroFoundAt`):** مقصدی را پیگیری می‌کند که عنصر غیرصفر بعدی باید در آنجا قرار گیرد.
2. **اشاره‌گر دوم (`i`):** کل آرایه را از ابتدا تا انتها پیمایش می‌کند.

در هر مرحله، اگر عنصر فعلی (`nums[i]`) مخالف صفر بود، جای آن را با خانه `lastNonZeroFoundAt` تعویض (Swap) کرده و سپس `lastNonZeroFoundAt` را یک واحد به جلو می‌بریم. با این الگوریتم، تمام صفرها بدون نیاز به حلقه دوم به انتهای آرایه رانده می‌شوند.

---

### ۳. پیاده‌سازی به زبان ++C

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        // اشاره‌گر برای ثبت موقعیت قرارگیری عنصر غیرصفر بعدی
        int lastNonZeroFoundAt = 0;
        
        // پیمایش یک‌باره آرایه
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != 0) {
                // جابه‌جایی عنصر غیرصفر به اولین موقعیت خالی در سمت چپ
                swap(nums[lastNonZeroFoundAt], nums[i]);
                lastNonZeroFoundAt++;
            }
        }
		
		for (int i = lastNonZeroFoundAt; i < nums.size(); i++) {
			nums[i] = 0;
		}
    }
};

```

---

### ۴. تحلیل پیچیدگی (Complexity Analysis)

* **پیچیدگی زمانی (Time Complexity):** برابر با $O(n)$ است؛ زیرا آرایه تنها یک بار پیمایش می‌شود ($n$ تعداد عناصر آرایه است).
* **پیچیدگی فضایی (Space Complexity):** برابر با $O(1)$ است؛ زیرا هیچ حافظه اضافی اختصاص داده نشده و جابه‌جایی‌ها تماماً درجا انجام می‌شوند.

---

## 🤝 مشارکت کنندگان

<div align="center">

| GitHub | LinkedIn | Email | Site | Telegram |
|--------|----------|-------|------|----------|
| [HadiAbbasi](https://github.com/HadiAbbasi) | [Hadi Abbasi](https://www.linkedin.com/in/hadi-abbasi-programmer/) | [Hadi Abbasi](hadi.abbasi.programmer@gmail.com) | [Hiens.org](https://hiens.org) | [Hadi Abbasi](@Hadi_Abbasi_Programmer) |

</div>