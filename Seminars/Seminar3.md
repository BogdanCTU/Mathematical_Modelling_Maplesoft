= = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = =

## SEMINAR 3: Stability of the equilibrium points

### S3 - EXERCISE 1

Let's consider the **difference equation**: **x(n+1) = f (x n)**
Determine, in each case, the **equilibrium points and study** their stability using the stability theorem in the first approximation.
Make some numerical simulations.

(a) $$x(n+1)=1/(2)xn$$

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
    valderivative := eval(df(x), x=pt);
    print("Checking point:", pt);
    print("Derivative value:", valderivative);

    if abs(valderivative) < 1 then
        print("Conclusion: Asymptotically Stable (Attractor)");
    elif abs(valderivative) > 1 then
        print("Conclusion: Unstable (Repeller)");
    else
        print("Conclusion: Inconclusive (Linearization fails)");
    end if;
end do;

##### --- Numerical Simulation ---
x0 := 100;
Nsteps := 10;
Orbit := Vector(Nsteps+1);
Orbit[1] := x0;

for i from 1 to Nsteps do
    Orbit[i+1] := f(Orbit[i]);
end do;

DataPoints := [seq([n-1, Orbit[n]], n=1..Nsteps+1)];

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
\text{EqPoints} := [ 0.0]
$$
$$
\text{"Equilibrium Point(s):"}, [ 0.0]
$$
$$
\text{df} :=  0.5
$$
$$
\text{valderivative} :=  0.5
$$
$$
\text{"Checking point:"}, 0.0
$$
$$
\text{"Derivative value:"}, 0.5
$$
$$
\text{"Conclusion: Asymptotically Stable (Attractor)"}
$$
$$
\text{x0} := 100
$$
$$
\text{Nsteps} := 10
$$
$$
\text{Orbit}_{1}:= 100
$$
$$
\text{Orbit}_{2}:=  50.0
$$
$$
\text{Orbit}_{3}:=  25.0
$$
$$
\text{Orbit}_{4}:=  12.500
$$
$$
\text{Orbit}_{5}:=  6.2500
$$
$$
\text{Orbit}_{6}:=  3.12500
$$
$$
\text{Orbit}_{7}:=  1.562500
$$
$$
\text{Orbit}_{8}:=  0.7812500
$$
$$
\text{Orbit}_{9}:=  0.39062500
$$
$$
\text{Orbit}_{10}:=  0.195312500
$$
$$
\text{Orbit}_{11}:=  0.0976562500
$$
$$
\text{DataPoints} :=
[[0,100], [1, 50.0], [2, 25.0], [3, 12.500], [4, 6.2500],
[5, 3.12500], [6, 1.562500], [7, 0.7812500], [8, 0.39062500],
[9, 0.195312500], [10, 0.0976562500]]
$$

![Plot](./Images/MM_GENERAL_FULL_DOCplot2d10.png)

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S3 - ESERCISE 2

Consider the following mosquito model:

$$ x_{n+1} = \left( a \cdot x_{n} + b \cdot x_{n-1} \cdot e^{-x_{n-1}} \right) \cdot e^{-x_{n}} $$

where $a \in (0, 1)$, $b \in [0, +\infty)$.

This equation describes the growth of a mosquito population. Mosquitoes lay eggs, some of which hatch as soon as conditions are favorable, while others remain dormant for a year or two. In this model, it is assumed that eggs are dormant for one year at most.

(a) Find the equilibrium points;

(b) Study the stability of the equilibrium points;

(c) Make numerical simulations.

