---
title: Clustering - Customer Personality (K-means/PCA)
description: Business analysis of a company’s ideal customers
authors:
  - name: Pierre Graef
    to: null
    avatar:
      src: https://i.pravatar.cc/128?u=2
image:
  src: /img/article_customer/customer_cover.png
date: 2024-10-10T00:00:00.000Z
---

In this project, we perform a **Customer Personality Analysis** to gain insights into the customer base of a company, using data provided from a kaggle dataset. You can find, download and modify all my code on my Kaggle :

[Clustering :  Customer Personality (K-means/PCA)](https://www.kaggle.com/code/pierregraef/clustering-customer-personnality-k-means-pca/notebook){:target="_blank"}

Understanding customer personalities helps businesses tailor their products and services to meet specific customer needs. By analyzing demographic, purchasing, and behavioral data, companies can segment their customers more effectively, improving marketing strategies, boosting sales, and enhancing overall customer satisfaction.

The primary objective of this analysis is to identify **customer segments** based on their purchasing habits and demographics. This segmentation will allow for more targeted marketing campaigns and better product recommendations. Using techniques like **exploratory data analysis (EDA)** and **clustering algorithms**, we will uncover key patterns and divide the customer base into distinct groups that exhibit similar behaviors.

## Exploratory Data Analysis

### Elbow Method for determining optimal clusters

The **Elbow Method** is a technique used to determine the optimal number of clusters (K) for K-Means clustering. The goal is to choose a K value that minimizes the sum of squared distances between each point and its assigned cluster center, known as the **Sum of Squared Errors (SSE)**.

**How it works :**

1. **Run K-Means** for a range of values for K (e.g., from 1 to 10).
2. For each K, calculate the **SSE**.
3. **Plot SSE** against the number of clusters K. As K increases, SSE decreases because the points are closer to their centroids.
4. Look for an "elbow" in the plot: a point where the SSE starts decreasing at a slower rate. This point suggests the optimal number of clusters.

**Mathematical formula :**

::latex
---
latex: \[ SSE = \sum_{i=1}^{n} \sum_{j=1}^{k} \min(||x_i - \mu_j||^2) \]
---
::

We want to minimize the squared Euclidean distance between the data point x and its closest cluster centroid.

The **elbow point** indicates where adding more clusters doesn't significantly improve the model, balancing model complexity and SSE reduction.

```js
# python code
```

#### Soon...
