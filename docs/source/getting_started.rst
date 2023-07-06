Getting started
=====

.. _iCLOTS basics:

General information
------------

iCLOTS software generates quantitative metrics from microscopy data obtained during use of a wide range of microfluidic and static assays.

Computational methods adapt to microscopy images and videomicroscopy of cells and cell solutions in a variety of common assay formats.

* The iCLOTS development team has demonstrated the use of these algorithms as applied to blood cells in our accepted *Nature Communications* manuscript "iCLOTS: open-source, artificial intelligence-enabled software for analyses of blood cells in microfluidic and microscopy-based assays" by Fay et al.  Experimental assays performed with blood cells are subject to unique requirements and constraints, including samples comprised of heterogenous cell types as well as frequent integration of fluid flow to better recapitulate physiologic conditions, and as such provide ideal test cases to demonstrate the adaptability of iCLOTS. iCLOTS has been extensively tested on all blood cell types (including red blood cells, white blood cells, platelets, and cell lines) and on various blood cell suspensions.
|
* The presented tools can be applied to other fields who share the same objective of tracking single cell or body features or conditions as a function of time. The development team has also tested iCLOTS adapted algorithms on additional cell types and on multicellular structures.
|
* iCLOTS can accomdate data obtained using a wide variety of experimental set ups and devices, including:

   * Standard microscopy slide or dish assays
   * Custom-made microfluidics
   * Commercially available microfluidics
   * Traditional flow chamber devices
|
* iCLOTS is a post-processing image analysis software. Users can continue to acquire imaging data using the methods they are accustomed to, and as such iCLOTS can be used with both previously collected data or new assays planned with the software's capabilities in mind.
|
* Image processing capabilities are separated into four main categories, with sub-applications for each:

   * Adhesion applications, useful for single-cell resolution measures of biological functionality.
   * Single cell tracking applications, useful for single-cell velocity measurements, typically as they flow through a microfluidic device. Video frames may contain multiple cells at once.
   * Velocity profile applications, useful for investigating rheological properties of suspensions under flow. Minimum, mean, and maximum velocity values for each video frame are also provided, suitable for monitoring changes in cell suspension speed.
   * Multiscale microfluidic accumulation applications, useful for insight into pathologic processes such as occlusion and obstruction in thrombosis.
* A suite of file pre-processing applications assists users with preparing their data for analysis. Type of file required is specific to each application and is provided in the documentation below.

Output files
----------------

Each application produces detailed output files specific to the application used.

* iCLOTS detects "events" in the imaging data provided by the user. Events typically represent individual cells or patterns of cells. Each event is labeled with a number/index on the original imaging files and the imaging files as processed by the image processing algorithms applied.
* Numerical output metrics are calculated for each event and are returned within an Excel sheet. For example, each cell "event" may be described by metrics like cell area or total fluorescence intensity of the cell.
* Metrics are given for every feature or cell (single-cell resolution) and can be traced back to the original image using the labeled index.
* iCLOTS may produce a large amount of data. In an effort to help researchers quickly make sense of their imaging dataset, iCLOTS automatically graphs results from the image processing analysis in common formats such as histograms or scatter plots.
|
* The development team would like users to keep in mind that computational analysis is never perfect - some spurious features are to be expected. Users might find these data points don't significantly affect their conclusions or may find that manually removing obvious outliers is less time consuming than performing the analysis by hand.
|
Should the user need further interpretation of their results, the produced Excel files can be used in the machine learning-based clustering application.

* Machine learning is a subset of artificial intelligence.
* Machine learning clustering algorithms are an unsupervised approach designed to detect and mathematically characterize natural groupings and patterns within complex datasets, e.g. healthy/clinical sample dichotomies or subpopulations from a single sample.
* iCLOTS implements k-means clustering algorithms, understood to be a strong general-purpose approach to clustering, where each data point is assigned a cluster label.

Interactive format
----------------

All iCLOTS applications follow a common, easy-to-use interactive format.

