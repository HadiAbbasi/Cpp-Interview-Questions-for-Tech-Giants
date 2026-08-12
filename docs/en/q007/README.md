<div align="right">

[🇺🇸 English](./README.md) | [🇮🇷 فارسی](../../fa/q007/README.md)

</div>

---
# Question 007: Amazon Math Question Count Primes

Problem statement: The goal of the problem is to count how many prime numbers exist between 1 and the input number N, excluding N itself. In other words, we count primes in the range from 1 to N-1.  Although we can include or exclude the N from counting the prime numbers or not!
------
##Brute Force

The first idea that usually comes to mind is to loop from 2 to N-1 (using a variable i). For each number i, we check whether it is divisible by any number between 2 and i/2.
If it is divisible by any of them, then it is not prime. If no divisor is found up to i/2, then the number is prime.
------
Because the smallest divisor after 1 can be 2. If a number N is divisible by 2:
```text
N = 2 × X          -->       X = N / 2
```
If N is divisible by 3:
```
N = 3 × Y                  -->        Y = N / 3
```
As the divisor increases, the paired multiplier decreases.
The largest possible divisor happens when the paired multiplier is the smallest possible (which is 2).
Therefore, no divisor can be greater than N/2.
------
## Logical implementation of a simple method
For each number i:
We define a boolean variable called is_prime.
If in the inner loop (j) we find that i % j == 0:
We set is_prime to false.
We skip the rest of the checks for that i.
If no zero remainder is found:
i is a prime number.
We increment the prime counter.

## Optimal Method
Sieve Algorithm This method is the famous Eratosthenes algorithm, also known as the "Sieve of Eratosthenes".

## Main idea
Instead of directly searching for prime numbers, we eliminate non-prime numbers.
We create a table from 0 to N.
We remove 0 and 1 because they are not prime.
We start from number 2:
Since it is not removed, we consider it prime.
Then we remove its multiples:
2×2, 2×3, 2×4, ...

## Why we started from 3 * 3 ?
When we reach number 3:
2×3 was already removed because it is the same as 3×2 and was eliminated earlier.
To avoid repetition, we start removing multiples from i×i.
Because:
i×(i−1) was already removed in the previous step.

## General rules:
If a number is already removed, we do not process its multiples.
We start removing multiples from i×i.
We continue as long as i×i < N.
Because once i×i exceeds N, there are no new multiples within range.

## Programming Implementation
### We define a boolean array of size N.
Indices range from 0 to N-1.
Initially, we set all values to true (assuming all are prime).
We set index 0 and 1 to false.
If we want to include N itself, we create an array of size N+1.
Then instead of focusing on whether N is included in the count, we focus on the size of the array!

## In multiplication tables we see:
```text
2×3 = 6
3×2 = 6
```
So we should not compute both.
To avoid repetition:
For each number i
Start removing from i×i
And continue while i×i < N.

## An Important Logical Point:
If a number X is formed as:
```text
A × B = X
```
And we have already removed multiples of A,
Then X and all its multiples have already been removed.
Therefore, if an array cell is already false,
We do not check that number or its multiples again.
Because they were eliminated earlier.

## Final conclusion
The brute force method has roughly O(N²) time complexity.
But the sieve method has about O(N log log N) time complexity.
Therefore, for large values of N, the sieve algorithm is much faster and more efficient.

## The Final Algorithm:
```cpp
int countPrimes(int n) {
    if (n <= 2) return 0;

    vector<bool> isPrime(n, true); 
          //vector with n items which all are true
    isPrime[0] = isPrime[1] = false; // 0  and 1 are not prime at all

    for (int i = 2; i * i < n; i++) {
        if (isPrime[i]) {
            for (int j = i * i; j < n; j += i) {
                isPrime[j] = false;
            }
        }
    }

    return count(isPrime.begin(), isPrime.end(), true);
}
```



## 🤝 Contributors

<div align="center">

| GitHub | LinkedIn | Email | Site | Telegram |
|--------|----------|-------|------|----------|
| [HadiAbbasi](https://github.com/HadiAbbasi) | [Hadi Abbasi](https://www.linkedin.com/in/hadi-abbasi-programmer/) | [Hadi Abbasi](hadi.abbasi.programmer@gmail.com) | [Hiens.org](https://hiens.org) | [Hadi Abbasi](@Hadi_Abbasi_Programmer) |

</div>