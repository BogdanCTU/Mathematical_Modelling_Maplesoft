= = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =

## SEMINAR 2: Solutions of the Difference Equations

### S2 - EXERCISE 1

Find the solution for the following initial value problems. Plot your data to observe patterns in the solutions:

(a) $a{n+1}$ = -1.2 * $a{n}$ + 50 , $a{0}$=1000;

```maple
> restart;

##### --- Define the Recurrence Relation ---
# Note: The coefficient is -1.2, not 1.2
deq := a(n+1) = -1.2 * a(n) + 50;

##### --- Solve the Initial Value Problem ---
# Find the general solution first (optional)
rsolve(deq, a(n));

# Solve with the initial condition a(0) = 1000
ans := rsolve(\{deq, a(0)=1000\}, a(n));

##### --- Define the Function for Evaluation ---
aa := unapply(ans, n);

##### --- Generate Data Points ---
# Display the first 10 values to check the oscillating pattern
[seq([n, aa(n)], n=0..10)];

##### --- Plot the Data ---
# Plot to observe the oscillating divergence caused by the negative coefficient < -1
plot([seq([n, aa(n)], n=0..20)],
    style=point,
    symbol=circle,
    title="Oscillating Divergence of a(n)",
    labels=["n", "a(n)"]
);
```
$$
\mathit{deq} := a \! \left(n +1\right)=- 1.2 a \! \left(n \right)+50
$$
$$
a \! \left(0\right) \left(-\frac{6}{5}\right)^{n}+\frac{250}{11}-\frac{250 \left(-\frac{6}{5}\right)^{n}}{11}
$$
$$
\mathit{ans} := \frac{10750 (-\frac{6}{5})^{n}}{11}+\frac{250}{11}
$$
$$
\mathit{aa} := n \mapsto \frac{10750\cdot (-\frac{6}{5})^{n}}{11}+\frac{250}{11}
$$
$$
\left[\left[0,1000\right],\left[1,-1150\right],\left[2,1430\right],\left[3,-1666\right],\left[4,\frac{10246}{5}\right],\left[5,-\frac{60226}{25}\right],\left[6,\frac{367606}{125}\right],\left[7,-\frac{2174386}{625}\right],\left[8,\frac{13202566}{3125}\right],\left[9,-\frac{78434146}{15625}\right],\left[10,\frac{474511126}{78125}\right]\right]
$$

![Plot](./Images/MM_GENERAL_FULL_DOCplot2d6.png)

---

### S2 - ESERCISE 2

Consider the simple interest formula: $S{n+1} = S{n} + p \cdot S{0}$
and the compound interest formula: $S{n+1} = S{n} + \frac{p}{r} \cdot S{n}$

There are three options to earn interest. Company A offers simple interest at a rate of 6%.

Company B offers compound interest at a 4\% rate with a conversion period of one month.

Company C offers compound interest at a 4\% rate with a conversion period of three months.

(a) Calculate for the three cases the amount on deposit after 5, 10, 15, and 20 years for any principal $S{0}$;

(b) Which interest offer maximizes the amount on deposit after 5, 10, 15,and 20 years?

```maple
> restart;

##### --- Part (a): Company A (Simple Interest) ---
##### Recurrence: S(n+1) = S(n) + p * S0
##### n = years, p = annual rate (0.06)
difeqA := SA(n+1) = SA(n) + p * S0;
solA := rsolve(\{difeqA, SA(0)=S0\}, SA(n));

##### Define function A(years, rate, principal)
CompA := unapply(solA, n, p, S0);

##### --- Part (a): Companies B and C (Compound Interest) ---
##### Recurrence: S(n+1) = S(n) + (p/k) * S(n)
##### n = periods, p = annual rate, k = frequency
difeqC := SC(n+1) = SC(n) + (p/k) * SC(n);
solC := rsolve(\{difeqC, SC(0)=S0\}, SC(n));

##### General helper function for compound interest
##### inputs: nperiods, annualrate, frequency, principal
HelperComp := unapply(solC, n, p, k, S0);

##### Define Company B (Monthly: k=12)
##### We convert input 't' (years) to months (t*12) automatically
CompB := (t, S0) -> HelperComp(t*12, 0.04, 12, S0);

##### Define Company C (Quarterly: k=4)
##### We convert input 't' (years) to quarters (t*4) automatically
CompC := (t, S0) -> HelperComp(t*4, 0.04, 4, S0);

##### --- Calculate and Compare for 5, 10, 15, 20 Years ---
##### We use S0 = 1000 to visualize the comparison
YearsList := [5, 10, 15, 20];
Principal := 1000;

print("Account Balance Calculation (Principal = 1000)");
printf("%-10s %-15s %-15s %-15s\n", "Years", "Company A", "Company B", "Company C");

for t in YearsList do
    valA := evalf(CompA(t, 0.06, Principal));
    valB := evalf(CompB(t, Principal));
    valC := evalf(CompC(t, Principal));

    printf("%-10d %-15.2f %-15.2f %-15.2f\n", t, valA, valB, valC);
end do;

##### --- Part (b): Maximize Amount ---
##### Plotting to visualize the crossover point (optional but helpful)
t := 't';
plot(
    [CompA(t, 0.06, 1000), CompB(t, 1000), CompC(t, 1000)],
    t=0..25,    # Increased to 25 to clearly see the crossover
    color=[red, blue, green],
    legend=["Company A (Simple 6%)", "Company B (Monthly 4%)", "Company C (Quarterly 4%)"],
    title="Growth Comparison Over Time",
    labels=["Years", "Amount"],
    thickness=2
);
```

