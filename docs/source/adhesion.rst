Adhesion applications
======================

| iCLOTS adhesion applications are designed to provide information about the morphology and function of cells or multicellular structures on biologically-relevant surfaces. These same metrics form the backbone of digital pathology of blood smears and other clinical diagnostic tools. 

| Several sub-applications exist, all described below, including applications specialized for static brightfield microscopy, static fluorescence microscopy, a specialized protrusion/filopodia counter, and a transient adhesion application to analyze videos.

.. _brightfield:

Brightfield microscopy
----------------------------------

| Application that analyzes static, brightfield images of cells or multicellular structures adhered to some surface. May also be suitable for use with preliminary digital pathology approaches, e.g. with blood smears. This application does not separate cells by type, but you could use the post-processing machine learning clustering algorithm to group cells.

Input files:

* This application is designed to analyze a single image or a folder of image files (.jpg, .png, and/or .tif).
* The same input parameters are applied to each image.
* Users are lead to select an "invert" setting for analysis: you can indicate you would like to look for dark cells on a light background or light cells on a dark background.


Parameters to interactively adjust:

* µm-to-pixel ratio: The ratio of microns (1e-6 m) to pixels for the image, used to convert pixel measurements into area or distance dimensions. Use 1 for no conversion.
* Maximum diameter: The maximum diameter of a cell to be detected (pixels), must be set as an odd integer to work with the analysis algorithms.
* Minimum intensity: Minimum summed intensity of a cell to be detected (a.u.).


Output metrics:

* Resolution: single-cell
* Metrics: area (pixels, µm²), circularity (a.u.)
* Circularity ranges from 0 (straight line) to 1 (perfect circle).


Output files:

* All files are saved in a new folder titled "Results," located within the folder the original imaging data was selected from. Time and date are included in the folder name to identify when the analysis was performed and distinguish between different analyses.
* Labeled imaging data: each original image with each detected cell labeled with an index. Each index corresponds to single-cell data within the optional numerical data exports.

An Excel file containing:

* Area and circularity for each cell - one sheet/image.
* Area and circularity for all cells - one sheet (this combined sheet is best for analyzing replicates)/all files.
* Descriptive statistics (minimum, mean, maximum, standard deviation values for area and circularity) for individual files and combined files. Cell density (n/mm²) is also provided.
* Parameters used and time/date analysis was performed, for reference.

Graphical data:

* Histogram graphs for area and circularity for each individual image.
* Pairplot graph for each individual image, for all images combined where one color represents all pooled data, and for all images combined where each color represents a different image file.
* Pairplot including area and circularity metrics.


Some tips from the iCLOTS team:

* Computational and experimental methods:

  * The tracking methods used search for particles represented by image regions with Gaussian-like distributions of pixel brightness.
  * Analysis methods cannot distinguish between overlapping cells.
  * If cells are significantly overlapping, repeat experiment with a lower cell concentration.
  * Owing to the heterogenous appearance of certain cell types (e.g. the classic biconcave red blood cell shape, or the textured appearance of activated white blood cells), brightfield analysis may be challenging.
  * Consider using a fluorescent membrane stain coupled with our fluorescence microscopy adhesion applications if this does not conflict with your experimental goals, especially for WBCs/neutrophils.

* Choosing parameters:

  * Be sure to use µm-to-pixel ratio, not pixel-to-µm ratio.
  * Err on the high side of maximum diameter and low side of minimum intensity parameters unless data is particularly noisy or there's a large amount of debris.
  * If you're unsure if what parameter values to select, run the analysis with an artificially high maximum diameter and low minimum intensity and compare indexed cells to the resultant metrics - for example, perhaps you see a cell typically has a diameter of "x" so you set maximum diameter slightly higher to exclude debris, and a cell typically has a pixel intensity of "y" so you set minimum intensity just below this to exclude noise.
  * The maximum diameter parameter can behave non-intuitively if set too high for the sample presented. If you cannot detect clear cells, try lowering this parameter.

* Output files:

  * Analysis files are named after the folder containing all images (.xlsx) or image names (.png).
  * Avoid spaces, punctuation, etc. within file names.
  * In use cases where several files are analyzed, individual sheets are named after individual files. These file names may be cropped to about 15 characters to prevent corrupting the output file. Please make sure individual files within a folder are named sufficiently differently.
  * Excel and pairplot data includes a sheet/graph with all images combined. Only use this when analyzing replicates of the same sample.

Learn more about the methods forming the basis of our brightfield microscopy adhesion application:

* Crocker and Grier particle tracking, used to find individual cells: 

  * Crocker JC, Grier DG. Methods of Digital Video Microscopy for Colloidal Studies. Journal of Colloid and Interface Science. 1996;179(1):298-310. 

* Python library Trackpy, used to implement these algorithms:

  * `Documentation/tutorial for Trackpy <http://soft-matter.github.io/trackpy/v0.5.0/tutorial/walkthrough.html>`_

.. _fluorescence:

Fluorescence microscopy
----------------------------------

| Application that analyzes static, fluorescence microscopy images of cells (tested extensively on platelets, RBCs, and WBCs) adhered to some surface. This application does not separate cells by type, but you could use the post-processing machine learning clustering algorithm to group cells.

Input files:

* This application is designed to analyze a single image or a folder of image files (.jpg, .png, and/or .tif)
* The same input parameters are applied to each image.
* Users are lead to select color channels for analysis, including: a membrane stain (red, green, blue, or grayscale/white), which typically represents the area/morphology of a cell and a secondary "functional" stain (red, green, or blue - cannot be the same color as the membrane stain), which is an optional additional color channel that typically represents some activity or characteristic.


