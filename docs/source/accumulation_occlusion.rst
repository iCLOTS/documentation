Multi-scale accumulation and occlusion
==========================================

| Our accumulation- and occlusion-based image processing applications are designed to be multi-scale to fit a variety of researcher's needs. Accumulation and occlusion, the aggregation or adhesion of cells/biomolecules on biological substrates and/or microvessels, has important implications for many diseases. Here, up to three fluorescence microscopy selected image  color channels (red, green, and/or blue) from a single image or a time series image sequence are binarized using user-defined thresholds. iCLOTS’ suite of multiscale microfluidic accumulation applications allows users to investigate occlusion on surfaces (a region of interest), in potentially complicated microfluidic vessel or channel geometries, or in small microvessels. 

.. _roi:

Region of interest image processing
-------------------------------------------

| This scale is used to analyze accumulation and occlusion as indicated by fluorescence microscopy signal in any square region of interest selected by the user. The iCLOTS manuscript demonstrates use of this application with a small region from a commercially-available ibidi device.

Input files:

* This application is designed to work with a single image or a folder of images describing timeseries data (.jpg, .png, and/or .tif).
* The same input parameters are applied to each image.
* After uploading one or several images, the user is prompted to choose an ROI from the first image.
* The same ROI is applied to all images, take care that all images represent the same field of view.
* It's important that frames are labeled sequentially in timeseries order.

Input parameters:

* µm-to-pixel ratio: The ratio of microns (1e-6 m) to pixels for the image. Use value = 1 for no conversion.
* Red, green, and/or blue threshold(s). Users may select which color channel(s) they would like to analyze. After channel selection, a spinbox to choose a threshold for each channel appears.

Output files:

* All files are saved in a new folder titled "Results," located within the folder the original imaging data was selected from. Time and date are included in the folder name to identify when the analysis was performed and distinguish between different analyses.
* Several images are returned: the ROI from each frame and all frames with image processing (threshold) steps applied.
* A corresponding .xlsx sheet for each selected color channel containing mean occlusion (%) and accumulation (area) for each frame. To convert accumulation per frame into per timepoint, divide frame number by FPS imaging rate.
* Occlusion/accumulation graph: for the time series, a line graph showing occlusion (titled, left) and accumulation (titled, right) for each color.

Some tips from the iCLOTS team:

* Computational and experimental methods:

  * See input requirements: a time series, in the same field of view.
  * We are planning a coupled brightfield/fluorescence microscopy application for future iCLOTS releases.
  * Time series images must be in the proper alphabetical/numerical order. If image names contain numbers, use preceding zeros to order properly, i.e. 01, 02... 10 instead of 1, 2... 10.

* Choosing parameters:

  * Be sure to use µm-to-pixel ratio, not pixel-to-µm ratio.
  * If you indicate more than one color channel, you might find the colors overlap in the analysis window, and you can't accurately see parameters as set. Individually choose each color, select a good threshold, and then combine with the thresholds you chose.
  * Depending on the threshold you set, while the "trend" of accumulation/occlusion should stay constant, but the degree of accumulation/occlusion will decrease as threshold increases.
  * If you are comparing conditions, make sure they were taken with the same imaging settings and use the same threshold values. Ideally these experiments are direct control-to-experimental comparisons taken on the same day.

* Output files:

  * Analysis files are named after the folder containing all images (.xlsx) or image names (.png). Avoid spaces, punctuation, etc. within file names.

Learn more about the methods forming the basis of our multiscale microfluidic accumulation applications:

* Region analysis via python library scikit-image: 

  * Relevant citation: van der Walt S, Schönberger JL, Nunez-Iglesias J, et al. scikit-image: image processing in Python. PeerJ. 2014;2:e453. 
  * `Documentation/tutorial for region analysis <https://scikit-image.org/docs/stable/auto_examples/segmentation/plot_regionprops.html>`_

.. _device:

Image processing for a microfluidic device with complex geometry
-------------------------------------------------------------------

| This scale of the accumulation and occlusion application is used to analyze accumulation and occlusion as indicated by fluorescence microscopy signal in a microfluidic device with complex geometry. Only the region of device indicated by a channel stain or the summed signal from a time course is quantified. The iCLOTS manuscript demonstrates use of this application with a branching microfluidic device.

Input files:

