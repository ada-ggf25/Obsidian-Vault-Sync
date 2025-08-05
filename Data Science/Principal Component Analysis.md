#algorithms #data_science #quantitative_finance #machine_learning

## Summary

Principal Component Analysis (PCA) is a linear dimensionality reduction technique that transforms a high-dimensional dataset into a new set of orthogonal axes (principal components) sorted by the amount of variance they capture. [Wikipedia](https://en.wikipedia.org/wiki/Principal_component_analysis?utm_source=chatgpt.com) The principal components are obtained by computing the eigenvectors of the data’s covariance matrix or by singular value decomposition (SVD) of the mean-centered data matrix. [GeeksforGeeks](https://www.geeksforgeeks.org/mathematical-approach-to-pca/?utm_source=chatgpt.com)[ranger.uta.edu](https://ranger.uta.edu/~chqding/PCAtutorial/PCA-tutor1.pdf?utm_source=chatgpt.com) By selecting the top _k_ components, PCA reduces the dataset’s dimensionality while retaining most of its variability, enabling improved visualization, noise reduction, and more efficient downstream modeling. [Wikipedia](https://en.wikipedia.org/wiki/Principal_component_analysis?utm_source=chatgpt.com)[Statistics Globe](https://statisticsglobe.com/scree-plot-pca?utm_source=chatgpt.com) PCA is implemented in popular libraries such as scikit‑learn, which provides a user-friendly `PCA` class for fitting and transforming data. [Scikit-Learn](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html?utm_source=chatgpt.com)

## What Is PCA?

Principal Component Analysis seeks a linear transformation of the original variables into a set of uncorrelated variables—called principal components—that capture the maximum variance in the data. [Wikipedia](https://en.wikipedia.org/wiki/Principal_component_analysis?utm_source=chatgpt.com) Each principal component is orthogonal to the previous ones and corresponds to an eigenvector of the covariance matrix, with its eigenvalue indicating the amount of variance explained along that direction. [Wikipedia](https://en.wikipedia.org/wiki/Principal_component_analysis?utm_source=chatgpt.com)

## Mathematical Foundations

### Covariance Matrix and Eigen Decomposition

Given an n×pn \times pn×p data matrix XXX where each column is mean-centered, PCA computes the p×pp \times pp×p covariance matrix

C=1n−1X⊤X.C = \frac{1}{n-1} X^\top X.

C=n−11X⊤X.

[GeeksforGeeks](https://www.geeksforgeeks.org/mathematical-approach-to-pca/?utm_source=chatgpt.com)

Eigen decomposition of CCC yields

C=UΛU⊤,C = U \Lambda U^\top,

C=UΛU⊤,

where the columns of UUU are the principal directions (eigenvectors) and the diagonal entries of Λ\LambdaΛ (eigenvalues) measure the variance explained by each component. [GeeksforGeeks](https://www.geeksforgeeks.org/mathematical-approach-to-pca/?utm_source=chatgpt.com)

### Singular Value Decomposition (SVD)

Alternatively, PCA can be performed via the singular value decomposition of XXX:

X=UΣV⊤,X = U \Sigma V^\top,

X=UΣV⊤,

where the columns of VVV are the principal directions, Σ\SigmaΣ contains the singular values, and Σ2/(n−1)\Sigma^2/(n-1)Σ2/(n−1) equals the eigenvalues of the covariance matrix. [ranger.uta.edu](https://ranger.uta.edu/~chqding/PCAtutorial/PCA-tutor1.pdf?utm_source=chatgpt.com)

## Algorithm Steps

1. **Center** the data by subtracting each feature’s mean. [GeeksforGeeks](https://www.geeksforgeeks.org/mathematical-approach-to-pca/?utm_source=chatgpt.com)
2. **Compute** the covariance matrix or perform SVD. [ranger.uta.edu](https://ranger.uta.edu/~chqding/PCAtutorial/PCA-tutor1.pdf?utm_source=chatgpt.com)
3. **Sort** eigenvalues in descending order. [GeeksforGeeks](https://www.geeksforgeeks.org/mathematical-approach-to-pca/?utm_source=chatgpt.com)
4. **Select** the top _k_ eigenvectors to form the projection matrix. [GeeksforGeeks](https://www.geeksforgeeks.org/mathematical-approach-to-pca/?utm_source=chatgpt.com)
5. **Project** the original data onto these _k_ principal components. [Stack Abuse](https://stackabuse.com/implementing-pca-in-python-with-scikit-learn/?utm_source=chatgpt.com)

## Choosing Number of Components

The **explained variance ratio** for each component is calculated as λi/∑jλj\lambda_i / \sum_j \lambda_jλi/∑jλj and is often visualized in a **scree plot**, which shows variance explained per component. [Statistics Globe](https://statisticsglobe.com/scree-plot-pca?utm_source=chatgpt.com) A **cumulative explained variance** plot displays the total variance retained by the first _k_ components, guiding the choice of _k_ to capture a desired threshold (e.g., 90 %). [Statology](https://www.statology.org/scree-plot-python/?utm_source=chatgpt.com)

## Properties and Practical Considerations

- **Linearity & Orthogonality**: PCA is inherently linear and produces orthogonal (uncorrelated) components. [Wikipedia](https://en.wikipedia.org/wiki/Principal_component_analysis?utm_source=chatgpt.com)
- **Scaling Sensitivity**: PCA maximizes variance, making it sensitive to feature scales and outliers. Standardizing features (zero mean, unit variance) is recommended. [GeeksforGeeks](https://www.geeksforgeeks.org/principal-component-analysis-pca/?utm_source=chatgpt.com) [Scikit-Learn](https://scikit-learn.org/stable/modules/decomposition.html?utm_source=chatgpt.com)
- **Interpretation**: Principal components can be interpreted via their loadings (coefficients), revealing how original features contribute to each component. [GeeksforGeeks](https://www.geeksforgeeks.org/principal-component-analysis-pca/?utm_source=chatgpt.com)

## Applications of PCA

- **Exploratory Data Analysis & Visualization**: Plotting data on the first two or three principal components for pattern discovery. [IBM - United States](https://www.ibm.com/think/topics/principal-component-analysis?utm_source=chatgpt.com)
- **Noise Reduction**: Discarding components with low variance, assumed to capture noise rather than signal. [GeeksforGeeks](https://www.geeksforgeeks.org/principal-component-analysis-pca/?utm_source=chatgpt.com)
- **Feature Extraction in Machine Learning**: Reducing dimensionality to speed up training, mitigate overfitting, and address multicollinearity. [Scikit-Learn](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html?utm_source=chatgpt.com)

## PCA in scikit-learn

### API Overview

The `sklearn.decomposition.PCA` class provides parameters such as:

- `n_components`: number of principal components to keep
- `whiten`: whether to scale components to unit variance
- `svd_solver`: algorithm for computing the decomposition (e.g., `'auto'`, `'full'`, `'arpack'`) [Scikit-Learn](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html?utm_source=chatgpt.com)

Key methods and attributes include:

- `.fit(X)`: compute principal components
- `.transform(X)`: project data onto computed components
- `.explained_variance_ratio_`: array of variance ratios per component [Scikit-Learn](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html?utm_source=chatgpt.com)

### Example Code

```python
python
CopyEdit
from sklearn.decomposition import PCA
from sklearn.preprocessing import StandardScaler

# Standardize features
X_scaled = StandardScaler().fit_transform(X)

# Fit PCA to reduce to 2 components
pca = PCA(n_components=2)
X_pca = pca.fit_transform(X_scaled)

print("Explained variance ratio:", pca.explained_variance_ratio_)

```

> The explained_variance_ratio_ attribute shows the proportion of total variance captured by each principal component. Scikit-Learn

## Extensions and Variants

- **Kernel PCA** uses the kernel trick to capture nonlinear structures by performing PCA in a high-dimensional feature space. [Wikipedia](https://en.wikipedia.org/wiki/Kernel_principal_component_analysis?utm_source=chatgpt.com)
- Other adaptations such as **Sparse PCA**, **Robust PCA**, and **Probabilistic PCA** tailor the core idea to handle sparsity, outliers, or probabilistic modeling goals. [Scikit-Learn](https://scikit-learn.org/stable/modules/decomposition.html?utm_source=chatgpt.com)

## Conclusion

PCA is a foundational tool for reducing dimensionality, uncovering latent structure, and enabling simpler, faster downstream analysis. [Wikipedia](https://en.wikipedia.org/wiki/Principal_component_analysis?utm_source=chatgpt.com) When applying PCA, always center (and typically scale) your data, inspect explained variance to determine the number of components, and examine loadings to interpret the resulting principal components in the context of your domain. [GeeksforGeeks](https://www.geeksforgeeks.org/mathematical-approach-to-pca/?utm_source=chatgpt.com)