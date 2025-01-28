# iso-BMI



## Calculation of iso-BMI from BMI LMS coefficients corresponding to the international (LOTF) cut-offs

This code allows to calculate BMI z-scores using LMS reference coefficients in R. 

Reference:
Cole TJ, Green PJ (1992). Smoothing reference centile curves: the LMS method and penalized likelihood. Statistics in Medicine, 11:1305–1319.


## BMI Calculation Formulas

### Formula 1: \( z_{alpha} \)
The formula for \( z_{alpha} \) is:
Z-scores are caculated using following formula: 


<a href="https://www.codecogs.com/eqnedit.php?latex=Z_{ind}&space;=&space;\frac{\left&space;[&space;y/M(t)&space;\right&space;]^{L(t)}&space;-&space;1}{S(t)L(t)}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?Z_{ind}&space;=&space;\frac{\left&space;[&space;y/M(t)&space;\right&space;]^{L(t)}&space;-&space;1}{S(t)L(t)}" title="Z_{ind} = \frac{\left [ y/M(t) \right ]^{L(t)} - 1}{S(t)L(t)}" /></a>

### Formula 2: \( z_{bmi} \) Based on Sex
The formula for \( z_{bmi} \) depends on the value of `Sex`:

- If `Sex == 1` (Boys)
  \
<a href="https://latex.codecogs.com/svg.image?Z_{BMI}=20.759\times\left(1&plus;\left(-1.487\right)\times&space;Z_{BMI}\right)^{\left(1/-1.487\right)}"><img src="https://latex.codecogs.com/svg.image?Z_{BMI}=20.759\times\left(1&plus;\left(-1.487\right)\times&space;Z_{BMI}\right)^{\left(1/-1.487\right)}" /></a>


 
<a href="https://www.codecogs.com/eqnedit.php?latex=Z_{BMI}&space;=&space;\frac{\left&space;[&space;y/M(t)&space;\right&space;]^{L(t)}&space;-&space;1}{S(t)L(t)}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?Z_{BMI}&space;=&space;\frac{\left&space;[&space;y/M(t)&space;\right&space;]^{L(t)}&space;-&space;1}{S(t)L(t)}" title="Z_{ind} = \frac{\left [ y/M(t) \right ]^{L(t)} - 1}{S(t)L(t)}" /></a>




  \

- If `Sex == 0` (Girls):
  \[
  z_{bmi} = 
<a href="https://latex.codecogs.com/svg.image?20.759\times\left(1&plus;\left(-1.487\right)\times&space;Z_{BMI}\right)^{\left(1/-1.487\right)}" target="_blank"><img src="https://latex.codecogs.com/svg.image?20.759\times\left(1&plus;\left(-1.487\right)\times&space;Z_{BMI}\right)^{\left(1/-1.487\right)}" /></a>

  \]

- If `age >= 18`:
  \[
  z_{bmi} = YH_{BMI}
  \]
