# iso-BMI



## Calculation of iso-BMI from BMI LMS coefficients corresponding to the international (International Obesity Task Force; IOTF) cut-offs

The iso-BMI is a method used to adjust BMI values for age and sex, allowing for consistent interpretation across childhood, adolescence, and adulthood. Since BMI distributions vary with age and sex during growth, the iso-BMI standardizes these values to adult equivalents, enabling the use of the same BMI cut-offs (e.g., overweight, obesity) for all ages.
- The method utilizes age- and sex-specific BMI distributions from six countries, incorporating parameters such as average age, L (skewness, λ), M (median µ), and S (coefficient of variation σ).
-  The LMS (Lambda-Mu-Sigma) approach generates smoothed BMI growth curves for each sex, averaged across countries.
-   Using coefficients and formulas from Cole and Lobstein, the BMI centile for each participant is estimated.
-   The calculated centile is then mapped to the corresponding BMI value at age 18 (adulthood), providing the iso-BMI.
-   "Example" , a 12.5-year-old girl with a BMI of 22.1 and a 16-year-old boy with a BMI of 23.9 would both be defined as overweight with an iso-BMI of 25. 


### Formula 1: 
Z-scores are caculated using following formula: 


<a href="https://latex.codecogs.com/svg.image?Z_{ind}=\frac{\left(BMI/M\right)^{L}-1}{L\times&space;S}"><img src="https://latex.codecogs.com/svg.image?Z_{ind}=\frac{\left(BMI/M\right)^{L}-1}{L\times&space;S}" /></a>


### Formula 2: 
The formula for <a href="https://latex.codecogs.com/svg.image?Z_{BMI}"><img src="https://latex.codecogs.com/svg.image?Z_{BMI}" /></a> depends on the value of `Sex` and `age < 18`:

- If `Sex == 1` (Boys)
  \
<a href="https://latex.codecogs.com/svg.image?Z_{BMI}=20.759\times\left(1&plus;\left(-1.487\right)\times&space;Z_{ind}\right)^{\left(1/-1.487\right)}"><img src="https://latex.codecogs.com/svg.image?Z_{BMI}=20.759\times\left(1&plus;\left(-1.487\right)\times&space;Z_{ind}\right)^{\left(1/-1.487\right)}" /></a>

- If `Sex == 0` (Girls):
  \
<a href="https://latex.codecogs.com/svg.image?Z_{BMI}=20.792\times\left(1&plus;\left(-1.423\right)\times&space;Z_{ind}\right)^{\left(1/-1.423\right)}"><img src="https://latex.codecogs.com/svg.image?Z_{BMI}=20.792\times\left(1&plus;\left(-1.423\right)\times&space;Z_{ind}\right)^{\left(1/-1.423\right)}" /></a>


- If `age >= 18`:
  \
<a href="https://latex.codecogs.com/svg.image?Z_{BMI}=BMI&space;"><img src="https://latex.codecogs.com/svg.image?Z_{BMI}=BMI&space;" /></a>


## Reference

Cole TJ, Lobstein T. Extended international (IOTF) body mass index cut‐offs for thinness, overweight and obesity. Pediatric obesity. 2012 Aug;7(4):284-94.