* Users follow a series of software menus to open a specific analysis window.
* All analysis windows are designed with the inputs on the left, the image processing steps as applied in the center, and the outputs on the right.
* The user uploads one or several microscopy images, time course microscopy series, or videomicroscopy files as inputs. These files automatically display on the screen and can be scrolled through using the scale beneath the files.
* Users may click on the scale or can use <Left> and <Right> keyboard keys to scroll through images or video frames.
|
Users are guided through a series of steps to describe their data.

* This could including choosing a region of interest (ROI) or indicating immunofluorescence staining color channels present.
* Users must then adjust parameters to fit the iCLOTS image processing algorithms to their specific set of data. Parameters are numerical factors that define how image processing algorithms should be applied. This could be a number such as minimum or maximum cell area. 
* Every effort has been made to ensure that parameters are intuitive. If the role of a parameter is unclear, please access the on-screen help documentation using the "Tutorial" button in the lower left-hand corner. Note that in iCLOTS, “a.u.” represents arbitrary units, typically used to describe pixel intensity values. 
* Effects of changing parameters are shown in real time.
* iCLOTS currently does not have a zoom function, but this is planned for a later release. In the meantime, if your data is relatively low-magnification, we suggest cropping a small region of interest using the video processing tools and testing parameters on that image, then applying the same parameters to the larger image.
|
The "Run analysis" button on the top right of the analysis screen initiates the finalized analysis using the parameters provided.

* Typically an analysis takes seconds-to-minutes - this depends heavily on file size and number.
* If analysis, particularly of video files, is taking more than 3-5 minutes, consider reducing the resolution or length of files using the video editing suite. 
* Graphical results are automatically displayed when the analysis is complete.
|
Output files include:

* Tabular data as an Excel file. In applications where several files are analyzed, individual sheets are named after individual files. These file names may be cropped to about 15 characters to prevent corrupting the output file. Please make sure individual files within a folder are named sufficiently differently.
* Graphical results as .png images
* The initial imaging dataset as transformed by the image processing algorithms and/or labeled with indices.
* Videos are returned as individual, sequentially numbered frames.

Experimental considerations
----------------

Users should consider practical experimental design concerns before use.

Choosing cell concentration:

* For all experiments involving quantification of single cell events, in our experimental and software testing we chose cell concentrations or hematocrits to ensure that we could operate within a quantifiable dynamic range of the microfluidic devices for both healthy or untreated controls and experimental samples. iCLOTS in its current iteration cannot distinguish between overlapping cell events. Typically we perform an initial experiment with a range of cell concentrations such that the most adhesive samples can adhere without overlap, then use this concentration for all future experiments.
|
Choosing brightfield illumination vs. fluorescence microscopy:

* Brightfield microscopy does not rely on any type of cell labeling. We're found some stains can affect cell membrane properties, i.e. R18 appears to damage the RBC membrane. In experiments where simple count or simple movement is quantified, brightfield microscopy is typically sufficient.
* Blood cells naturally have a heterogenous membrane appearance, which can affect area or other morphology measurements. To obtain the highest signal-to-noise ratio (e.g. the most apparent difference between image background and cell signal) we recommend staining cells or cell solutions with a stain indicating the cell membrane and using fluorescence microscopy. The fluorescence microscopy adhesion assay quantifies a secondary stain indicating some biological activity. Future version of iCLOTS will incorporate secondary "functional" quantification in additional applications.
|
Choosing constant perfusion vs. pressure-driven flow in microfluidic experiments:

* iCLOTS has been shown to produce accurate, reliable analyses of both constant perfusion (syringe pump) and pressure-driven flow across a range of microfluidic, flow-based experiments. While pressure-driven flow is more physiologically relevant, users may find they are limited by equipment availability or small sample sizes, or experimental set up may necessitate the greater simplicity or ease-of-use of constant perfusion systems. Users should carefully consider the importance of physiological relevance in their assays. If constant perfusion is used, consider designing microfluidic devices with large bypass channels to prevent significant changes in pressure from channel clogging.
* Over the course of long microfluidic experiments, factors such as a build up of adhesive factors on channel walls, cell suspension settling, or other variables may lead to artifacts within data. The iCLOTS team suggests plotting quantitative metrics with frame number as the x-variable to ensure results are reasonably consistent over time.
|
Users may always access the application-specific documentation available here using the "Tutorial" button in the bottom left of the analysis window.
