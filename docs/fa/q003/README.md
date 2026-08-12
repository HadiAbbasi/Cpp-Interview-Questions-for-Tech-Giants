<div align="right">

[🇺🇸 English](../../en/q003/README.md) | [🇮🇷 فارسی](./README.md)

</div>

---
# سوال 003: (سوال مایکروسافت) یافتن تنها عدد گمشده از آرایه نامرتب اعداد بین 1 و N

------

مساله یافتن عدد گم شده مختص اینترویوی مایکروسافت (آسان):

مساله: آرایه ای از اعداد صحیح را به شما داده اند و گفته اند این اعداد، اعداد بین 1 تا N پشت سر هم هستند که به صورت سورت نشده قرار گرفته اند و شما باید تنها عددی را که در این آرایه قرار ندارد را بیابید! به بیان بهتر مثلا اعداد پشت سر هم بین 1 تا 50 در یک آرایه به صورت سورت نشده قرار دارند و فقط یکی از این اعداد را از آرایه حذف کرده اند و شما باید همان یک عدد را در سریعترین روش ممکن بیابید!

پاسخ:
مجموع اعداد بین 1 تا N برابر است با:

```cpp
SumOf1ToN = (N*(N+1))/2
```

شما در این مساله، N را می دانید. پس با این فرمول، مجموع کل را حساب می کنید (SumOf1ToN ) و سپس یکی یکی اعداد آرایه مد نظر را با متغیر Array_SUM جمع می کنید و نهایتا عدد SumOf1ToN   را از عدد Total_SUM کم می کنید و نتیجه می شود عددی که در آرایه وجود ندارد.

```cpp
Int SumOf1ToN = (N*(N+1))/2
Int Array_SUM = 0;
For (int I = 0; I < Len(Array);i++)
         Array_SUM += Array[i];
Int Lost_Num = SumOf1ToN  - Array_SUM ;
```

## 🤝 مشارکت کنندگان

<div align="center">

| GitHub | LinkedIn | Email | Site | Telegram |
|--------|----------|-------|------|----------|
| [HadiAbbasi](https://github.com/HadiAbbasi) | [Hadi Abbasi](https://www.linkedin.com/in/hadi-abbasi-programmer/) | [Hadi Abbasi](hadi.abbasi.programmer@gmail.com) | [Hiens.org](https://hiens.org) | [Hadi Abbasi](@Hadi_Abbasi_Programmer) |

</div>