```maple
> restart;
with(plots):

##### --- Define the Model Function ---
f := (u,v) -> (a*u + b*v*exp(-v)) * exp(-u);

##### --- Part (a): Find Equilibrium Points ---
EqRelation := x = f(x,x);
EqSolutions := solve(EqRelation, x);

xtrivial := 0;
xnontrivial := EqSolutions[1];

print("Trivial Equilibrium:", xtrivial);
print("Non-Trivial Equilibrium (Symbolic):", xnontrivial);

##### --- Part (b): Stability Analysis Setup ---
# Derivatives for Linearization
# Characteristic Eq will be: q^2 - J1*q - J2 = 0
p1 := diff(f(u,v), u); # J1
p2 := diff(f(u,v), v); # J2

##### --- CASE 1: EXTINCTION (a=0.4, b=0.5) ---
print("----------- CASE 1 -----------");
a1 := 0.4;
b1 := 0.5;

##### 1. Stability Check, Since a+b = 0.9 (which is <= 1), the only equilibrium is x=0
print("Condition Check: a+b <= 1, so population should die out to 0.");
valJ11 := evalf(eval(p1, {u=0, v=0, a=a1, b=b1}));
valJ21 := evalf(eval(p2, {u=0, v=0, a=a1, b=b1}));
print("Eigenvalues at 0:", solve(q^2 - valJ11*q - valJ21=0, q));

##### 2. Numerical Simulation
steps := 50;
X := Vector(steps);
X[1] := 0.1; X[2] := 0.1; # Initial conditions

for i from 2 to steps-1 do
    X[i+1] := evalf( (a1*X[i] + b1*X[i-1]*exp(-X[i-1])) * exp(-X[i]) );
end do:
Plot1 := pointplot([seq([n, X[n]], n=1..steps)], style=line, color=blue, title="Case 1: Extinction (x->0)");

##### --- CASE 2: SURVIVAL (a=0.7, b=2.0) ---
print("----------- CASE 2 -----------");
a2 := 0.7;
b2 := 2.0;

##### 1. Stability Check, Since a+b = 2.7 (> 1), a positive equilibrium exists
xstar2 := evalf( ln( (a2 + sqrt(a2^2 + 4*b2))/2 ) );
print("Equilibrium Point x*:", xstar2);

##### Evaluate Jacobian at x*
valJ12 := evalf(eval(p1, {u=xstar2, v=xstar2, a=a2, b=b2}));
valJ22 := evalf(eval(p2, {u=xstar2, v=xstar2, a=a2, b=b2}));

##### Solve Characteristic Equation
roots2 := [solve(q^2 - valJ12*q - valJ22 = 0, q)];
print("Eigenvalues:", roots2);
# Check if stable (|q| < 1)
if abs(roots2[1]) < 1 and abs(roots2[2]) < 1 then print("STABLE"); else print("UNSTABLE"); end if;

##### 2. Numerical Simulation
X := Vector(steps);
X[1] := 0.1; X[2] := 0.1;

for i from 2 to steps-1 do
    X[i+1] := evalf( (a2*X[i] + b2*X[i-1]*exp(-X[i-1])) * exp(-X[i]) );
end do:
Plot2 := pointplot([seq([n, X[n]], n=1..steps)], style=line, color=red, title="Case 2: Survival (x->x*)");

##### --- Display Both Plots ---
display(Array([Plot1, Plot2]));
```

