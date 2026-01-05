%% Created by Maple 2025.1, Windows 11
%% Source Worksheet: MM_GENERAL_FULL_DOC
%% Generated: Mon Jan 05 16:53:47 EET 2026
## SEMINAR 1: Modelling Change with Difference Equations
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### EXERCIE 1

Write the first five terms of the following sequences:

(a) $a_{n+1}$ = 3*$a_{n}$ , $a_{0}$ = 1

```maple
> a[0] := 1000;
for i from 0 to 5 do
    a[i + 1] := 1.01*a[i];
end do;
a[5];
seq(a[i], i = 0 .. 5);
plot([[m, a[m]] $ (m = 1 .. 5)], style = point, symbol = circle);
```
$$
a_{0}:= 1000
$$
$$
a_{1}:=  1010.0
$$
$$
a_{2}:=  1020.1000
$$
$$
a_{3}:=  1030.301000
$$
$$
a_{4}:=  1040.604010
$$
$$
a_{5}:=  1051.010050
$$
$$
a_{6}:=  1061.520150
$$
$$
1051.010050
$$
$$
1000, 1010.0, 1020.1000, 1030.301000, 1040.604010, 1051.010050
$$
![Plot](MM_GENERAL_FULL_DOCplot2d1.png)

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### EXERCISE 2

Write the first 5 terms of the sequence satisfying the following difference equations and draw the corresponding graph of the generated dynamical system:

$Δa_{n}$ =
$ \frac{1}{3}a_ {{n }} $ , $a_{0}$=1
```maple
> restart;

##### --- Define the Data ---
a[0] := 1;
for n from 0 to 5 do
    a[n + 1] := 3*a[n];
end do;
seq(a[i], i = 0 .. 5);

##### --- Plot the Data ---
plot([[h, a[h]] $ (h = 0 .. 5)], style = point);
```
$$
a_{0}:= 1
$$
$$
a_{1}:= 3
$$
$$
a_{2}:= 9
$$
$$
a_{3}:= 27
$$
$$
a_{4}:= 81
$$
$$
a_{5}:= 243
$$
$$
a_{6}:= 729
$$
$$
1,3,9,27,81,243
$$
![Plot](MM_GENERAL_FULL_DOCplot2d2.png)

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### EXERCISE 3

(Decay of Digoxin in the Bloodstream) Digoxin is used in treatment of heart disease.\

Doctors must prescribe an amount of medicine that keeps the concetration of digoxin in the bloodstream above an e§ective level without exceeding the safe level.
For an initial dosage of 0:5 mg in the bloodstream , table shows the amount of digoxin an remaining in the bloodstream of a particular pacient after n days.

n 0 1 2 3 4 5 6 7 8\

an 0.5 0.345 0.238 0.164 0.113 0.078 0.054 0.037 0.026

(a) Get $Δa_{n}$ and plot $Δa_{n}$ versus $a_{n}$;

(b) Find a proportional constant for $Δa_{n}$ and $a_{n}$

```maple
> restart;

##### --- Define the Data ---
days := [0,1,2,3,4,5,6,7,8];
an := [0.5, 0.345, 0.238, 0.164, 0.113, 0.078, 0.054, 0.037, 0.026];

##### --- Part (a): Calculate Delta a_\{n\} and Plot ---
for i from 1 to 8 do
    delta_an[i] := an[i+1] - an[i];
end do:

##### Plot Delta a_\{n\} (y-axis) versus a_\{n\} (x-axis)
plot(
    [seq([an[i], delta_an[i]], i=1..8)],
    style=point,
    symbol=circle,
    symbolsize=15,
    color=blue,
    labels=["Amount (a\{n\})", "Change (Delta a\{n\})"],
    title="Change in Digoxin vs Amount in Bloodstream",
    labeldirections=[HORIZONTAL, VERTICAL]
);

##### --- Part (b): Find the Proportional Constant ---
##### The relationship is Delta a_\{n\} = k * a_\{n\}, therefore, k = Delta a_\{n\} / a_\{n\}
##### Calculate this ratio for every point to check consistency

ratios := [seq(delta_an[i] / an[i], i=1..8)];

##### Calculate the average of these ratios to find the constant 'k'
##### 'add' sums the list, 'nops' counts the number of items
k_constant := add(x, x in ratios) / nops(ratios);

print("The proportional constant k is approximately:", k_constant);
```
$$
\mathit{days} := \left[0,1,2,3,4,5,6,7,8\right]
$$
$$
\mathit{an} := \left[ 0.5, 0.345, 0.238, 0.164, 0.113, 0.078, 0.054, 0.037, 0.026\right]
$$

![Plot](MM_GENERAL_FULL_DOCplot2d3.png)

$$
\mathit{ratios} := \left[- 0.3100000000,- 0.3101449275,- 0.3109243697,- 0.3109756098,- 0.3097345133,- 0.3076923077, - 0.3148148148,- 0.2972972973\right]
$$
$$
\textit{k} := - 0.3089479800
$$
$$
\text{``The proportional constant k is approximately:''},- 0.3089479800
$$

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S1 - EXERCISE 4

The following data were obtained for the growth of a sheep population introduced into new enviroment on the island of Tasmania:
Year 1814 1824 1834 1844 1854 1864\

Population 125 275 830 1200 1750 1650

(a) Plot the data. Is there a trend?

(b) Formulate discrete dynamical system that resonable approximate the change observed. Compare real data with estimated data obtained from the model.

```maple
> restart;
with(plots):
with(Statistics):

##### --- Define the Data ---
Years := Vector([1814, 1824, 1834, 1844, 1854, 1864]);
Population := Vector([125, 275, 830, 1200, 1750, 1650]);

##### --- Plot the Data ---
data_plot := pointplot(Years, Population,
    style=point,
    symbol=solidcircle,
    symbolsize=15,
    color=blue,
    labels=["Year", "Population"],
    title="Sheep Population in Tasmania (1814-1864)",
    gridlines=true
);

display(data_plot);
```
![Plot](MM_GENERAL_FULL_DOCplot2d4.png)
![Plot](MM_GENERAL_FULL_DOCplot2d5.png)

\newpage
\

= = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =

## SEMINAR 2: Solutions of the Difference Equations

### S2 - }\textbf{EXERCISE 1

Find the solution for the following initial value problems. Plot your data to observe patterns in the solutions:

