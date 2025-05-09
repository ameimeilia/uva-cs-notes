# 2 - Recurrence and Matrix Multiplication

## Recurrence Solving Techniques
1. Tree
2. Guess/Check
3. “Cookbook”
4. Substitution

## Tree Method
$T(n) = 2T(\frac{n}{2}) +n$ → $T(n) = \sum_{i=1}^{log_2n}n = nlog_2n$
![[Screenshot 2025-02-23 at 2.37.12 PM.png | center | 500]]

### (Aside) Finite Geometric Series
![[Screenshot 2025-02-23 at 2.42.34 PM.png | center | 400]]

![[Screenshot 2025-02-23 at 2.42.52 PM.png | center | 400]]

![[Screenshot 2025-02-23 at 2.43.04 PM.png | center | 400]]

## Matrix Multiplication
- given two $n$ digit numbers:
	- input size is measured by the number of digits
	- run time is the number of elementary operations (single-digit multiplications)

### ”Schoolbook” Multiplication
![[Screenshot 2025-02-23 at 2.47.26 PM.png | center | 500]]

### Divide and Conquer Multiplication
- for simplicity, assume that $n=2^k$ is a power of $2$
- **Divide**: break $n$-digit numbers into four numbers of $n/2$ digits each (call them $a,b,c,d$)
- **Conquer**:
	- if $n>1$: recursively compute $ac,ad,bc,bd$
	- if $n=1$: compute $ac,ad,bc,bd$ directly (base case)
- **Combine**: $10^n(ac)+10^{n/2}(ad+bc)+bd$
- **Recurrence**: $T(n) = 4T(\frac{n}{2})+5n$ (compute 4 multiplications, each of size $n/2$, then add 2 shifts and 3 additions on $n$ bit values)
![[Screenshot 2025-02-23 at 2.48.28 PM.png | center | 400]]

*Example - Simplify $T(n) = 3T(\frac{n}{2})+8n$*
![[Screenshot 2025-02-23 at 3.11.36 PM.png | center | 500]]

- end result is same as the schoolbook method
![[Screenshot 2025-02-23 at 3.12.33 PM.png | center | 250]]

### Karatsuba Multiplication
- **Divide**: break $n$-digit numbers into four numbers of $n/2$ digits each (call them $a,b,c,d$)
- **Conquer**:
	- if $n>1$: recursively compute $ac,bd,(a+b)(c+d)$
	- if $n=1$: compute  $ac,bd,(a+b)(c+d)$ directly (base case)
- **Combine**: $10^n(ac)+10^{n/2}((a+b)(c+d)-ac-bd)+bd$
- **Recurrence**: $T(n) = 3T(\frac{n}{2})+8n$ (compute 3 multiplications, each of size $n/2$, then add 2 shifts and 6 additions on $n$-bit values)
![[Screenshot 2025-02-23 at 2.59.05 PM.png | center | 500]]

> [!example] Karatsuba Multiplication Pseudocode
$x \leftarrow$ Karatsuba$(a,c)$
$y \leftarrow$ Karatsuba$(a,d)$
$z \leftarrow$ Karatsuba$(a+b,c+d)-x-y$
return $10^nx + 10^{n/2}z+y$

*Example - Simplify $T(n) = 3T(\frac{n}{2})+8n$
![[Screenshot 2025-02-23 at 3.13.34 PM.png | center | 500]]

- end result is strictly better than the schoolbook method
![[Screenshot 2025-02-23 at 3.21.12 PM.png | center]]

![[Screenshot 2025-02-23 at 3.21.48 PM.png | center | 400]]

## Guess and Check
- **Show**: $T(n) = O(g(n))$
- **Consider**: $g_*(n) = c \cdot g(n)$
- **Goal**: show $\exists n_0$ such that $\forall n > n_0,  \ T(n) ≤ g_*(n)$
- **Technique**: Induction
	- Base cases: Show $T(1) ≤ g_*(1)$ (sometimes may need to consider additional base cases)
	- Hypothesis: $\forall n ≤ x_0, \ T(n) ≤ g_*(n)$
	- Inductive step: Show that $T(x_0 + 1) ≤ g_*(x_0 +1)$

### Mergesort Guess and Check Summary
- $T(n) = 2T(\frac{n}{2})+n$
- **Goal**: $T(n) ≤ nlog_2n = O(nlog_2n)$
- **Base cases**:
	- $T(1) = 0$
	- $T(2) = 2 ≤ 2log_22$
	- … up to some small $k$
- **Hypothesis**: $\forall n ≤ x_0, \ T(n) ≤ nlog_2n$
- **Inductive Step**: $T(x_0+1) ≤ (x_0+1)log_2(x_0+1)$

### Karatsuba Analysis Guess and Check Summary
 $T(n) = 3T(\frac{n}{2})+8n$
- **Goal**: $T(n) ≤ 3000n^{1.6} = O(n^{1.6})$
- **Base case**: $T(1) = 8 ≤ 3000$
- **Hypothesis**: $\forall n ≤ x_0, \  T(n) ≤ 3000n^{1.6}$
- **Inductive Step**: Show $T(x_0+1) ≤ 3000(x_0+1)^{1.6}$

![[Screenshot 2025-02-23 at 3.39.36 PM.png | center | 500]]

## Master Theorem (“Cookbook”)
- 3 cases:
![[Screenshot 2025-02-23 at 3.42.20 PM.png | center]]

- compare:
	- $f(n)$: how much work per subproblem
	- the number of subproblems per level: $a^{log_bn} = n^{log_ba}$
![[Screenshot 2025-02-23 at 3.45.34 PM.png | center | 500]]
![[Screenshot 2025-02-23 at 3.46.00 PM.png]]