$$
f := (u, v) \mapsto (a \cdot u + b \cdot v \cdot {\mathrm e}^{-v}) \cdot {\mathrm e}^{-u}
$$
$$
\text{EqRelation} := x = (a x + b x {\mathrm e}^{-x}) {\mathrm e}^{-x}
$$
$$
\text{EqSolutions} := 0, -\ln \left(\frac{-a +\sqrt{a^{2}+4 b}}{2 b}\right), -\ln \left(-\frac{a +\sqrt{a^{2}+4 b}}{2 b}\right)
$$
$$
\text{xtrivial} := 0
$$
$$
\text{xnontrivial} := 0
$$
$$
\text{"Trivial Equilibrium:"}, 0
$$
$$
\text{"Non-Trivial Equilibrium (Symbolic):"}, 0
$$
$$
\mathit{p1} := a {\mathrm e}^{-u} - (a u + b v {\mathrm e}^{-v}) {\mathrm e}^{-u}
$$
$$
\mathit{p2} := (b {\mathrm e}^{-v} - b v {\mathrm e}^{-v}) {\mathrm e}^{-u}
$$
$$
\text{"----------- CASE 1 -----------"}
$$
$$
\text{a1} :=  0.4
$$
$$
\text{b1} :=  0.5
$$
$$
\text{"Condition Check: a+b <= 1, so population should die out to 0."}
$$
$$
\text{valJ11} :=  0.4
$$
$$
\text{valJ21} :=  0.5
$$
$$
\text{"Eigenvalues at 0:"}, 0.9348469228, -0.5348469228
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
\text{"----------- CASE 2 -----------"}
$$
$$
\text{a2} :=  0.7
$$
$$
\text{b2} :=  2.0
$$
$$
\text{xstar2} :=  0.5916017272
$$
$$
\text{"Equilibrium Point x*:"}, 0.5916017272
$$
$$
\text{valJ12} := -0.2041936473
$$
$$
\text{valJ22} :=  0.2501814825
$$
$$
\text{roots2} := [0.4083982732, -0.6125919205]
$$
$$
\text{"Eigenvalues:"}, [0.4083982732, -0.6125919205]
$$
$$
\text{"STABLE"}
$$
$$
X_{1}:=  0.1
$$
$$
X_{2}:=  0.1
$$

![Plot](./Images/MM_GENERAL_FULL_DOCplot2d12.png)

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S3 - ESERCISE 3

Flour Beetles model:

$$ x_{n+1} = \alpha x_{n} + \beta x_{n-2} e^{-c_{1}x_{n-2}-c_{2}x_{n}} $$

where $\alpha, \beta > 0$.

(a) Find the equilibrium points;

(b) Study the stability of the equilibrium points;

(c) Make numerical simulations.

**Hint**: Use the **Schur-Cohn Criterion (noted in LECTURE 4)**

```maple
> restart;
with(plots):

##### --- Define the Model Function ---
f := (u,w) -> alpha * u + beta * w * exp(-c1 * w - c2 * u);

##### --- Part (a): Find Equilibrium Points ---
EqRelation := x = f(x,x);
EqSolutions := solve(EqRelation, x);

##### 2. Extract solutions
xtrivial := 0;
xnontrivial := EqSolutions[1];

print("Trivial Equilibrium:", xtrivial);
print("Non-Trivial Equilibrium (Symbolic):", xnontrivial);

##### --- Part (b): Stability Analysis (Symbolic Setup) ---
Ju := diff(f(u,w), u);
Jw := diff(f(u,w), w);

##### --- Part (b) & (c): Case 1 - Stable Parameters ---
print("CASE 1: STABLE");
alpha1 := 0.5;
beta1  := 2.0;
c11    := 0.1;
c21    := 0.1;

##### 1. Calculate specific equilibrium point
xstar1 := evalf( ln(beta1 / (1 - alpha1)) / (c11 + c21) );
print("Equilibrium Point (Case 1):", xstar1);

##### 2. Calculate Jacobian values at xstar
valJ01 := evalf(eval(Ju, {u=xstar1, w=xstar1, alpha=alpha1, beta=beta1, c1=c11, c2=c21}));
valJ21 := evalf(eval(Jw, {u=xstar1, w=xstar1, alpha=alpha1, beta=beta1, c1=c11, c2=c21}));

##### 3. Solve Characteristic Equation: z^3 - J0*z^2 - J2 = 0
rootsval1 := [solve(z^3 - valJ01*z^2 - valJ21 = 0, z)];
MaxMod1 := max(seq(abs(r), r in rootsval1));
print("Max Modulus of Roots:", MaxMod1);
##### If MaxMod < 1, it is Stable

##### 4. Numerical Simulation (Case 1)
steps := 60;
X := Vector(steps);
X[1] := 0.1; X[2] := 0.1; X[3] := 0.1;

for i from 3 to steps-1 do
    X[i+1] := evalf( alpha1*X[i] + beta1*X[i-2]*exp(-c11*X[i-2] - c21*X[i]) );
end do:
Plot1 := pointplot([seq([n, X[n]], n=1..steps)], style=line, color=blue, title="Case 1: Stable");

##### --- Part (b) & (c): Case 2 - Unstable Parameters ---
print("CASE 2: UNSTABLE");
alpha2 := 0.5;
beta2  := 20.0;
c12    := 0.1;
c22    := 0.1;

##### 1. Calculate specific equilibrium point
xstar2 := evalf( ln(beta2 / (1 - alpha2)) / (c12 + c22) );
print("Equilibrium Point (Case 2):", xstar2);

##### 2. Calculate Jacobian values at xstar
valJ02 := evalf(eval(Ju, {u=xstar2, w=xstar2, alpha=alpha2, beta=beta2, c1=c12, c2=c22}));
valJ22 := evalf(eval(Jw, {u=xstar2, w=xstar2, alpha=alpha2, beta=beta2, c1=c12, c2=c22}));

##### 3. Solve Characteristic Equation
rootsval2 := [solve(z^3 - valJ02*z^2 - valJ22 = 0, z)];
MaxMod2 := max(seq(abs(r), r in rootsval2));
print("Max Modulus of Roots:", MaxMod2);
##### If MaxMod > 1, it is Unstable

##### 4. Numerical Simulation (Case 2)
X := Vector(steps);
X[1] := 0.1; X[2] := 0.1; X[3] := 0.1;

for i from 3 to steps-1 do
    X[i+1] := evalf( alpha2*X[i] + beta2*X[i-2]*exp(-c12*X[i-2] - c22*X[i]) );
end do:
Plot2 := pointplot([seq([n, X[n]], n=1..steps)], style=line, color=red, title="Case 2: Unstable");

##### --- Display Both Plots ---
display(Array([Plot1, Plot2]));
```

