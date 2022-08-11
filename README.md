# Intern-training_Ruiqi-Wu
Optimal Lag Selection for Multivariate Time Series by mRMR-Recursive Feature Elimination(Tree-based models)

![Pipeline_Optimal_Lag](https://user-images.githubusercontent.com/47016879/184218379-32a495da-27d6-4b9b-9c32-1e3a38c942f9.png)

Framework of mRMR+Recursive Feature Elimination:

1. Finding all pairs of highly correlated variables exceeding a correlation threshold T.The threshold ranges from (0, 1), and the starting point and end point can be adjusted based on the overall numerical level of the correlation matrix.

2. Transform feature pairs with same features into redundant feature groups.
![Correlated_feature_group](https://user-images.githubusercontent.com/47016879/184219436-d30ad29f-fb63-4a8b-b661-c221cc06fdc6.png)

3. Compute the mutual information score of all the features in the highly-correlated set to the target variable.The features in redundant feature group are then assigned with corresponding rankings.
4. For each redundant feature cluster,the feature with highest ranking is preserved and other features are abandoned. This step means knocking off the ones with lower mutual information for each pair of highly-correlated features.
![MI_Rank](https://user-images.githubusercontent.com/47016879/184219469-f56b4755-7bc4-4cb4-b356-9c2e5e1154c6.png)

5. Concatenate the remaining features in the highly-correlated feature sets with the less-correlated features in step 1. What is left now is the ones with the highest information scores and most negligible correlation with each other.

we apply XGBoost, LightGBM, and Random Forest regression models to recursively select features. The following explains how it works:

![Recursive Feature](https://user-images.githubusercontent.com/47016879/184219222-11e34274-0f8d-4522-a7db-00b36b0bc44d.png)
1. we select the top K features in the whole feature set. At the same time, we apply the validation set for early stopping to prevent over-fitting.
2. We remove the top α% features and find that top K features in the last (1 − α%) features.α is defined as 20% in this paper. If K is larger than the 20% of the feature set,
then the K will automatically stop at the 20% length of the feature set. Such a scheme is designed to deal with highly-correlated time series in finance and other domain.
3. Repeat the second step 5 times. The final feature subset is a union set of features selected at each round.

![Recursive Feature](https://user-images.githubusercontent.com/47016879/184219853-339fa891-3707-41ee-97ce-7d5cebce0a03.png)


Final Feature Subsets:

![final_subsets](https://user-images.githubusercontent.com/47016879/184220402-6f6d8a78-ec7b-4c59-96e2-56a535536213.png)



Framework of mRMR+Forward Selection

![Experiment_Flowchart](https://user-images.githubusercontent.com/47016879/184218429-47674701-2d70-483d-9a56-8b304e52bd84.png)
