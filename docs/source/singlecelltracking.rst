Single cell tracking applications
====================================

| This series of applications track one or many cells within a frame using adapted Crocker and Grier particle tracking methods. Cells are linked into individual trajectories. Cells can travel in any direction(s). iCLOTS provides a distance traveled, transit time, and velocity (distance/time) for each tracked cell. Typically this application would be used to track cells transiting a microfluidic device, but other uses may be possible. A specialized application analyzes fluorescence microscopy videos of cells transiting any device (see below). A separate specialized application analyzes instances of channel flow, where cells are traveling only in the x-direction.

| The current version of the iCLOTS single cell tracking application provides an overall distance traveled, which is calculated from the first instance to the last instance as a straight line. Planned updates include frame-by-frame resolution, more precise directionality, and measures of acceleration.


.. _brightfield sct:

Brightfield microscopy single cell tracking
------------------------------------------------

| The brightfield microscopy sub-application of the single cell tracking algorithm tracks cells within brightfield video microscopy.

Input files:

* This application is designed to analyze a single video (.avi).
* The same input parameters are applied to every frame.
* The application will display the video in the center of the analysis window - users can scroll through frames using the scale bar below.
* If your data is saved as a series of frames, please see the suite of video editing tools to convert to .avi.
* Users can optionally choose a region of interest from the video for analysis. Currently, regions of interest are selected using a draggable rectangle. Later versions of iCLOTS will incorporate options for ROIs of other shapes.

Parameters to interactively adjust:

* µm-to-pixel ratio: The ratio of microns (1e-6 m) to pixels for the image, used to convert pixel measurements into area or distance dimensions. Use 1 for no conversion.
* Maximum diameter: The maximum diameter of a cell to be detected (pixels), must be set as an odd integer to work with the analysis algorithms.
* Minimum intensity: Minimum summed intensity of a cell to be detected (a.u.).
* Search range: the distance (in pixels) to search for a feature in the subsequent frame. Short values may miss fast-moving cells, but larger values may increase computational expense and result in cell movement erroneously being considered as the trajectory of a separate cell. 
* Minimum distance traveled: the total detected distance a cell must travel to be considered a valid data point (in pixels). This should be used mostly to filter out obvious noise. Keep in mind, depending on the rate of imaging you may not see a cell at every position it transits - e.g., for the biophysical flow cytometer we typically set minimum distance as one-third the length of the channel - maybe the first time the microscope captures the cell, it's 10-30 microns into the channel, but the last time it captures the cell, it's 10-30 microns from the end. In this case, it would appear the cell traveled less than the length of the channel.
* Frames per second (FPS): the rate  of imaging, a microscopy parameter. Note that FPS values pulled  directly from videos can be inaccurate, especially if the video has been resized or edited in any way. Higher FPS imaging settings provide more precise distance and transit time values but increase computational expense.

Output metrics:

* Resolution: single-cell
* Metrics: area (pixels), velocity or (µm/s)
* Transit time (s) and distance traveled (µm) are also provided. Velocity is equal to distance traveled divided by transit time.
* Area is provided in pixels only because the algorithm primarily detects moving shapes - this may include changes in intensity of the channel walls as the cell travels nearby. Please consider area a relative measurement.

Output files:

* All files are saved in a new folder titled "Results," located within the folder the original imaging data was selected from. Time and date are included in the folder name to identify when the analysis was performed and distinguish between different analyses.
* Labeled imaging data (optional). Each frame of the video with each detected cell labeled with an index. Each index corresponds to single-cell data within the optional numerical data exports. While exporting the labeled frames takes extra time, the developers suggest doing so anyways. It will be useful for troubleshooting outliers, etc.

* An Excel file containing:

  * Area and velocity for each cell - one sheet/video.
  * Descriptive statistics (minimum, mean, maximum, standard deviation values for area, distance traveled, transit time and velocity).
  * Parameters used and time/date analysis was performed, for reference.

Graphical data:

* Histogram graphs for area, distance traveled, transit time, and velocity for the individual video.
* Pairplot graph.

