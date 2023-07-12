Velocity application
====================================

| This application tracks detected features (typically patterns of cells) and their displacement using adapted Shi-Tomasi corner detection and Kanade-Lucas-Tomasi feature tracking methods. This application is ideal for suspensions of cells where cells are significantly overlapping. Cell suspensions can travel in any direction(s). iCLOTS provides velocity (displacement/time) for each tracked feature. Minimum, mean, and maximum velocity values within a frame are reported for each frame. A velocity profile is generated based on a user-specified bin number from all events. Typically this application would be used to track cells transiting a microfluidic device, but other uses may be possible.

| This video has been extensively tested on brightfield videomicroscopy. Usage with fluorescently labeled features is likely also possible.

.. _velocity:

Quantification of velocity timecourse and profile
------------------------------------------------------

Input files:

* This application is designed to analyze a single video (.avi). If your data is saved as a series of frames, please see the suite of video editing tools to convert to .avi.
* The same input parameters are applied to every frame.
* The application will display the video in the center of the analysis window - users can scroll through frames using the scale bar below.
* Users are lead to choose a region of interest from the video for analysis. Small defects in device walls or minor changes in microscopy illumination can present as features to be detected and tracked. Please choose only the channel area, excluding walls or other background. Current region of interest tools provide methods for choosing a rectangular shape. Later versions of iCLOTS will allow for customizable ROIs of varying geometries.

Parameters to interactively adjust:

* Video descriptors: 

  * µm-to-pixel ratio: The ratio of microns (1e-6 m) to pixels for the image, used to convert pixel measurements into area or distance dimensions. Use 1 for no conversion.
  * Frames per second (FPS): The rate  of imaging, a microscopy parameter. Note that FPS values pulled  directly from videos can be inaccurate, especially if the video has been resized or edited in any way.
  * Number of bins: The number of bins to divide the channel into to create a profile. The developers suggest roughly the size of a cell is best - e.g. a 5 µm bin size was used for the iCLOTS manuscript.

Image labeling settings: The developers suggest exporting at least some frames with trajectories labeled to ensure that the analysis is working as intended. As videomicroscopy of videos taken with velocity measurements in mind can have a large number of frames, the team has provided two export options:

* Export first 100 frames.
* Export one frame every 100 frames.
* Users can select either/or/neither using a series of checkboxes.

Shi-Tomasi corner finding:

* Block size (pixels): A block size to compute eigenvalues to pick optimal features is required. For best results, the developers find that roughly the size of a cell (in pixels) works.
* The developers have set the maximum number of features that may be detected in a frame as 500, with a high required "quality level," to ensure the most representative data points are used. All features must be a minimum of two pixels apart. For users with coding experience, methods are available as scripts that can be further customized at github.com/LamLabEmory.

Kanade-Lucas-Tomasi feature tracking:

* Window size: The most important parameter to select for this application! This is the area (in pixels) to search for a feature in the subsequent frame. Err on the high side, as values too small can miss the fastest moving features. If the created profiles look unusually blunted, your window size is likely too small. An x-range and a y-range are set independently. If flow is unidirectional in the x-direction, you could set a low y-range to reduce computational expense, or vice versa. Keep in mind, even if flow is unidirectional, the algorithm will search for features in every direction, e.g., if you're sure cells travel no more than 10 pixels to the right in the subsequent frame, you would set your x-direction window as 20 pixels, to account for the algorithm treating both sides as potential next locations. 
* The developers have set the possible number of iterations of the search such that only one subsequent frame is checked for a previously detected feature (1 iteration). The minimum distance a feature must travel is 1 pixel. For users with coding experience, methods are available as scripts that can be further customized at github.com/LamLabEmory.

Output metrics:

* Resolution: single-feature (e.g. single pattern of cells)
* Metric: velocity (µm/s)

Output files:

