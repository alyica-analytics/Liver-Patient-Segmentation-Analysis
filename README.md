# Liver Patient Segmentation Analysis
An unsupervised machine learning project exploring whether liver function measurements can reveal distinct patient profiles through K-means clustering and demographic analysis.
## Project Overview
This project explores whether patient demographics and liver function measurements can reveal distinct patient profiles through unsupervised learning.
## Business Question
**Can patient demographics and liver function measurements reveal distinct patient profiles?**
## Dataset
- **Source:** Kaggle - Liver Patient Dataset
- **Original Dataset Size:** 583 rows x 10 columns
- **Cleaned Dataset Size:** 570 rows x 10 columns

For accurate analysis, 13 duplicate records were identified and removed from the dataset.
## Data Cleaning
The dataset initially contained 583 rows and 10 columns.

Data quality checks identified:
- No missing values across the dataset.
- 13 exact duplicate records were identified.
- The duplicate records contained no unique information and were removed.
- After removing duplicates, the dataset contained 570 unique records.

Summary statistics were recalculated after duplicate removal to confirm the cleaned dataset.
## Exploratory Data Analysis
Exploratory data analysis (EDA) was conducted to understand the distribution of patient demographics and liver function measurements, identify potential outliers, and examine relationships between variables. 
### Distribution of Variables
Histograms were created for the numerical variables to examine their distributions.

Key observations included:
- Age was relatively symmetrically distributed with patients ranging from 4 to 90 years old.
- Total Bilirubin (TB), Direct Bilirubin (DB), Alkaline Phosphatase (Alkphos), SGPT, and SGOT were strongly right skewed.
- SGOT showed the greatest degree of skewness, followed by SGPT.
- Total Protein (TP) and Albumin (ALB) were relatively close to symmetric distributions.
- The A/G Ratio showed moderate right skewness.
### Outlier Analysis
Boxplots were used to identify potential outliers across the numerical variables.

Several liver function measurements contained observations substantially above their typical ranges, particularly SGPT and SGOT.

These observations were not removed during EDA because extreme values may represent meaningful differences in patient liver function rather than data-entry errors. Their potential impact on clustering will instead be considered during the preprocessing stage.
### Gender Distribution
A bar chart was used to examine the distribution of patients by gender.

Gender was treated as a categorical demographic variable and was not included in the numerical correlation analysis.
### Correlation Analysis
A correlation matrix was created to examine linear relationships between numerical variables.

Several strong relationships were identified:
- TB and DB showed a strong positive correlation (**0.87**).
- SGPT and SGOT showed a strong positive correlation (**0.79**).
- TP and ALB showed a strong positive correlation (**0.78**). 
- ALB  and A/G Ratio showed a moderately strong positive correlation (**0.68**).

Age showed relatively weak correlations with most liver function measurements, although moderate negative relationships were observed with ALB, TP, and A/G Ratio.

These correlations indicate that some liver function measurements contain overlapping information, which will be considered when preparing the data for clustering.
### Skewness Analysis
Skewness was calculated to quantify the distribution of the numerical variables.

The largest skewness values were observed for:
- **SGOT: 10.56**
- **SGPT: 6.70**
- **TB: 4.87**
- **Alkphos: 3.79**
- **DB: 3.19**

The strong right skewness and presence of extreme observations indicate that the liver function variables may require transformation before clustering. Because clustering methods such as k-means are sensitive to feature scale and distance, these distributions will be addressed during the data preprocessing stage.
## Data Preprocessing
Because the analysis uses K-means clustering, the numerical features were prepared to ensure that highly skewed variables and differences in measurement scale did not disproportionately influence the clustering results.
### Feature Selection
Eight liver function measurements were selected as the primary clustering features:
- TB
- DB
- Alkphos
- Sgpt
- Sgot
- TP
- ALB
- A/G Ratio

Age and Gender were excluded from the clustering model and were instead retained for demographic profiling of the resulting patient segments.
### Log Transformation
Several liver function measurements showed substantial right skewness during EDA. The following variables were transformed using 'log1p()':
- TP
- DB
- Alkphos
- Sgpt
- Sgot
- A/G Ratio

This reduced the influence of extreme values while retaining the observations for analysis.
### Standardization
The transformed features were standardized using 'StandardScaler()' so that each variable had a mean of approximately 0 and a standard deviation of 1. This step was important because k-means clustering relies on distance calculations, and variables measured on different numerical scales could otherwise disproportionately influence cluster formation. The resulting dataset contained 570 observations and 8 standardized clustering features.
## Patient Segmentation
K-means  clustering was used to identify distinct patient segments based on the eight standardized liver function measurements selected during preprocessing.
### Selecting the Number of Clusters
The Elbow Method was evaluated across cluster counts from 2 to 10. The plot showed a substantial reduction in inertia at lower values of k, followed by diminishing improvements as the number of clusters increased. 