$$
f := (u, w) \mapsto \alpha \cdot u + \beta \cdot w \cdot {\mathrm e}^{-\mathit{c1} \cdot w - \mathit{c2} \cdot u}
$$
$$
\text{EqRelation} := x = \alpha x + \beta x {\mathrm e}^{-\mathit{c1} x - \mathit{c2} x}
$$
$$
\text{EqSolutions} := 0, -\frac{\ln (-\frac{\alpha -1}{\beta})}{\mathit{c1} + \mathit{c2}}
$$
$$
\text{xtrivial} := 0
$$
$$
\text{xnontrivial} := 0
$$
$$
\text{"Trivial Equilibrium:"}, 0
$$
$$
\text{"Non-Trivial Equilibrium (Symbolic):"}, 0
$$
$$
\text{Ju} := \alpha - \beta w \mathit{c2} {\mathrm e}^{-\mathit{c1} w - \mathit{c2} u}
$$
$$
\text{Jw} := \beta {\mathrm e}^{-\mathit{c1} w - \mathit{c2} u} - \beta w \mathit{c1} {\mathrm e}^{-\mathit{c1} w - \mathit{c2} u}
$$
$$
\text{"CASE 1: STABLE"}
$$
$$
\text{alpha1} :=  0.5
$$
$$
\text{beta1} :=  2.0
$$
$$
\text{c11} :=  0.1
$$
$$
\text{c21} :=  0.1
$$
$$
\text{xstar1} :=  6.931471805
$$
$$
\text{"Equilibrium Point (Case 1):"}, 6.931471805
$$
$$
\text{valJ01} :=  0.1534264098
$$
$$
\text{valJ21} :=  0.1534264098
$$
$$
\text{rootsval1} := [0.5916803502, -0.2191269702 + 0.4596625021 \,\mathrm{I}, -0.2191269702 - 0.4596625021 \,\mathrm{I}]
$$
$$
\text{MaxMod1} :=  0.5916803502
$$
$$
\text{"Max Modulus of Roots:"}, 0.5916803502
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
$$
\text{"CASE 2: UNSTABLE"}
$$
$$
\text{alpha2} :=  0.5
$$
$$
\text{beta2} :=  20.0
$$
$$
\text{c12} :=  0.1
$$
$$
\text{c22} :=  0.1
$$
$$
\text{xstar2} :=  18.44439727
$$
$$
\text{"Equilibrium Point (Case 2):"}, 18.44439727
$$
$$
\text{valJ02} := -0.4222198635
$$
$$
\text{valJ22} := -0.4222198635
$$
$$
\text{rootsval2} := [0.2491448889 + 0.6297677103 \,\mathrm{I}, -0.9205096412, 0.2491448889 - 0.6297677103 \,\mathrm{I}]
$$
$$
\text{MaxMod2} :=  0.9205096412
$$
$$
\text{"Max Modulus of Roots:"}, 0.9205096412
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

