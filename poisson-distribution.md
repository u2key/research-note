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

## 3. C Program
```c
#include <stdio.h>
#include <math.h>

int main(int argc, char *argv[]) {
  double N, s, P;
  int k;

  printf("Average number of events per unit time: ");
  scanf("%lf", &N);
  printf("Evaluation time divided by unit time: ");
  scanf("%lf", &s);
  printf("Number of evaluation events");
  scanf("%d", &k);

  P = 1.0;
  for (int a = 0; a < k; a++) {
    P = P * ((N * s) / (double)(k - a));
  }
  P = P * exp((-1) * N * s);

  printf("P = %f\n", P);
  return 0;
}
```
