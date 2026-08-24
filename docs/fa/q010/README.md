<div align="center">

[🇺🇸 English](../../en/q010/README.md) | [🇮🇷 فارسی](./README.md)

</div>

---

# سوال 010:  (سوال گوگل): آرایه اعداد شکل کوه از کم به زیاد و سپس از زیاد به کم تا انتها

<div align="center">
  <img src="../../../assets/img010-001.png" alt="Image" />
</div>

سوال: آرایه ای از اعداد صحیح را به شما داده اند و از شما خواسته اند تعیین کنید که آیا به ترتیب از آیتم اول تا آخر آرایه، حالت کوه دارد یا خیر؟

به بیان بهتر باید از ابتدا تا جایی از آرایه، اعداد صعودی باشند و از آن نقطه تا پایان، اعداد نزولی باشند!

```
class Solution {
public:
    bool validMountainArray(vector<int>& A) {
        int i = 1;

        while (i < A.size() && A[i] > A[i - 1]) {
            i++;
        }

        if (i == 1 || i == A.size()) {
            return false;
        }

        while (i < A.size() && A[i] < A[i - 1]) {
            i++;
        }

        return i == A.size();
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