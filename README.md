# iso-BMI



## Calculation of iso-BMI from BMI LMS coefficients corresponding to the international (LOTF) cut-offs

This code allows to calculate BMI z-scores using LMS reference coefficients in R. 

Reference:
Cole TJ, Green PJ (1992). Smoothing reference centile curves: the LMS method and penalized likelihood. Statistics in Medicine, 11:1305–1319.


## BMI Calculation Formulas

### Formula 1: \( z_{alpha} \)
The formula for \( z_{alpha} \) is:
Z-scores are caculated using following formula: 


<a href="https://latex.codecogs.com/svg.image?Z_{ind}=\frac{\left(BMI/M\right)^{l}-L}{L\times&space;S}"><img src="https://latex.codecogs.com/svg.image?Z_{ind}=\frac{\left(BMI/M\right)^{l}-L}{L\times&space;S}" /></a>

### Formula 2: \( z_{bmi} \) Based on Sex
The formula for \( z_{bmi} \) depends on the value of `Sex`:

- If `Sex == 1` (Boys)
  
  \
<a href="https://latex.codecogs.com/svg.image?Z_{BMI}=20.759\times\left(1&plus;\left(-1.487\right)\times&space;Z_{BMI}\right)^{\left(1/-1.487\right)}"><img src="https://latex.codecogs.com/svg.image?Z_{BMI}=20.759\times\left(1&plus;\left(-1.487\right)\times&space;Z_{BMI}\right)^{\left(1/-1.487\right)}" /></a>

- If `Sex == 0` (Girls):
  \
<a href="https://latex.codecogs.com/svg.image?Z_{BMI}=20.792\times\left(1&plus;\left(-1.423\right)\times&space;Z_{BMI}\right)^{\left(1/-1.423\right)}"><img src="https://latex.codecogs.com/svg.image?Z_{BMI}=20.792\times\left(1&plus;\left(-1.423\right)\times&space;Z_{BMI}\right)^{\left(1/-1.423\right)}" /></a>


- If `age >= 18`:
  \
<a href="https://latex.codecogs.com/svg.image?Z_{BMI}=BMI&space;"><img src="https://latex.codecogs.com/svg.image?Z_{BMI}=BMI&space;" /></a>

  \