$$ \text{difeqA} := \text{SA}(n +1) = \text{SA}(n) + p \cdot \mathit{S0} $$
$$
\text{solA} := \mathit{S0} + p \cdot \mathit{S0} \cdot (n +1) - p \cdot \mathit{S0}
$$
$$
\text{CompA} := (n, p, \mathit{S0}) \mapsto \mathit{S0} + p \cdot \mathit{S0} \cdot (n +1) - p \cdot \mathit{S0}
$$
$$
\text{difeqC} := \text{SC}(n +1) = \text{SC}(n) + \frac{p \cdot \text{SC}(n)}{k}
$$
$$
\text{solC} := \mathit{S0} \cdot \left(\frac{k + p}{k}\right)^{n}
$$
$$
\text{HelperComp} := (n, p, k, \mathit{S0}) \mapsto \mathit{S0} \cdot \left(\frac{k + p}{k}\right)^{n}
$$
$$
\text{CompB} := (t, \mathit{S0}) \mapsto \text{HelperComp}(12 \cdot t, 0.04, 12, \mathit{S0})
$$
$$
\text{CompC} := (t, \mathit{S0}) \mapsto \text{HelperComp}(4 \cdot t, 0.04, 4, \mathit{S0})
$$
$$
\mathit{YearsList} := [5, 10, 15, 20]
$$
$$
\mathit{Principal} := 1000
$$
$$
\text{"Account Balance Calculation (Principal = 1000)"}
$$
```maple
Years        Company A        Company B        Company C
```
$$
\text{valA} :=  1300.0
$$
$$
\text{valB} :=  1220.996570
$$
$$
\text{valC} :=  1220.190040
$$
```maple
5            1300.00          1221.00          1220.19
```
$$
\text{valA} :=  1600.0
$$
$$
\text{valB} :=  1490.832623
$$
$$
\text{valC} :=  1488.863734
$$
```maple
10           1600.00          1490.83          1488.86
```
$$
\text{valA} :=  1900.0
$$
$$
\text{valB} :=  1820.301519
$$
$$
\text{valC} :=  1816.696699
$$
```maple
15           1900.00          1820.30          1816.70
```
$$
\text{valA} :=  2200.0
$$
$$
\text{valB} :=  2222.581910
$$
$$
\text{valC} :=  2216.715217
$$
```maple
20           2200.00          2222.58          2216.72
```
$$
t := t
$$

![Plot](./Images/MM_GENERAL_FULL_DOCplot2d7.png)

                                                    

### S2 - ESERCISE 3:

The loan on a house is $200,000.

The mathematical model that describes the repayment of the loan is: Sn+1 = Sn +p/r Sn - R

(a) Calculate the monthly repayment R needed to have the loan repaid after 30 years. The annualy interest rate is 5%.

(b) Calculate the total amount paid back on the loan.

