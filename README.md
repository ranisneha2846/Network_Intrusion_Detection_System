# Network_Intrusion_Detection_System
<img width="1584" height="396" alt="Black and Orange Aesthetic LinkedIn Banner" src="https://github.com/user-attachments/assets/d86736e6-0691-4ae8-a9fd-b4f893fcf691" />

Network security is critical in today's interconnected digital world. Intrusion Detection Systems (IDS) are designed to identify unauthorized access attempts and malicious network activity. Traditional rule-based IDS fail to detect novel and evolving attack patterns, making machine learning approaches essential for modern cybersecurity
<img width="1584" height="396" alt="Black and Orange Aesthetic LinkedIn Banner (1)" src="https://github.com/user-attachments/assets/4789ad51-38b0-4331-a45b-0b34012ae466" />
Most existing IDS research focuses on single-dataset evaluation — models are trained and tested on the same dataset (e.g., NSL-KDD). This approach has a critical flaws.
Data Loading and Inspection

The notebook begins by importing necessary libraries and loading the NSL-KDD training and testing datasets.

It performs initial data exploration, displaying the dataset's shape and summary statistics, and checking for missing values and duplicates (finding none).

A difficulty column, present only in the original NSL-KDD data, is noted but later dropped.

# Exploratory Data Analysis (EDA)

Class Distribution: Visualizes the distribution of the binary_label (Normal vs. Attack) and the multi-class attack_type (DoS, Probe, R2L, U2R) in both training and testing sets. The attack_type is derived from the label column using a custom mapping function.

Feature Distributions: Analyzes the distribution of key features like src_bytes, dst_bytes, service, protocol_type, and flag using bar plots and Kernel Density Estimates (KDE) to understand their relationship with normal and attack traffic.

Correlation Analysis: Generates a heatmap of feature correlations to identify highly correlated features.

Service and Attack Type Analysis: Examines the relationship between services (service column) and attack types, visualizing which services are most commonly associated with attacks.

# Data Preprocessing

Feature Encoding: Categorical features (protocol_type, service, flag) are encoded using LabelEncoder.

Target Encoding: A binary label (binary_label) is created where normal is mapped to 0 and all attacks to 1. A multi-class target (attack_encoded) is also created based on the attack_type.

Feature Scaling: Numerical features are standardized using StandardScaler.

Feature Selection: The final features set for modeling includes 41 numerical features, excluding label, attack_type, binary_label, attack_encoded, and difficulty. The dataset is split into training and validation sets.

# Model Training and Evaluation on NSL-KDD

A wide array of machine learning models are trained and evaluated on the NSL-KDD test set. Models include Naive Bayes, Logistic Regression, Decision Tree, Random Forest, AdaBoost, KNN, LinearSVM, LightGBM, XGBoost, and CatBoost.

Performance Metrics: Models are evaluated on Accuracy, Precision, Recall, and F1-Score. The training time for each model is also measured.

Top Performance: CatBoost achieves the highest accuracy (80.67%) on the test set, followed closely by XGBoost (80.09%) and LightGBM (79.70%).

Overfitting Check: A comparison of train and test accuracies reveals that many models, especially tree-based ones (e.g., Random Forest, Decision Tree, XGBoost), show signs of significant overfitting, with near-perfect training accuracy but significantly lower test accuracy. CatBoost has one of the lowest overfitting gaps (19.25%).

# Cross-Dataset Evaluation on CICIDS2017

A 10% sample of the larger CICIDS2017 dataset is preprocessed and used for cross-dataset evaluation.

The models are retrained on the NSL-KDD dataset and tested on the preprocessed CICIDS2017 sample.

Generalization Performance: The results show excellent generalization, with most models achieving over 99% accuracy. XGBoost and LightGBM achieve the highest accuracies of 99.89% and 99.88%, respectively. This is a stark contrast to the NSL-KDD results, suggesting that the CICIDS2017 dataset, while larger, may contain patterns more easily learned or that the feature sets and class distributions differ significantly.

# Cross-Dataset Evaluation on KDD99

A third dataset, KDD99, which is structurally very similar to NSL-KDD, is used for further cross-dataset testing.

The models trained on NSL-KDD are tested on the KDD99 dataset.

Near-Perfect Accuracy: Most models achieve near-perfect or perfect accuracy on KDD99. XGBoost and Random Forest achieve 100% accuracy. This indicates that the patterns learned by these models generalize extremely well to similar data from the same source (the original KDD Cup 1999 dataset).

# Results and Insights

Cross-Dataset Comparison: The results highlight a massive performance drop when models are transferred from NSL-KDD to CICIDS2017 (e.g., CatBoost drops from 99.86% to 80.67%), indicating that models are highly sensitive to differences in data distributions between datasets.

Within-Source Generalization: Conversely, the high accuracy on KDD99 validates that the models successfully learned the underlying patterns of the KDD'99 data and can generalize well to new samples from the same distribution.

Model Comparison: Tree-based models (XGBoost, LightGBM, Random Forest, CatBoost) consistently outperform linear models (Logistic Regression, LinearSVM) and the simple Naive Bayes classifier across all datasets, particularly excelling in terms of accuracy and F1-Score.
