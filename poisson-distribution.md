# Poisson Distribution

## 1. Probability that an event that occurs an average of N times per second will occur k times

```math
{\Large
\begin{align}
P(k) &=& \lim_{n \to \infty} {}_n C_k \left ( \dfrac{N}{n} \right )^{k} \left ( \dfrac{N}{n} \right )^{n - k} \\
&=& \lim_{n \to \infty} \dfrac{n \cdot \left ( n - 1 \right ) \cdots \left (n - \left ( k - 1\right ) \right)}{k !} \left ( \dfrac{N}{n} \right ) ^{k} \left ( 1 - \dfrac{N}{n} \right ) ^{n - k} \\
&=& \lim_{n \to \infty} \prod ^{k - 1} _{i = 0} \left ( \dfrac{n - i}{n} \right ) \dfrac{N^{k}}{k !} \left ( 1 - \dfrac{N}{n} \right ) ^{n - k} \\
&=& \lim_{n \to \infty} \prod ^{k - 1} _{i = 0} \left ( 1 - \dfrac{i}{n} \right ) \dfrac{N^{k}}{k !} \left ( 1 - \dfrac{N}{n} \right ) ^{n} \left ( 1 - \dfrac{N}{n} \right ) ^{- k} \\
&=& \lim_{n \to \infty} \prod ^{k - 1} _{i = 0} 1 \cdot \dfrac{N^{k}}{k !} \cdot e^{- N} \cdot 1 \\
&=& \dfrac{N^{k}}{k !} \cdot e^{- N} \\
\end{align}
}
```

## 2. Probability that an event that occurs an average of N times per second will occur k times in s seconds

```math
{\Large
\begin{align}
P_s (k) &=& \dfrac{\left ( Ns \right )^{k}}{k !} \cdot e^{- \left ( Ns \right )} \\
\end{align}
}
```

## 3. Python Program
```python
#! /usr/bin/python3

import numpy as np

N = np.float128(input("Average number of events per unit time: "))
s = np.float128(input("Evaluation time divided by unit time: "))
k_min = int(input("Number of evaluation events minimum: "))
k_max = int(input("Number of evaluation events maximum: "))
k_step = int(input("Number of evaluation events step: "))

with open("poisson.csv", "w") as f:
  for k in range(k_min, k_max+1, k_step): 
    P = np.float128(1.0)
    for a in range(k):
      P *= ((N * s) / np.float128(k - a))
    P *= np.exp((-1) * N * s)
    f.write(f"{k:5d},{P:1.20e}\n")
    print(f"k: {k:5d}, P: {P:1.20e}\r", end="")
```