Some tips from the iCLOTS team:

* Computational and experimental methods:

  * An algorithm called "background subtraction" is applied to the video frames before tracking algorithms are used to detect cell movement. This removes features that don't move - like microfluidic channel walls, etc. The first displayed analysis image of any video will be white. If cells are stuck in channels for exceedingly long amounts of time, the background subtraction algorithm may also remove them. You may need to adjust experimental variables, like pump speed or device height, if cells are stuck for too long. Our application that tracks transient adhesion (see above) does not use background subtraction.
  * Cells transiting a device so closely that they clump together will be detected as one cell. Check area measurements for especially large values to ensure this has not happened. You may need to adjust experimental variables such as cell concentration to prevent clumping, particularly for WBCs.
  * Some quality measures are imposed on data points which may affect quantitative results. To calculate an accurate velocity measurement, cells must be present in at least 3 frames. Users may need to reduce pump speed if cells are transiting a device too quickly.

* Choosing parameters:

  * Be sure to use µm-to-pixel ratio, not pixel-to-µm ratio.
  * Err on the high side of maximum diameter and low side of minimum intensity parameters unless data is particularly noisy or there's a large amount of debris. 
  * If you're unsure if what parameter values to select, run the analysis with an artificially high maximum diameter and low minimum intensity and compare indexed cells to the resultant metrics - for example, perhaps you see a cell typically has a diameter of "x" so you set maximum diameter slightly higher to exclude debris, and a cell typically has a pixel intensity of "y" so you set minimum intensity just below this to exclude noise.
  * Maximum diameter can behave non-intuitively if set unnecessarily high. Lower if obvious cells are being missed.

* Output files:

  * Analysis files are named after the folder containing all images (.xlsx) or image names (.png).
  * Avoid spaces, punctuation, etc. within file names.
  * Excel and pairplot data includes a sheet/graph with all images combined. Only use this when analyzing replicates of the same sample.

Learn more about the methods forming the basis of our single cell tracking application:

* Crocker and Grier particle tracking, used to find and track individual cells: 

  * Crocker JC, Grier DG. Methods of Digital Video Microscopy for Colloidal Studies. Journal of Colloid and Interface Science. 1996;179(1):298-310. 

* Python library Trackpy, used to implement these algorithms:

  * `Trackpy documentation/tutorial <http://soft-matter.github.io/trackpy/v0.5.0/tutorial/walkthrough.html>`_

.. fluorescence sct:

Fluorescence microscopy single cell tracking
-----------------------------------------------

| This application works in the same way as our single cell tracking image processing application for brightfield microscopy videos, but with an added fluorescence cell intensity output metric to describe the summed intensity of individual cells within a fluorescence microscopy video.

| All inputs/outputs, methods, and tips and tricks remain the same. Ideally, the stain used for cells describes some functionality or property of the cell. The developers have found that it can be challenging to detect a strong fluorescence signal from moving cells. If troubleshooting experimental variables such as stain concentration, pump speed, and device height do not result in a stronger signal, use the "Edit contrast" video processing application with a gain (alpha) value that increases signal. Then, divide the output cell intensity metrics by this alpha value to remove any bias.

| Users may optionally choose a region of interest to analyze. The application builds a map of potential channels from all fluorescence signal in the video. Usually this is a suitable representation of the microfluidic device.

.. channel flow:

Channel flow and deformability
----------------------------------

| The iCLOTS *Nature Communications* manuscript demonstrates use of the single cell tracking algorithms primarily with the use of the Lam lab "biophysical flow cytometer" microfluidic device, a research-developed microfluidic designed to provide a relative measure of cell deformability, a mechanical property. This specialized assay relies on channel flow, or flow only in the x-direction of a series of frames.

| Previous manuscripts describing the deformability device, detailed experimental protocols, and microfluidic mask files are all available on request.

| If you have a separate instance of channel flow, this sub-application is more computationally efficient. For use with channel flow, rotate the video using our suite of video editing tools so that flow is horizontal.


Input files:

* Users are lead to choose a region of interest from the video for analysis. When using with the biophysical flow cytometer device, please choose only the area of the smallest channels. For any microfluidic device you may use, choose a region with only straight channel portions perpendicular to the bottom of the image (x-direction flow only.)

Parameters to interactively adjust:

* µm-to-pixel ratio: The ratio of microns (1e-6 m) to pixels for the image, used to convert pixel measurements into area or distance dimensions. Use 1 for no conversion.
* Maximum diameter: The maximum diameter of a cell to be detected (pixels), must be set as an odd integer to work with the analysis algorithms.
* Minimum intensity: Minimum summed intensity of a cell to be detected (a.u.).
* Search range: search range is automatically set to no more than 1/3 the channel length to ensure only the highest-quality data points are detected
* Minimum distance traveled: minimum distance traveled is automatically set to 1/3 the channel length to ensure only the highest-quality data points are detected
* Frames per second (FPS): the rate  of imaging, a microscopy parameter. Note that FPS values pulled  directly from videos can be inaccurate, especially if the video has been resized or edited in any way. A higher FPS rate provides more resolution, but may be more computationally expensive.

Output metrics:

* Resolution: single-cell
* Metrics: area (pixels), single cell deformability index (sDI, µm/s). sDI is velocity, but presented as sDI to indicate that it represents a relative measure of cellular mechanical properties.
* Transit time (s) and distance traveled (µm) are also provided. sDI is equal to distance traveled divided by transit time.
* Area is provided in pixels only because the algorithm primarily detects moving shapes - this may include changes in intensity of the channel walls as the cell travels nearby. Please consider area a relative measurement.

Output files:

* All files are saved in a new folder titled "Results," located within the folder the original imaging data was selected from. Time and date are included in the folder name to identify when the analysis was performed and distinguish between different analyses.
* Labeled imaging data (optional). Each frame of the video with each detected cell labeled with an index. Each index corresponds to single-cell data within the optional numerical data exports. While exporting the labeled frames takes extra time, the developers suggest doing so anyways. It will be useful for troubleshooting outliers, etc.

* An Excel file containing:

  * Area and sDI for each cell - one sheet/video,
  * Descriptive statistics (minimum, mean, maximum, standard deviation values for area and sDI).
  * Parameters used and time/date analysis was performed, for reference.

* Graphical data:

  * Histogram graphs for area and sDI.
  * Pairplot graph.


Some tips from the iCLOTS team:

* Computational and experimental methods:

  * An algorithm called "background subtraction" is applied to the video frames before tracking algorithms are used to detect cell movement. This removes features that don't move - like microfluidic channel walls, etc. If cells are stuck in channels for exceedingly long amounts of time, the background subtraction algorithm may also remove them. You may need to adjust experimental variables, like pump speed or device height, if cells are stuck for too long.
  * Cells transiting a device so closely that they clump together will be detected as one cell. Check area measurements for especially large values to ensure this has not happened. You may need to adjust experimental variables such as cell concentration to prevent clumping, particularly for WBCs.
  * Some quality measures are imposed on data points which may affect quantitative results. To calculate an accurate velocity measurement, cells must be present in at least 3 frames. Users may need to reduce pump speed if cells are transiting the device too quickly.
  * You may see cells that were detected in the on-screen background substraction and cell detection analysis window that were not labeled in the output data, indicating they were not treated as suitable data points. This is because those cells did not meet these quality standards.
  * Choose the target device height of the biophysical flow cytometer device during fabrication methods carefully. The width of the smallest channels in the device is about 6 µm. The Lam lab typically fabricates microfluidic device masks at a height of 5 µm for red blood cells or a height of 12-15 µm for white blood cells and associated cell lines. Cells must deform to fit through the device for meaningful deformability metrics.
  * Depending on how "sticky" cells are, the assay may measure adherence vs. deformability. Coat channels with a bovine serum albumin (BSA) solution prior to using to prevent non-specific binding. The developers suggest not reusing devices for multiple experiments. 
  * We typically use a pump speed of 1 µL/min coupled with a FPS rate of about 25 for use with the biophysical flow cytometer.

