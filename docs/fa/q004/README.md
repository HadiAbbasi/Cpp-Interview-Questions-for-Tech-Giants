<div align="right">

[🇺🇸 English](../../en/q004/README.md) | [🇮🇷 فارسی](./README.md)

</div>

---
# سوال 004:  (سوال گوگل) نجات مسافران کشتی در حال غرق شدن با کمترین تعداد قایق های نجات

---

یک کشتی (مثل تایتانیک) در حال غرق شدن است! N نفر سوار این کشتی هستند! باید همه شان نجات پیدا کنند! می خواهیم به کمترین تعداد ممکن قایق اعزام کنیم تا همه مسافران کشتی را نجات دهیم! لیستی از وزن افراد داخل کشتی داریم (لیست با طول N). هر قایق نهایتا 2 نفر را می تواند سوار کند  به شرطی که مجموع وزن افراد کمتر یا مساوی Limit  باشد! همه قایق ها هم عینا مثل هم هستند! پس حتی اگر 3 نفر وجود داشته باشند که مجموع وزنشان کمتر از Limit  باشند، باز هم فقط 2 نفرشان می توانند سوار قایق شوند! وزن هیچ کدام از افراد این کشتی بزرگتر یا حتی مساوی Limit  نیست و قطعا وزن همه آنها از Limit  کمتر است ولی ممکن است مجموع وزن دو نفر از این افراد بزرگتر یا مساوی Limit  باشد! برای مثال خودمان، چیزی شبیه به این مقدار را داریم و بهترین روش حل این مساله را بیابید:

```
// لیست وزن مسافران
std::List<int> weights = {5,3,5,6,6,2,1,3,5,6,4,6,5,6,6,4,5,2,3,5,6,4,1,2,3,5,2,1,4,3,2,5,4,1,2,6,3,2,5,1,2,6,5,5,4};
N = 45     //تعداد کل مسافران
Limit = 10   //مجموع وزن های دو مسافر باید کوچکتر یا مساوی 10 باشد چون قایق ها نمی توانند وزن های بیش از 10 را تحمل کنند
// پس مجموع کل وزن هر دو مسافر باید کوچکتر یا مساوی 10 باشد
```

راهکار نهایی:
در ابتدا لیست مد نظر را سورت می کنیم! سپس در پر کردن هر قایق، ابتدا از سنگین وزن ترین سمت  لیست افراد باقیمانده در کشتی، یک نفر را برمی داریم و در قایق جدید می گذاریم و سپس چک می کنیم که با قرار دادن یک نفر از سمت سبک وزن ترین افراد باقیمانده در لیست، آیا مجموع وزنشان مورد تحمل قایق می شود یا خیر؟ اگر بله که او را هم سوار می کنیم و اگر نه که همان قایق را با همان یک نفر سنگین وزن ترین راهی ساحل می کنیم و همین رویه تکرار می شود تا کل افراد کشتی خالی شود و سایز لیست وزن ها 0 شود!

```
int NumRescueBoats(std::List<int> Weights , int Limit)
{
     Weights.sort();
	
     int lightest_person = 0; //the index of lightest person who is not survived yet
     int heaviest_person = Weights.length()-1; //the index of heaviest person who is not survived yet
     int boats_num = 0; //number of boats
     while ( heaviest_person >= lightest_person )
     {
          if (Weights[heaviest_person] + Weights[lightest_person] <= Limit)
          {
               lightest_person++;
		                             
               heaviest_person--;
               boats_num++;
          }
          else
          {
               heaviest_person--;
               boats_num++;
          }
     }
     return boats_num;
}

```

## 🤝 مشارکت کنندگان

<div align="center">

| GitHub | LinkedIn | Email | Site | Telegram |
|--------|----------|-------|------|----------|
| [HadiAbbasi](https://github.com/HadiAbbasi) | [Hadi Abbasi](https://www.linkedin.com/in/hadi-abbasi-programmer/) | [Hadi Abbasi](hadi.abbasi.programmer@gmail.com) | [Hiens.org](https://hiens.org) | [Hadi Abbasi](@Hadi_Abbasi_Programmer) |

</div>