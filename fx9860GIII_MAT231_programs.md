# MAT231 – Casio fx-9860GIII programs (Lectures 1, 3, 4, 5)

10 programs cover every example and exercise in the MAT231 lectures:
L1 Normal distribution, L3 Sampling methods, L4 CLT, L5 χ² / t / F.
Lecture 2 (AI agents, AIE 111) has no numerical problems, so it has no program.

---

## 0. How to type the code

Create a program: `MENU` → **PRGM** → `F3 (NEW)` → type the name → `EXE`.

| Symbol in the code | Where to find it |
|---|---|
| `→` | the **→** key (next to `,`) |
| `?` and `◢` | `SHIFT` `VARS`(PRGM) → `F4 (?)` / `F5 (◢)` |
| `If Then Else IfEnd For To Next` | `SHIFT` `VARS` → `F1 (COM)` |
| `Lbl Goto` | `SHIFT` `VARS` → `F3 (JUMP)` |
| `= ≠ > < ≥ ≤` | `SHIFT` `VARS` → `F6` → `F3 (REL)` |
| `ClrText` | `SHIFT` `VARS` → `F6` → `F1 (CLR)` → `F1` |
| `List  Dim  Sum` | `OPTN` → `F1 (LIST)` |
| `[nCr]` (shown as **C** on screen) | `OPTN` → `F6` → `F3 (PROB)` → `F3 (nCr)` |
| `RanInt#` | `OPTN` → `F6` → `F3 (PROB)` → `F4 (RAND)` → `F2 (Int)` |
| `Int  RndFix` | `OPTN` → `F6` → `F4 (NUM)` |
| `NormCD InvNormCD` | `OPTN` → `F5 (STAT)` → `F3 (DIST)` → `F1 (NORM)` |
| `tCD InvTCD` / `ChiCD InvChiCD` / `InvFCD` | same DIST menu → `F2 (t)` / `F3 (CHI)` / `F4 (F)` |
| `E` in `1E99` | the **×10ˣ** (EXP) key |
| negative sign in `-1E99` | the **(−)** key, not the minus key |
| `"` | `ALPHA` + `×10ˣ` key |

* Every line ends with `EXE` (a new line). `:` is also allowed as a separator.
* The text in quotes is only a label, so you can shorten it to save typing.
* `1E99` means "+infinity" and `-1E99` means "−infinity", for example P(X > a) → lower = a, upper = 1E99.
* `◢` pauses the program. Press `EXE` to see the next result.
* Lists are entered in `MENU` → **STAT** (List 1, List 2 …).

---

## 1. NORMPROB – any normal probability (L1 and L4)

Handles P(a < X < b) for one value (n = 1) and P(a < X̄ < b) for a sample mean (σ/√n), with σ or s.
It can round z to 2 decimals so the answer matches the z-table in the slides.

```
ClrText
"MU"?→M
"SIGMA OR S"?→S
"N (1=ONE X)"?→N
S÷√N→E
If N>1
Then "STD ERROR=":E◢
IfEnd
"LOWER (-1E99=NONE)"?→A
"UPPER (1E99=NONE)"?→B
(A-M)÷E→Y
(B-M)÷E→Z
"ROUND Z 2DP? 1=YES"?→T
If T=1
Then RndFix(Y,2)→Y
RndFix(Z,2)→Z
IfEnd
If A>-1E98
Then "Z1=":Y◢
IfEnd
If B<1E98
Then "Z2=":Z◢
IfEnd
NormCD(Y,Z,1,0)→P
"P=":P
```

## 2. INVNORM – find x when the area is known (L4, P(X̄ < x) = 0.95)

```
ClrText
"MU"?→M
"SIGMA OR S"?→S
"N (1=ONE X)"?→N
"AREA LEFT OF X"?→P
S÷√N→E
InvNormCD(P,1,0)→Z
"Z=":Z◢
M+Z×E→X
"X=":X
```
For an area to the **right**, enter `1 − area`.
If your OS shows a syntax error, use `InvNormCD(-1,P,1,0)`, where −1 means the left tail.

## 3. SAMPMEAN – all samples of size 2 or 3 and the sampling distribution (L3)

Put the population in **List 1** first. The output goes to these lists:
List 2, List 3 and List 4 hold the members of each sample (List 4 is 0 when n = 2), and **List 6** holds each sample's mean.

