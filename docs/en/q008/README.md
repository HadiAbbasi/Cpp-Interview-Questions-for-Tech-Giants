<div align="right">

[🇺🇸 English](./README.md) | [🇮🇷 فارسی](../../fa/q008/README.md)

</div>

---

# Question 008: Amazon Math Question Robot return to origin Easy

<div align="center">
  <img src="../../../assets/img008-001.png" alt="Image" />
</div>

# The Robot Return to Origin Problem

## What is the problem?

Suppose a robot is placed on a two-dimensional grid. The robot starts moving from the starting point, which is the coordinate `(0, 0)`.

The robot is given a string of characters where each character represents one movement:

* `U` → one step up
* `D` → one step down
* `L` → one step left
* `R` → one step right

For example, if the input is:

```text
"UDLR"
```

the robot first moves up, then down, then left, and finally right.

At the end, we need to determine:

> Does the robot return to the starting point `(0, 0)`?

---

## The main idea

To solve this problem, we don't need to create a grid or store the entire path.

We only need to keep track of the robot's **current position** using two variables, `x` and `y`.

Initially:

```cpp
x = 0;
y = 0;
```

Each movement changes one of these two coordinates.

### Moving up — `U`

The `y` coordinate increases by one:

```cpp
y += 1;
```

### Moving down — `D`

The `y` coordinate decreases by one:

```cpp
y -= 1;
```

### Moving right — `R`

The `x` coordinate increases by one:

```cpp
x += 1;
```

### Moving left — `L`

The `x` coordinate decreases by one:

```cpp
x -= 1;
```

At the end, if:

```cpp
x == 0 && y == 0;
```

then the robot has returned exactly to the starting point.

---

## A simple example

Suppose the input is:

```text
"UD"
```

The robot starts at `(0, 0)`.

After the `U` movement:

```text
(0, 0) → (0, 1)
```

After the `D` movement:

```text
(0, 1) → (0, 0)
```

So the robot has returned to `(0, 0)`.

Therefore, the answer is:

```text
true
```

---

## Another example

Suppose the input is:

```text
"LLRR"
```

The robot's path is:

```text
Start: (0, 0)

L → (-1, 0)

L → (-2, 0)

R → (-1, 0)

R → (0, 0)
```

So the robot has returned to the starting point.

The answer is:

```text
true
```

---

## When does the answer become `false`?

Suppose the input is:

```text
"UUR"
```

The movements are:

```text
(0, 0)
   ↓ U
(0, 1)
   ↓ U
(0, 2)
   ↓ R
(1, 2)
```

The robot ends at `(1, 2)`, not `(0, 0)`.

Therefore:

```text
false
```

---

# An important point about the problem

We don't actually need to store the robot's entire path.

For example, we don't need something like:

```text
[(0,0), (0,1), (0,2), (1,2)]
```

because the only thing that matters for the final answer is the **final position**.

So we have a very simple approach:

1. Set `x` and `y` to zero.
2. Read each character in the string one by one.
3. Change `x` or `y` based on the character.
4. At the end, check whether both `x` and `y` are zero.

---

# An even more interesting point: we can think about it without fully simulating the path

For the robot to return to the starting point, the number of upward movements must be equal to the number of downward movements.

That means:

```text
Number of U = Number of D
```

And similarly, the number of left movements must be equal to the number of right movements:

```text
Number of L = Number of R
```

For example:

```text
"UUDDLLRR"
```

We have two `U` movements and two `D` movements.

We also have two `L` movements and two `R` movements.

Therefore, the robot returns to the starting point.

However, in a technical interview, the `x` and `y` approach is usually cleaner and easier to explain because we are directly simulating the robot's position.

---

# Time Complexity

Suppose the length of the string is `n`.

We examine each character exactly once.

Therefore:

```text
Time Complexity: O(n)
```

And we only keep two variables, `x` and `y`:

```text
Space Complexity: O(1)
```

So the extra memory is constant and does not depend on the size of the string.

---

# Simple implementation

For example, in Python:

```cpp
#include <string>
using namespace std;

bool judgeCircle(string moves) {
    int x = 0;
    int y = 0;

    for (char move : moves) {
        if (move == 'U') {
            y += 1;
        }
        else if (move == 'D') {
            y -= 1;
        }
        else if (move == 'R') {
            x += 1;
        }
        else if (move == 'L') {
            x -= 1;
        }
    }

    return x == 0 && y == 0;
}
```

For example:

```cpp
std::cout<<judgeCircle("UDLR");
```

Output:

```text
True
```

------

## 🤝 Contributors

<div align="center">

| GitHub | LinkedIn | Email | Site | Telegram |
|--------|----------|-------|------|----------|
| [HadiAbbasi](https://github.com/HadiAbbasi) | [Hadi Abbasi](https://www.linkedin.com/in/hadi-abbasi-programmer/) | [Hadi Abbasi](hadi.abbasi.programmer@gmail.com) | [Hiens.org](https://hiens.org) | [Hadi Abbasi](@Hadi_Abbasi_Programmer) |

</div>