* All files are saved in a new folder titled "Results," located within the folder the original imaging data was selected from. Time and date are included in the folder name to identify when the analysis was performed and distinguish between different analyses.
* Labeled imaging data (optional): depending on settings selected, the first 100 frames and/or every 100th frame, labeled with displacement of features from the subsequent frame. While exporting the labeled frames takes extra time, the developers suggest doing so anyways. It will be useful for troubleshooting outliers, etc.
* A comma separated value (.csv) sheet containing the initial position and velocity of every feature tracked. This may be up to 500 features are tracked per frame, for possibly several thousand frames,  so these files tend to be large.  They are most suitable for further computational analysis.
* An Excel file containing:

  * Minimum, mean, and maximum velocity values per frame.
  * Minimum, mean, maximum, and standard deviation of velocity values for the entire video.
  * Profile information: mean velocity and standard deviation (separate sheets) for all values within the video.
  * Parameters used and time/date analysis was performed, for reference.

Graphical data:

* Time course graph containing minimum (blue), mean (green), and maximum (red) velocity values for each frame.
* Profile graph with the position and velocity of each feature tracked as single points, overlaid with a line representing the profile values.

Some tips from the iCLOTS team:

* Computational and experimental methods:

  * Tracking cell suspension velocity accurately is highly dependent on the quality of data. All data presented in iCLOTS was taken at a frame rate of at least 160 frames per second. You may need a high-speed camera, depending on experimental goals. The Lam lab can provide recommendations for equipment upon request.
  * Calculating an accurate velocity profile relies on automatically-calculated linearly-spaced bins spanning the height of the channel. While cropping to a region of interest excluding channel walls is not required, for best profile results the team suggests doing so to avoid wall velocities that appear artificially slower.
  * The channel size used in the experimental data presented in iCLOTS was artificially shallow at ~15 µm tall. Deeper channels are hard to detect distinct features from.
  * The channel width used in the experimental data presented in iCLOTS was 70 µm, which we found to be ideal for calculating a velocity profile. At smaller widths, a "clean" profile may be hard to calculate. If channels are so small individual cells appear distinct, please try our single cell tracking methods.
  * Features are typically patterns of cells rather than a single individual cell. As such, no label index is provided. Exported frames are labeled with trajectories only.
  * Files taken at a high FPS rate can be quite large. To reduce computational expense, you may be able to resize the video to a smaller resolution using the suite of video editing tools available in iCLOTS. Typically you do not need a very long region of interest to calculate an accurate velocity profile. Try selecting only a short portion of the channel. If you are not looking for changes in velocity over time, use relatively short video clips. The developers have found that 10 seconds at a high FPS is oftentimes sufficient to establish mean values and a profile. You can shorten longer videos using the suite of video editing tools.

* Output files:

  * Analysis files are named after the folder containing all images (.xlsx) or image names (.png).
  * Avoid spaces, punctuation, etc. within file names.
  * Excel and pairplot data includes a sheet/graph with all images combined. Only use this when analyzing replicates of the same sample.

Learn more about the methods forming the basis of our velocity profile application:

* Shi-Tomasi corner detection, used for finding features to track: 

  * Shi J, Tomasi C. Good Features to Track. Proceedings / CVPR, IEEE Computer Society Conference on Computer Vision and Pattern Recognition IEEE Computer Society Conference on Computer Vision and Pattern Recognition. 2000;600. 

* Kanade-Lucas-Tomasi feature displacement algorithms, used for calculating velocity measurements:

  * Lucas B, Kanade T. An Iterative Image Registration Technique with an Application to Stereo Vision (IJCAI). Vol. 81; 1981. 

* Python library Open-CV, used to implement these algorithms:

  * Initial description: Bradski G. The OpenCV Library. Dr Dobb’s Journal of Software Tools 2000. 2000.
  * `Documentation/tutorial for optical flow methods <https://docs.opencv.org/3.4/d4/dee/tutorial_optical_flow.html>`_