```
ClrText
"POPULATION=LIST 1"
"SAMPLE SIZE 2 OR 3"?→R
Dim List 1→N
N[nCr]R→M
M→Dim List 2
M→Dim List 3
M→Dim List 4
M→Dim List 6
0→K
For 1→I To N-R+1
For I+1→J To N-R+2
If R=2
Then K+1→K
List 1[I]→List 2[K]
List 1[J]→List 3[K]
(List 1[I]+List 1[J])÷2→List 6[K]
Else
For J+1→L To N
K+1→K
List 1[I]→List 2[K]
List 1[J]→List 3[K]
List 1[L]→List 4[K]
(List 1[I]+List 1[J]+List 1[L])÷3→List 6[K]
Next
IfEnd
Next
Next
Sum List 1÷N→A
√(Sum ((List 1-A)²)÷N)→S
Sum List 6÷M→B
√(Sum ((List 6-B)²)÷M)→T
"NO. SAMPLES=":M◢
"POP MEAN=":A◢
"MEAN OF MEANS=":B◢
"POP SD=":S◢
"SD OF MEANS=":T◢
"SEE LIST 2,3,4,6"
```

## 4. POPSTAT – population mean, σ and standard error from data (L4 Spence Sprockets)

```
ClrText
"DATA IN LIST 1"
"SAMPLE SIZE N"?→N
Dim List 1→D
Sum List 1÷D→A
√(Sum ((List 1-A)²)÷D)→S
"POP MEAN=":A◢
"POP SD=":S◢
"STD ERROR=":S÷√N
```

## 5. UNIFCLT – CLT for a continuous uniform population (L4)

```
ClrText
"A"?→A
"B"?→B
"N"?→N
(A+B)÷2→M
(B-A)²÷12→V
"MU=":M◢
"VAR X=":V◢
"VAR XBAR=":V÷N◢
"SD XBAR=":√(V÷N)
```

## 6. BALLS – exactly x successes, with and without replacement (L3)

```
ClrText
"TOTAL ITEMS"?→N
"NO. OF WANTED TYPE"?→K
"NO. DRAWN"?→D
"EXACTLY HOW MANY"?→X
(K[nCr]X)×((N-K)[nCr](D-X))÷(N[nCr]D)→H
"NO REPLACE P=":H◢
K÷N→Q
(D[nCr]X)×Q^X×(1-Q)^(D-X)→B
"WITH REPLACE P=":B
```

## 7. SAMPLING – simple random sample probability and systematic k (L3)

```
ClrText
"POPULATION SIZE"?→N
"SAMPLE SIZE"?→S
S÷N→P
"SRS P(EACH)=":P◢
Int (N÷S)→K
"SYSTEMATIC K=":K◢
RanInt#(1,K)→R
"RANDOM START=":R◢
"FIRST 5 PICKED:"
For 0→I To 4
R+I×K◢
Next
```

## 8. STRATA – proportional stratified allocation (L3)

Put the stratum sizes in **List 1** first.
```
ClrText
"STRATA SIZES=LIST 1"
"TOTAL SAMPLE"?→S
Sum List 1→N
List 1÷N→List 2
Dim List 1→D
D→Dim List 3
For 1→I To D
Int (List 2[I]×S+0.5)→List 3[I]
Next
"REL FREQ=LIST 2"
"SAMPLED=LIST 3"
"TOTAL=":Sum List 3
```

## 9. CHIGOF – χ² goodness-of-fit test (L5)

Put the observed counts in **List 1**. Put either the claimed probabilities or the expected counts in **List 2**.
```
ClrText
"OBSERVED=LIST 1"
"LIST2: 1=PROBS 2=EXPECTED"?→T
Sum List 1→N
If T=1
Then N×List 2→List 3
Else List 2→List 3
IfEnd
Sum ((List 1-List 3)²÷List 3)→X
Dim List 1→D
D-1→D
"ALPHA"?→A
InvChiCD(A,D)→C
ChiCD(X,1E99,D)→P
"EXPECTED=LIST 3"
"CHI2=":X◢
"DF=":D◢
"CRITICAL=":C◢
"P-VALUE=":P◢
If X>C
Then "REJECT H0"
Else "FAIL TO REJECT H0"
IfEnd
```

## 10. DISTTAB – replaces the χ², t and F tables (L5)

Press `AC/ON` to exit.
```
Lbl 0
ClrText
"1:CHI2 AREA ABOVE X"
"2:CHI2 CRITICAL"
"3:T CRITICAL"
"4:T AREA ABOVE X"
"5:F CRITICAL"
"CHOICE"?→C
If C=1
Then "X"?→X
"DF"?→D
"AREA=":ChiCD(X,1E99,D)◢
IfEnd
If C=2
Then "ALPHA RIGHT"?→A
"DF"?→D
"CHI2=":InvChiCD(A,D)◢
IfEnd
If C=3
Then "AREA RIGHT"?→A
"DF"?→D
"T=":InvTCD(A,D)◢
IfEnd
If C=4
Then "X"?→X
"DF"?→D
"AREA=":tCD(X,1E99,D)◢
IfEnd
If C=5
Then "ALPHA"?→A
"DF1 NUMERATOR"?→N
"DF2 DENOMINATOR"?→D
"F=":InvFCD(A,N,D)◢
IfEnd
Goto 0
```
For a two-tailed t with α = 5%, enter AREA RIGHT = 0.025.

