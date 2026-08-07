<div align="right">

[🇺🇸 English](./README.md) | [🇮🇷 فارسی](../../fa/q004/README.md)

</div>

---
# Question 004:  Google Array Question Boats to save people (Medium)

------

A ship (like Titanic) is sinking. There are N people on the ship. They all need to be rescued. We want to send the minimum number of boats possible to rescue everyone. We have a list of people's weights (list of length N). Each boat can carry at most 2 people if their total weight is less than or equal to the Limit. All boats are identical. So even if there are 3 people whose total weight is less than the Limit, only 2 of them can go on the boat. No person's weight is greater than or equal to the Limit. Their weights are all less than the Limit but the total weight of two people might be greater than or equal to or less than the Limit. For example, we have something like this and you have to find the best solution.

```
// the list of passenger’s weight
std::List<int> weights = {5,3,5,6,6,2,1,3,5,6,4,6,5,6,6,4,5,2,3,5,6,4,1,2,3,5,2,1,4,3,2,5,4,1,2,6,3,2,5,1,2,6,5,5,4};
N = 45     //the number of all of passengers
Limit = 10   //the sum of the weight of all of 1 or 2 
// persons totally must be lower than or equal to 10
```

Optimal Solution: 
First, we sort the list! Then, when filling each boat, we start by taking one person from the heaviest side of the remaining people on the ship and put them in the new boat. Next, we check if adding one person from the lightest side of the remaining people on the list would make the total weight manageable for the boat. If yes, we add them, and if not, we send the boat with just the one heaviest person to the shore. This process repeats until the entire ship is empty and the size of the weight list becomes 0.

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

## 🤝 Contributors

<div align="center">

| GitHub | LinkedIn | Email | Site | Telegram |
|--------|----------|-------|------|----------|
| [HadiAbbasi](https://github.com/HadiAbbasi) | [Hadi Abbasi](https://www.linkedin.com/in/hadi-abbasi-programmer/) | [Hadi Abbasi](hadi.abbasi.programmer@gmail.com) | [Hiens.org](https://hiens.org) | [Hadi Abbasi](@Hadi_Abbasi_Programmer) |

</div>