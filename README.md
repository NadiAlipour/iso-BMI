# iso-BMI



## Calculation of iso-BMI from BMI LMS coefficients corresponding to the international (LOTF) cut-offs

This code allows to calculate BMI z-scores using LMS reference coefficients in R. 
The iso-BMI is a method used to adjust BMI values for age and sex, allowing for consistent interpretation across childhood, adolescence, and adulthood. Since BMI distributions vary with age and sex during growth, the iso-BMI standardizes these values to adult equivalents, enabling the use of the same BMI cut-offs (e.g., overweight, obesity) for all ages.
- The method utilizes age- and sex-specific BMI distributions from six countries, incorporating parameters such as average age, skewness (L, λ), median BMI (M, µ), and coefficient of variation (S, σ).
-  The LMS (Lambda-Mu-Sigma) approach generates smoothed BMI growth curves for each sex, averaged across countries.
-   Using coefficients and formulas from Cole and Lobstein, the BMI centile for each participant is estimated.
-   The calculated centile is then mapped to the corresponding BMI value at age 18 (adulthood), providing the iso-BMI.

### Formula 1: 
Z-scores are caculated using following formula: 


<a href="https://latex.codecogs.com/svg.image?Z_{ind}=\frac{\left(BMI/M\right)^{L}-1}{L\times&space;S}"><img src="https://latex.codecogs.com/svg.image?Z_{ind}=\frac{\left(BMI/M\right)^{L}-1}{L\times&space;S}" /></a>



```{R}
processed_data <- processed_data %>%
  mutate(
    z_alpha = ifelse(Age < 18,
                     (((BMI / M)^L) - 1) / (L * S),
                     NA_real_)
  )
```

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


```{R}
# Calculate z_bmi using Cole & Lobstein formula
processed_data <- processed_data %>%
  mutate(
    z_bmi = case_when(
      Age >= 18 ~ BMI,  # Use original BMI for adults
      Sex == "Male" ~ 20.759 * (1 + (-1.487) * 0.12395 * z_alpha)^(1 / (-1.487)),
      Sex == "Female" ~ 20.792 * (1 + (-1.423) * 0.13033 * z_alpha)^(1 / (-1.423)),
      TRUE ~ NA_real_
    ),
    z_bmi = round(z_bmi, 4)  # Round to 4 decimal places
  )

```



```{R}

# BMI Cut-off Assignment 
adult_ref <- c(16, 17, 18.5, 25, 30, 35) # Adult BMI reference values

# Function to assign iso-BMI based on cut-offs
assign_iso_bmi <- function(bmi, cutoffs) {
  tryCatch({
    if (any(is.na(cutoffs)) || is.na(bmi)) return(NA_real_)
    
    # Check which range the BMI falls into
    if (bmi < cutoffs[1]) return(adult_ref[1])  # Below BMI_16
    if (bmi >= cutoffs[1] & bmi < cutoffs[2]) return(adult_ref[1])  # Between BMI_16 and BMI_17
    if (bmi >= cutoffs[2] & bmi < cutoffs[3]) return(adult_ref[2])  # Between BMI_17 and BMI_18.5
    if (bmi >= cutoffs[3] & bmi < cutoffs[4]) return(adult_ref[3])  # Between BMI_18.5 and BMI_25
    if (bmi >= cutoffs[4] & bmi < cutoffs[5]) return(adult_ref[4])  # Between BMI_25 and BMI_30
    if (bmi >= cutoffs[5] & bmi < cutoffs[6]) return(adult_ref[5])  # Between BMI_30 and BMI_35
    if (bmi >= cutoffs[6]) return(adult_ref[6])  # Above BMI_35
    
    return(NA_real_)  # Default case
  }, error = function(e) NA_real_)
}

# Apply the assignment function
final_data <- processed_data %>%
  mutate(
    # Create a list of cut-offs for each row
    cutoffs = pmap(select(., BMI_16, BMI_17, BMI_18.5, BMI_25, BMI_30, BMI_35), c),
    
    # Assign iso-BMI based on cut-offs
    iso_bmi = ifelse(Age < 18,
                     pmap_dbl(list(z_bmi, cutoffs), ~ assign_iso_bmi(..1, ..2)),
                     z_bmi),  # Use z_bmi for adults (Age >= 18)
    
    # Round to whole numbers
    iso_bmi = round(iso_bmi, 2)
  ) %>%
  select(-cutoffs)  # Remove temporary cutoffs column



# Save results
write.csv(final_data, "final_iso_bmi_results2.csv", row.names = FALSE)

# Show sample output
head(final_data)

```

| Age (years) | Boys L | Boys M | Boys S | Girls L | Girls M | Girls S |
|-------------|--------|--------|--------|---------|---------|---------|
| 2           | -0.624 | 16.482 | 0.07950| -0.816  | 16.206  | 0.08447 |
| 2.5         | -0.758 | 16.237 | 0.07911| -0.928  | 15.983  | 0.08417 |
| 3           | -0.888 | 16.019 | 0.07892| -1.029  | 15.793  | 0.08424 |
| 3.5         | -1.012 | 15.831 | 0.07905| -1.121  | 15.628  | 0.08476 |
| 4           | -1.130 | 15.676 | 0.07968| -1.207  | 15.481  | 0.08580 |
| 4.5         | -1.240 | 15.550 | 0.08107| -1.286  | 15.356  | 0.08755 |
| 5           | -1.342 | 15.452 | 0.08353| -1.356  | 15.255  | 0.09019 |

## Reference

Cole TJ, Lobstein T. Extended international (IOTF) body mass index cut‐offs for thinness, overweight and obesity. Pediatric obesity. 2012 Aug;7(4):284-94.