Silhouette scores were also calculated for k = 2 through k = 10. The highest silhouette score occurred at k = 2 (approximately 0.34), indicating that two clusters provided the strongest separation among the tested solutions. 

Based on the Elbow Method and Silhouette analysis, k = 2 was selected for the final K-means model.
### K-Means Clustering
A K-means model was fitted using two clusters with a fixed random state and 10 initializations.

The resulting segments contained:
- **Cluster 0:** 391 patients (68.6%)
- **Cluster 1:** 179 patients (31.4%)
### Cluster Separation
PCA was used to visualize the resulting clusters in two dimensions. The first two principal components explained 67.3% of the total variance, with PC1 explaining 42.8% and PC2 explaining 24.5%.

The PCA visualization showed meaningful separation between the two clusters, primarily along PC1, although some overlap remained.
### Cluster Quality
Three clustering metrics were used to evaluate the segmentation:

- **Silhouette Score:** 0.34
- **Calinski-Harabasz Score:** 250.10
- **Davies-Bouldin Score:** 1.393

Together, these metrics suggest moderate cluster separation rather than perfectly distinct groups.
### Cluster Characteristics
**Cluster 0** generally exhibited lower bilirubin and liver enzyme measurements, while **Cluster 1** showed substantially higher value across these measurements.

**Cluster 1** also had lower average total protein, albumin, and A/G ratio values compared with **Cluster 0.**

The demographic profiles showed that **Cluster 1** was slightly older on average and had a higher proportion of male patients. These demographic variables were not used to form the clusters and were instead examined after clustering to help characterize the resulting segments.
### Cluster Profile Visualization
A standard heatmap was used to compare the average feature levels across the two clusters. The visualization showed a clear contrast between the clusters: **Cluster 0** had below-average levels of TB, DB, Alkphos, Sgpt, and Sgot and above-average levels of TP, ALB, A/G Ratio, while **Cluster 1** showed the opposite pattern.
## Cluster Profiles
The resulting clusters were examined using the original liver function measurements to identify the characteristics that distinguished each patient segment.
### Cluster 0
Cluster 0 contained **391 patients** and generally exhibited lower bilirubin and liver enzyme measurements.

The cluster had:
- **Mean TB**: 1.04
- **Mean DB**: 0.35
- **Mean Alkphos**: 217.69
- **Mean Sgpt**: 35.93
- **Mean Sgot**: 42.23
- **Mean TP**: 6.63
- **Mean ALB**: 3.36
- **Mean A/G Ratio**: 1.02

Overall, Cluster 0 showed lower average TB, DB, Alphos, Sgpt, and Sgot measurements alongside higher average TP, ALB, and A/G Ratio values.
### Cluster 1
Cluster 1 contained **179 patients** and exhibited substantially higher bilirubin and liver enzyme measurements compared with Cluster 0.

The cluster had:
- **Mean TB**: 8.30
- **Mean DB**: 4.01
- **Mean Alkphos**: 453.54
- **Mean Sgpt**: 175.40
- **Mean Sgot**: 256.07
- **Mean TP**: 6.21
- **Mean ALB**: 2.69
- **Mean A/G Ratio**: 0.78

Overall, Cluster 1 showed substantially higher average TB, DB, Alkphos, Sgpt, and Sgot measurements alongside lower average TP, ALB, and A/G Ratio values.
### Distribution Analysis
- Boxplots were used to compare the distributions of each liver function measurement across the two clusters.
- The distributions showed that the differences between clusters were not limited to the mean values. Several measurements, particularly TB, DB, Sgpt, and Sgot, showed substantially higher central values and greater variability in Cluster 1.
- ALB and A/G Ratio showed the opposite pattern, with higher central values in Cluster 0.
- Extreme observations were retained in the analysis because they may represent meaningful variation within the dataset rather than data-entry errors.
### Overall Cluster Characteristics
The cluster profiles revealed two distinct patterns in the liver function measurements:
- **Cluster 0**: Lower bilirubin and liver enzyme measurements with higher TP, ALB, and A/G Ratio values.
- **Cluster 1**: Higher bilirubin and liver enzyme measurements with lower TP, ALB, and A/G Ratio values.

These profiles provide a descriptive characterization of the patient segments identified by the K-means model.
## Key Findings
The analysis identified two distinct patient segments based on patterns in eight liver function measurements.
### The Patient Segments Were Identified
K-means clustering identified two patient segments:
- Cluster 0: 391 patients (68.6%)
- Cluster 1: 179 patients (31.4%)

The clusters were not equal in size, with Cluster 0 representing the larger segment.
### Cluster 1 Exhibited Higher Bilirubin and Liver Enzyme Measurements
Cluster 1 had substantially higher mean values for:
- Total Bilirubin (TB): 8.30 vs. 1.04
- Direct Bilirubin (DB): 4.01 vs. 0.35
- Alkaline Phosphatase (Alkphos): 453.54 vs. 217.69
- SGPT: 175.40 vs. 35.93
- SGOT: 256.07 vs. 42.23

