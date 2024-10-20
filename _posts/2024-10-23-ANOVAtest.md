---
title: "Analysis of Variance (ANOVA)"
date: "2024-10-20"
tags: ["statistics", "ANOVA", "RStudio", "parametric test", "data science"]
---

ANOVA, or Analysis of Variance, is a statistical test used to compare the means of three or more groups to determine if at least one group mean is significantly different from the others. It extends the t-test, which is used to compare only two groups, by allowing comparisons across multiple groups at once.


# Theory Behind ANOVA:

ANOVA operates under the assumption that the populations being compared are normally distributed and have equal variances. The test examines the variability between group means and within each group, then calculates an F-ratio:
  - **F-ratio** = (variance between groups) / (variance within groups)
    - If the F-ratio is significantly large, it suggests that the group means differ more than would be expected by chance.

## Hypothesis in ANOVA

 - **Null Hypothesis** : All group means are equal.
 - **Alternative Hypothesis** : At least one group mean is different.

## Types of ANOVA
 1. **One-way ANOVA**: Tests for differences between the means of three or more independent groups based on one factor.
 2. **Two-way ANOVA**: Tests for interaction effects between two factors on the dependent variable.
 3. **Repeated Measures ANOVA**: Tests within-subjects effects, where the same subjects are measured multiple times over different conditions or time points.

# Practical Example: One-Way ANOVA in RStudio

## Case: Testing the Effect of Different Diets on Weight Loss
Let’s say we have data on the weight loss of individuals across three different diet groups: A, B, and C. We want to determine whether the average weight loss differs between these diet plans.

#### Code:
```r
# Example data
weight_loss <- c(5, 6, 7, 8, 9, 5, 6, 8, 10, 12, 3, 4, 5, 3, 6)
diet_group <- factor(c(rep("A", 5), rep("B", 5), rep("C", 5)))

# Combine data into a data frame
data <- data.frame(weight_loss, diet_group)

# Perform One-way ANOVA
anova_result <- aov(weight_loss ~ diet_group, data = data)

# View the ANOVA table
summary(anova_result)
```
- If the **p-value** is less than 0.05, we reject the null hypothesis, indicating that at least one diet plan leads to a significantly different weight loss.
- If the **p-value** is greater than 0.05, we fail to reject the null hypothesis, meaning that there is no significant difference between the diet plans.

# Two-Way ANOVA in RStudio

## Case: Examining the Effect of Diet and Exercise on Weight Loss
 Now, we want to see if there is an interaction effect between diet and exercise levels (low, medium, high) on weight loss.
#### Code:
```r

# Example data
exercise_level <- factor(c(rep("low", 5), rep("medium", 5), rep("high", 5)))

# Combine into a data frame
data <- data.frame(weight_loss, diet_group, exercise_level)

# Perform Two-way ANOVA
anova_result <- aov(weight_loss ~ diet_group * exercise_level, data = data)

# View the ANOVA table
summary(anova_result)

```
- The **diet_group** and **exercise_level** main effects tell us if either factor independently affects weight loss.
- The interaction term *(diet_group:exercise_level)* tells us whether the effect of the diet differs depending on the level of exercise.


# Example Where ANOVA Doesn't Work

ANOVA requires the data to meet specific assumptions:
1. The data should be normally distributed.
2. Homogeneity of variances (equal variances among groups).



## Case: Violating the Homogeneity of Variances Assumption
Let’s simulate a case where the variances between groups are significantly different:

#### Code:
```r
# Example data with unequal variances
group1 <- rnorm(30, mean=50, sd=10) # Group 1, high variance
group2 <- rnorm(30, mean=50, sd=20) # Group 2, even higher variance
group3 <- rnorm(30, mean=50, sd=5)  # Group 3, lower variance

# Combine data into a data frame
data <- data.frame(
  values = c(group1, group2, group3),
  group = factor(rep(c("Group1", "Group2", "Group3"), each = 30))
)

# Perform One-way ANOVA
anova_result <- aov(values ~ group, data = data)

# View the ANOVA table
summary(anova_result)

# Test for homogeneity of variances using Bartlett's test
bartlett.test(values ~ group, data = data)

```

- In this case, the **Bartlett’s test** will likely indicate that the variances are not equal (a significant p-value).
- ANOVA may give misleading results because the assumption of homogeneity of variances is violated. In such cases, a **Welch’s ANOVA** or a **Kruskal-Wallis test** (non-parametric alternative) would be more appropriate.

# Conclusion
ANOVA is a powerful tool for comparing the means of multiple groups, but its validity depends on key assumptions such as normality and homogeneity of variances. By understanding these limitations and using alternative methods like Welch’s ANOVA when assumptions are violated, we can draw more reliable conclusions from our data.

# References:
- Fisher, R. A. (1925). Statistical Methods for Research Workers.
- Montgomery, D. C. (2017). Design and Analysis of Experiments.
 