* Choosing parameters:

  * Be sure to use µm-to-pixel ratio, not pixel-to-µm ratio.
  * Err on the high side of maximum diameter and low side of minimum intensity parameters unless data is particularly noisy or there's a large amount of debris.
  * If you're unsure if what parameter values to select, run the analysis with an artificially high maximum diameter and low minimum intensity and compare indexed cells to the resultant metrics - for example, perhaps you see a cell typically has a diameter of "x" so you set maximum diameter slightly higher to exclude debris, and a cell typically has a pixel intensity of "y" so you set minimum intensity just below this to exclude noise.
  * Maximum diameter can behave non-intuitively if set unnecessarily high. Lower if obvious cells are being missed.

* Output files:

  * Analysis files are named after the folder containing all images (.xlsx) or image names (.png)
  * Avoid spaces, punctuation, etc. within file names.
  * Excel and pairplot data includes a sheet/graph with all images combined. Only use this when analyzing replicates of the same sample.

Learn more about the methods forming the basis of our deformability application:

* Crocker and Grier particle tracking, used to find and track individual cells: 

  * Crocker JC, Grier DG. Methods of Digital Video Microscopy for Colloidal Studies. Journal of Colloid and Interface Science. 1996;179(1):298-310. 

* Python library Trackpy, used to implement these algorithms:

  * `Trackpy documentation/tutorial (also above) <http://soft-matter.github.io/trackpy/v0.5.0/tutorial/walkthrough.html>`_

Manuscripts detailing the use of the biophysical flow cytometer device:

* Original manuscript: Rosenbluth MJ, Lam WA, Fletcher DA. Analyzing cell mechanics in hematologic diseases with microfluidic biophysical flow cytometry. Lab Chip. 2008 Jul;8(7):1062-70. doi: 10.1039/b802931h. Epub 2008 Jun 5. PMID: 18584080; PMCID: PMC7931849.
* Use with neutrophils: Fay ME, Myers DR, Kumar A, Turbyfield CT, Byler R, Crawford K, Mannino RG, Laohapant A, Tyburski EA, Sakurai Y, Rosenbluth MJ, Switz NA, Sulchek TA, Graham MD, Lam WA. Cellular softening mediates leukocyte demargination and trafficking, thereby increasing clinical blood counts. Proc Natl Acad Sci U S A. 2016 Feb 23;113(8):1987-92. doi: 10.1073/pnas.1508920113. Epub 2016 Feb 8. PMID: 26858400; PMCID: PMC4776450.
* Use with red blood cells, sickle cell disease: Guruprasad P, Mannino RG, Caruso C, Zhang H, Josephson CD, Roback JD, Lam WA. Integrated automated particle tracking microfluidic enables high-throughput cell deformability cytometry for red cell disorders. Am J Hematol. 2019 Feb;94(2):189-199. doi: 10.1002/ajh.25345. Epub 2018 Nov 28. PMID: 30417938; PMCID: PMC7007699.
* Use with red blood cells, iron deficiency anemia: Caruso C, Fay ME, Cheng X, Liu AY, Park SI, Sulchek TA, Graham MD, Lam WA. Pathologic mechanobiological interactions between red blood cells and endothelial cells directly induce vasculopathy in iron deficiency anemia. iScience. 2022 Jun 15;25(7):104606. doi: 10.1016/j.isci.2022.104606. PMID: 35800766; PMCID: PMC9253485.
* Use with hematopoietic stem cells: Ni F, Yu WM, Wang X, Fay ME, Young KM, Qiu Y, Lam WA, Sulchek TA, Cheng T, Scadden DT, Qu CK. Ptpn21 Controls Hematopoietic Stem Cell Homeostasis and Biomechanics. Cell Stem Cell. 2019 Apr 4;24(4):608-620.e6. doi: 10.1016/j.stem.2019.02.009. Epub 2019 Mar 14. PMID: 30880025; PMCID: PMC6450721.
* *iCLOTS manuscript pending peer-reviewed publication contains additional data describing use with red blood cells, reticulocytes, and cancer cell lines.*



