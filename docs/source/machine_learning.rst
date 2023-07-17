Machine Learning-enabled Interpretation
==========================================

| iCLOTS may generate large datasets, depending on file inputs. Should users need additional interpretation of these large datasets, our machine learning application mathematically characterizes natural groupings within any number of pooled datasets, proving useful for detecting cell sample subpopulations or healthy/clinical sample differences. Methods to apply k-means clustering are provided.

.. _ml:

Post-processing clustering algorithm application
-----------------------------------------------------

| This machine learning application applies clustering algorithms to any properly-formatted data, including iCLOTS data. Typically, a series of data points (e.g., cells) are represented by multiple metrics (e.g. velocity, size, or fluorescence intensity). Clustering is an unsupervised machine learning technique designed to mathematically characterize natural groupings within datasets (e.g., cell subpopulations from a single dataset or healthy-clinical dichotomies).


| The iCLOTS development team suggests the `review paper <https://www.nature.com/articles/s41580-021-00407-0>`_ "A guide to machine learning for biologists" (Greener, Nature Reviews Molecular Cell Biology, 2021) for a better understanding of machine learning. Please also see this documentation's guidance on reporting computational results.

| Machine learning workflow (steps), briefly:

Step 1: load data

* The user provides one or more excel files, each representing a single sample. Excel files must have only one sheet, which should contain only individual data points (e.g. cells) described using the same features (numerical metrics, e.g. velocity, size, or fluorescence intensity). A minimum of two features are required.
* File loading is set up such that user selects a folder that contains all relevant excel sheets.
* Your files may fail to load correctly if they are already open in Excel. This application will read temporary files created by having the file open. Please close any workbooks you're using before selecting a folder of files.

Step 2: select features

* After data is loaded, all datasets are combined into one "pool." Clustering is an unsupervised algorithm: no "labels" such as sample names are considered during clustering. Later outputs do provide the number of sample data points found in each cluster.
* iCLOTS detects all numerical columns shared between files. Because iCLOTS does not produce any text metrics that could be considered a feature, opportunity to use text-based metrics are not included in v0.1.1. In later versions, text metrics will be converted to categorical values. In the meantime, you could change text-based categories to numerical categories on your own in Excel.
* A correlation matrix is automatically displayed. A correlation matrix is a visualization of how much each pairwise combination of variables is correlated, or related. Highly related variables (value approaching -1 or 1) may bias results, e.g. considering both area (pix) and area (µm²) gives area undue influence on clustering.
* In this step, users have the option to select what metrics they would like to retain for final analysis.

Step 3: select number of clusters to retain

* After features are selected and submitted, a scree plot is generated. A scree plot indicates a suggested optimal number of mathematically significant clusters to retain.It is presented as a line plot of the sum of squared errors (SSE) of the distance to the closest centroid for all data points for each number of potential clusters (iCLOTS allows up to 12 clusters). Typically, as the number of clusters increases, the variance, or sum of squares, for each cluster group decreases.
* The "elbow" point of the graph represents the best balance between minimizing the number of clusters and minimizing the variance in each cluster.
* The user can choose any number of clusters to group data into using the clustering algorithm.

Step 4: k-means clustering algorithms are applied

* After the number of clusters are selected and submitted, k-means clustering algorithms are applied to the pooled datasets.
* Several types of clustering algorithms exist, but a k-means algorithm was selected for iCLOTS v0.1.1 as it is understood to be a robust general-purpose approach to discovering natural groupings within high-dimensional data.
* The pooled data points are automatically partitioned into clusters that minimize differences between shared metrics.

Step 5: review outputs

* iCLOTS creates a series of graphs:
  
  * A mosaic plot, a specialized stacked bar chart, displays the number of stacked data points from each dataset in each of the clusters. This is designed to assist the user in visualizing the contribution of each dataset to each cluster.
  * A pair plot, a pairwise series of scatterplots and histograms, shows each dataset (marker type) and cluster (color).
  
* An excel file with all data points is also created:
  
  * This sheet has the original sample name, all numerical metrics used, and a cluster label for each data point.
  * Descriptive statistics for clusters, cluster label count per dataset, cluster number, and silhouette score are also included.
  * Silhouette score is a metric with a value from -1 (inappropriate clusters) to 1 (best clustering)


Some tips from the iCLOTS team:

* Clustering techniques are well-suited to exploring distinguishing features between known populations and to finding new, previously imperceptible groupings within a single population. However, metrics describing populations of cells typically follow Gaussian distributions which may have significant overlap.

Learn more about the methods forming the basis of our machine learning application:

* K-means clustering:

  * Relevant citation, algorithm: Lloyd, Stuart P. "Least squares quantization in PCM." Information Theory, IEEE Transactions on 28.2 (1982): 129-137.
  * Relevant citation, assessing goodness of clustering: Rousseeuw, P. J. Silhouettes: A graphical aid to the interpretation and validation of cluster analysis. Journal of Computational and Applied Mathematics 20, 53-65, doi:https://doi.org/10.1016/0377-0427(87)90125-7 (1987).

* Clustering via python library scikit-lean: 

  * Relevant citation: Pedregosa, F. et al. Scikit-learn: Machine Learning in Python. J. Mach. Learn. Res. 12, 2825–2830 (2011).
  * `Clustering documentation/tutorial <https://scikit-learn.org/stable/modules/clustering.html>`_