These differences represented the strongest distinctions between the two patient segments.
### Cluster 0 Had Higher Protein-Related Measurements
Cluster 0 had higher average values for:
- Total Protein (TP): 6.63 vs. 6.21
- Albumin (ALB): 3.36 vs. 2.69
- A/G Ratio: 1.02 vs. 0.78

This created a contrasting profile between the two clusters, with Cluster 0 showing higher protein-related measurements and Cluster 1 showing higher bilirubin and enzyme measurements.
### The Clusters Showed Moderate Separation
- The selected two-cluster solution produced a silhouette score of approximately 0.34. The Calinski-Harabasz score was 250.10, and the Davies-Bouldin score was 1.393.
- PCA visualization showed meaningful separation between the clusters, primarily along the first principal component. The first two principal components explained 67.3% of the total variance.
- Overall, the results indicate that the identified clusters have meaningful differences, although the separation is not complete and some overlap remains.
### Demographic Differences Were More Modest
- Age and Gender were excluded from the clustering model and were instead used to characterize the resulting segments.
- Cluster 0 had a mean age of 43.74 years, compared with 47.27 years for Cluster 1. Both clusters were majority male, although Cluster 1 had a higher proportion of male patients.
- These demographic differences were smaller than the differences observed across the liver function measurements, suggesting that the primary distinction between the segments was based on liver function measurement patterns rather than demographic characteristics.
## Limitations
Several limitations should be considered when interpreting the results of this analysis.
### Unsupervised Learning Approach
The patient segments were created using unsupervised K-means clustering. Because the analysis does not use a predefined clinical outcome or diagnosis as a target variable, the resulting clusters represent statistical patterns in the selected measurements rather than confirmed clinical categories.
### Moderate Cluster Separation
The selected two cluster solution showed moderate rather than perfect separation. The silhouette score was approximately 0.34, and the PCA visualized showed some overlap between the clusters. Therefore, the segments should be interpreted as distinct patterns with some shared characteristics rather than completely separate patient groups.
### Sensitivity to Feature Selection and Preprocessing
K-means clustering can be influenced by the features included in the model and the preprocessing methods applied. Log transformation and standardization were used to reduce the influence of skewed distributions and differences in measurement scale, but alternative preprocessing choices or clustering methods could produce different segmentations.
### Dataset Size and Representativeness
The analysis was based on 570 unique observations from the available dataset. The dataset may not represent the broader population of patients with liver function measurements, so the identified segments should not be assumed to generalize to other populations without additional validation.
### Extreme Observations
Several liver function measurements contained extreme observations. These values were retained because they may represent meaningful variation rather than data entry errors. Although log transformations reduced their influence during clustering, extreme observations may still affect the resulting cluster profiles.
### Limited Demograpic Variables
Age and Gender were available for demographic profiling, but the dataset did not include a broader range of demographic, clinical, lifestyle, or medical history variables. Additional information could provide further context for understanding differences between patient segments.
### No Clinical Outcome Validation
The clustering results were not validated against clinical diagnoses, disease severity, treatment outcomes, or other external clinical outcomes. Further analysis would be needed to determine whether the identified statistical segments correspond to clinically meaningful patient groups.
## Tools & Techniques
- **Python** - Data analysis and machine learning
- **Pandas** - Data cleaning, manipulation, and aggregation
- **NumPy** - Numerical operations and feature transformations
- **Matplotlib** - Data visualization
- **Seaborn** - Statistical visualizations and heatmaps
- **Scikit-learn** - Data preprocessing, K-means clustering, PCA, and clustering evaluation
- **Jupyter Notebook** - Interactive analysis and documentation
## Conclusion
This analysis used unsupervised K-means clustering to identify distinct patient profiles based on eight liver function measurements.

The analysis identified two patient segments with different patterns across bilirubin, liver enzyme, and protein-related measurements. **Cluster 0** generally exhibited lower bilirubin and liver enzyme measurements alongside higher total protein, albumin, and A/G ratio values. **Cluster 1** exhibited substantially higher bilirubin and liver enzyme measurements alongside lower total protein, albumin, and A/G ratio values.

Age and Gender were excluded from the clustering model and were instead used to characterize the resulting segments. The demographic differences between the clusters were relatively modest compared with the differences observed in the liver function measurements.

The clustering evaluation indicated moderate separation between the two segments, with some overlap remaining. These results suggest that liver function measurements can reveal distinct statistical patterns among patients in this dataset. However, the resulting clusters should be interpreted as data driven profiles rather than clinical diagnoses, and additional clinical information and external validation would be needed to determine whether these segments correspond to clinically meaningful patient groups.