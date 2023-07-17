Tips for reporting computational results
=============================================

| Journals have increasingly high standards for resulting computational results of any kind, including image analysis and machine learning analysis.

.. _image_results:
  
Reporting image analysis results
-------------------------------------

When reporting image analysis results, we suggest performing three specialized analyses:

* Parameter sensitivity analysis: repeat image processing analysis with a wide range of all parameters. Ideally your conclusions should not significantly change with small changes in parameters (your results are robust). You may want to try a negative control (parameter values where you would expect to see no detected events/cells, like a maximum diameter of 0 pixels) and a positive control (like a minimum threshold of 0 a.u.).
* Reproducibility analysis: repeat image processing analysis on several subsets of data that you should be able to draw the same conclusion from (e.g., in a video where you're quantifying suspension velocity, 3 10-second clips of the same suspension in the same experiment).
* Comparison to gold standard analysis (typically a manual analysis): this may not be possible for all analyses, but you may want to manually characterize metrics like number and area of cells from a small subset of your data to make sure iCLOTS is providing you with reasonable results. `ImageJ <https://imagej.nih.gov/ij/download.html>`_ is a good basic-use image visualization and measuring tool.

.. _ml_results:

Reporting machine learning results
-------------------------------------

| iCLOTS v0.1.1 provides methods to implement k-means clustering. 

| Clustering seeks to group data points in a specified number of k clusters such that data points in the same group are more similar (in some sense) to each other than those in other groups.  

| The metric silhouette score, a mean measure of how similar objects are to their own cluster compared to other clusters, is returned to assess consistency of clustering. This score ranges from -1 to 1, where a high value indicates clusters are assigned well. Low or negative values may indicate too many/too few clusters.

| k-means clustering is understood to be a strong, general-purpose approach to clustering, but it may not be the best algorithm for your specific set of data or the hypothesis you're trying to answer. 

| While iCLOTS retains information about what sample the data points came from, this label is not considered during the clustering process (the algorithm is "unsupervised", it doesn't consider if the data point was from a healthy control or a clinical sample). 

| Data points are returned with a cluster label/number, but this does not indicate what the cluster may be (e.g. healthy, clinical, a certain subpopulation or type of cell, etc.) The iCLOTS manuscript compared these cluster labels with the known label (healthy, clinical) to do a Chi-Squared test comparing expected frequencies (e.g. healthy) to observed frequencies (e.g. clinical). iCLOTS returns the count of each cluster label within each sample and a mosaic plot to facilitate this type of analysis for users.

| Always consider the correlation matrix provided - numerical data inputs with too strong a correlation value (either positive or inverse) may bias results. 

| Always consider the ideal number of clusters as indicated by the scree plot.

*This is not an exhaustive resource for interpreting and reporting machine learning results. Your journal likely has more specific guidelines. For more information, please also see:*

* An excellent, accessibly written guide to machine learning for biologists and life scientists: Greener, J.G., Kandathil, S.M., Moffat, L. et al. A guide to machine learning for biologists. Nat Rev Mol Cell Biol 23, 40–55 (2022). https://doi.org/10.1038/s41580-021-00407-0
* `More details on clustering algorithms specifically <https://en.wikipedia.org/wiki/Cluster_analysis>`_
