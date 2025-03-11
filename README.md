# README

## Team Members
- https://github.com/Manar01
- https://github.com/sarahbelal20
- https://github.com/Ahmedbhassar
- https://github.com/ahmadalharbi21

## Introduction
This project aims to analyze popular TV shows from IMDb, identify underlying patterns, and group them into clusters using unsupervised machine learning. IMDb is a widely recognized, reliable source for movie and TV show ratings, making it ideal for gathering data on popular titles.

## Dataset Synopsis and Origin
We scraped data from IMDb’s “Most Popular” lists, encompassing both TV shows and movies. This initial combined dataset provided a broad perspective on viewer preferences and ratings. After analyzing and deriving insights from both movies and TV shows, we narrowed our focus to TV shows for model building, given their episodic nature and ongoing viewer engagement. 
IMDb’s data is highly regarded in the film and television industry due to its large user base and continuous updates, making it a reliable source for identifying popularity trends.

## Model Selection
We chose to explore KMeans and DBSCAN for their contrasting approaches to clustering:
- KMeans partitions data into k clusters around centroids, which is straightforward to interpret and widely used in many clustering tasks.
- DBSCAN, on the other hand, is a density-based algorithm that can discover arbitrarily shaped clusters and more effectively handle outliers.
  
We selected the features **['IMDb Rating', 'Total Ratings', 'run_length', 'ongoing']** because they collectively capture a show’s popularity (ratings count, average rating), duration (run_length), and current status (ongoing). By integrating these indicators, we aimed to reveal natural groupings around both audience appeal and production status.
After evaluating both methods with the Silhouette Score, KMeans performed slightly better (0.5278) than DBSCAN (0.5063), so we chose KMeans for our final model.

## Feature Engineering
To improve the models’ performance, we carried out additional **feature engineering**:

1. **Separating Rank and Series Name**  
   Initially, the dataset included **popularity rank** and the **series name** combined in one column. We split this into two distinct features—**Rank** and **Name**—so they could be used independently.

2. **Parsing Start and End Years**  
   The original year information appeared as a **single string** (e.g., “2000 - 2001”). We split this into separate **start_year** and **end_year** columns. We then derived **run_length** by subtracting the start year from the end year or the current year (if the show was ongoing).

3. **Identifying Ongoing Shows**  
   If the data indicated something like “2025 - ” (with no second year listed), we treated it as an ongoing show and created a binary **ongoing** feature (1 for ongoing, 0 for concluded). This helps the model distinguish active productions from those that have finished airing.


## Hyperparameter Optimization
For **DBSCAN**, we experimented with multiple configurations of **eps** 
(`eps_values = [0.3, 0.5, 1.0, 1.5, 2.0]`) and **min_samples** 
(`min_samples_values = [3, 5, 10, 15]`) to find the best balance between 
cluster quality and outlier detection. We evaluated each combination’s 
clustering performance using the **Silhouette Score**.

For **KMeans**, we tested various **numbers of clusters** (k) and used 
the **WCSS elbow method** alongside the **Silhouette Score** to pinpoint 
the optimal cluster count. This systematic approach ensured we found a 
solution that maximized separation between the clusters.


## Best Model Determination
We compared both models using the Silhouette Score, a metric that measures how similar an object is to its own cluster compared to other clusters. 
The higher the score, the more coherent the clustering. In our analysis, KMeans achieved a 0.5278 Silhouette Score, surpassing DBSCAN, which scored 0.5063. Because KMeans provided tighter, more well-defined clusters, we selected it as our final model for subsequent analyses and interpretations.

## Feature and Prediction Insights

1. **Cluster 0: Concluded**  
   - **Total Ratings (mean):** 16,606.96  
     - *Range:* 2,000 → 265,000 (median: 5,900)  
   - **Run Length (mean):** 3.16 years  
     - *Range:* 0 → 28 years (median: 2)  
   - **Proportion Ongoing:** 0.00 (0% still airing)  
   - **Content Rating (Min → Max):** Not Rated → TV-Y7-FV  
   - **Size:** 4,757 shows  

   This cluster comprises shorter-running series that have already ended. Their total ratings are moderate—some as low as 2,000, others reaching 265,000—but none are currently ongoing.

2. **Cluster 1: Running**  
   - **Total Ratings (mean):** 16,440.66  
     - *Range:* 2,100 → 273,000 (median: 5,300)  
   - **Run Length (mean):** 8.36 years  
     - *Range:* 0 → 75 years (median: 4)  
   - **Proportion Ongoing:** 0.977 (~98% still airing)  
   - **Content Rating (Min → Max):** Not Rated → TV-Y7  
   - **Size:** 1,306 shows  

   Nearly all shows in this cluster are still airing, with some running for an exceptionally long time. The total ratings range widely, and the majority remain actively in production.

3. **Cluster 2: Blockbuster**  
   - **Total Ratings (mean):** 521,402.30  
     - *Range:* 25,500 → 2,400,000 (median: 415,000)  
   - **Run Length (mean):** 8.28 years  
     - *Range:* 1 → 36 years (median: 7)  
   - **Proportion Ongoing:** 0.218 (~22% still airing)  
   - **Content Rating (Min → Max):** Not Rated → TV-Y7-FV  
   - **Size:** 87 shows  

   Despite being the smallest group, these titles have significantly higher total ratings (often in the hundreds of thousands to millions), indicating broad appeal or a strong fan base. Some are still ongoing, but many are completed, long-running hits.

### Streamlit app of the deplpoyd model
https://project-8-app-1.streamlit.app/