![Plot](./Images/MM_GENERAL_FULL_DOCplot2d14.png)

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

### S3 - EXERCISE 4

Study the stability of the equilibrium point (0,0) for the following systems. For the cases where stability occurs, verify this property numerically.

(a)
$$
\begin{cases}
x_{n+1} = x_{n} + 4 \cdot y_{n} \\
y_{n+1} = \frac{1}{4} \cdot x_{n} + y_{n}
\end{cases}
$$

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
AbsEig1 := abs(Eigs[1]);
AbsEig2 := abs(Eigs[2]);

if AbsEig1 < 1 and AbsEig2 < 1 then
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
\text{Eigs} :=  2.0, 0.0
$$
$$
\text{"The eigenvalues are:"}, 2.0, 0.0
$$
$$
\text{AbsEig1} :=  2.0
$$
$$
\text{AbsEig2} :=  0.0
$$
$$
\text{"Conclusion: The system is UNSTABLE (At least one |lambda| > 1)."}
$$
$$
N := 20
$$
$$
x := [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0, \cdots 0 .. 20 \text{ Array}]
$$
$$
y := [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0, \cdots 0 .. 20 \text{ Array}]
$$
$$
x_{0}:=  0.1
$$
$$
y_{0}:=  0.1
$$

![Plot](./Images/MM_GENERAL_FULL_DOCplot2d15.png)
![Plot](./Images/MM_GENERAL_FULL_DOCplot2d16.png)
![Plot](./Images/MM_GENERAL_FULL_DOCplot2d17.png)
![Plot](./Images/MM_GENERAL_FULL_DOCplot2d18.png)
![Plot](./Images/MM_GENERAL_FULL_DOCplot2d19.png)

_ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _ _

S3 - EXERCISE 5

Find and study the stability of the equilibrium points for the following systems.
For locally asymptotically stable equilibrium points, determine some initial conditions for which the corresponding solution converges to that equilibrium point.

(a)
$$
\begin{cases}
x_{n+1} = x_{n} + \frac{1}{6} \cdot x_{n} \cdot (1 - x_{n} - y_{n}) \\
y_{n+1} = y_{n} \cdot (1 + x_{n} - y_{n})
\end{cases}
$$

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
EqSystem := \{x = f1(x,y), y = f2(x,y)\};
Points := solve(EqSystem, \{x,y\});
print("The equilibrium points are:", Points);

##### --- 3. Stability Analysis (Linearization) ---
J := jacobian([f1(x,y), f2(x,y)], [x,y]);

##### --- Check Point 1: (0, 0) ---
print("--- Analysis of (0, 0) ---");
##### Substitute x=0, y=0 into Jacobian
J1 := subs(\{x=0, y=0\}, evalm(J));
##### Calculate Eigenvalues
Eigs1 := eigenvals(J1);
print("Eigenvalues:", Eigs1);
##### Result: 1.16 (>1) and 1. Unstable.

##### --- Check Point 2: (1, 0) ---
print("--- Analysis of (1, 0) ---");
J2 := subs(\{x=1, y=0\}, evalm(J));
Eigs2 := eigenvals(J2);
print("Eigenvalues:", Eigs2);
##### Result: 0.83 and 2. Unstable (since 2 > 1).

