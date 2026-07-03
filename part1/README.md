Part 1 - Data Acquisition, Cleaning, and Exploratory Analysis

Dataset Details: 

Dataset Name: Health Insurance Cost & Risk Dataset

Source: Kaggle

Dataset Link: https://www.kaggle.com/datasets/mjawad17/health-insurance-cost-and-risk-dataset

Description: The Health Insurance Cost & Risk Dataset is used to predict individual medical insurance charges using demographic, lifestyle, health, and socioeconomic factors. It contains 1,338 records and 12 attributes, including age, sex, BMI, number of children, smoking status, region, blood pressure, exercise frequency, pre-existing medical conditions, occupation risk, annual income, and the target variable charges.This dataset was selected because it contains a diverse combination of numerical, categorical, and boolean features, making it well suited for given tasks.

1. The dataset contains 1,338 rows and 12 columns, including 6 numerical features (age, bmi, children, charges, blood_pressure, and annual_income), 5 categorical features (sex, smoker, region, exercise_frequency, and occupation_risk), and 1 boolean feature (pre_existing_condition).

2. Only the age, bmi, and children columns contains 1 missing value in each. Hence no column exceeded the 20% threshold. The missing column value is filled using median. The median was used for imputation because it is less affected by outliers and skewed distributions than the mean. 

3. No duplicates in the dataset. No dropping of row applied.  

4. The age and children column were converted from a floating-point type to an integer. String columns sex, smoker, region, exercise frequency, and occupation risk were converted to the category data type. These conversions optimized memory usage reducing it from 493455 bytes to 73863 bytes.

5. The charges column had the highest absolute skewness of about 1.5159 indicating a positively or right skewed distribution. This means that most beneficiaries have lower insurance charges, while a few have very high charges, creating a long right tail. In contrast, a negatively skewed distribution has a long left tail due to a few unusually low values. Because extreme values pull the mean toward the tail.Using the mean for imputation would be influenced by extreme high values resulting in higher imputed values than typical observations. Therefore the median was preferred as it is more robust to outliers and better represents the central tendency of skewed data.

6. The IQR method identified 139 outliers in the charges column and 9 outliers in the bmi column. These outliers were retained because they likely represent genuine observations rather than data entry errors. They will not be removed in Part 2 as they may contain valuable information and the selected machine learning models are generally robust to such extreme values.

7. The line plot shows a steady increase in annual income after sorting the records.
![alt text](<visual output/line_plot_annual_income.png>)

The bar chart shows that smokers have significantly higher average insurance charges than non-smokers.
![alt text](<visual output/barchart.png>)

The histogram of insurance charges shows a positively skewed or right skewed distribution. Most beneficiaries have relatively low insurance charges - while a few incur very high medical expenses and creating a long right tail.
![alt text](<visual output/histogram_charges.png>)

The scatter plot indicates a fair positive relationship between BMI and insurance charges. Higher BMI values are generally associated with higher insurance charges, but the wide spread of points suggests that other factors, particularly smoking status, also contribute significantly to insurance costs.
![alt text](<visual output/scatterplot.png>)

The box plot shows that smokers have a higher median insurance charge and a wider spread than non-smokers, indicating greater variability in healthcare costs. Non-smokers generally have lower and more consistent charges, although a few outliers are present, representing individuals with unusually high medical expenses.
![alt text](<visual output/boxplot.png>)

8. The Pearson correlation analysis showed that age and charges had the highest absolute correlation r = 0.2984, indicating a weak to moderate positive relationship. This suggests that insurance charges generally increase with age. However this correlation does not necessarily imply a causal relationship. A plausible alternative explanation is that older individuals are more likely to have chronic health conditions, require more medical care or have a higher prevalence of smoking and other health risks, all together may contribute to increased insurance charges. Therefore, age alone should not be considered the sole cause of higher medical expenses.
![alt text](<visual output/correlation_heatmap.png>)

9. a. The charges column had a mean of 13270.42 and a median of 9382.03. The children column had a mean of 1.10 and a median of 1.00. The noticeable difference between the mean and median for charges reflects its positive skewness, supporting the use of the median for imputation. After imputation, both columns contained no missing values.

b. The Spearman correlation matrix was compared with the Pearson correlation matrix to identify relationships that may be consistent but not perfectly linear. The largest differences were observed for Age and Charges of 0.2350, BMI and Charges of 0.0789, and Charges and Children of 0.0658. The Age and Charges pair showed the greatest difference, indicating that insurance charges generally increase with age although the relationship is not perfectly linear. The BMI and Charges pair suggests that higher BMI is generally associated with higher insurance charges, but the increase is not proportional across all values. The Charges and Children pair showed only a small difference, indicating a weak relationship between the number of children and insurance charges. For Part 2, Pearson correlation will be used as the primary guide for feature selection because it is more suitable for identifying linear relationships commonly used in predictive models. Spearman correlation will be used as a supporting measure to identify any important monotonic but non-linear relationships.

Grouped aggregation was performed using categorical attribute - smoking status and numerical attribute - insurance charges. Smokers had the highest average insurance charge of about 32,050 and the highest standard deviation of about 11,542 indicating greater variation in medical costs. The high standard deviation suggests that smoking status alone cannot accurately predict insurance charges, as other factors also contribute. The mean ratio of 3.80 between smokers and non-smokers indicates a substantial difference showing that smoking status may be a strong predictive feature for Part 2.

10. The cleaned dataset is saved.