Parameters to interactively adjust:

* µm-to-pixel ratio: The ratio of microns (1e-6 m) to pixels for the image, used to convert pixel measurements into area or distance dimensions. Use 1 for no conversion.
* Minimum area: The minimum area (pixels) of a region (ideally, a cell) to be quantified - this can be used to filter out obvious noise.
* Maximum area: The maximum area (pixels) of a region to be quantified - this can be used to filter out obvious debris or cell clusters.
* Membrane stain threshold: Integer between 0 (black) and 255 (white/brightest) to be used for the main channel threshold. Any value below this threshold becomes background. Any value greater than or equal to this threshold becomes signal to further quantify.
* Secondary stain threshold: like the membrane stain threshold, but for the functional/characteristic stain.


Output metrics:

* Resolution: single-cell
* Metrics from membrane stain: area (pixels, µm²), circularity (a.u.), texture (a.u.)

  * Circularity ranges from 0 (straight line) to 1 (perfect circle).
  * Texture is the standard deviation of all pixel intensity values within one cell, a method for describing membrane heterogeneity.

* Metrics from functional stain: binary positive/negative stain, total fluorescence intensity of functional stain per cell (a.u.).


Output files:

* All files are saved in a new folder titled "Results," located within the folder the original imaging data was selected from.
* Labeled imaging data: each original image with each detected cell labeled with an index. Each index corresponds to single-cell data within the optional numerical data exports.
* An Excel file containing:

  * Area, circularity, texture, and functional stain metrics for each cell - one sheet/image.
  * Area, circularity, texture, and functional stain metrics for all cells - one sheet (this combined sheet is best for analyzing replicates)/all files.
  * Descriptive statistics (minimum, mean, maximum, standard deviation values for area, circularity, texture, and functional stain metrics) for individual files and combined files. Cell density (n/mm²) is also provided.
  * Parameters used and time/date analysis was performed, for reference.

Graphical data:

* Histogram graphs for area and circularity and a positive/negative functional stain pie chart for each individual image.
* Pairplot graph for each individual image, for all images combined where one color represents all pooled data, and for all images combined where each color represents a different image file.


Some tips from the iCLOTS team:

* Computational and experimental methods:

  * For all fluorescence microscopy applications, each stain to quantify must be solely in one red/green/blue channel, no other colors are accepted in the current version of iCLOTS.
  * See the export options on your microscopy acquisition software.
  * After application of the thresholds, the image processing algorithms analyze each interconnected region of signal as a cell. Application cannot distinguish between overlapping cells. If cells are significantly overlapping, please repeat the experiment with a lower cell concentration.
  * The developers and associated collaborators have found that red blood cells can be difficult to stain fluorescently. Antibody staining signal is typically weak and we've found membrane stains such as R18 can affect mechanical properties of the red blood cells. Consider using our brightfield adhesion application if this does not conflict with your experimental goals.
  * Functional stain represents some activity or characteristic of the cell, e.g. expression of a surface marker.
  * Consider that all pixel values should be below 255, the brightest color possible. If many pixels are equal to 255, any information about degree of intensity of the functional stain above the 255 value is lost.  Most microscope acquisition software has a function to detect if laser power, gain, etc. settings are producing "maxed-out," too-high values.

* Choosing parameters:

  * Be sure to use µm-to-pixel ratio, not pixel-to-µm ratio.
  * Sometimes cells (e.g., activated platelets) have a high-intensity "body" and low-intensity spreading or protrusions. Choose a high membrane stain threshold if you're primarily quantifying number of cells. Choose a low membrane stain threshold if you're primarily quantifying the morphology of cells.
  * Err on the high side of maximum area and low side of minimum area parameters unless data is particularly noisy or there's a large amount of debris.
  * If you're unsure if what parameter values to select, run the analysis with an artificially high maximum area and low minimum area and compare indexed cells to the resultant metrics - for example, perhaps you see a cluster typically has an area greater than "x" so you set maximum area slightly lower, and obvious noise typically has an area less than "y" so you set minimum area slightly higher.

* Output files:

  * Analysis files are named after the folder containing all images (.xlsx) or image names (.png).
  * Avoid spaces, punctuation, etc. within file names.
  * In use cases where several files are analyzed, individual sheets are named after individual files. These file names may be cropped to about 15 characters to prevent corrupting the output file. Please make sure individual files within a folder are named sufficiently differently.
  * Excel and pairplot data includes a sheet/graph with all images combined. Only use this when analyzing replicates of the same sample.
  * Functional/secondary stain metrics are reported in two ways: (1) signal (binary): 0 indicates negative for staining, 1 indicates positive for staining. This can be useful for calculating a percent expression. and (2) functional stain intensity (a.u.): summed value of all functional stain pixels within the membrane stain area. Take care interpreting this number, as range of intensity can vary image-to-image or even within image due to changes in laser power, bleaching, etc.
  * No intensity metrics are reported from the main color in the current version of iCLOTS, as this color should indicate morphology only.

Learn more about the methods forming the basis of our fluorescence microscopy adhesion application:

* Region analysis via python library scikit-image: 

  * Relevant citation: van der Walt S, Schönberger JL, Nunez-Iglesias J, et al. scikit-image: image processing in Python. PeerJ. 2014;2:e453. 
  * `Documentation/tutorial for scikit-image region analysis <https://scikit-image.org/docs/stable/auto_examples/segmentation/plot_regionprops.html>`_