##### --- Check Point 3: (1/2, 1/2) ---
print("--- Analysis of (1/2, 1/2) ---");
J3 := subs(\{x=1/2, y=1/2\}, evalm(J));
Eigs3 := eigenvals(J3);
evalf(Eigs3);
##### Result: Real numbers with modulus < 1. Modulus calculation to confirm stability.
Modulus := abs(Eigs3[1]);
print("Modulus of eigenvalues:", evalf(Modulus));
if evalf(Modulus) < 1 then
    print("Conclusion: (1/2, 1/2) is LOCALLY ASYMPTOTICALLY STABLE.");
else
    print("Conclusion: Unstable.");
end if;

##### --- 4. Numerical Simulation for Stable Point ---
##### Verify convergence to (0.5, 0.5) starting from nearby conditions (e.g., 0.4, 0.4)
N := 50;
xsim := Array(0..N);
ysim := Array(0..N);

##### Initial Conditions
xsim[0] := 0.4;
ysim[0] := 0.4;

for i from 0 to N-1 do
    xsim[i+1] := f1(xsim[i], ysim[i]);
    ysim[i+1] := f2(xsim[i], ysim[i]);
end do:
PlotX := plot([seq([n, xsim[n]], n=0..N)], style=point, color=red, legend="x(n)");
PlotY := plot([seq([n, ysim[n]], n=0..N)], style=point, color=blue, legend="y(n)");
display( [PlotX, PlotY], title="Convergence to Stable Equilibrium (0.5, 0.5)", labels=["n", "Value"] );
```

$$
\text{f1} := (x, y) \mapsto x + \frac{x \cdot (1 - x - y)}{6}
$$
$$
\text{f2} := (x, y) \mapsto y \cdot (1 + x - y)
$$
$$
\text{EqSystem} := \{x = x + \frac{x (1 - x - y)}{6}, y = y (1 + x - y)\}
$$
$$
\text{Points} := \{x = 0, y = 0\}, \{x = 1, y = 0\}, \{x = \frac{1}{2}, y = \frac{1}{2}\}
$$
$$
\text{"The equilibrium points are:"}, \{x = 0, y = 0\}, \{x = 1, y = 0\}, \{x = \frac{1}{2}, y = \frac{1}{2}\}
$$
$$
\text{"--- Analysis of (0, 0) ---"}
$$
$$
\text{Eigs1} := \frac{7}{6}, 1
$$
$$
\text{"Eigenvalues:"}, \frac{7}{6}, 1
$$
$$
\text{"--- Analysis of (1, 0) ---"}
$$
$$
\text{Eigs2} := \frac{5}{6}, 2
$$
$$
\text{"Eigenvalues:"}, \frac{5}{6}, 2
$$
$$
\text{"--- Analysis of (1/2, 1/2) ---"}
$$
$$
\text{Eigs3} := \frac{3}{4}, \frac{2}{3}
$$
$$
0.7500000000, 0.6666666667
$$
$$
\text{Modulus} := \frac{3}{4}
$$
$$
\text{"Modulus of eigenvalues:"}, 0.7500000000
$$
$$
\text{"Conclusion: (1/2, 1/2) is LOCALLY ASYMPTOTICALLY STABLE."}
$$
$$
N := 50
$$
$$
\text{xsim} := [0,0,0,0,0,0,0,0,0,0, \cdots 0 .. 50 \text{ Array}]
$$
$$
\text{ysim} := [0,0,0,0,0,0,0,0,0,0, \cdots 0 .. 50 \text{ Array}]
$$
$$
\text{xsim}_{0}:=  0.4
$$
$$
\text{ysim}_{0}:=  0.4
$$

![Plot](MM_GENERAL_FULL_DOCplot2d20.png)
![Plot](MM_GENERAL_FULL_DOCplot2d21.png)
![Plot](MM_GENERAL_FULL_DOCplot2d22.png)
