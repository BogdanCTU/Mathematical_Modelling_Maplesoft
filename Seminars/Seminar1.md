\## SEMINAR 1: Modelling Change with Difference Equations

\_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_



\### EXERCIE 1



Write the first five terms of the following sequences:



(a) $a\_{n+1}$ = 3 \* $a\_{n}$    , $a\_{0}$ = 1



```maple

> a\[0] := 1000;

for i from 0 to 5 do

&nbsp;   a\[i + 1] := 1.01\*a\[i];

end do;

a\[5];

seq(a\[i], i = 0 .. 5);

plot(\[\[m, a\[m]] $ (m = 1 .. 5)], style = point, symbol = circle);

```

$$

a\_{0}:= 1000

$$

$$

a\_{1}:=  1010.0

$$

$$

a\_{2}:=  1020.1000

$$

$$

a\_{3}:=  1030.301000

$$

$$

a\_{4}:=  1040.604010

$$

$$

a\_{5}:=  1051.010050

$$

$$

a\_{6}:=  1061.520150

$$

$$

1051.010050

$$

$$

1000, 1010.0, 1020.1000, 1030.301000, 1040.604010, 1051.010050

$$

!\[Plot](MM\_GENERAL\_FULL\_DOCplot2d1.png)



\_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_



\### EXERCISE 2



Write the first 5 terms of the sequence satisfying the following difference equations and draw the corresponding graph of the generated dynamical system:



$\\Delta a\_{n} = \\frac{1}{3}a\_{n}, \\quad a\_{0}=1$



```maple

> restart;



\##### --- Define the Data ---

a\[0] := 1;

for n from 0 to 5 do

&nbsp;   a\[n + 1] := 3\*a\[n];

end do;

seq(a\[i], i = 0 .. 5);



\##### --- Plot the Data ---

plot(\[\[h, a\[h]] $ (h = 0 .. 5)], style = point);

```

$$

a\_{0}:= 1

$$

$$

a\_{1}:= 3

$$

$$

a\_{2}:= 9

$$

$$

a\_{3}:= 27

$$

$$

a\_{4}:= 81

$$

$$

a\_{5}:= 243

$$

$$

a\_{6}:= 729

$$

$$

1,3,9,27,81,243

$$

!\[Plot](MM\_GENERAL\_FULL\_DOCplot2d2.png)



\_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_



\### EXERCISE 3



(Decay of Digoxin in the Bloodstream) Digoxin is used in treatment of heart disease.\\



Doctors must prescribe an amount of medicine that keeps the concetration of digoxin in the bloodstream above an e§ective level without exceeding the safe level.

For an initial dosage of 0:5 mg in the bloodstream , table shows the amount of digoxin an remaining in the bloodstream of a particular pacient after n days.



n 0 1 2 3 4 5 6 7 8\\



an 0.5 0.345 0.238 0.164 0.113 0.078 0.054 0.037 0.026



(a) Get $Δa\_{n}$ and plot $Δa\_{n}$ versus $a\_{n}$;



(b) Find a proportional constant for $Δa\_{n}$ and $a\_{n}$



```maple

> restart;



\##### --- Define the Data ---

days := \[0,1,2,3,4,5,6,7,8];

an := \[0.5, 0.345, 0.238, 0.164, 0.113, 0.078, 0.054, 0.037, 0.026];



\##### --- Part (a): Calculate Delta a\_\\{n\\} and Plot ---

for i from 1 to 8 do

&nbsp;   delta\_an\[i] := an\[i+1] - an\[i];

end do:



\##### Plot Delta a\_\\{n\\} (y-axis) versus a\_\\{n\\} (x-axis)

plot(

&nbsp;   \[seq(\[an\[i], delta\_an\[i]], i=1..8)],

&nbsp;   style=point,

&nbsp;   symbol=circle,

&nbsp;   symbolsize=15,

&nbsp;   color=blue,

&nbsp;   labels=\["Amount (a\\{n\\})", "Change (Delta a\\{n\\})"],

&nbsp;   title="Change in Digoxin vs Amount in Bloodstream",

&nbsp;   labeldirections=\[HORIZONTAL, VERTICAL]

);



\##### --- Part (b): Find the Proportional Constant ---

\##### The relationship is Delta a\_\\{n\\} = k \* a\_\\{n\\}, therefore, k = Delta a\_\\{n\\} / a\_\\{n\\}

\##### Calculate this ratio for every point to check consistency



ratios := \[seq(delta\_an\[i] / an\[i], i=1..8)];



\##### Calculate the average of these ratios to find the constant 'k'

\##### 'add' sums the list, 'nops' counts the number of items

k\_constant := add(x, x in ratios) / nops(ratios);



print("The proportional constant k is approximately:", k\_constant);

```

$$

\\mathit{days} := \\left\[0,1,2,3,4,5,6,7,8\\right]

$$

$$

\\mathit{an} := \\left\[ 0.5, 0.345, 0.238, 0.164, 0.113, 0.078, 0.054, 0.037, 0.026\\right]

$$



!\[Plot](MM\_GENERAL\_FULL\_DOCplot2d3.png)



$$

\\mathit{ratios} := \\left\[- 0.3100000000,- 0.3101449275,- 0.3109243697,- 0.3109756098,- 0.3097345133,- 0.3076923077, - 0.3148148148,- 0.2972972973\\right]

$$

$$

\\textit{k} := - 0.3089479800

$$

$$

\\text{``The proportional constant k is approximately:''},- 0.3089479800

$$



\_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_ \_



\### S1 - EXERCISE 4



The following data were obtained for the growth of a sheep population introduced into new enviroment on the island of Tasmania:

Year 1814 1824 1834 1844 1854 1864\\



Population 125 275 830 1200 1750 1650



(a) Plot the data. Is there a trend?



(b) Formulate discrete dynamical system that resonable approximate the change observed. Compare real data with estimated data obtained from the model.



```maple

> restart;

with(plots):

with(Statistics):



\##### --- Define the Data ---

Years := Vector(\[1814, 1824, 1834, 1844, 1854, 1864]);

Population := Vector(\[125, 275, 830, 1200, 1750, 1650]);



\##### --- Plot the Data ---

data\_plot := pointplot(Years, Population,

&nbsp;   style=point,

&nbsp;   symbol=solidcircle,

&nbsp;   symbolsize=15,

&nbsp;   color=blue,

&nbsp;   labels=\["Year", "Population"],

&nbsp;   title="Sheep Population in Tasmania (1814-1864)",

&nbsp;   gridlines=true

);



display(data\_plot);

```

!\[Plot](MM\_GENERAL\_FULL\_DOCplot2d4.png)

!\[Plot](MM\_GENERAL\_FULL\_DOCplot2d5.png)