(a) $a_{n+1}$ = -1.2 * $a_{n}$ + 50 , $a_{0}$=1000;

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
![Plot](MM_GENERAL_FULL_DOCplot2d6.png)

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S2 - }\textbf{ESERCISE 2:

Consider the simple interest formula:
$S_{n+1}$ = $S_{n}$ + p * $S_{0}$
and the compound interest formula\

$S_{n+1}$ = $S_{n}$ +

$ \frac{p}{r} $
* $S_{n}$
There are three options to earn interest. Company A offers simple interest at a rate of 6\%.\

Company B offers compound interest at a 4\% rate with a conversion period of one month.
Company C offers compound interest at a 4\% rate with a conversion period of three months.

(a) Calculate for the three cases the amount on deposit after 5, 10, 15, and 20 years for any principal $S_{0}$;

(b) Which interest offer maximizes the amount on deposit after 5, 10, 15,and 20 years?

```maple
> restart;

##### --- Part (a): Company A (Simple Interest) ---
##### Recurrence: S(n+1) = S(n) + p * S0
##### n = years, p = annual rate (0.06)
dif_eq_A := S_A(n+1) = S_A(n) + p * S0;
sol_A := rsolve(\{dif_eq_A, S_A(0)=S0\}, S_A(n));

##### Define function A(years, rate, principal)
Comp_A := unapply(sol_A, n, p, S0);

##### --- Part (a): Companies B and C (Compound Interest) ---
##### Recurrence: S(n+1) = S(n) + (p/k) * S(n)
##### n = periods, p = annual rate, k = frequency
dif_eq_C := S_C(n+1) = S_C(n) + (p/k) * S_C(n);
sol_C := rsolve(\{dif_eq_C, S_C(0)=S0\}, S_C(n));

##### General helper function for compound interest
##### inputs: n_periods, annual_rate, frequency, principal
Helper_Comp := unapply(sol_C, n, p, k, S0);

##### Define Company B (Monthly: k=12)
##### We convert input 't' (years) to months (t*12) automatically
Comp_B := (t, S0) -> Helper_Comp(t*12, 0.04, 12, S0);

##### Define Company C (Quarterly: k=4)
##### We convert input 't' (years) to quarters (t*4) automatically
Comp_C := (t, S0) -> Helper_Comp(t*4, 0.04, 4, S0);

##### --- Calculate and Compare for 5, 10, 15, 20 Years ---
##### We use S0 = 1000 to visualize the comparison
YearsList := [5, 10, 15, 20];
Principal := 1000;

print("Account Balance Calculation (Principal = 1000)");
printf("%-10s %-15s %-15s %-15s\n", "Years", "Company A", "Company B", "Company C");

for t in YearsList do
    val_A := evalf(Comp_A(t, 0.06, Principal));
    val_B := evalf(Comp_B(t, Principal));
    val_C := evalf(Comp_C(t, Principal));

    printf("%-10d %-15.2f %-15.2f %-15.2f\n", t, val_A, val_B, val_C);
end do;

##### --- Part (b): Maximize Amount ---
##### Plotting to visualize the crossover point (optional but helpful)
t := 't';
plot(
    [Comp_A(t, 0.06, 1000), Comp_B(t, 1000), Comp_C(t, 1000)],
    t=0..25,   # Increased to 25 to clearly see the crossover
    color=[red, blue, green],
    legend=["Company A (Simple 6%)", "Company B (Monthly 4%)", "Company C (Quarterly 4%)"],
    title="Growth Comparison Over Time",
    labels=["Years", "Amount"],
    thickness=2
);
```
$$
\textit{dif\_eq\_A} := \textit{S\_A} \! \left(n +1\right)=\textit{S\_A} \! \left(n \right)+p \mathit{S0}
$$
$$
\textit{sol\_A} := \mathit{S0} +p \mathit{S0} (n +1)-p \mathit{S0}
$$
$$
\textit{Comp\_A} := \left(n ,p ,\mathit{S0} \right)\hiderel{\mapsto }\mathit{S0} +p \cdot \mathit{S0} \cdot \left(n +1\right)-p \cdot \mathit{S0}
$$
$$
\textit{dif\_eq\_C} := \textit{S\_C} \! \left(n +1\right)=\textit{S\_C} \! \left(n \right)+\frac{p \textit{S\_C} \! \left(n \right)}{k}
$$
$$
\textit{sol\_C} := \mathit{S0} \left(\frac{k +p}{k}\right)^{n}
$$
$$
\textit{Helper\_Comp} := \left(n ,p ,k ,\mathit{S0} \right)\hiderel{\mapsto }\mathit{S0} \cdot \left(\frac{k +p}{k}\right)^{n}
$$
$$
\textit{Comp\_B} := \left(t ,\mathit{S0} \right)\hiderel{\mapsto }\textit{Helper\_Comp} \! \left(12\cdot t , 0.04,12,\mathit{S0} \right)
$$
$$
\textit{Comp\_C} := \left(t ,\mathit{S0} \right)\hiderel{\mapsto }\textit{Helper\_Comp} \! \left(4\cdot t , 0.04,4,\mathit{S0} \right)
$$
$$
\mathit{YearsList} := \left[5,10,15,20\right]
$$
$$
\mathit{Principal} := 1000
$$
$$
\text{``Account Balance Calculation (Principal = 1000)''}
$$
```maple
Years      Company A       Company B       Company C
```
$$
\textit{val\_A} :=  1300.0
$$
$$
\textit{val\_B} :=  1220.996570
$$
$$
\textit{val\_C} :=  1220.190040
$$
```maple
5          1300.00         1221.00         1220.19
```
$$
\textit{val\_A} :=  1600.0
$$
$$
\textit{val\_B} :=  1490.832623
$$
$$
\textit{val\_C} :=  1488.863734
$$
```maple
10         1600.00         1490.83         1488.86
```
$$
\textit{val\_A} :=  1900.0
$$
$$
\textit{val\_B} :=  1820.301519
$$
$$
\textit{val\_C} :=  1816.696699
$$
```maple
15         1900.00         1820.30         1816.70
```
$$
\textit{val\_A} :=  2200.0
$$
$$
\textit{val\_B} :=  2222.581910
$$
$$
\textit{val\_C} :=  2216.715217
$$
```maple
20         2200.00         2222.58         2216.72
```
$$
t := t
$$
![Plot](MM_GENERAL_FULL_DOCplot2d7.png)

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S2 - ESERCISE 3:
\

The loan on a house is \$200,000.\

The mathematical model that describes the repayment of the loan is: Sn+1 = Sn +p/r Sn - R\

\

(a) Calculate the monthly repayment R needed to have the loan repaid after 30 years. The annualy interest rate is 5\%.\

(b) Calculate the total amount paid back on the loan.\

```maple
> restart;

##### --- Define the Model ---
##### S(n+1) = (1 + p/r)*S(n) - R
##### S0: Principal, p: Annual Rate, r: Frequency, R: Monthly Payment
LR_eq := S(n+1) = (1 + p/r) * S(n) - R;

##### Solve the recurrence relation generally
sol := rsolve(\{LR_eq, S(0)=S0\}, S(n));

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
eq_target := Balance(TotalMonths, Rate, Freq, Principal, R_needed) = 0;

##### Solve for R
R_val := solve(eq_target, R_needed);

##### Display result formatted as currency
printf("Part (a) Monthly Repayment: $%.2f\n", R_val);

##### --- Part (b): Total Amount Paid Back ---
##### Total = Monthly Payment * Number of Months
TotalPaid := R_val * TotalMonths;

##### Display result
printf("Part (b) Total Amount Paid: $%.2f\n", TotalPaid);
printf("Total Interest Paid: $%.2f\n", TotalPaid - Principal);

##### --- Visualization (Amortization Curve) ---
##### Plot the balance decreasing over the 30 years (360 months)
plot(
    Balance(n, Rate, Freq, Principal, R_val),
    n=0..TotalMonths,
    color=blue,
    thickness=2,
    title="Loan Balance Over 30 Years",
    labels=["Months", "Balance ($)"],
    gridlines=true
);

```
$$
\textit{LR\_eq} := S \! \left(n +1\right)=\left(1+\frac{p}{r}\right) S \! \left(n \right)-R
$$
$$
\mathit{sol} := \mathit{S0} \left(\frac{p +r}{r}\right)^{n}+\frac{r R}{p}-\frac{R r \left(\frac{p +r}{r}\right)^{n}}{p}
$$
$$
\mathit{Balance} := \left(n ,p ,r ,\mathit{S0} ,R \right)\hiderel{\mapsto }\mathit{S0} \cdot \left(\frac{p +r}{r}\right)^{n}+\frac{r \cdot R}{p}-\frac{R \cdot r \cdot \left(\frac{p +r}{r}\right)^{n}}{p}
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
\textit{eq\_target} :=  893548.9696- 832.2587640 \textit{R\_needed} =0
$$
$$
\textit{R\_val} :=  1073.643208
$$
```maple
Part (a) Monthly Repayment: $1073.64
```
$$
\mathit{TotalPaid} :=  386511.5549
$$
```maple
Part (b) Total Amount Paid: $386511.55
Total Interest Paid: $186511.55
```
![Plot](MM_GENERAL_FULL_DOCplot2d8.png)

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S2 - ESERCISE 4:

Find the solutions of the following **difference equations** and plot them graphically.\

\

(a) x(n+2) - 5x(n+1) + 6x(n) = 0; x0 = 1; x1 = 1;

```maple
> restart;

##### --- Define the Difference Equation ---
equation_a := x(n+2) - 5*x(n+1) + 6*x(n) = 0;

##### --- General Solution (Optional check) ---
rsolve(equation_a, x(n));

##### --- Solve with Initial Conditions ---
ans_equation_a := rsolve(\{equation_a, x(0)=1, x(1)=1\}, x(n));

##### --- Define Function for Plotting ---
solution_a := unapply(ans_equation_a, n);

##### --- Generate Data Points ---
[seq([n, solution_a(n)], n=0..10)];

##### --- Plot the Data ---
plot([seq([n, solution_a(n)], n=0..8)],
    style=point,
    symbol=circle,
    color=blue,
    title="Solution of x(n+2) - 5x(n+1) + 6x(n) = 0",
    labels=["n", "x(n)"]
);
```
$$
\textit{equation\_a} := x \! \left(n +2\right)-5 x \! \left(n +1\right)+6 x \! \left(n \right)=0
$$
$$
-\left(x \! \left(1\right)-3 x \! \left(0\right)\right) 2^{n}-\left(-x \! \left(1\right)+2 x \! \left(0\right)\right) 3^{n}
$$
$$
\textit{ans\_equation\_a} := 2 \,2^{n}-3^{n}
$$
$$
\textit{solution\_a} := n \mapsto 2\cdot 2^{n}-3^{n}
$$
$$
\left[\left[0,1\right],\left[1,1\right],\left[2,-1\right],\left[3,-11\right],\left[4,-49\right],\left[5,-179\right],\left[6,-601\right],

\left[7,-1931\right],\left[8,-6049\right],\left[9,-18659\right],\left[10,-57001\right]\right]
$$
![Plot](MM_GENERAL_FULL_DOCplot2d9.png)

\newpage
\

= = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =

## SEMINAR 3: Stability of the equilibrium points

### S3 - EXERCISE 1

Let's consider the **difference equation**: **x(n+1) = f (x n)**
Determine, in each case, the **equilibrium points and study** their stability using the stability theorem in the first approximation.
Make some numerical simulations.

(a) x(n+1 )=1/(2)xn

```maple
> restart;

##### --- Define the Function ---
f := x -> 0.5 * x;

##### --- Find Equilibrium Points ---
EqPoints := [solve(f(x) = x, x)];
print("Equilibrium Point(s):", EqPoints);

##### --- Stability Analysis (Stability Theorem) ---
##### Theorem: Stable if |f'(x*)| < 1
df := D(f);

for pt in EqPoints do
    val_derivative := eval(df(x), x=pt);
    print("Checking point:", pt);
    print("Derivative value:", val_derivative);

    if abs(val_derivative) < 1 then
        print("Conclusion: Asymptotically Stable (Attractor)");
    elif abs(val_derivative) > 1 then
        print("Conclusion: Unstable (Repeller)");
    else
        print("Conclusion: Inconclusive (Linearization fails)");
    end if;
end do;

##### --- Numerical Simulation ---
x0 := 100;
N_steps := 10;
Orbit := Vector(N_steps+1);
Orbit[1] := x0;

for i from 1 to N_steps do
    Orbit[i+1] := f(Orbit[i]);
end do;

DataPoints := [seq([n-1, Orbit[n]], n=1..N_steps+1)];

##### --- Plot the Results ---
plot(DataPoints,
    style=pointline,
    symbol=solidcircle,
    color=blue,
    title="Time Series: Convergence to 0",
    labels=["n", "x(n)"]
);
```
$$
f := x \mapsto  0.5\cdot x
$$
$$
\mathit{EqPoints} := \left[ 0.0\right]
$$
$$
\text{``Equilibrium Point(s):''},\left[ 0.0\right]
$$
$$
\mathit{df} :=  0.5
$$
$$
\textit{val\_derivative} :=  0.5
$$
$$
\text{``Checking point:''}, 0.0
$$
$$
\text{``Derivative value:''}, 0.5
$$
$$
\text{``Conclusion: Asymptotically Stable (Attractor)''}
$$
$$
\mathit{x0} := 100
$$
$$
\textit{N\_steps} := 10
$$
$$
\mathit{Orbit}_{1}:= 100
$$
$$
\mathit{Orbit}_{2}:=  50.0
$$
$$
\mathit{Orbit}_{3}:=  25.0
$$
$$
\mathit{Orbit}_{4}:=  12.500
$$
$$
\mathit{Orbit}_{5}:=  6.2500
$$
$$
\mathit{Orbit}_{6}:=  3.12500
$$
$$
\mathit{Orbit}_{7}:=  1.562500
$$
$$
\mathit{Orbit}_{8}:=  0.7812500
$$
$$
\mathit{Orbit}_{9}:=  0.39062500
$$
$$
\mathit{Orbit}_{10}:=  0.195312500
$$
$$
\mathit{Orbit}_{11}:=  0.0976562500
$$
$$
\mathit{DataPoints} :=

\left[\left[0,100\right],\left[1, 50.0\right],\left[2, 25.0\right],\left[3, 12.500\right],\left[4, 6.2500\right],

\left[5, 3.12500\right],\left[6, 1.562500\right],\left[7, 0.7812500\right],\left[8, 0.39062500\right],

\left[9, 0.195312500\right],\left[10, 0.0976562500\right]\right]
$$
![Plot](MM_GENERAL_FULL_DOCplot2d10.png)

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S3 - ESERCISE 2:

Consider the following mosquito model\

$x_{n+1}$ = ( a*x{n} + b*$x_{n-1}$ * $e^{-$x_{n-1}$}$ ) * $e^{-$x_{n}$}$ ,where a∈(0; 1), b∈[0; +∞).

This equation describes the growth of a mosquito population. Mosquitoes lay eggs, some of which hatch as soon as conditions are favorable,\

while others remain dormant for a year or two. In this model, it is assumed that eggs are dormant for one year at most.
\

(a) Find the equilibrium points;
\

(b) Study the stability of the equilibrium points;
\

(c) Make numerical simulations.\

```maple
> restart;
with(plots):

##### --- Define the Model Function ---
f := (u,v) -> (a*u + b*v*exp(-v)) * exp(-u);

##### --- Part (a): Find Equilibrium Points ---
Eq_Relation := x = f(x,x);
Eq_Solutions := solve(Eq_Relation, x);

x_trivial := 0;
x_nontrivial := Eq_Solutions[1];

print("Trivial Equilibrium:", x_trivial);
print("Non-Trivial Equilibrium (Symbolic):", x_nontrivial);

##### --- Part (b): Stability Analysis Setup ---
# Derivatives for Linearization
# Characteristic Eq will be: q^2 - J1*q - J2 = 0
p1 := diff(f(u,v), u); # J1
p2 := diff(f(u,v), v); # J2

##### --- CASE 1: EXTINCTION (a=0.4, b=0.5) ---
print("----------- CASE 1 -----------");
a_1 := 0.4;
b_1 := 0.5;

##### 1. Stability Check, Since a+b = 0.9 (which is <= 1), the only equilibrium is x=0
print("Condition Check: a+b <= 1, so population should die out to 0.");
val_J1_1 := evalf(eval(p1, \{u=0, v=0, a=a_1, b=b_1\}));
val_J2_1 := evalf(eval(p2, \{u=0, v=0, a=a_1, b=b_1\}));
print("Eigenvalues at 0:", solve(q^2 - val_J1_1*q - val_J2_1=0, q));

##### 2. Numerical Simulation
steps := 50;
X := Vector(steps);
X[1] := 0.1; X[2] := 0.1; # Initial conditions

for i from 2 to steps-1 do
    X[i+1] := evalf( (a_1*X[i] + b_1*X[i-1]*exp(-X[i-1])) * exp(-X[i]) );
end do:
Plot1 := pointplot([seq([n, X[n]], n=1..steps)], style=line, color=blue, title="Case 1: Extinction (x->0)");

##### --- CASE 2: SURVIVAL (a=0.7, b=2.0) ---
print("----------- CASE 2 -----------");
a_2 := 0.7;
b_2 := 2.0;

##### 1. Stability Check, Since a+b = 2.7 (> 1), a positive equilibrium exists
x_star_2 := evalf( ln( (a_2 + sqrt(a_2^2 + 4*b_2))/2 ) );
print("Equilibrium Point x*:", x_star_2);

##### Evaluate Jacobian at x*
val_J1_2 := evalf(eval(p1, \{u=x_star_2, v=x_star_2, a=a_2, b=b_2\}));
val_J2_2 := evalf(eval(p2, \{u=x_star_2, v=x_star_2, a=a_2, b=b_2\}));

##### Solve Characteristic Equation
roots_2 := [solve(q^2 - val_J1_2*q - val_J2_2 = 0, q)];
print("Eigenvalues:", roots_2);
# Check if stable (|q| < 1)
if abs(roots_2[1]) < 1 and abs(roots_2[2]) < 1 then print("STABLE"); else print("UNSTABLE"); end if;

##### 2. Numerical Simulation
X := Vector(steps);
X[1] := 0.1; X[2] := 0.1;

for i from 2 to steps-1 do
    X[i+1] := evalf( (a_2*X[i] + b_2*X[i-1]*exp(-X[i-1])) * exp(-X[i]) );
end do:
Plot2 := pointplot([seq([n, X[n]], n=1..steps)], style=line, color=red, title="Case 2: Survival (x->x*)");

##### --- Display Both Plots ---
display(Array([Plot1, Plot2]));
```
$$
f := \left(u ,v \right)\hiderel{\mapsto }\left(a \cdot u +b \cdot v \cdot {\mathrm e}^{-v}\right)\cdot {\mathrm e}^{-u}
$$
$$
\textit{Eq\_Relation} := x =(a x +b x {\mathrm e}^{-x}) {\mathrm e}^{-x}
$$
$$
\textit{Eq\_Solutions} := 0,-\ln \! \left(\frac{-a +\sqrt{a^{2}+4 b}}{2 b}\right),-\ln \! \left(-\frac{a +\sqrt{a^{2}+4 b}}{2 b}\right)
$$
$$
\textit{x\_trivial} := 0
$$
$$
\textit{x\_nontrivial} := 0
$$
$$
\text{``Trivial Equilibrium:''},0
$$
$$
\text{``Non-Trivial Equilibrium (Symbolic):''},0
$$
$$
\mathit{p1} := a {\mathrm e}^{-u}-(a u +b v {\mathrm e}^{-v}) {\mathrm e}^{-u}
$$
$$
\mathit{p2} := (b {\mathrm e}^{-v}-b v {\mathrm e}^{-v}) {\mathrm e}^{-u}
$$
$$
\text{``----------- CASE 1 -----------''}
$$
$$
\textit{a\_1} :=  0.4
$$
$$
\textit{b\_1} :=  0.5
$$
$$
\text{``Condition Check: a+b \textless = 1, so population should die out to 0.''}
$$
$$
\textit{val\_J1\_1} :=  0.4
$$
$$
\textit{val\_J2\_1} :=  0.5
$$
$$
\text{``Eigenvalues at 0:''}, 0.9348469228,- 0.5348469228
$$
$$
\mathit{steps} := 50
$$
$$
X_{1}:=  0.1
$$
$$
X_{2}:=  0.1
$$
![Plot](MM_GENERAL_FULL_DOCplot2d11.png)
$$
\text{``----------- CASE 2 -----------''}
$$
$$
\textit{a\_2} :=  0.7
$$
$$
\textit{b\_2} :=  2.0
$$
$$
\textit{x\_star\_2} :=  0.5916017272
$$
$$
\text{``Equilibrium Point x*:''}, 0.5916017272
$$
$$
\textit{val\_J1\_2} := - 0.2041936473
$$
$$
\textit{val\_J2\_2} :=  0.2501814825
$$
$$
\textit{roots\_2} := \left[ 0.4083982732,- 0.6125919205\right]
$$
$$
\text{``Eigenvalues:''},\left[ 0.4083982732,- 0.6125919205\right]
$$
$$
\text{``STABLE''}
$$
$$
X_{1}:=  0.1
$$
$$
X_{2}:=  0.1
$$
![Plot](MM_GENERAL_FULL_DOCplot2d12.png)
\begin{center}
\begin{tabular}{ c c }
& \\
\end{tabular}
\end{center}

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S3 - ESERCISE 3:

Flour Beetles model: $x_{n+1}$ = α*$x_{n}$ + β*$x_{n-2}$*$e^{-$c_{1}$*$x_{n-2}$-$c_{2}$*$x_{n}$}$ , where α, β > 0.\

\

(a) Find the equilibrium points;\

\

(b) Study the stability of the equilibrium points;\

\

(c) Make numerical simulations.\

\

**Hint**: Use the **Schur-Cohn Criterion (noted in LECTURE 4)**\

```maple
> restart;
with(plots):

##### --- Define the Model Function ---
f := (u,w) -> alpha * u + beta * w * exp(-c1 * w - c2 * u);

##### --- Part (a): Find Equilibrium Points ---
Eq_Relation := x = f(x,x);
Eq_Solutions := solve(Eq_Relation, x);

##### 2. Extract solutions
x_trivial := 0;
x_nontrivial := Eq_Solutions[1];

print("Trivial Equilibrium:", x_trivial);
print("Non-Trivial Equilibrium (Symbolic):", x_nontrivial);

##### --- Part (b): Stability Analysis (Symbolic Setup) ---
J_u := diff(f(u,w), u);
J_w := diff(f(u,w), w);

##### --- Part (b) & (c): Case 1 - Stable Parameters ---
print("CASE 1: STABLE");
alpha_1 := 0.5;
beta_1  := 2.0;
c1_1    := 0.1;
c2_1    := 0.1;

##### 1. Calculate specific equilibrium point
x_star_1 := evalf( ln(beta_1 / (1 - alpha_1)) / (c1_1 + c2_1) );
print("Equilibrium Point (Case 1):", x_star_1);

##### 2. Calculate Jacobian values at x_star
val_J0_1 := evalf(eval(J_u, \{u=x_star_1, w=x_star_1, alpha=alpha_1, beta=beta_1, c1=c1_1, c2=c2_1\}));
val_J2_1 := evalf(eval(J_w, \{u=x_star_1, w=x_star_1, alpha=alpha_1, beta=beta_1, c1=c1_1, c2=c2_1\}));

##### 3. Solve Characteristic Equation: z^3 - J0*z^2 - J2 = 0
roots_val_1 := [solve(z^3 - val_J0_1*z^2 - val_J2_1 = 0, z)];
MaxMod_1 := max(seq(abs(r), r in roots_val_1));
print("Max Modulus of Roots:", MaxMod_1);
##### If MaxMod < 1, it is Stable

##### 4. Numerical Simulation (Case 1)
steps := 60;
X := Vector(steps);
X[1] := 0.1; X[2] := 0.1; X[3] := 0.1;

for i from 3 to steps-1 do
    X[i+1] := evalf( alpha_1*X[i] + beta_1*X[i-2]*exp(-c1_1*X[i-2] - c2_1*X[i]) );
end do:
Plot1 := pointplot([seq([n, X[n]], n=1..steps)], style=line, color=blue, title="Case 1: Stable");

##### --- Part (b) & (c): Case 2 - Unstable Parameters ---
print("CASE 2: UNSTABLE");
alpha_2 := 0.5;
beta_2  := 20.0;
c1_2    := 0.1;
c2_2    := 0.1;

##### 1. Calculate specific equilibrium point
x_star_2 := evalf( ln(beta_2 / (1 - alpha_2)) / (c1_2 + c2_2) );
print("Equilibrium Point (Case 2):", x_star_2);

##### 2. Calculate Jacobian values at x_star
val_J0_2 := evalf(eval(J_u, \{u=x_star_2, w=x_star_2, alpha=alpha_2, beta=beta_2, c1=c1_2, c2=c2_2\}));
val_J2_2 := evalf(eval(J_w, \{u=x_star_2, w=x_star_2, alpha=alpha_2, beta=beta_2, c1=c1_2, c2=c2_2\}));

##### 3. Solve Characteristic Equation
roots_val_2 := [solve(z^3 - val_J0_2*z^2 - val_J2_2 = 0, z)];
MaxMod_2 := max(seq(abs(r), r in roots_val_2));
print("Max Modulus of Roots:", MaxMod_2);
##### If MaxMod > 1, it is Unstable

##### 4. Numerical Simulation (Case 2)
X := Vector(steps);
X[1] := 0.1; X[2] := 0.1; X[3] := 0.1;

for i from 3 to steps-1 do
    X[i+1] := evalf( alpha_2*X[i] + beta_2*X[i-2]*exp(-c1_2*X[i-2] - c2_2*X[i]) );
end do:
Plot2 := pointplot([seq([n, X[n]], n=1..steps)], style=line, color=red, title="Case 2: Unstable");

##### --- Display Both Plots ---
display(Array([Plot1, Plot2]));
```
$$
f := \left(u ,w \right)\hiderel{\mapsto }\alpha \cdot u +\beta \cdot w \cdot {\mathrm e}^{-\mathit{c1} \cdot w -\mathit{c2} \cdot u}
$$
$$
\textit{Eq\_Relation} := x =\alpha  x +\beta  x {\mathrm e}^{-\mathit{c1} x -\mathit{c2} x}
$$
$$
\textit{Eq\_Solutions} := 0,-\frac{\ln (-\frac{\alpha -1}{\beta})}{\mathit{c1} +\mathit{c2}}
$$
$$
\textit{x\_trivial} := 0
$$
$$
\textit{x\_nontrivial} := 0
$$
$$
\text{``Trivial Equilibrium:''},0
$$
$$
\text{``Non-Trivial Equilibrium (Symbolic):''},0
$$
$$
\textit{J\_u} := \alpha -\beta  w \mathit{c2} {\mathrm e}^{-\mathit{c1} w -\mathit{c2} u}
$$
$$
\textit{J\_w} := \beta  {\mathrm e}^{-\mathit{c1} w -\mathit{c2} u}-\beta  w \mathit{c1} {\mathrm e}^{-\mathit{c1} w -\mathit{c2} u}
$$
$$
\text{``CASE 1: STABLE''}
$$
$$
\textit{alpha\_1} :=  0.5
$$
$$
\textit{beta\_1} :=  2.0
$$
$$
\textit{c1\_1} :=  0.1
$$
$$
\textit{c2\_1} :=  0.1
$$
$$
\textit{x\_star\_1} :=  6.931471805
$$
$$
\text{``Equilibrium Point (Case 1):''}, 6.931471805
$$
$$
\textit{val\_J0\_1} :=  0.1534264098
$$
$$
\textit{val\_J2\_1} :=  0.1534264098
$$
$$
\textit{roots\_val\_1} := \left[ 0.5916803502,- 0.2191269702+ 0.4596625021 \,\mathrm{I},- 0.2191269702- 0.4596625021 \,\mathrm{I}\right]
$$
$$
\textit{MaxMod\_1} :=  0.5916803502
$$
$$
\text{``Max Modulus of Roots:''}, 0.5916803502
$$
$$
\mathit{steps} := 60
$$
$$
X_{1}:=  0.1
$$
$$
X_{2}:=  0.1
$$
$$
X_{3}:=  0.1
$$
![Plot](MM_GENERAL_FULL_DOCplot2d13.png)
$$
\text{``CASE 2: UNSTABLE''}
$$
$$
\textit{alpha\_2} :=  0.5
$$
$$
\textit{beta\_2} :=  20.0
$$
$$
\textit{c1\_2} :=  0.1
$$
$$
\textit{c2\_2} :=  0.1
$$
$$
\textit{x\_star\_2} :=  18.44439727
$$
$$
\text{``Equilibrium Point (Case 2):''}, 18.44439727
$$
$$
\textit{val\_J0\_2} := - 0.4222198635
$$
$$
\textit{val\_J2\_2} := - 0.4222198635
$$
$$
\textit{roots\_val\_2} := \left[ 0.2491448889+ 0.6297677103 \,\mathrm{I},- 0.9205096412, 0.2491448889- 0.6297677103 \,\mathrm{I}\right]
$$
$$
\textit{MaxMod\_2} :=  0.9205096412
$$
$$
\text{``Max Modulus of Roots:''}, 0.9205096412
$$
$$
X_{1}:=  0.1
$$
$$
X_{2}:=  0.1
$$
$$
X_{3}:=  0.1
$$
![Plot](MM_GENERAL_FULL_DOCplot2d14.png)
\begin{center}
\begin{tabular}{ c c }
& \\
\end{tabular}
\end{center}

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S3 - ESERCISE 4:

Study the stability of the equilibrium point (0,0) for the following systems. For the caseswhere stability occurs, verify this property numerically.

(a)
$ \displaystyle \left{\begin{array}{cc}
\boldsymbol{\mathrm{x_}}{{\boldsymbol{\mathrm{n}}\boldsymbol{+}\boldsymbol{1}}}=x_ {{n }}+4\cdot y_ {{n }} &
\\
y_ {{n +1}}=\frac{1}{4}\cdot x_ {{n }}+y_ {{n }} &
\end{array} $
```maple
> restart;
with(linalg):
with(plots):

##### --- Define the System Matrix ---
##### x(n+1) = 1*x(n) + 4*y(n)
##### y(n+1) = 0.25*x(n) + 1*y(n)
A := matrix([[1, 4], [0.25, 1]]);

##### --- Part (a): Stability Analysis ---
Eigs := eigenvals(A);
print("The eigenvalues are:", Eigs);

##### Check Stability Criteria: Stable if all |lambda| < 1. Unstable if any |lambda| > 1.
Abs_Eig1 := abs(Eigs[1]);
Abs_Eig2 := abs(Eigs[2]);

if Abs_Eig1 < 1 and Abs_Eig2 < 1 then
    print("Conclusion: The system is STABLE (All |lambda| < 1).");
else
    print("Conclusion: The system is UNSTABLE (At least one |lambda| > 1).");
end if;

##### --- Part (b): Numerical Verification ---
##### Even though it is unstable, we plot to demonstrate the divergence.
N := 20;

##### Use Arrays to store the sequence (Global variables)
x := Array(0..N);
y := Array(0..N);

##### Initial Conditions (Start close to equilibrium 0,0)
x[0] := 0.1;
y[0] := 0.1;

##### Loop to calculate the sequence
for i from 0 to N-1 do
    # x(n+1) = x(n) + 4*y(n)
    x[i+1] := x[i] + 4*y[i];

    # y(n+1) = 0.25*x(n) + y(n)
    y[i+1] := 0.25*x[i] + y[i];
end do:

##### --- Plot the Results ---
PlotX := plot([seq([n, x[n]], n=0..N)], style=pointline, color=red, legend="x(n)");
PlotY := plot([seq([n, y[n]], n=0..N)], style=pointline, color=blue, legend="y(n)");

display(
    [PlotX, PlotY],
    title="Divergence from Equilibrium (Unstable)",
    labels=["n", "Value"],
    labeldirections=[HORIZONTAL, VERTICAL]
);

##### Phase Plane Plot (y vs x) to see the trajectory moving away from origin
PhasePlot := plot(
    [seq([x[n], y[n]], n=0..N)],
    style=pointline,
    color=green,
    title="Phase Plane (y vs x)",
    labels=["x(n)", "y(n)"]
);
display(PhasePlot);
```
$$
\mathit{Eigs} :=  2.0, 0.0
$$
$$
\text{``The eigenvalues are:''}, 2.0, 0.0
$$
$$
\textit{Abs\_Eig1} :=  2.0
$$
$$
\textit{Abs\_Eig2} :=  0.0
$$
$$
\text{``Conclusion: The system is UNSTABLE (At least one |lambda| \textgreater 1).''}
$$
$$
N := 20
$$
$$
x :=

\left[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,\mathrm{\cdots  0 .. 20 Array}\right]
$$
$$
y :=

\left[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,\mathrm{\cdots  0 .. 20 Array}\right]
$$
$$
x_{0}:=  0.1
$$
$$
y_{0}:=  0.1
$$
![Plot](MM_GENERAL_FULL_DOCplot2d15.png)
![Plot](MM_GENERAL_FULL_DOCplot2d16.png)
![Plot](MM_GENERAL_FULL_DOCplot2d17.png)
![Plot](MM_GENERAL_FULL_DOCplot2d18.png)
![Plot](MM_GENERAL_FULL_DOCplot2d19.png)

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S3 - ESERCISE 5:

Find and study the stability of the equilibrium points for the following systems.
For locally asymptotically stable equilibrium points, determine some initial conditions for which the corresponding solution converges to that equilibrium point.

(a)
$ \left{\begin{array}{cc}
\boldsymbol{\mathrm{x_}}{{\boldsymbol{\mathrm{n}}\boldsymbol{+}\boldsymbol{1}}}=x_ {{n }}+\frac{1}{6}\cdot x_ {{n }}\cdot (1-x_ {{n }}-y {{n }}) &
\\
y_ {{n +1}}=y_ {{n }}\cdot \boldsymbol{(\boldsymbol{1}\boldsymbol{+}\boldsymbol{\mathrm{x_}}\boldsymbol{{{\boldsymbol{n} }}}\boldsymbol{-}\boldsymbol{y} \boldsymbol{{{\boldsymbol{n} }}})} &
\end{array} $

```maple
> restart;
with(linalg):
with(plots):

##### --- 1. Define the System ---
##### x(n+1) = f1(x, y)
##### y(n+1) = f2(x, y)
f1 := (x,y) -> x + (1/6)*x*(1 - x - y);
f2 := (x,y) -> y*(1 + x - y);

##### --- 2. Find Equilibrium Points ---
##### Solve the system x = f1(x,y) and y = f2(x,y)
##### This corresponds to finding fixed points (x*, y*)
Eq_System := \{x = f1(x,y), y = f2(x,y)\};
Points := solve(Eq_System, \{x,y\});
print("The equilibrium points are:", Points);

##### --- 3. Stability Analysis (Linearization) ---
J := jacobian([f1(x,y), f2(x,y)], [x,y]);

##### --- Check Point 1: (0, 0) ---
print("--- Analysis of (0, 0) ---");
##### Substitute x=0, y=0 into Jacobian
J_1 := subs(\{x=0, y=0\}, evalm(J));
##### Calculate Eigenvalues
Eigs_1 := eigenvals(J_1);
print("Eigenvalues:", Eigs_1);
##### Result: 1.16 (>1) and 1. Unstable.

##### --- Check Point 2: (1, 0) ---
print("--- Analysis of (1, 0) ---");
J_2 := subs(\{x=1, y=0\}, evalm(J));
Eigs_2 := eigenvals(J_2);
print("Eigenvalues:", Eigs_2);
##### Result: 0.83 and 2. Unstable (since 2 > 1).

##### --- Check Point 3: (1/2, 1/2) ---
print("--- Analysis of (1/2, 1/2) ---");
J_3 := subs(\{x=1/2, y=1/2\}, evalm(J));
Eigs_3 := eigenvals(J_3);
evalf(Eigs_3);
##### Result: Complex numbers with modulus < 1. Modulus calculation to confirm stability.
Modulus := abs(Eigs_3[1]);
print("Modulus of eigenvalues:", evalf(Modulus));
if evalf(Modulus) < 1 then
    print("Conclusion: (1/2, 1/2) is LOCALLY ASYMPTOTICALLY STABLE.");
else
    print("Conclusion: Unstable.");
end if;

##### --- 4. Numerical Simulation for Stable Point ---
##### Verify convergence to (0.5, 0.5) starting from nearby conditions (e.g., 0.4, 0.4)
N := 50;
x_sim := Array(0..N);
y_sim := Array(0..N);

##### Initial Conditions
x_sim[0] := 0.4;
y_sim[0] := 0.4;

for i from 0 to N-1 do
    x_sim[i+1] := f1(x_sim[i], y_sim[i]);
    y_sim[i+1] := f2(x_sim[i], y_sim[i]);
end do:
PlotX := plot([seq([n, x_sim[n]], n=0..N)], style=point, color=red, legend="x(n)");
PlotY := plot([seq([n, y_sim[n]], n=0..N)], style=point, color=blue, legend="y(n)");
display( [PlotX, PlotY], title="Convergence to Stable Equilibrium (0.5, 0.5)", labels=["n", "Value"] );
```
$$
\mathit{f1} := \left(x ,y \right)\hiderel{\mapsto }x +\frac{x \cdot \left(1-x -y \right)}{6}
$$
$$
\mathit{f2} := \left(x ,y \right)\hiderel{\mapsto }y \cdot \left(1+x -y \right)
$$
$$
\textit{Eq\_System} := \left\{x  \hiderel{=} x +\frac{x \left(1-x -y \right)}{6},y  \hiderel{=} y \left(1+x -y \right)\right\}
$$
$$
\mathit{Points} := \left\{x  \hiderel{=} 0,y  \hiderel{=} 0\right\},\left\{x  \hiderel{=} 1,y  \hiderel{=} 0\right\},\left\{x  \hiderel{=} \frac{1}{2},y  \hiderel{=} \frac{1}{2}\right\}
$$
$$
\text{``The equilibrium points are:''},\left\{x  \hiderel{=} 0,y  \hiderel{=} 0\right\},\left\{x  \hiderel{=} 1,y  \hiderel{=} 0\right\},\left\{x  \hiderel{=} \frac{1}{2},y  \hiderel{=} \frac{1}{2}\right\}
$$
$$
\text{``--- Analysis of (0, 0) ---''}
$$
$$
\textit{Eigs\_1} := \frac{7}{6},1
$$
$$
\text{``Eigenvalues:''},\frac{7}{6},1
$$
$$
\text{``--- Analysis of (1, 0) ---''}
$$
$$
\textit{Eigs\_2} := \frac{5}{6},2
$$
$$
\text{``Eigenvalues:''},\frac{5}{6},2
$$
$$
\text{``--- Analysis of (1/2, 1/2) ---''}
$$
$$
\textit{Eigs\_3} := \frac{3}{4},\frac{2}{3}
$$
$$
0.7500000000, 0.6666666667
$$
$$
\mathit{Modulus} := \frac{3}{4}
$$
$$
\text{``Modulus of eigenvalues:''}, 0.7500000000
$$
$$
\text{``Conclusion: (1/2, 1/2) is LOCALLY ASYMPTOTICALLY STABLE.''}
$$
$$
N := 50
$$
$$
\textit{x\_sim} :=

\left[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,

0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,

\mathrm{\cdots  0 .. 50 Array}\right]
$$
$$
\textit{y\_sim} :=

\left[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,

0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,

\mathrm{\cdots  0 .. 50 Array}\right]
$$
$$
\textit{x\_sim}_{0}:=  0.4
$$
$$
\textit{y\_sim}_{0}:=  0.4
$$
![Plot](MM_GENERAL_FULL_DOCplot2d20.png)
![Plot](MM_GENERAL_FULL_DOCplot2d21.png)
![Plot](MM_GENERAL_FULL_DOCplot2d22.png)

\newpage
\

= = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =

## SEMINAR 4:  Solving Differential Equations with MAPLE

### S4 - EXERCISE 1:

Find the general solution of the differential equations:

(a) 2 * $x^{2}$ * y' = $x^{2}$ + $y^{2}$

(b) y' = -((x+y)/y)

(e) y'' + y = sin(x) + cos(x)

(g) y'' - y' = 1/(1+$e^{x}$)

```maple
> restart;
with(plots):

##### --- Part (a) ---
##### Equation: 2*x^2*y' = x^2 + y^2
differential_equation_a := 2*x^2*diff(y(x), x) = x^2 + y(x)^2;
print("General Solution for (a):");
dsolve(differential_equation_a, y(x));

##### --- Part (b) ---
##### Equation: y' = -(x+y)/y
differential_equation_b := diff(y(x), x) = -(x + y(x))/y(x);
print("General Solution for (b):");
dsolve(differential_equation_b, y(x));

##### -- Visualization (Optional but good) --
##### Calculate implicit solution for plotting
answer_b := dsolve(differential_equation_b, y(x), implicit);

##### Prepare for plotting: remove y(x) dependency and define a function
##### Note: dsolve usually returns constants as _C1.
##### We extract the expression and substitute y(x) with y for plotting.
expr_b := lhs(answer_b);
fsol_b := unapply(subs(y(x)=y, expr_b), x, y, _C1);

##### Plot a sample curve where _C1 = -1
print("Plotting solution for _C1 = -1");
implicitplot(fsol_b(x, y, -1) = 0, x=-10..10, y=-10..10, numpoints=10000, title="Solution Curve for (b)");

##### --- Part (e) ---
##### Equation: y'' + y = sin(x) + cos(x)
differential_equation_e := diff(y(x), x$2) + y(x) = sin(x) + cos(x);
print("General Solution for (e):");
dsolve(differential_equation_e, y(x));

##### --- Part (g) ---
##### Equation: y'' - y' = 1/(1+e^x)
differential_equation_g := diff(y(x), x$2) - diff(y(x), x) = 1/(1 + exp(x));
print("General Solution for (g):");
dsolve(differential_equation_g, y(x));
```
$$
\textit{differential\_equation\_a} := 2 x^{2} \left(\frac{d}{d x}y \! \left(x \right)\right)=x^{2}+y \! \left(x \right)^{2}
$$
$$
\text{``General Solution for (a):''}
$$
$$
y \! \left(x \right)=\frac{x \left(\ln \! \left(x \right)+c_{1}-2\right)}{\ln \! \left(x \right)+c_{1}}
$$
$$
\textit{differential\_equation\_b} := \frac{d}{d x}y \! \left(x \right)=-\frac{x +y \! \left(x \right)}{y \! \left(x \right)}
$$
$$
\text{``General Solution for (b):''}
$$
$$
y \! \left(x \right)=\frac{\sqrt{3}\, x \tan \! \left(\mathit{RootOf} \! \left(\sqrt{3}\, \ln \! \left(\frac{3 x^{2}}{4}+\frac{3 x^{2} \tan \left(\textit{\_Z} \right)^{2}}{4}\right)+2 \sqrt{3}\, c_{1}-2 \textit{\_Z} \right)\right)}{2}-\frac{x}{2}
$$
$$
\textit{answer\_b} := -\frac{\ln \! \left(\frac{x^{2}+x y \left(x \right)+y \left(x \right)^{2}}{x^{2}}\right)}{2}+\frac{\sqrt{3}\, \arctan \! \left(\frac{\left(2 y \left(x \right)+x \right) \sqrt{3}}{3 x}\right)}{3}-\ln \! \left(x \right)-c_{1}=0
$$
$$
\textit{expr\_b} := -\frac{\ln \! \left(\frac{x^{2}+x y \left(x \right)+y \left(x \right)^{2}}{x^{2}}\right)}{2}+\frac{\sqrt{3}\, \arctan \! \left(\frac{\left(2 y \left(x \right)+x \right) \sqrt{3}}{3 x}\right)}{3}-\ln \! \left(x \right)-c_{1}
$$
$$
\textit{fsol\_b} := \left(x ,y ,c_{1}\right)\hiderel{\mapsto }-\frac{\ln \! \left(\frac{x^{2}+y \cdot x +y^{2}}{x^{2}}\right)}{2}+\frac{\sqrt{3}\cdot \arctan \! \left(\frac{\left(2\cdot y +x \right)\cdot \sqrt{3}}{3\cdot x}\right)}{3}-\ln \! \left(x \right)-c_{1}
$$
$$
\text{``Plotting solution for \_C1 = -1''}
$$
![Plot](MM_GENERAL_FULL_DOCplot2d23.png)
$$
\textit{differential\_equation\_e} := \frac{d^{2}}{d x^{2}}y \! \left(x \right)+y \! \left(x \right)=\sin \! \left(x \right)+\cos \! \left(x \right)
$$
$$
\text{``General Solution for (e):''}
$$
$$
y \! \left(x \right)=\sin \! \left(x \right) c_{2}+\cos \! \left(x \right) c_{1}+\frac{\left(1-x \right) \cos \! \left(x \right)}{2}+\frac{\sin \! \left(x \right) x}{2}
$$
$$
\textit{differential\_equation\_g} := \frac{d^{2}}{d x^{2}}y \! \left(x \right)-\frac{d}{d x}y \! \left(x \right)=\frac{1}{1+{\mathrm e}^{x}}
$$
$$
\text{``General Solution for (g):''}
$$
$$
y \! \left(x \right)=-x +{\mathrm e}^{x} c_{1}+\ln \! \left(1+{\mathrm e}^{x}\right) \left(1+{\mathrm e}^{x}\right)-1-{\mathrm e}^{x} \ln \! \left({\mathrm e}^{x}\right)+c_{2}
$$

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S4 - ESERCISE 2:

Solve the following IVPs and plot the solution graph:

(a) y' = 1 + $y^{2}$ , $y_{0}$ = 1;

(d) y'' - 5*y' + 4*y = 0 , $y_{0}$=5, y'_{0}=3;

```maple
> restart;
with(DEtools):

##### --- Part (a) ---
diff_a := diff(y(x), x) = 1 + y(x)^2;
init_cond_a := y(0) = 1;

##### Solve analytically
print("Solution for (a):");
dsolve(\{diff_a, init_cond_a\}, y(x));

##### Plot graph (Tangent function has vertical asymptotes, careful with range)
DEplot(diff_a, y(x), x=-2..0.75, [[init_cond_a]], linecolor=blue, title="Solution to y'=1+y^2");

##### --- Part (d) ---
diff_d := diff(y(x), x$2) - 5*diff(y(x), x) + 4*y(x) = 0;

##### Initial Conditions: y(0)=5, y'(0)=3
init_cond_d := y(0)=5, D(y)(0)=3;

##### Solve analytically
print("Solution for (d):");
dsolve(\{diff_d, init_cond_d\}, y(x));

##### Plot graph
DEplot(diff_d, y(x), x=-1..2, [[init_cond_d]], linecolor=red, title="Solution to Second Order IVP");
```
$$
\textit{diff\_a} := \frac{d}{d x}y \! \left(x \right)=1+y \! \left(x \right)^{2}
$$
$$
\textit{init\_cond\_a} := y \! \left(0\right)=1
$$
$$
\text{``Solution for (a):''}
$$
$$
y \! \left(x \right)=\tan \! \left(x +\frac{\pi}{4}\right)
$$
![Plot](MM_GENERAL_FULL_DOCplot2d24.png)
$$
\textit{diff\_d} := \frac{d^{2}}{d x^{2}}y \! \left(x \right)-5 \frac{d}{d x}y \! \left(x \right)+4 y \! \left(x \right)=0
$$
$$
\textit{init\_cond\_d} := y \! \left(0\right) \hiderel{=} 5,\mathrm{D}\! \left(y \right)\! \left(0\right) \hiderel{=} 3
$$
$$
\text{``Solution for (d):''}
$$
$$
y \! \left(x \right)=\frac{17 {\mathrm e}^{x}}{3}-\frac{2 {\mathrm e}^{4 x}}{3}
$$
![Plot](MM_GENERAL_FULL_DOCplot2d25.png)

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S4 - ESERCISE 3:

Consider the di§erential equation:
y'(x) + (k/x)*y(x) = $x^{3}$ , where k∈ℝ

(a) Find the general solution;

(b) For k = 1 draw the solution curve;

(c) For k = 1 solve the IVP { [ y'(x) + (k/x)*y(x) = $x^{3}$ ] , [ y(1) = 0 ] } and draw the graph of solution;

(d) Use animate command to see the dependence of the solution for the IVP { [ y'(x) + (k/x)*y(x) = $x^{3}$ ] , [ y(1) = 0 ] } with respect to the parameter k.

```maple
> restart;
with(plots):

##### --- Part (a): General Solution ---
deq := diff(y(x), x) + (k/x)*y(x) = x^3;
print("General Solution:");
gen_sol := dsolve(deq, y(x));

##### --- Part (b): Solution for k = 1 ---
sol_k1 := eval(gen_sol, k=1);
print("General Solution for k=1:", sol_k1);

##### Plot a family of curves for different constants _C1 (e.g., -2 to 2)
##### Start x from 0.1 to avoid division by zero at x=0
FamilyCurves := [seq(subs(_C1=c, rhs(sol_k1)), c=-5..5)];
plot(FamilyCurves, x=0.1..2, title="Part (b): Family of Solutions (k=1)", color=blue);

##### --- Part (c): Solve IVP for k = 1 ---
##### IVP: \{Equation with k=1, y(1)=0\}
ivp_sol_k1 := dsolve(\{eval(deq, k=1), y(1)=0\}, y(x));
print("IVP Solution for k=1:", ivp_sol_k1);

##### Draw the specific solution graph
plot(rhs(ivp_sol_k1), x=0..2.5, title="Part (c): IVP Solution (k=1, y(1)=0)", color=red, thickness=2);

##### --- Part (d): Animation with respect to k ---
##### 1. Solve the IVP symbolically for ANY k
ivp_gen := dsolve(\{deq, y(1)=0\}, y(x));

##### 2. Create a function y(x, k) from the result
##### rhs() takes the right hand side of the solution y(x) = ...
ysol := unapply(rhs(ivp_gen), x, k);

##### 3. Create the Animation
##### Syntax: animate(plot, [function, range], parameter_range)
print("Click the plot below and press Play to see the animation.");
animate(plot,
    [ysol(x, k), x=0..2, color=green, thickness=2],
    k=-3..5,
    view=[0..2, -2..2],
    title="Part (d): Dependence on parameter k"
);
```
$$
\mathit{deq} := \frac{d}{d x}y \! \left(x \right)+\frac{k y \! \left(x \right)}{x}=x^{3}
$$
$$
\text{``General Solution:''}
$$
$$
\textit{gen\_sol} := y \! \left(x \right)=\frac{x^{4}}{4+k}+x^{-k} c_{1}
$$
$$
\textit{sol\_k1} := y \! \left(x \right)=\frac{x^{4}}{5}+\frac{c_{1}}{x}
$$
$$
\text{``General Solution for k=1:''},y \! \left(x \right)=\frac{x^{4}}{5}+\frac{c_{1}}{x}
$$
$$
\mathit{FamilyCurves} := \left[\frac{x^{4}}{5}-\frac{5}{x},\frac{x^{4}}{5}-\frac{4}{x},\frac{x^{4}}{5}-\frac{3}{x},\frac{x^{4}}{5}-\frac{2}{x},\frac{x^{4}}{5}-\frac{1}{x},\frac{x^{4}}{5},\frac{x^{4}}{5}+\frac{1}{x},\frac{x^{4}}{5}+\frac{2}{x},\frac{x^{4}}{5}+\frac{3}{x},\frac{x^{4}}{5}+\frac{4}{x},\frac{x^{4}}{5}+\frac{5}{x}\right]
$$
![Plot](MM_GENERAL_FULL_DOCplot2d26.png)
$$
\textit{ivp\_sol\_k1} := y \! \left(x \right)=\frac{x^{5}-1}{5 x}
$$
$$
\text{``IVP Solution for k=1:''},y \! \left(x \right)=\frac{x^{5}-1}{5 x}
$$
![Plot](MM_GENERAL_FULL_DOCplot2d27.png)
$$
\textit{ivp\_gen} := y \! \left(x \right)=\frac{x^{4}-x^{-k}}{4+k}
$$
$$
\mathit{ysol} := \left(x ,k \right)\hiderel{\mapsto }\frac{x^{4}-x^{-k}}{4+k}
$$
$$
\text{``Click the plot below and press Play to see the animation.''}
$$
![Plot](MM_GENERAL_FULL_DOCplot2d28.png)

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S4 - ESERCISE 4:

Find the solution of the following IVP

$ \displaystyle \left{\begin{array}{cc}
y\esapos\esapos-y\esapos-2y=0 &
\\
y (0)=a ,y \esapos (0)=2 &
\end{array} $

And the value of the parameter a such that y(x)-->0 as x ∞+1

```maple
> restart;
with(plots):

##### --- 1. Solve the Initial Value Problem ---
eqd := diff(y(x), x$2) - diff(y(x), x) - 2*y(x) = 0;
init_cond := y(0) = a, D(y)(0) = 2;

ans := dsolve(\{eqd, init_cond\}, y(x));
ysol := unapply(rhs(ans), x, a);

print("The solution y(x) is:");
print(ysol(x, a));

##### --- 2. Find parameter 'a' for Stability ---
##### The solution has terms involving exp(2*x) and exp(-1*x).
##### As x -> infinity, exp(2*x) goes to infinity (diverges) and exp(-x) goes to 0.
##### For y(x) -> 0, the coefficient of the diverging term exp(2*x) must be zero.

##### Isolate the coefficient of exp(2x)
Coeff_Unstable := coeff(ysol(x, a), exp(2*x));

##### Solve for 'a' such that this coefficient is 0
a_stable := solve(Coeff_Unstable = 0, a);

print("Value of 'a' for convergence:", a_stable);

##### --- 3. Verify the Limit ---
##### Check the limit with the found value
Limit_Check := limit(ysol(x, a_stable), x=infinity);
print("Limit when a = -2:", Limit_Check);

##### --- 4. Visual Verification ---
###### Plot stable case (a = -2) vs unstable case (e.g., a = 0)
PlotStable := plot(ysol(x, a_stable), x=0..5, color=green, thickness=2, legend="Stable (a=-2)");
PlotUnstable := plot(ysol(x, 0), x=0..2, color=red, linestyle=dash, legend="Unstable (a=0)");

display([PlotStable, PlotUnstable], title="Behavior of y(x) as x->infinity");
```
$$
\mathit{eqd} := \frac{d^{2}}{d x^{2}}y \! \left(x \right)-\frac{d}{d x}y \! \left(x \right)-2 y \! \left(x \right)=0
$$
$$
\textit{init\_cond} := y \! \left(0\right) \hiderel{=} a ,\mathrm{D}\! \left(y \right)\! \left(0\right) \hiderel{=} 2
$$
$$
\mathit{ans} := y \! \left(x \right)=\frac{\left(-2+2 a \right) {\mathrm e}^{-x}}{3}+\frac{\left(a +2\right) {\mathrm e}^{2 x}}{3}
$$
$$
\mathit{ysol} := \left(x ,a \right)\hiderel{\mapsto }\frac{\left(-2+2\cdot a \right)\cdot {\mathrm e}^{-x}}{3}+\frac{\left(a +2\right)\cdot {\mathrm e}^{2\cdot x}}{3}
$$
$$
\text{``The solution y(x) is:''}
$$
$$
\frac{(-2+2 a) {\mathrm e}^{-x}}{3}+\frac{(a +2) {\mathrm e}^{2 x}}{3}
$$
$$
\textit{Coeff\_Unstable} := \frac{a}{3}+\frac{2}{3}
$$
$$
\textit{a\_stable} := -2
$$
$$
\text{``Value of 'a' for convergence:''},-2
$$
$$
\textit{Limit\_Check} := 0
$$
$$
\text{``Limit when a = -2:''},0
$$
![Plot](MM_GENERAL_FULL_DOCplot2d29.png)
![Plot](MM_GENERAL_FULL_DOCplot2d30.png)
![Plot](MM_GENERAL_FULL_DOCplot2d31.png)

\newpage
\

= = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =

## SEMINAR 5:  Modelling with First order differential equations

### S5 - EXERCISE 1:

Find the decay constant for a radioactive substance for the given half-life value

(a) $T_{1/2}$ = 5730 years for $C^{14}$

(b) $T_{1/2}$ = 4,468 * $10^{9}$ years for $U^{238}$

(c) $T_{1/2}$ = 706 * $10^{6}$ years for $U^{235}$

```maple
> restart;

##### --- Formula: k = ln(2) / Half_Life ---

##### --- Part (a): Carbon-14 ---
T12_C14 := 5730;
k_C14 := ln(2) / T12_C14;
print("Decay constant for C-14:", evalf(k_C14));

##### --- Part (b): Uranium-238 ---
T12_U238 := 4.468 * 10^9;
k_U238 := ln(2) / T12_U238;
print("Decay constant for U-238:", evalf(k_U238));

##### --- Part (c): Uranium-235 ---
T12_U235 := 706 * 10^6;
k_U235 := ln(2) / T12_U235;
print("Decay constant for U-235:", evalf(k_U235));
```
$$
\textit{T12\_C14} := 5730
$$
$$
\textit{k\_C14} := \frac{\ln \! \left(2\right)}{5730}
$$
$$
\text{``Decay constant for C-14:''}, 0.0001209680943
$$
$$
\textit{T12\_U238} :=  4.468000000\times 10^{9}
$$
$$
\textit{k\_U238} :=  2.238137869\times 10^{-10} \ln \! \left(2\right)
$$
$$
\text{``Decay constant for U-238:''}, 1.551358954\times 10^{-10}
$$
$$
\textit{T12\_U235} := 706000000
$$
$$
\textit{k\_U235} := \frac{\ln \! \left(2\right)}{706000000}
$$
$$
\text{``Decay constant for U-235:''}, 9.817948734\times 10^{-10}
$$

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S5 - ESERCISE 2:

Find the decay constant in two years 3g of radioisotope decay to 0,9g. Find the half-life and the decay constant.

```maple
> restart;

##### --- 1. Define the Differential Equation ---
##### Model: x'(t) = -k * x(t)
RD_eq := diff(x(t), t) = -k * x(t);
sol := dsolve(\{RD_eq, x(0) = x0\}, x(t));
x_sol := unapply(rhs(sol), t, x0, k);

##### --- 2. Find Decay Constant (k) ---
##### Data: Initial (x0) = 3g, Time (t) = 2 years, Final = 0.9g
eq_data := x_sol(2, 3, k) = 0.9;
k_val := solve(eq_data, k);
print("The Decay Constant k is:", evalf(k_val));

##### --- 3. Find Half-Life ---
##### Method A: Formula T = ln(2)/k
T_half := ln(2) / k_val;
print("Half-Life (years):", evalf(T_half));

##### Method B: Derivation
##### Solve when x(t) = x0 / 2
eq_half := x_sol(T, x0, k_val) = x0/2;
T_derived := solve(eq_half, T);

print("Verified Half-Life:", evalf(T_derived));
```
$$
\textit{RD\_eq} := \frac{d}{d t}x \! \left(t \right)=-k x \! \left(t \right)
$$
$$
\mathit{sol} := x \! \left(t \right)=\mathit{x0} {\mathrm e}^{-k t}
$$
$$
\textit{x\_sol} := \left(t ,\mathit{x0} ,k \right)\hiderel{\mapsto }\mathit{x0} \cdot {\mathrm e}^{-k \cdot t}
$$
$$
\textit{eq\_data} := 3 {\mathrm e}^{-2 k}= 0.9
$$
$$
\textit{k\_val} :=  0.6019864022
$$
$$
\text{``The Decay Constant k is:''}, 0.6019864022
$$
$$
\textit{T\_half} :=  1.661167090 \ln \! \left(2\right)
$$
$$
\text{``Half-Life (years):''}, 1.151433285
$$
$$
\textit{eq\_half} := \mathit{x0} {\mathrm e}^{- 0.6019864022 T}=\frac{\mathit{x0}}{2}
$$
$$
\textit{T\_derived} :=  1.151433285
$$
$$
\text{``Verified Half-Life:''}, 1.151433285
$$

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S5 - ESERCISE 3:

(Carbon dating of Shroud from Turin)
In 1988 three independent dating tests reveald thatthe quantity of C14 in the shroud was between 91:57\% and 93:021\%.
Using the decay constant for C14 found it in the previous exercise determine when shroud was made.

```maple
> restart;

##### --- 1. Define Model and Parameters ---
##### Differential Equation: x'(t) = -k * x(t)
RD_eq := diff(x(t), t) = -k * x(t);
sol := dsolve(\{RD_eq, x(0)=x0\}, x(t));
x_sol := unapply(rhs(sol), t, x0, k);

##### Constants
T12_C14 := 5730;          # Half-life of C14
k_val := ln(2) / T12_C14; # Decay constant

##### --- 2. Calculate Age for the Given Percentages ---
##### Case 1: 91.57% Remaining
##### Set current amount x(t) = 0.9157 * Initial_Amount (x0)
eq1 := x_sol(t1, x0, k_val) = (91.57/100) * x0;
Age1 := solve(eq1, t1);

##### Case 2: 93.021% Remaining
eq2 := x_sol(t2, x0, k_val) = (93.021/100) * x0;
Age2 := solve(eq2, t2);

print("Estimated Age Range (Years before 1988):");
print(evalf(Age2), "to", evalf(Age1));

##### --- 3. Determine the Calendar Date ---
##### The test was conducted in 1988. Subtract the age from 1988.
Year1 := 1988 - Age1;
Year2 := 1988 - Age2;

print("Estimated Year of Origin:");
printf("Between %.0f and %.0f AD\n", Year1, Year2);
```
$$
\textit{RD\_eq} := \frac{d}{d t}x \! \left(t \right)=-k x \! \left(t \right)
$$
$$
\mathit{sol} := x \! \left(t \right)=\mathit{x0} {\mathrm e}^{-k t}
$$
$$
\textit{x\_sol} := \left(t ,\mathit{x0} ,k \right)\hiderel{\mapsto }\mathit{x0} \cdot {\mathrm e}^{-k \cdot t}
$$
$$
\textit{T12\_C14} := 5730
$$
$$
\textit{k\_val} := \frac{\ln \! \left(2\right)}{5730}
$$
$$
\mathit{eq1} := \mathit{x0} {\mathrm e}^{-\frac{\ln \left(2\right) \mathit{t1}}{5730}}= 0.9157000000 \mathit{x0}
$$
$$
\mathit{Age1} :=  728.0141045
$$
$$
\mathit{eq2} := \mathit{x0} {\mathrm e}^{-\frac{\ln \left(2\right) \mathit{t2}}{5730}}= 0.9302100000 \mathit{x0}
$$
$$
\mathit{Age2} :=  598.0495293
$$
$$
\text{``Estimated Age Range (Years before 1988):''}
$$
$$
598.0495293,\text{``to''}, 728.0141045
$$
$$
\mathit{Year1} :=  1259.985896
$$
$$
\mathit{Year2} :=  1389.950471
$$
$$
\text{``Estimated Year of Origin:''}
$$
```maple
Between 1260 and 1390 AD
```
_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S5 - ESERCISE 4:

Find room temperature variation in a summer day knowing that the outside temperature variation is given by the function T_out(t) = 35 * e^(-(t-12)^2 / 74)
(the time variable is measured in hours, t = 0 means the midnight, notice that at t = 12, the midday, we have the highest outside temperature of 35°C
and at the midnight we have the lowest outside temperature, aprox. 5°C).
Suppose that the initial room temperature at t = 0 is T_0 = 15°C and the room thermic coefficient is k = 0.2 * hours⁻¹.
Plot the solution on a day interval [0; 24] and estimate the time when the room temperature is highest.

```maple
> restart;
with(plots):

##### --- 1. Define Functions and Parameters ---
Tout := t -> 35 * exp(-(t-12)^2 / 74);

##### Parameters
k := 0.2;     # Thermic coefficient
T0 := 15;     # Initial temperature

##### Differential Equation: T' = -k(T - Tout)
eqd := diff(T(t), t) = -k * (T(t) - Tout(t));

##### --- 2. Solve the Differential Equation ---
sol := dsolve(\{eqd, T(0)=T0\}, T(t));

##### Define the solution as a function Tsol(t)
Tsol := unapply(rhs(sol), t);

##### --- 3. Find Time of Maximum Temperature ---
##### Need to find t where T'(t) = 0.
##### Because the solution involves 'erf' (Error Function), symbolic 'solve' struggles.
##### Use 'fsolve' (numeric solve) to find the root in the afternoon (t=10..24).
t_max := fsolve(diff(Tsol(t), t) = 0, t=10..24);

print("Time of highest room temperature:", t_max);
print("Maximum temperature value:", evalf(Tsol(t_max)));

##### --- 4. Visualization ---
PlotOut := plot(Tout(t), t=0..24, color=red, linestyle=dash, legend="Outside Temp");
PlotRoom := plot(Tsol(t), t=0..24, color=blue, thickness=2, legend="Room Temp");
display([PlotOut, PlotRoom],
    title="Room vs Outside Temperature (Thermal Lag)",
    labels=["Time (hours)", "Temperature (C)"],
    labeldirections=[HORIZONTAL, VERTICAL]
);
```
$$
\mathit{Tout} := t \mapsto 35\cdot {\mathrm e}^{-\frac{(t -12)^{2}}{74}}
$$
$$
k :=  0.2
$$
$$
\mathit{T0} := 15
$$
$$
\mathit{eqd} := \frac{d}{d t}T \! \left(t \right)=- 0.2 T \! \left(t \right)+ 7.0 {\mathrm e}^{-\frac{\left(t -12\right)^{2}}{74}}
$$
$$
\mathit{sol} := T \! \left(t \right)=\frac{{\mathrm e}^{-\frac{t}{5}} \left(7 \sqrt{\pi}\, \sqrt{74}\, \left(\mathrm{erf}\! \left(\frac{\sqrt{74}\, \left(5 t -97\right)}{370}\right)+\mathrm{erf}\! \left(\frac{97 \sqrt{74}}{370}\right)\right) {\mathrm e}^{\frac{157}{50}}+30\right)}{2}
$$
$$
\mathit{Tsol} := t \hiderel{\mapsto }\frac{{\mathrm e}^{-\frac{t}{5}}\cdot \left(7\cdot \sqrt{\pi}\cdot \sqrt{74}\cdot \left(\mathrm{erf}\! \left(\frac{\sqrt{74}\cdot \left(5\cdot t -97\right)}{370}\right)+\mathrm{erf}\! \left(\frac{97\cdot \sqrt{74}}{370}\right)\right)\cdot {\mathrm e}^{\frac{157}{50}}+30\right)}{2}
$$
$$
\textit{t\_max} :=  15.53621149
$$
$$
\text{``Time of highest room temperature:''}, 15.53621149
$$
$$
\text{``Maximum temperature value:''}, 29.55829425
$$
![Plot](MM_GENERAL_FULL_DOCplot2d32.png)
![Plot](MM_GENERAL_FULL_DOCplot2d33.png)
![Plot](MM_GENERAL_FULL_DOCplot2d34.png)

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S5 - ESERCISE 5:     -->   Gompertz Differential Equation

Letís consider the Gompertz Differential Equation model used in the growth of tumors:

$ \displaystyle \left{\begin{array}{cc}
x\esapos{(t)}=r\cdotx{(t)}\cdot\ln\cdot{(\frac{k}{x{\textcolor{DarkOrchid{(}t)}}})} &
\\
x (0)=x_ {{0}} &
\end{array},\mathit{where} x_ {{0}}>0\boldsymbol{\land}r >0 $

(a) Find the model solution;
\

(b) Make numerical simulation.

```maple
> restart;
with(DEtools):

##### --- Part (a): Find the Model Solution ---
##### Define the Gompertz Differential Equation: x'(t) = r * x(t) * ln(K / x(t))
eqd := diff(x(t), t) = r * x(t) * ln(K/x(t));

##### Solve the Initial Value Problem symbolically
print("General Solution x(t):");
gen_sol := dsolve(\{eqd, x(0) = x0\}, x(t));

##### --- Part (b): Numerical Simulation ---
##### Parameters: r=0.1 (growth rate), K=200 (carrying capacity), x0=10 (initial tumor size)
r := 0.1;
K := 200;

##### 1. Plot trajectories for different initial conditions
##### Case 1: x0 = 10 (Small tumor grows to K)
##### Case 2: x0 = 200 (At capacity, stays constant)
##### Case 3: x0 = 300 (Above capacity, shrinks to K)
DEplot(eqd, x(t), t=0..60,
    [[x(0)=10], [x(0)=200], [x(0)=300]],
    linecolor=[blue, green, red],
    title="Gompertz Growth Model (Convergence to K=200)",
    labels=["Time (t)", "Tumor Size x(t)"]
);

##### --- Part (c): Equilibrium Analysis (Optional but helpful) ---
f := x -> r * x * ln(K/x);
Equilibrium := solve(f(x) = 0, x);
print("Equilibrium Points:", Equilibrium);

##### Check stability at K = 200
##### If derivative < 0, it is stable.
Stability_Check := eval(D(f)(x), x=200);
print("Derivative at K=200:", Stability_Check);
##### Since r=0.1, the result is -0.1. Negative -> Stable.
```
$$
\mathit{eqd} := \frac{d}{d t}x \! \left(t \right)=r x \! \left(t \right) \ln \! \left(\frac{K}{x \! \left(t \right)}\right)
$$
$$
\text{``General Solution x(t):''}
$$
$$
\textit{gen\_sol} := x \! \left(t \right)=K {\mathrm e}^{-\left(\ln \left(\frac{K}{\mathit{x0}}\right)+2 \,\mathrm{I} \pi  \_Z2 \right) {\mathrm e}^{-t r}}
$$
$$
r :=  0.1
$$
$$
K := 200
$$
![Plot](MM_GENERAL_FULL_DOCplot2d35.png)
$$
f := x \hiderel{\mapsto }r \cdot x \cdot \ln \! \left(\frac{K}{x}\right)
$$
$$
\mathit{Equilibrium} :=  200.0
$$
$$
\text{``Equilibrium Points:''}, 200.0
$$
$$
\textit{Stability\_Check} := - 0.1
$$
$$
\text{``Derivative at K=200:''},- 0.1
$$

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S5 - ESERCISE 6:

Letís consider the mathematical model used in the growth of cells:

$ \left{\begin{array}{cc}
x\esapos{(t)}=\frac{b\cdotx{(t)}}{1+x{\textcolor{DarkOrchid{(}t)}}}-d\cdotx{(t)} &
\\
x (0)=x_ {{0}} &
\end{array} $, where b is the cells birth rate, d is the cells death rate. The factor 1/(1+x(t)) simulates the crowding effect.

(a) Find the equilibrium solutions and study their stability;
\

(b) Make numerical simulation.

```maple
> restart;
with(plots):
with(DEtools):

##### --- Part (a): Equilibrium and Stability ---
##### Define the rate function f(x): x' = f(x)
f := x -> (b*x)/(1+x) - d*x;

##### 1. Find Equilibrium Points (where f(x) = 0)
Eq_Points := solve(f(x) = 0, x);
print("Equilibrium Points:", Eq_Points);

##### Results interpretation:
##### x1 = 0 (Trivial)
##### x2 = (b - d)/d  (Non-trivial, exists only if b > d)

##### 2. Analyze Stability using the First Derivative Test
##### If f'(x*) < 0, the point is Stable.
df := diff(f(x), x);

print("--- Stability of x=0 ---");
Stab_0 := simplify(eval(df, x=0));
##### Result is b - d.
##### If b < d (Death > Birth), then b-d < 0 => Stable (Extinction).
##### If b > d (Birth > Death), then b-d > 0 => Unstable.

print("--- Stability of Non-Trivial Point ---");
##### Substitute the second equilibrium point into derivative
x_star := (b-d)/d;
Stab_Star := simplify(eval(df, x=x_star));
##### Result is d*(d-b)/b.
##### If b > d, then (d-b) is negative => Stable (Survival).

##### --- Part (b): Numerical Simulation ---
##### Simulate two cases to visualize the stability findings.

##### CASE 1: Survival (b > d)
##### Let Birth Rate b = 2.0, Death Rate d = 1.0
##### Expected Equilibrium: x* = (2-1)/1 = 1.0
b := 2.0; d := 1.0;
deq_1 := diff(x(t), t) = (b*x(t))/(1+x(t)) - d*x(t);

PlotSurvival := DEplot(deq_1, x(t), t=0..10,
    [[x(0)=0.1], [x(0)=2.0]],
    linecolor=green,
    title="Case 1: Survival (b > d)",
    labels=["Time", "Population"]
);

##### CASE 2: Extinction (b < d)
##### Let Birth Rate b = 0.5, Death Rate d = 1.0
##### Expected Equilibrium: x=0 is stable.
b := 0.5; d := 1.0;
deq_2 := diff(x(t), t) = (b*x(t))/(1+x(t)) - d*x(t);

PlotExtinction := DEplot(deq_2, x(t), t=0..10,
    [[x(0)=2.0], [x(0)=0.5]],
    linecolor=red,
    title="Case 2: Extinction (b < d)",
    labels=["Time", "Population"]
);

display(Array([PlotSurvival, PlotExtinction]));
```
$$
f := x \mapsto \frac{b \cdot x}{1+x}-d \cdot x
$$
$$
\textit{Eq\_Points} := 0,\frac{b -d}{d}
$$
$$
\text{``Equilibrium Points:''},0,\frac{b -d}{d}
$$
$$
\mathit{df} := \frac{b}{1+x}-\frac{b x}{(1+x)^{2}}-d
$$
$$
\text{``--- Stability of x=0 ---''}
$$
$$
\textit{Stab\_0} := b -d
$$
$$
\text{``--- Stability of Non-Trivial Point ---''}
$$
$$
\textit{x\_star} := \frac{b -d}{d}
$$
$$
\textit{Stab\_Star} := -\frac{(b -d) d}{b}
$$
$$
b :=  2.0
$$
$$
d :=  1.0
$$
$$
\textit{deq\_1} := \frac{d}{d t}x \! \left(t \right)=\frac{ 2.0 x \! \left(t \right)}{1+x \! \left(t \right)}- 1.0 x \! \left(t \right)
$$
![Plot](MM_GENERAL_FULL_DOCplot2d36.png)
$$
b :=  0.5
$$
$$
d :=  1.0
$$
$$
\textit{deq\_2} := \frac{d}{d t}x \! \left(t \right)=\frac{ 0.5 x \! \left(t \right)}{1+x \! \left(t \right)}- 1.0 x \! \left(t \right)
$$
![Plot](MM_GENERAL_FULL_DOCplot2d37.png)
\begin{center}
\begin{tabular}{ c c }
& \\
\end{tabular}
\end{center}

\newpage
\

= = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =

### TEST ON TEAMS:  Modelling with First order differential equations

**TEST - EXERCISE 1:**\

\

Find the solution for the following difference equations:\

\

**Difference equation:**\

a) $x_{n+1}$ = [(n+1)/(n+2)]^2 * $x_{n}$ + [1/(n+2)] , where $x_{0}$=1\

\

**Higher-order difference equation:**\

b) $x_{n+3}$ - 4 * $x_{n+2}$ = $x_{n+1}$ + 6 * $x_{n}$ = 60 * $4^{n}$ , where $x_{0}$=2, $x_{1}$=12, $x_{2}$=12\

\

**Non-linear difference equation:**\

c) $x_{n+1}$ = ( 2 * $x_{n}$ ) / ( 1 + 4 * $x_{n}$ ) ,where $x_{0}$=1, Hint: use substitution $x_{n}$ = 1/$y_{n}$
```maple
> ##### a) x_\{n+1\} = [(n+1)/(n+2)]^2 * x_\{n\} + [1/(n+2)] , where x_\{0\}=1
restart;

Equation_a:=x(n+1)=((n+1)/(n+2))^2*x(n)+1/(n+2);
simplify(%);
x(0) = 1;
Solution_a := rsolve(\{Equation_a, x(0)=1\}, x(n));
Function_a := unapply(Solution_a, n);   # converts solution to afunction
seq(Function_a(n), n=0..10);
```
$$
\textit{Equation\_a} := x \! \left(n +1\right)=\frac{\left(n +1\right)^{2} x \! \left(n \right)}{\left(n +2\right)^{2}}+\frac{1}{n +2}
$$
$$
x \! \left(n +1\right)=\frac{\left(n +1\right)^{2} x \! \left(n \right)+n +2}{\left(n +2\right)^{2}}
$$
$$
x \! \left(0\right)=1
$$
$$
\textit{Solution\_a} := \frac{n +2}{2 (n +1)}
$$
$$
\textit{Function\_a} := n \mapsto \frac{n +2}{2\cdot (n +1)}
$$
$$
1,\frac{3}{4},\frac{2}{3},\frac{5}{8},\frac{3}{5},\frac{7}{12},\frac{4}{7},\frac{9}{16},\frac{5}{9},\frac{11}{20},\frac{6}{11}
$$
```maple
> ##### b) x_\{n+3\} - 4 * x_\{n+2\} = x_\{n+1\} + 6 * x_\{n\} = 60 * 4^[n] , where x_\{0\}=2, x_\{1\}=12, x_\{2\}=12
restart;

Equation_b:=x(n+3)-4*x(n+2)+x(n+1)+6*x(n)=60*4^n;
simplify(%);
Solution_b:=rsolve(\{Equation_b, x(0)=2, x(1)=12, x(2)=12\}, x(n));
Function_b:=unapply(Solution_b, n);
seq(Function_b(n), n=0..10);
```
$$
\textit{Equation\_b} := x \! \left(n +3\right)-4 x \! \left(n +2\right)+x \! \left(n +1\right)+6 x \! \left(n \right)=60 \,4^{n}
$$
$$
x \! \left(n +3\right)-4 x \! \left(n +2\right)+x \! \left(n +1\right)+6 x \! \left(n \right)=60 \,4^{n}
$$
$$
\textit{Solution\_b} := -4 \left(-1\right)^{n}-16 \,3^{n}+16 \,2^{n}+6 \,4^{n}
$$
$$
\textit{Function\_b} := n \hiderel{\mapsto }-4\cdot \left(-1\right)^{n}-16\cdot 3^{n}+16\cdot 2^{n}+6\cdot 4^{n}
$$
$$
2,12,12,84,492,2772,13932,65364,292332,1266132,5363052
$$
```maple
> ##### Hint: use substitute x(n) with 1/y(n)
##### x_\{n+1\} = ( 2 * x_\{n\} ) / ( 1 + 4 * x_\{n\} ) ,where x_\{0\}=1
restart;

Equation_c_y:=y(n+1)=y(n)/2+2;
simplify(%);
Solution_c_y:=rsolve(\{Equation_c_y, y(0)=1\}, y(n));
Conversion_x:=1/Solution_c_y;
Function_x:=unapply(Conversion_x,n);
seq(Function_x(n), n=0..10);
```
$$
\textit{Equation\_c\_y} := y \! \left(n +1\right)=\frac{y \! \left(n \right)}{2}+2
$$
$$
y \! \left(n +1\right)=\frac{y \! \left(n \right)}{2}+2
$$
$$
\textit{Solution\_c\_y} := -3 \left(\frac{1}{2}\right)^{n}+4
$$
$$
\textit{Conversion\_x} := \frac{1}{-3 \left(\frac{1}{2}\right)^{n}+4}
$$
$$
\textit{Function\_x} := n \hiderel{\mapsto }\frac{1}{-3\cdot \left(\frac{1}{2}\right)^{n}+4}
$$
$$
1,\frac{2}{5},\frac{4}{13},\frac{8}{29},\frac{16}{61},\frac{32}{125},\frac{64}{253},\frac{128}{509},\frac{256}{1021},\frac{512}{2045},\frac{1024}{4093}
$$

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### TEST - ESERCISE 2:

Let us consider the difference equation: $x_{n+1}$ = ( $x^{x}$_{n} + 7 ) / ( 2 * $x_{n}$ )\

\

a) Find the equilibrium points and study their stability;
\

b) Do some numerical simulations.\

```maple
> ##### a) Find the equilibrium points and study their stability;
restart;
Function:=x->(x^2+7)/(2*x);
simplify(%);
Fixed_Points:=solve(x = Function(x),x);
Function_Derivative := D(Function);
Fixed_Point_1:=Fixed_Points[1];
Stability_Point_1 := evalf(Function_Derivative(Fixed_Point_1));
Fixed_Point_2:=Fixed_Points[2];
Stability_Point_2 := evalf(Function_Derivative(Fixed_Point_2));
solve(\{Function(x)=x\},\{x\});

##### Note: "Stability_Point< 1", it is stable point. Points are asymtotically stable.
##### b) Do some numerical simulations.
x[0]:=1; N:=10;   # initial condition and N iterations
for n from 0 to N-1 do
    x[n+1]:=Function(x[n]);
end do:
seq(evalf(x[n]), n=0..N);
plot([[[m, x[m]]$ m=0..N], [[m,1]$m=0..N]], style=[point,point], symbol=[circle,cross], color=[red,blue]);
```
$$
\mathit{Function} := x \mapsto \frac{x^{2}+7}{2\cdot x}
$$
$$
x \mapsto \frac{x^{2}+7}{2\cdot x}
$$
$$
\textit{Fixed\_Points} := \sqrt{7},-\sqrt{7}
$$
$$
\textit{Function\_Derivative} := x \mapsto 1-\frac{x^{2}+7}{2\cdot x^{2}}
$$
$$
\textit{Fixed\_Point\_1} := \sqrt{7}
$$
$$
\textit{Stability\_Point\_1} :=  0.0
$$
$$
\textit{Fixed\_Point\_2} := -\sqrt{7}
$$
$$
\textit{Stability\_Point\_2} :=  0.0
$$
$$
{\{x =-\sqrt{7}\}},{\{x =\sqrt{7}\}}
$$
$$
x_{0}:= 1
$$
$$
N := 10
$$
$$
1.0, 4.0, 2.875000000, 2.654891304, 2.645767044, 2.645751311, 2.645751311, 2.645751311, 2.645751311, 2.645751311, 2.645751311
$$
![Plot](MM_GENERAL_FULL_DOCplot2d38.png)

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### TEST - ESERCISE 3:

Let us consider the system of **difference equations**:\

{ $x_{n+1}$ = $x_{n}$ - $x_{n}$^[2] - $x_{n}$ * $y_{n}$\

{ $y_{n+1}$ = 2 * $y_{n}$ - y{n}^[2] - 3 * $x_{n}$ * $y_{n}$\

\

a) Find and study the stability of the equilibrium points;\

b) Do some numerical simulations.\

\

**Note**: From Lecture 4 --> The zero solution is locally stable if and only if " |lambda| <= 1 "and the eigenvalues of unit modulus are simple.\

```maple
> ##### a) Find and study the stability of the equilibrium points;
##### Solve the system x = f(x,y) and y = g(x,y)
restart;
with(linalg):   #used for matrixes and eigenvalue operations

Function_f:=(x,y)->x-x^2-x*y;
simplify(%);
Function_g:=(x,y)->2*y-y^2-3*x*y;
simplify(%);
Equilibrium_Points:=solve(\{Function_f(x,y)=x,Function_g(x,y)=y\},\{x,y\});;
Jacobian_Matrix:=jacobian([Function_f(x,y), Function_g(x,y)], [x,y]);
Matrix_1:=subs(x=0,y=0,evalm(Jacobian_Matrix));
Evaluation_Solution_1 := eigenvals(Matrix_1);
##### Note: Since results are "1,2", the point is unstable.

Matrix_2:=subs(x=0, y=1, evalm(Jacobian_Matrix));
Evaluation_Solution_2 := eigenvals(Matrix_2);
##### Note: Since results are "0, 0". Since |0| < 1, this point is locally asymtotically stable.

##### b) Do some numerical simulations.
x[0] := 0.1;
y[0] := 0.2;
N := 50;
for i from 0 to N-1 do
    x[i+1] := Function_f(x[i], y[i]);
    y[i+1] := Function_g(x[i], y[i]);
end do:
seq([x[i],y[i]],i=0..50);
plot([[n, x[n]]$n=0..N], style=point, symbol=circle, color=red);
plot([[n,y[n]]$n=0..N], style=point, symbol=circle, color=blue);
plot([[x[n],y[n]]$n=0..50], style=point, symbol=circle);
xx:=0.001;
plot([[[n,x[n]]$n=0..N],[[n,xx]$n=0..N]],style=[point,point],symbol=[circle,cross],color=[red,blue]);
```
$$
\textit{Function\_f} := \left(x ,y \right)\hiderel{\mapsto }x -x^{2}-x \cdot y
$$
$$
\left(x ,y \right)\hiderel{\mapsto }x -x^{2}-x \cdot y
$$
$$
\textit{Function\_g} := \left(x ,y \right)\hiderel{\mapsto }2\cdot y -y^{2}-3\cdot x \cdot y
$$
$$
\left(x ,y \right)\hiderel{\mapsto }2\cdot y -y^{2}-3\cdot x \cdot y
$$
$$
\textit{Equilibrium\_Points} := \left\{x  \hiderel{=} \frac{1}{2},y  \hiderel{=} -\frac{1}{2}\right\},\left\{x  \hiderel{=} 0,y  \hiderel{=} 0\right\},\left\{x  \hiderel{=} 0,y  \hiderel{=} 1\right\}
$$
$$
\textit{Evaluation\_Solution\_1} := 1,2
$$
$$
\textit{Evaluation\_Solution\_2} := 0,0
$$
$$
x_{0}:=  0.1
$$
$$
y_{0}:=  0.2
$$
$$
N := 50
$$
$$
\left[ 0.1, 0.2\right],\left[ 0.07, 0.30\right],\left[ 0.0441, 0.4470\right],\left[ 0.02244249, 0.63505290\right],\left[ 0.00768665628, 0.8240571091\right],\left[ 0.001293327842, 0.9500413676\right],\left[ 0.000062940193, 0.9938179899\right],\left[ 3.8513544\times 10^{-7}, 0.9997741297\right],\left[ 8.68424\times 10^{-11}, 0.9999987935\right],\left[ 1.0477\times 10^{-16}, 0.9999999997\right],\left[ 0.0, 0.9999999996\right],\left[ 0.0, 0.9999999998\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right],\left[ 0.0, 1.0\right]
$$
![Plot](MM_GENERAL_FULL_DOCplot2d39.png)
![Plot](MM_GENERAL_FULL_DOCplot2d40.png)
![Plot](MM_GENERAL_FULL_DOCplot2d41.png)
$$
\mathit{xx} :=  0.001
$$
![Plot](MM_GENERAL_FULL_DOCplot2d42.png)

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### TEST - ESERCISE 4:

Let's consider the simple interest model: S(n+1)=S(n)+p*S(0)\

And the compound interest model: S(n+1)=s(n)+(p/r)*S(n)\

\

Company A offers simple interest at an annual rate of 4\%.\

Company B offers compound interest at an annual rate of 3\% with a conversion period of one month.\

\

a) Calculate for the both cases the amount on deposit after 5, 10, 15, and 20 years for principal S(0) = 1000;
\

b) Determine which interest offer maximizes the amount on deposit after 5, 10, 15 and 20 years?\

```maple
> restart;
with(plots):

##### --- Part (a): Define Models and Calculate Amounts ---
##### --- Company A: Simple Interest ---
##### S(n+1) = S(n) + p * S0
Difference_Equation_A := S_A(n+1) = S_A(n) + p * S0;
Solution_A := rsolve(\{Difference_Equation_A, S_A(0)=S0\}, S_A(n));

##### Function A: n = years, p = annual rate, S0 = principal
Function_A := unapply(Solution_A, n, p, S0);

##### --- Company B: Compound Interest ---
##### S(n+1) = S(n) + (p/r) * S(n)
Difference_Equation_B := S_B(n+1) = S_B(n) + (p/r) * S_B(n);
Solution_B := rsolve(\{Difference_Equation_B, S_B(0)=S0\}, S_B(n));

##### Function B: n = periods (months), p = annual rate, r = frequency, S0 = principal
Function_B := unapply(Solution_B, n, p, r, S0);

##### --- Calculate and Display Values ---
years := [5, 10, 15, 20];
S0_val := 1000;
p_A := 0.04;
p_B := 0.03;
r_B := 12;

print("Comparison of Account Balances:");
printf("%-10s %-15s %-15s\n", "Year", "Company A", "Company B");

for t in years do
    Val_A := evalf(Function_A(t, p_A, S0_val));
    Val_B := evalf(Function_B(t * 12, p_B, r_B, S0_val));
    printf("%-10d %-15.2f %-15.2f\n", t, Val_A, Val_B);
end do;

##### --- Part (b): Maximize Amount (Visualization) ---
##### Plot both functions over 25 years to see the crossover
##### Note: For Function_B, the x-axis input must be scaled by 12 (x*12) inside the function call
plot([
    Function_A(x, p_A, S0_val),
    Function_B(x * 12, p_B, r_B, S0_val)
    ],
    x=0..25,
    color=[blue, red],
    legend=["Company A (Simple 4%)", "Company B (Compound 3%)"],
    title="Growth Comparison: Simple vs Compound Interest",
    labels=["Years", "Amount"],
    thickness=2
);

##### Conclusion:
print("From the table and plot:");
print("At Year 5: Company A (1200) > Company B (1161)");
print("At Year 10: Company A (1400) > Company B (1349)");
print("At Year 15: Company A (1600) > Company B (1567)");
print("At Year 20: Company A (1800) < Company B (1820)");
print("Company A is the better offer for 5, 10, and 15 years.");
print("Company B becomes the better offer by year 20.");
```
$$
\textit{Difference\_Equation\_A} := \textit{S\_A} \! \left(n +1\right)=\textit{S\_A} \! \left(n \right)+p \mathit{S0}
$$
$$
\textit{Solution\_A} := \mathit{S0} -p \mathit{S0} +p \mathit{S0} (n +1)
$$
$$
\textit{Function\_A} := \left(n ,p ,\mathit{S0} \right)\hiderel{\mapsto }\mathit{S0} -p \cdot \mathit{S0} +p \cdot \mathit{S0} \cdot \left(n +1\right)
$$
$$
\textit{Difference\_Equation\_B} := \textit{S\_B} \! \left(n +1\right)=\textit{S\_B} \! \left(n \right)+\frac{p \textit{S\_B} \! \left(n \right)}{r}
$$
$$
\textit{Solution\_B} := \mathit{S0} \left(\frac{p +r}{r}\right)^{n}
$$
$$
\textit{Function\_B} := \left(n ,p ,r ,\mathit{S0} \right)\hiderel{\mapsto }\mathit{S0} \cdot \left(\frac{p +r}{r}\right)^{n}
$$
$$
\mathit{years} := \left[5,10,15,20\right]
$$
$$
\textit{S0\_val} := 1000
$$
$$
\textit{p\_A} :=  0.04
$$
$$
\textit{p\_B} :=  0.03
$$
$$
\textit{r\_B} := 12
$$
$$
\text{``Comparison of Account Balances:''}
$$
```maple
Year       Company A       Company B
```
$$
\textit{Val\_A} :=  1200.0
$$
$$
\textit{Val\_B} :=  1161.616782
$$
```maple
5          1200.00         1161.62
```
$$
\textit{Val\_A} :=  1400.0
$$
$$
\textit{Val\_B} :=  1349.353547
$$
```maple
10         1400.00         1349.35
```
$$
\textit{Val\_A} :=  1600.0
$$
$$
\textit{Val\_B} :=  1567.431725
$$
```maple
15         1600.00         1567.43
```
$$
\textit{Val\_A} :=  1800.0
$$
$$
\textit{Val\_B} :=  1820.754995
$$
```maple
20         1800.00         1820.75
```
![Plot](MM_GENERAL_FULL_DOCplot2d43.png)
$$
\text{``From the table and plot:''}
$$
$$
\text{``At Year 5: Company A (1200) \textgreater Company B (1161)''}
$$
$$
\text{``At Year 10: Company A (1400) \textgreater Company B (1349)''}
$$
$$
\text{``At Year 15: Company A (1600) \textgreater Company B (1567)''}
$$
$$
\text{``At Year 20: Company A (1800) \textless \ Company B (1820)''}
$$
$$
\text{``Company A is the better offer for 5, 10, and 15 years.''}
$$
$$
\text{``Company B becomes the better offer by year 20.''}
$$
