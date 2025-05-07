# **Kruskal-Wallis Test Automation in Python**

This project focuses on building a **Python-based automation tool** for performing the **Kruskal-Wallis H-test** across multiple parameters (variables) and groups within a dataset. Designed for researchers, data analysts, and statisticians, the tool enables efficient non-parametric comparison of **more than two independent groups** without assuming normal distribution.

The script is scalable and customizable, making it especially useful for exploratory data analysis in fields such as **agriculture, healthcare, social sciences, and biology**, where datasets often violate normality assumptions.

---

### 🎯 **Objectives**

* ✅ Automate the **Kruskal-Wallis test** across **multiple parameters** in a dataset.
* ✅ Perform **group-wise comparisons** based on a categorical grouping variable.
* ✅ Handle **missing data**, rank ties, and group filtering efficiently.
* ✅ Return results in a clean, exportable format (e.g., CSV or DataFrame).

---

### 🛠️ **Key Features**

* 💻 **Python implementation** using `scipy`, `pandas`, and `seaborn/matplotlib`.
* 🔁 Loop-based structure to test multiple variables at once.
* 📈 Optional **boxplot visualizations** for visual comparison.
* 📄 Export of summary results including test statistics, p-values, and significance flags.

---

### Methodology

1. **Input Handling**

   * The function accepts a dataset, a list of numeric variables, and two categorical grouping factors.

2. **Iteration Over Numeric Variables**

   * For each numeric variable in the dataset:

3. **Group Comparison for First Factor**

   * Data is grouped based on unique levels of the first categorical factor.
   * A Kruskal-Wallis H-test is applied to evaluate whether there are statistically significant differences in the variable across these groups.
   * Test results (H-statistic, p-value, and interpretation) are recorded.

4. **Group Comparison for Second Factor**

   * The same procedure is repeated using the second categorical factor.

5. **Interaction Effect Evaluation**

   * A combined interaction term is created by pairing levels of the two factors.
   * The Kruskal-Wallis test is then applied to these interaction groups to assess combined effects.
   * Results are recorded similarly.

6. **Result Compilation**

   * All test outcomes for each variable and grouping source are compiled into a structured table containing:

     * Variable name
     * Source of variation (Factor 1, Factor 2, or Interaction)
     * H-statistic
     * p-value
     * Significance interpretation

7. **Output**

   * A final summary table is generated as a `DataFrame`, suitable for review, reporting, or visualization.
  
---

###  **Usage**

#### 1. **Single-Factor Kruskal-Wallis Test**

```python
from scipy.stats import kruskal
import pandas as pd

def Kruskal_Wallis_test(data, numerical_columns, group_column):
    results = []
    for response_column in numerical_columns:
        groups = [data[response_column][data[group_column] == group] for group in data[group_column].unique()]
        stat, p_value = kruskal(*groups)
        interpretation = "Significant difference" if p_value < 0.05 else "No significant difference"
        results.append({
            "Variable": response_column,
            "Source of Variation": group_column,
            "H-Statistic": stat,
            "p-Value": p_value,
            "Interpretation": interpretation
        })
    return pd.DataFrame(results)

# Example usage
group_column = "Fertilizer"
numerical_columns = df.select_dtypes(include=["float64", "int64"]).columns
kruskal_results_df = Kruskal_Wallis_test(df, numerical_columns, group_column)
print(kruskal_results_df)
```

#### 2. **Two-Factor and Interaction Kruskal-Wallis Test**

