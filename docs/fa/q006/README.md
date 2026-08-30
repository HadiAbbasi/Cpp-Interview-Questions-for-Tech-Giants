<div align="center">

[🇺🇸 English](../../en/q006/README.md) | [🇮🇷 فارسی](./README.md)

</div>

---

# سوال 006:  سوال ریاضیاتی Airbnb  در مورد یافتن عدد تکی 

---

فرض کنید آرایه ای از اعداد صحیح را به شما داده اند و به شما گفته اند تمام اعداد در این آرایه به صورت نامرتب هستند و همگی آنها 2 بار تکرار شده اند و فقط یک عدد وجود دارد که یک بار تکرار شده است! بهترین الگریتم یافتن همان یک عدد چیست؟

روش حل مساله: شاید مانند خیلی ها اولین چیزی که به ذهن شما برسد، استفاده از std::unordered_set باشد! به این صورت که یک به یک روی آیتم های آرایه iterate انجام می دهید و به هر عدد که برسید، اگر آن عدد را در کلید های std::unordered_set نداشته باشید، آن کلید را به آن اضافه می کنید و اگر داشته باشید، آنرا از std::unordered_set حذف می کنید و نهایتا همان یک عددی که یک بار تکرار شده در std::unordered_set مربوطه می ماند! 

راهکار بهتر: استفاده از جادوی xor
عملگر xor  (با نماد ^) عملگر عجیبی است! وقتی دو عدد را با هم xor می کنید، ماشین شما بیت به بیت آن دو عدد را با هم xor می کند! عملگر xor اینگونه عمل می کند که اگر دو بیت مشابه هم باشند (2 بیت با مقدار 0 یا 2 بیت با مقدار 1)، عدد 0 را بر می گرداند و اگر نامشابه هم باشند (یکی عدد 1 و دیگری عدد 0) عدد 1 را بر می گرداند!

<div align="center">

| A | Operator | B | Result |
|:---:|:---:|:---:|:---:|
| 0 | ^ | 0 | 0 |
| 0 | ^ | 1 | 1 |
| 1 | ^ | 0 | 1 |
| 1 | ^ | 1 | 0 |

</div>

در نتیجه xor هر عددی در خودش مساوی 0 می شود! چون بیت هایشان برابر هم هستند و xor دو بیت مشابه برابر 0 است!

و نتیجه xor هر عددی در 0  خود همان عدد می شود! چون اگر بیتی 0 باشد، xor اش با 0 برابر 0 می شود و اگر بیتی برابر 1 باشد، xor اش با 0 برابر خودش یعنی 1 می شود!

نکته بسیار مهم: xor خاصیت جا به جایی دارد! یعنی حاصل اعداد زیر یکیست:

```cpp
A ^ B ^ C ^ D ^ E ^ F  ==  E ^ F ^ A ^ C ^ B ^ D   ==   F ^ E ^ D ^ C ^ B ^ A
```

حال با تمام حقایق مطرح شده، اگر هر عدد  جز یکی، 2 بار در آرایه تکرار شده، با xor کردن تمام اعداد آرایه در هم، نهایتا هر عدد خودش را خنثی می کند و تنها عدد باقیمانده برابر همان یک عددی می شود که فقط 1 بار تکرار شده است!

نتیجه ای که این الگریتم به ما می ده:

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

## 🤝 مشارکت کنندگان

<div align="center">

| GitHub | LinkedIn | Email | Site | Telegram |
|--------|----------|-------|------|----------|
| [HadiAbbasi](https://github.com/HadiAbbasi) | [Hadi Abbasi](https://www.linkedin.com/in/hadi-abbasi-programmer/) | [Hadi Abbasi](hadi.abbasi.programmer@gmail.com) | [Hiens.org](https://hiens.org) | [Hadi Abbasi](@Hadi_Abbasi_Programmer) |

</div>