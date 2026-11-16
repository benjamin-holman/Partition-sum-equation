# Partition-sum-equation
This project focuses on finding solutions to the equation $p(x) + p(y) = p(z)$ with $1 \le x \le y < z$, where $p(n)$ is the number of partitions of $n$.

*”I came up with this question during my 2nd-year research project on partitions, where I spotted solutions to $p(x) + p(y) = p(z)$ for small $x, y, z$ that seemed to vanish for larger values. This project investigates whether only finitely many solutions exist.”*

## Solutions found

| $x$ | $y$ | $z$ | Equation |
|---|---|---|---|
| 1 | 1 | 2 | $1 + 1 = 2$ |
| 1 | 2 | 3 | $1 + 2 = 3$ |
| 2 | 3 | 4 | $2 + 3 = 5$ |
| 2 | 4 | 5 | $2 + 5 = 7$ |
| 5 | 7 | 8 | $7 + 15 = 22$ |
| 6 | 6 | 8 | $11 + 11 = 22$ |
| 7 | 7 | 9 | $15 + 15 = 30$ |


## Pentagonal Number Theorem

```math
p(z) = p(z-1) + p(z-2) - p(z-5) - p(z-7) + p(z-12) + p(z-15) - \cdots
```

**Small solutions.** For $z \le 4$ all terms after $p(z-2)$ vanish, so $p(z) = p(z-1) + p(z-2)$ exactly. This gives the solutions $(z-2\, z-1\, z)$ for $z = 2, 3, 4$, which we call *trivial*. (For $z = 2$ this uses $p(0) = p(1) = 1$.)

If we group the terms after $p(z-2)$ into pairs,

```math
g_2 = p(z-5) + p(z-7), \quad g_3 = p(z-12) + p(z-15), \quad g_4 = p(z-22) + p(z-26), \quad \ldots
```

then the pentagonal number theorem becomes

```math
p(z) = p(z-1) + p(z-2) - g_2 + g_3 - g_4 + g_5 - \cdots
```

**The pairs shrink.** Each pair uses smaller arguments than the one before, and $p$ is non-decreasing, so

```math
g_2 \ge g_3 \ge g_4 \ge \cdots \ge 0.
```

**The signs alternate, starting with minus.** So the tail can be written as

```math
-(g_2 - g_3) - (g_4 - g_5) - \cdots \le 0,
```

since each bracket is non-negative.

**Strictness.** For $z \ge 5$, $g_2 \ge p(0) = 1 > 0$. For $5 \le z \le 11$, $g_3 = 0$, so $g_2 - g_3 > 0$. For $z \ge 12$, the arguments of $g_3$ are smaller than those of $g_2$ and $p$ is strictly increasing for $n \ge 1$, so $g_3 < g_2$. Either way the tail is strictly negative, giving

```math
p(z) < p(z-1) + p(z-2) \quad \text{for all } z \ge 5.
```

For $z \le 4$ every argument in $g_2$ is negative, so $g_2 = 0$ and equality holds, which gives the trivial solutions.

which means
```math
p(z) < 2p(z-1)
```
so we cannot use the trivial bound $p(z) > 2p(z-1)$ to show there are finitely many solutions.


## Search method

The search is in [`Partition_Search.ipynb`](Partition_Search.ipynb).

1. **Computing $p(n)$.** All values $p(0), \dots, p(N)$ are computed exactly using the pentagonal number theorem recurrence.
2. **Reverse lookup.** A dictionary maps each value $p(n)$ back to $n$, so checking whether $p(z) - p(x)$ is a partition number takes one lookup.
3. **Narrowing the search.** For each $z$, we only need to check $x$ where $x$ satisfies:

$$p(z) - p(z-1) \le p(x) \le \tfrac{1}{2}p(z)$$

- The upper bound comes from assuming $x \le y$.
- The lower bound holds because if $p(x) < p(z) - p(z-1)$, then $p(z-1) < p(z) - p(x) < p(z)$, which lies strictly between two consecutive partition numbers and so cannot equal $p(y)$.

Since $p$ is increasing, both bounds are found by binary search.

Searching up to $N = 100{,}000$ takes about 20 seconds and finds only the seven solutions above.

## Heuristic argument

We model $p(z) - p(x)$ as a uniformly random integer of its size, and calculate the probability that it equals some $p(y)$.

To do this we estimate the gap between $p(n)$ and $p(n-1)$.

By the Hardy–Ramanujan formula,

$$\log p(n) \approx f(n) := \pi\sqrt{\frac{2n}{3}} - \log\left(4\sqrt{3}\,n\right).$$

Since $f$ is defined for all real $n$, the mean value theorem gives

$$f(n) - f(n-1) = f'(\xi) \quad \text{for some } \xi \in (n-1, n).$$

Differentiating gives

$$f'(n) = \frac{\pi}{\sqrt{6n}} - \frac{1}{n}$$

and differentiating again gives

$$f''(n) = -\frac{\pi}{2\sqrt{6}\,n^{3/2}} + \frac{1}{n^2}$$

Since $f''(n) = O(n^{-3/2})$, $f'(\xi) = f'(n) + O(n^{-3/2})$. The $\frac{1}{n}$ term is also smaller than $\frac{\pi}{\sqrt{6n}}$, so

$$\log p(n) - \log p(n-1) \approx \frac{\pi}{\sqrt{6n}}$$