```python
def kruskal_walis_test(data, Numerical_columns, factor1, factor2):
    results = []
    for response_column in Numerical_columns:
        # Factor 1
        groups_factor1 = [data[response_column][data[factor1] == group] for group in data[factor1].unique()]
        stat_factor1, p_value_factor1 = kruskal(*groups_factor1)
        interpretation_factor1 = "Significant difference" if p_value_factor1 < 0.05 else "No significant difference"

        results.append({
            "Variable": response_column,
            "Source": factor1,
            "H-Statistic": stat_factor1,
            "p-Value": p_value_factor1,
            "Interpretation": interpretation_factor1
        })

        # Factor 2
        groups_factor2 = [data[response_column][data[factor2] == group] for group in data[factor2].unique()]
        stat_factor2, p_value_factor2 = kruskal(*groups_factor2)
        interpretation_factor2 = "Significant difference" if p_value_factor2 < 0.05 else "No significant difference"

        results.append({
            "Variable": response_column,
            "Source": factor2,
            "H-Statistic": stat_factor2,
            "p-Value": p_value_factor2,
            "Interpretation": interpretation_factor2
        })

        # Interaction
        data['Interaction'] = data[factor1].astype(str) + " x " + data[factor2].astype(str)
        groups_interaction = [data[response_column][data['Interaction'] == group] for group in data['Interaction'].unique()]
        stat_interaction, p_value_interaction = kruskal(*groups_interaction)
        interpretation_interaction = "Significant difference" if p_value_interaction < 0.05 else "No significant difference"

        results.append({
            "Variable": response_column,
            "Source": f"{factor1} x {factor2}",
            "H-Statistic": stat_interaction,
            "p-Value": p_value_interaction,
            "Interpretation": interpretation_interaction
        })

    return pd.DataFrame(results)

# Example usage
factor1 = "Fertilizer"
factor2 = "Light Exposure"
Numerical_columns = df.select_dtypes(include=["float64", "int64"]).columns
nonparametric_results_df = kruskal_walis_test(df, Numerical_columns, factor1, factor2)
pd.set_option("display.float_format", "{:.4f}".format)
print(nonparametric_results_df)
```

---

### ✅ **Sample results**

The script will return a structured DataFrame like the example below, showing Kruskal-Wallis results across all specified variables and factors:

**Single-Factor Kruskal-Wallis Test Results**

| Variable                         | Source of Variation   |   H-Statistic |    p-Value | Interpretation            |
|:---------------------------------|:----------------------|--------------:|-----------:|:--------------------------|
| Plant Height (cm)                | Fertilizer            |      10.1237  | 0.00633388 | Significant difference    |
| Leaf Area (cm²)                  | Fertilizer            |       4.22287 | 0.121064   | No significant difference |
| Chlorophyll Content (SPAD units) | Fertilizer            |       3.64577 | 0.161559   | No significant difference |
| ..............................   | ..........            |      .......  | .........  | ..........                |

**Two-Factor and Interaction Kruskal-Wallis Test Results**

| Variable                         | Source                      |   H-Statistic |     p-Value | Interpretation            |
|:---------------------------------|:----------------------------|--------------:|------------:|:--------------------------|
| Plant Height (cm)                | Fertilizer                  |      10.1237  | 0.00633388  | Significant difference    |
| Plant Height (cm)                | Light Exposure              |      85.277   | 3.03616e-19 | Significant difference    |
| Plant Height (cm)                | Fertilizer x Light Exposure |     102.149   | 1.55174e-18 | Significant difference    |
| Leaf Area (cm²)                  | Fertilizer                  |       4.22287 | 0.121064    | No significant difference |
| Leaf Area (cm²)                  | Light Exposure              |      88       | 7.78096e-20 | Significant difference    |
| Leaf Area (cm²)                  | Fertilizer x Light Exposure |     102.478   | 1.32882e-18 | Significant difference    |
| Chlorophyll Content (SPAD units) | Fertilizer                  |       3.64577 | 0.161559    | No significant difference |
| ..............................   | ..........                  |      .......  | ..........  | ..........                |

This allows for **automated evaluation** of non-parametric group differences and their interactions across your dataset.


---

This analysis was performed by **Jabulente**, a passionate and dedicated data scientist with a strong commitment to using data to drive meaningful insights and solutions. For inquiries, collaborations, or further discussions, please feel free to reach out via.  

---

<div align="center">  
    
[![GitHub](https://img.shields.io/badge/GitHub-Jabulente-black?logo=github)](https://github.com/Jabulente)  [![LinkedIn](https://img.shields.io/badge/LinkedIn-Jabulente-blue?logo=linkedin)](https://linkedin.com/in/jabulente-208019349)  [![X (Twitter)](https://img.shields.io/badge/X-@Jabulente-black?logo=x)](https://x.com/Jabulente)  [![Instagram](https://img.shields.io/badge/Instagram-@Jabulente-purple?logo=instagram)](https://instagram.com/Jabulente)  [![Threads](https://img.shields.io/badge/Threads-@Jabulente-black?logo=threads)](https://threads.net/@Jabulente)  [![TikTok](https://img.shields.io/badge/TikTok-@Jabulente-teal?logo=tiktok)](https://tiktok.com/@Jabulente)  [![Email](https://img.shields.io/badge/Email-jabulente@hotmail.com-red?logo=gmail)](mailto:Jabulente@hotmail.com)  

</div>