---

# What to enter for every problem

"ROUND=1" is the answer when you round z to 2 dp, which is the z-table method used in the slides. "ROUND=0" is the exact answer. The slides add rounded table values, so their last digit can differ by 1.

## Lecture 1 – Normal distribution → `NORMPROB` (N = 1)

| Problem | MU, SIGMA | LOWER, UPPER | z | ROUND=1 | ROUND=0 |
|---|---|---|---|---|---|
| z for $1100 / $900 | 1000, 100 | 1100 / 900 (other = ±1E99) | 1.00 / −1.00 | – | – |
| P(1000<X<1100) | 1000, 100 | 1000, 1100 | 0, 1 | 0.3413 | 0.3413 |
| Ex2 P(790<X<1000) | 1000, 100 | 790, 1000 | −2.10 | 0.4821 | 0.4821 |
| Ex3 P(X<790) | 1000, 100 | −1E99, 790 | −2.10 | 0.0179 | 0.0179 |
| Ex4 P(840<X<1200) | 1000, 100 | 840, 1200 | −1.6, 2 | 0.9225 (slides 0.9224) | 0.9225 |
| P(1150<X<1250) | 1000, 100 | 1150, 1250 | 1.5, 2.5 | 0.0606 | 0.0606 |
| Cartwright (a) 400–482 | 400, 50 | 400, 482 | 1.64 | 0.4495 | 0.4495 |
| Cartwright (b) >482 | 400, 50 | 482, 1E99 | 1.64 | 0.0505 | 0.0505 |
| Hand ex 1) 1000–1090 | 1000, 100 | 1000, 1090 | 0.9 | 0.3159 | |
| 2) 810–1000 | 1000, 100 | 810, 1000 | −1.9 | 0.4713 | |
| 3) X ≤ 800 | 1000, 100 | −1E99, 800 | −2 | 0.0228 | |
| 4) X > 900 | 1000, 100 | 900, 1E99 | −1 | 0.8413 | |
| 5) X ≤ 1050 | 1000, 100 | −1E99, 1050 | 0.5 | 0.6915 | |
| 6) X > 1150 | 1000, 100 | 1150, 1E99 | 1.5 | 0.0668 | |
| 7) 1050–1080 | 1000, 100 | 1050, 1080 | 0.5, 0.8 | 0.0967 (slides 0.0966) | 0.0967 |
| **Ex 17** a / b / c | 50, 4 | 44,55 / 55,1E99 / 52,55 | | 0.8275 / 0.1056 / 0.2029 | same |
| **Ex 18** a / b / c | 80, 14 | 75,90 / −1E99,75 / 55,70 | | 0.4017 / 0.3594 / 0.2021 | 0.4020 / 0.3605 / 0.2005 |
| **Ex 19** a / b / c | 7, 0.1 | 7.1,7.25 / 7.25,1E99 / 6.8,7.25 | | 0.1524 / 0.0062 / 0.9710 | same |
| **Ex 20** a / b / c | 70000, 20000 | 80000,1E99 / 65000,80000 / 65000,1E99 | | 0.3085 / 0.2902 / 0.5987 | same |
| **Ex 21** a / b / c | 15, 3.5 | 20,1E99 / −1E99,20 / 10,12 | | 0.0764 / 0.9236 / 0.1185 | 0.0766 / 0.9234 / 0.1191 |
| **Ex 22** a / b / c | 36280, 3300 | 35000,40000 / 45000,1E99 / 40000,45000 | | 0.5225 / 0.0041 / 0.1251 | 0.5211 / 0.0041 / 0.1257 |

Ex 22 asks for percentages: multiply by 100, which gives about 52%, 0.4% and 12.6%.

## Lecture 3 – Sampling

| Problem | Program and input | Answer |
|---|---|---|
| Box 10 white / 5 black, draw 4, exactly 2 black | `BALLS`: 15, 5, 4, 2 | no replacement **0.3297**, with replacement **0.2963** |
| 250 employees, sample 25 | `SAMPLING`: 250, 25 | P = **0.1**, k = 10 |
| Nitra 845 employees, sample 52 | `SAMPLING`: 845, 52 | k = **16** |
| 1000 employees, sample 100 | `SAMPLING`: 1000, 100 | k = **10**, start 1–10 |
| Stratified 352 firms, sample 50 | List 1 = {8,35,189,115,5}, `STRATA`: 50 | **1, 5, 27, 16, 1** (total 50) |
| Tartus Industries | List 1 = {7,7,8,8,7,8,9}, `SAMPMEAN`: 2 | 21 samples, μ = μx̄ = **7.714**; means 7, 7.5, 8, 8.5 appear 3, 9, 6, 3 times |
| **Ex 1** {12,12,14,16} | `SAMPMEAN`: 2 | 6 samples: 12, 13, 14, 13, 14, 15; μx̄ = μ = **13.5**; σ = 1.658 > σx̄ = 0.957 |
| **Ex 2** {2,2,4,4,8} | `SAMPMEAN`: 2 | 10 samples; μx̄ = μ = **4**; σ = 2.191 > σx̄ = 1.342 |
| **Ex 3** {12,12,14,15,20} | `SAMPMEAN`: 3 | 10 samples; μx̄ = μ = **14.6**; σ = 2.939 > σx̄ = 1.2 |
| **Ex 4** {0,0,1,3,6} | `SAMPMEAN`: 3 | 10 samples; μx̄ = μ = **2**; σ = 2.280 > σx̄ = 0.931 |