```maple
> restart;

##### --- Define the Model ---
##### S(n+1) = (1 + p/r)*S(n) - R
##### S0: Principal, p: Annual Rate, r: Frequency, R: Monthly Payment
LReq := S(n+1) = (1 + p/r) * S(n) - R;

##### Solve the recurrence relation generally
sol := rsolve(\{LReq, S(0)=S0\}, S(n));

##### Create a function for easier calculation --> Balance(n, p, r, S0, R)
Balance := unapply(sol, n, p, r, S0, R);

##### --- Part (a): Calculate Monthly Repayment (R) ---
Principal := 200000;
Rate := 0.05;
Freq := 12;
Years := 30;
TotalMonths := Years * Freq;

##### Balance after 360 months should be 0.
##### Solve the equation Balance = 0 for R.
eqtarget := Balance(TotalMonths, Rate, Freq, Principal, Rneeded) = 0;

##### Solve for R
Rval := solve(eqtarget, Rneeded);

##### Display result formatted as currency
printf("Part (a) Monthly Repayment: $%.2f\n", Rval);

##### --- Part (b): Total Amount Paid Back ---
##### Total = Monthly Payment * Number of Months
TotalPaid := Rval * TotalMonths;

##### Display result
printf("Part (b) Total Amount Paid: $%.2f\n", TotalPaid);
printf("Total Interest Paid: $%.2f\n", TotalPaid - Principal);

##### --- Visualization (Amortization Curve) ---
##### Plot the balance decreasing over the 30 years (360 months)
plot(
    Balance(n, Rate, Freq, Principal, Rval),
    n=0..TotalMonths,
    color=blue,
    thickness=2,
    title="Loan Balance Over 30 Years",
    labels=["Months", "Balance ($)"],
    gridlines=true
);
```

$$
\text{LReq} := S(n +1) = \left(1+\frac{p}{r}\right) S(n) - R
$$
$$
\mathit{sol} := \mathit{S0} \left(\frac{p +r}{r}\right)^{n}+\frac{r R}{p}-\frac{R r \left(\frac{p +r}{r}\right)^{n}}{p}
$$
$$
\mathit{Balance} := (n, p, r, \mathit{S0}, R) \mapsto \mathit{S0} \cdot \left(\frac{p +r}{r}\right)^{n}+\frac{r \cdot R}{p}-\frac{R \cdot r \cdot \left(\frac{p +r}{r}\right)^{n}}{p}
$$
$$
\mathit{Principal} := 200000
$$
$$
\mathit{Rate} :=  0.05
$$
$$
\mathit{Freq} := 12
$$
$$
\mathit{Years} := 30
$$
$$
\mathit{TotalMonths} := 360
$$
$$
\text{eqtarget} :=  893548.9696 - 832.2587640 \cdot \text{Rneeded} = 0
$$
$$
\text{Rval} :=  1073.643208
$$
(((maple
Part (a) Monthly Repayment: $1073.64
)))
$$
\mathit{TotalPaid} :=  386511.5549
$$
```maple
Part (b) Total Amount Paid: $386511.55
Total Interest Paid: $186511.55
```

![Plot](./Images/MM_GENERAL_FULL_DOCplot2d8.png)

                                                    

### S2 - ESERCISE 4:

Find the solutions of the following **difference equations** and plot them graphically.

(a) x(n+2) - 5x(n+1) + 6x(n) = 0; x0 = 1; x1 = 1;

```maple
> restart;

##### --- Define the Difference Equation ---
equationa := x(n+2) - 5*x(n+1) + 6*x(n) = 0;

##### --- General Solution (Optional check) ---
rsolve(equationa, x(n));

##### --- Solve with Initial Conditions ---
ansequationa := rsolve(\{equationa, x(0)=1, x(1)=1\}, x(n));

##### --- Define Function for Plotting ---
solutiona := unapply(ansequationa, n);

##### --- Generate Data Points ---
[seq([n, solutiona(n)], n=0..10)];

##### --- Plot the Data ---
plot([seq([n, solutiona(n)], n=0..8)],
    style=point,
    symbol=circle,
    color=blue,
    title="Solution of x(n+2) - 5x(n+1) + 6x(n) = 0",
    labels=["n", "x(n)"]
);
```

$$
\text{equationa} := x(n +2) - 5 x(n +1) + 6 x(n) = 0
$$
$$
-(x(1) - 3 x(0)) 2^{n} - (-x(1) + 2 x(0)) 3^{n}
$$
$$
\text{ansequationa} := 2 \cdot 2^{n} - 3^{n}
$$
$$
\text{solutiona} := n \mapsto 2 \cdot 2^{n} - 3^{n}
$$
$$
[[0,1], [1,1], [2,-1], [3,-11], [4,-49], [5,-179], [6,-601],
[7,-1931], [8,-6049], [9,-18659], [10,-57001]]
$$

![Plot](./Images/MM_GENERAL_FULL_DOCplot2d9.png)
