# MSCS 634 – Lab 5: Hierarchical and DBSCAN Clustering

**Name:** Sowmya Cheruku  
**Course:** Advanced Big Data and Data Mining (MSCS 634)  
**University:** University of the Cumberlands  
**Instructor:** Satish Penmatsa

## Purpose
This lab compares two clustering algorithms, Agglomerative (Hierarchical) Clustering and DBSCAN, on the Wine dataset from scikit-learn (178 wines, 13 chemical features, 3 grape cultivars). I standardized the features, tried different parameter settings for both algorithms, visualized the clusters (using PCA only for plotting), and evaluated them with Silhouette, Homogeneity, and Completeness scores.

**Files**
- `MSCS_634_Lab_5.ipynb` – full notebook with code, outputs, and written analysis

## Key Insights
- **Hierarchical (Ward linkage, 3 clusters) worked best:** Silhouette 0.277, Homogeneity 0.790, Completeness 0.783. The three clusters (sizes 58, 56, 64) closely match the real cultivars.
- **The dendrogram supports k = 3.** There is a big jump in merge distance before the last two merges, so cutting just below it gives 3 clusters.
- **Linkage mattered a lot.** Single and average linkage put about 174 of the 178 wines in one cluster (Homogeneity close to 0), while Ward and complete linkage gave balanced clusters.
- **DBSCAN struggled.** My best setting (`eps=2.5`, `min_samples=10`) found only 2 clusters and labeled 32 points (about 18%) as noise. Homogeneity was 0.504 and Completeness 0.560. Its Silhouette (0.323) is higher only because noise points are excluded from that calculation.
- **DBSCAN is very sensitive to parameters.** At `eps=1.5` almost everything is noise, and at `eps >= 3` almost everything merges into one cluster. Higher `min_samples` produced more noise.

## Challenges and Decisions
- **Scaling:** Features like `proline` have much larger values than others, so I used `StandardScaler` before clustering.
- **Visualization:** With 13 features I couldn't plot the data directly, so I used a 2-component PCA (about 55% of the variance) for the plots only. All clustering used the full 13 standardized features.
- **Choosing DBSCAN parameters:** The k-distance graph had no clear elbow, so I ran a grid of `eps` (1.5 to 4) and `min_samples` (3, 5, 10) and compared the results.
- **Metric decisions:** Silhouette for DBSCAN is calculated on non-noise points only (and needs at least 2 clusters). For Homogeneity and Completeness I treated noise as its own label. This means the DBSCAN scores are not perfectly comparable to the hierarchical ones, and I noted that in the notebook.

## How to Run
```
pip install numpy pandas matplotlib seaborn scikit-learn scipy jupyter
jupyter notebook MSCS_634_Lab_5.ipynb
```