## Lecture 4 – CLT and the sample mean → `NORMPROB` (N > 1)

| Problem | MU, SIGMA, N | LOWER, UPPER | Answer |
|---|---|---|---|
| Spence Sprockets (40 values in List 1) | `POPSTAT`, N = 5 | – | μ = 4.8, σ = 4.859, SE = 2.173 |
| Uniform 4 to 6, n = 40 | `UNIFCLT`: 4, 6, 40 | – | μ = 5, σ² = 1/3, Var(X̄) = 1/120 = 0.00833, SD = 0.0913 |
| Cola 31.2, 0.4, n = 16 | 31.2, 0.4, 16 | 31.38, 1E99 | z = 1.80, **0.0359** |
| Water bottle | 30, 1.5, 45 | −1E99, 29.85 | SE 0.2236, z = −0.67, **0.2514** (exact 0.2512) |
| Female weights | 65, 14, 50 | 63, 1E99 | SE 1.9799, z = −1.01, **0.8438** |
| Resistors | 100, 10, 25 | −1E99, 95 | SE 2, z = −2.5, **0.0062** |
| σ unknown, s = 10 | 50, 10, 100 | 51, 1E99 | z = 1, **0.1587** (z of x̄=52 is 2) |
| Problem 197 < X̄ < 203 | 200, 15, 100 | 197, 203 | z = ±2, **0.9545** |
| Income P(X̄ < x) = 0.95 | `INVNORM`: 3000, 600, 100, 0.95 | – | z = 1.645, x = **3098.7** (slides use z = 1.64, giving 3098.4) |
| **Ex 15** a / b / c | 60, 12, 9 | 63,1E99 / −1E99,56 / 56,63 | **0.2266 / 0.1587 / 0.6147** |
| **Ex 16** a / b / c / d | 75, 5, 40 | −1E99,74 / 74,76 / 76,77 / 77,1E99 | **0.1038 / 0.7923 / 0.0981 / 0.0057** (exact 0.1030 / 0.7941 / 0.0972 / 0.0057) |
| **Ex 17** rent | 2200, 250, 50 | 1950, 1E99 | z = −7.07, P ≈ **1** |
| **Ex 18** IRS: b SE, c / d / e | 330, 80, 40 | 320,1E99 / 320,350 / 350,1E99 | SE = **12.65**; **0.7852 / 0.7282 / 0.0571** (exact 0.7854 / 0.7285 / 0.0569) |

Ex 18 a: n = 40 ≥ 30, so by the CLT you need no assumption about the shape of the population.

## Lecture 5 – χ², t and F

| Problem | Program and input | Answer |
|---|---|---|
| Die table 8,12,18,9,7,6 (E = 10) | List 1 = obs, List 2 = {10,10,10,10,10,10}, `CHIGOF` with 2 | χ² = **9.8**, df 5 |
| Fair die, 120 rolls, α = 0.05 | List 1 = {18,22,19,24,20,17}, List 2 = {1/6 ×6}, `CHIGOF` with 1 and 0.05 | χ² = **1.7**, crit **11.070**, fail to reject H₀ |
| Majors A, B, C, D | List 1 = {62,48,55,35}, List 2 = {.3,.25,.25,.2}, `CHIGOF` with 1 and 0.05 | χ² = **1.272**, df 3, crit **7.815**, fail to reject |
| Area above 17, df = 9 | `DISTTAB` 1: 17, 9 | 0.0487, so answer **(c) between 0.02 and 0.05** |
| Area above 30, df = 10 | `DISTTAB` 1: 30, 10 | 0.00086, so answer **(c) less than 0.001** |
| t vs z, n = 5, 95% | `DISTTAB` 3: 0.025, 4 | **2.776** (z is 1.96) |
| F upper 5%, df 4 and 5 | `DISTTAB` 5: 0.05, 4, 5 | **5.19** |

`DISTTAB` also replaces any χ² critical value from the table (option 2), for example α = 0.05 with df 3 gives 7.815.
