# Partition-sum-equation
This project focuses on finding solutions to the equation $p(x) + p(y) = p(z)$ with $1 \le x \le y < z$, where $p(n)$ is the number of partitions of $n$.

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

No further solutions exist for $z \le 20{,}000$.

## Pentagonal Number Theorem

```math
p(z) = p(z-1) + p(z-2) - p(z-5) - p(z-7) + p(z-12) + p(z-15) - \cdots
```

**Small solutions.** For $z \le 4$ the correction terms vanish, so $p(z) = p(z-1) + p(z-2)$ exactly. This gives the solutions $(z-2,\, z-1,\, z)$ for $z = 2, 3, 4$, which we call *trivial*. (For $z = 2$ this uses $p(0) = p(1) = 1$.)

For $z \ge 5$ we then get 
```math
p(z) < p(z-1) + p(z-2)
```

<!-- add your argument here for why the correction terms sum to a negative number -->

which means
```math
p(z) < 2p(z-1)
```
so we cannot use the trivial bound $p(z) > 2p(z-1)$ to show there are no solutions.
