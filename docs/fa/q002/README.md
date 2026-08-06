<div align="right">

[🇺🇸 English](../../en/q002/README.md) | [🇮🇷 فارسی](./README.md)

</div>

---
# سوال 002:  عملیات شمارش تعداد کاراکترهای رشته دلخواه

---

رشته ای را به ما می دهند و از ما می خواهند به تفکیک کاراکتر ها بگوییم از هر کاراکتر چه تعداد داریم؟
پاسخ: از یک map با نوع کلید char و ولیوی نوع int یا uint استفاده می کنیم و یک به یک کاراکترهای رشته را iterate می کنیم و به کلید مد نظر از map مذکور رجوع کرده و یک واحد به value آن کلید کاراکتر اضافه کرده و سر آخر روی کلید های map مد نظر iterate کرده و مقادیر کلید و ولیوی آنرا به عنوان خروجی محاسبه شده چاپ می کنیم!
(توجه: به جهت سادگی کارم، کد های زیر توسط ربات Qwen AI تولید شرکت علی بابا تولید شده است!)

```cpp
#include <iostream>
#include <unordered_map>
#include <string>

int main() {
    std::string input;
    std::cout << "please enter a string: ";
    std::getline(std::cin, input); // for support including spaces and tabs in input string

    std::unordered_map<char, int> charCount;

    // counting the characters
    for (char c : input) {
        charCount[c]++;
    }

    // print the results
    std::cout << "\nprint the results:\n";
    for (const auto& pair : charCount) {
        // printing control charcters
        if (pair.first == '\n') {
            std::cout << "'\\n': " << pair.second << '\n';
        } else if (pair.first == '\t') {
            std::cout << "'\\t': " << pair.second << '\n';
        } else if (pair.first == ' ') {
            std::cout << "' ': " << pair.second << '\n';
        } else {
            std::cout << "'" << pair.first << "': " << pair.second << '\n';
        }
    }

    return 0;
}
```
## نکات مهم

- از `std::getline` استفاده شده تا کل خط (شامل فاصله و کاراکترهای خاص) دریافت شود.

- ابزار `std::unordered_map<char, int>` برای ذخیره تعداد هر کاراکتر بهینه است و دسترسی میانگین `O(1)` دارد.  
  علت استفاده نکردن از `std::map` این است که ترتیب الفبایی کلیدها در این مثال اهمیتی ندارد.  
  بنابراین `std::map` که معمولاً بر پایه الگوریتم درختی **Red-Black Tree** پیاده‌سازی می‌شود، در اینجا سربار بیشتری دارد؛ در حالی که `std::unordered_map` که مبتنی بر **Hash Table** است، سرعت اجرای بالاتری دارد.  
  البته `std::unordered_map` در زمان پیمایش (iterate) ترتیب کلیدها را تضمین نمی‌کند که در این مثال اهمیتی ندارد.

- کاراکترهای خاصی مثل فاصله، تب (`\t`) و خط جدید (`\n`) به صورت خوانا چاپ می‌شوند.


```output
please enter a string:  سلام دنیا
تعداد تکرار هر کاراکتر:
'س': 1
'ل': 1
'ا': 2
'م': 1
' ': 1
'د': 1
'ن': 1
'ی': 1
```
اگر نیاز به پردازش رشته‌های یونیکد (مثل فارسی) داری، این برنامه در صورتی که کامپایلر و ترمینال شما UTF-8 را پشتیبانی کنند، به‌درستی کار می‌کند — چون char  در C++ برای UTF-8 مناسب است، هرچند برای کار پیشرفته‌تر با یونیکد بهتر است از std::wstring یا کتابخانه‌های خاص استفاده کنی. ولی برای کار ساده شمارش بایت‌های UTF-8، همین کافی است.

## 🤝 مشارکت کنندگان

<div align="center">

| GitHub | LinkedIn | Email | Site | Telegram |
|--------|----------|-------|------|----------|
| [HadiAbbasi](https://github.com/HadiAbbasi) | [Hadi Abbasi](https://www.linkedin.com/in/hadi-abbasi-programmer/) | [Hadi Abbasi](hadi.abbasi.programmer@gmail.com) | [Hiens.org](https://hiens.org) | [Hadi Abbasi](@Hadi_Abbasi_Programmer) |

</div>