* This application is designed to work with a single image or a folder of images describing timeseries data (.jpg, .png, and/or .tif).
* The same input parameters are applied to each image.
* After uploading one or several images, the user is prompted to choose an ROI from the first image.
* The same ROI is applied to all images, take care that all images represent the same field of view.
* It's important that frames are labeled sequentially in timeseries order.

Input parameters:

* µm-to-pixel ratio: The ratio of microns (1e-6 m) to pixels for the image. Use value = 1 for no conversion.
* Red, green, and/or blue threshold(s). Users may select which color channel(s) they would like to analyze. After channel selection, a spinbox to choose a threshold for each channel appears. The "map" comprising the area of the microfluidic device is created from the sum of all signal above threshold.

Output files:

* All files are saved in a new folder titled "Results," located within the folder the original imaging data was selected from. Time and date are included in the folder name to identify when the analysis was performed and distinguish between different analyses.
* Images include the selected ROI from each frame, the "map" used - the channel region(s) as detected, and all frames with image processing (threshold) steps applied. Images also include each selected color as detected by the set threshold overlaid on the channel map (white color) for each color channel.
* A corresponding .xlsx sheet containing mean occlusion (%) and accumulation (area) per frame for each color channel selected. To convert accumulation per frame into per timepoint, divide frame number by FPS imaging rate.
* An occlusion and accumulation graph: for the time series, a line graph showing occlusion (titled, left) and accumulation (titled, right) for each selected color channel.

Some tips from the iCLOTS team:

* Computational and experimental methods:

  * See input requirements: a time series, in the same field of view, with "complete" microfluidic channel signal.
  * Creating the map requires some signal at every point in that channel. Consider staining the microfluidic channels - if this isn't possible, you may benefit from the region of interest-scale accumulation application.
  * We are planning a coupled brightfield/fluorescence microscopy application for future iCLOTS releases. This would not require some bright, single-color signal at every height point in the channel.
  * Time series images must be in the proper alphabetical/numerical order. If image names contain numbers, use preceding zeros to order properly, i.e. 01, 02... 10 instead of 1, 2... 10.
  * The Lam lab has developed these methods on an "endothelialized" branching microfluidic device. See "Endothelialized Microfluidics for Studying Microvascular Interactions in Hematologic Diseases" manuscript by Myers and Sakurai et al., 2012, JOVE. We are happy to share a detailed endothelialization protocol upon request. We are happy to share the microfluidic mask design files and instructions for fabrication upon request.

* Choosing parameters:

  * Be sure to use µm-to-pixel ratio, not pixel-to-µm ratio.
  * If you indicate more than one color channel, you might find the colors overlap in the analysis window, and you can't accurately see parameters as set. Individually choose each color, select a good threshold, and then combine with the thresholds you chose.
  * Depending on the threshold you set, while the "trend" of accumulation/occlusion should stay constant, but the degree of accumulation/occlusion will decrease as threshold increases.
  * If you are comparing conditions, make sure they were taken with the same imaging settings and use the same threshold values. Ideally these experiments are direct control-to-experimental comparisons taken on the same day.

* Output files:

  * Analysis files are named after the folder containing all images (.xlsx) or image names (.png). Avoid spaces, punctuation, etc. within file names.

Learn more about the methods forming the basis of our multiscale microfluidic accumulation applications:

* Region analysis via python library scikit-image: 

  * Relevant citation: van der Walt S, Schönberger JL, Nunez-Iglesias J, et al. scikit-image: image processing in Python. PeerJ. 2014;2:e453. 
  * `Documentation/tutorial for region analysis (also above) <https://scikit-image.org/docs/stable/auto_examples/segmentation/plot_regionprops.html>`_

* Learn more about endothelialized microfluidic devices:

  * Myers DR, Sakurai Y, Tran R, et al. Endothelialized microfluidics for studying microvascular interactions in hematologic diseases. J Vis Exp. 2012(64). 

.. _microchannel:

Image processing for a microfluidic microchannels
-------------------------------------------------------

This scale of the accumulation and occlusion application is used to analyze accumulation and occlusion as indicated by fluorescence microscopy signal in a series of straight microchannel(s) within some larger device. This sub-application provides spatial information on where cells have occluded a channel. Individual channels as indicated by a channel stain or left-right extension of the summed signal from a frame are quantified. The iCLOTS manuscript demonstrates use of this application with a set of 32 of the smallest channels within a branching microfluidic device. 

Input files:

* This application is designed to work with a single image or a folder of images describing timeseries data (.jpg, .png, and/or .tif).
* The same input parameters are applied to each image.
* Each image should consist of one or many straight portions of a microfluidic device.
* After uploading one or several images, the user is prompted to choose an ROI from the first image. This ROI should contain the straight channel portions. The same ROI is applied to all images, take care that all images represent the same field of view. The algorithm relies on left-to-right indexing to form the channel regions to analyze. As such, channels should be perfectly horizontal. iCLOTS provides a video-editing rotation tool that does not affect aspect ratio. In order to create a complete channel area to analyze, some fluorescence signal must be present at every y pixel of the channel. Staining the channels, or some feature of the channel, like a cell layer, helps with this.

Input parameters:

* µm-to-pixel ratio: The ratio of microns (1e-6 m) to pixels for the image. Use value = 1 for no conversion.
* Red, green, and/or blue threshold(s). Users may select which color channel(s) they would like to analyze. After channel selection, a spinbox to choose a threshold for each channel appears.

Output files:

* Region of signal is calculated with single pixel resolution. Region of signal may not represent single cells.
* All files are saved in a new folder titled "Results," located within the folder the original imaging data was selected from. Time and date are included in the folder name to identify when the analysis was performed and distinguish between different analyses.
* Images include the ROI from each frame, the "map" used (the channel region(s) as detected), frames with image processing (threshold) steps applied, and, for each frame, each selected color as detected by the set threshold overlaid on the channel map (white color).
* A corresponding .xlsx sheet containing, for each selected channel:

  * Raw data: A percent y-occlusion for very frame, channel, x-position within the channel. Obstruction, or percent y-occlusion, indicates what percentage of the height of the microchannel contains signal.
  * Per-channel data: Occlusion (area of signal), accumulation (pixels, µm²) and obstruction (%y occlusion) for each channel in each frame.
  * Per-frame data: mean occlusion, accumulation, and obstruction per frame (all channels)
  * *To convert accumulation per frame into per timepoint, divide frame number by FPS imaging rate. To convert x-pixel coordinate to a measurement, multiply by µm-to-pixel ratio.*

* An occlusion/accumulation graph for the time series, showing: occlusion (titled, left) and accumulation (titled, right) for each channel (light lines) and mean (dark lines) for each color.

Some tips from the iCLOTS team:

* Computational and experimental methods:

  * See input requirements: a time series, in the same field of view, with "complete" y-height horizontal channels. The left-to-right indexing to form the channels requires some signal at every height point in that channel. Consider staining the microfluidic channels.
  * We are planning a coupled brightfield/fluorescence microscopy application for future iCLOTS releases. This would not require some bright, single-color signal at every height point in the channel.
  * Time series images must be in the proper alphabetical/numerical order. If image names contain numbers, use preceding zeros to order properly, i.e. 01, 02... 10 instead of 1, 2... 10.
  * The Lam lab has developed these methods on an "endothelialized" branching microfluidic device. See "Endothelialized Microfluidics for Studying Microvascular Interactions in Hematologic Diseases" manuscript by Myers and Sakurai et al., 2012, JOVE. We are happy to share a detailed endothelialization protocol upon request. We are happy to share the mask design files and instructions for fabrication upon request.

* Choosing parameters:

  * Be sure to use µm-to-pixel ratio, not pixel-to-µm ratio.
  * Depending on the threshold you set, while the "trend" of accumulation/occlusion should stay constant, but the degree of accumulation/occlusion will decrease as threshold increases. 
  * If you are comparing conditions, make sure they were taken with the same imaging settings and use the same threshold values. Ideally these experiments are direct control-to-experimental comparisons taken on the same day.

* Output files:

  * Analysis files are named after the folder containing all images (.xlsx) or image names (.png). Avoid spaces, punctuation, etc. within file names.

Learn more about the methods forming the basis of our multiscale microfluidic accumulation applications:

* Region analysis via python library scikit-image: 

  * Relevant citation: van der Walt S, Schönberger JL, Nunez-Iglesias J, et al. scikit-image: image processing in Python. PeerJ. 2014;2:e453. 
  * `Documentation/tutorial for region analysis (also above, twice) <https://scikit-image.org/docs/stable/auto_examples/segmentation/plot_regionprops.html>`_

* Learn more about endothelialized microfluidic devices:

  * Myers DR, Sakurai Y, Tran R, et al. Endothelialized microfluidics for studying microvascular interactions in hematologic diseases. J Vis Exp. 2012(64). 
