Video editing tools
==========================================

| Each iCLOTS application requires a specific type of file to analyze, whether that's an image, series of images, or a video. Some videos or images may not be suitable for analysis as-is and could benefit from additional image manipulation. iCLOTS provides a suite of video and image file editing tools to help users format their data for iCLOTS analysis. Briefly, users select a single file or a folder of .png, .jpg, .tif, and/or .avi files for modification. Users may need to perform some operation or edit indicated parameters, some image processing step is applied, and all edited files are returned in a new directory within the original folder. 

.. _choose roi:

Choose a region of interest (ROI)
------------------------------------

This application is designed to crop file(s) to a region of interest. After file(s) upload, a window displaying the file will appear, and a draggable rectangle allows the user to select an area they are interested in analyzing further. In assays using microfluidics, small defects in microfluidic walls are often detected as cells or patterns of cells. The iCLOTS team suggests cropping all data taken using microfluidic systems to the channel area only.

.. _crop frames:

Crop a video to a specified frame range
-------------------------------------------

This application is designed to shorten a video to a specifed start:finish frame range. Input variables include the start frame number and the end frame number. If multiple files are selected, all will be cropped to the same range. This script is most useful for removing portions of a video clearly affected by changes in microscopy acquisition settings such as changes in illumination or laser power, or for shortening videos to reduce computational analysis time. To start and end at roughly a given time, multiply that time (in seconds) by the frames per second (FPS) imaging rate, a microscope acquisition setting you should be able to reference.

.. _edit contrast:

Edit the contrast of files
-------------------------------

This application edits the contrast of file(s) using a two point process: multiplication followed by addition with a constant. Input variables include alpha, the contast each pixel's intensity value is multiplied by, and beta, the constant added (or subtracted) from each pixel's intensity value. Alpha is oftentimes called gain, and should be >1. This value controls contrast. High values of alpha cause the relatively bright pixels to become even brighter. Any value of alpha leaves black pixels as black (value 0). Beta is oftentimes called bias. <0 decreases the overall brightness of the file, and >0 increases the overall brightness of the image. Editing contrast can be useful in applications detecting movement. Features of interest, like a cell, and more easily distinguished from background, like channels. Take care in interpreting pixel intensity values after editing contrast - it may lead to bias in fluorescence-based results. It can be hard to detect a strong signal from fluorescently stained, moving cells. If you adjust the contrast of those videos to better detect the cells, return to the original fl. int. values by performing the inverse calculation:

| Fl. int, original = (Fl. int., calculated - beta) / alpha

.. _img to vid:

Convert an image sequence to a video
----------------------------------------

This application may be useful depending on the timecourse outputs of your microscopy software. Single cell tracking, specialized deformability, and velocity applications require an .avi video input. One single video is made from all images within the selected file folder. All images must have the same dimensions. Images must be named in the proper alphabetical/numerical order. If image names contain numbers, use preceding zeros to order properly, i.e. 01, 02, ... 10 vs. 1, 2, ... 10. The video uses the file folder name as a filename - please avoid spaces or punctuation within this name. Inputs include a frames per second rate, the rate at which you would like your created video to play. This parameter does not affect later analysis. 

.. _normalize:

Normalize range of pixel intensity values
-------------------------------------------

All images are scaled such that the lowest pixel value is 0 (black) and the highest pixel value is 255 (white). This can be useful for standardizing images taken during different experiments, etc. This application is for use with image files only. All channels (red, green, blue) are normalized to the same range. Later iterations of iCLOTS will include options for normalizing all frames of a video file and for normalizing certain color channels only. Use caution normalizing images to the same range. Normalizing can remove bias that comes from different laser power, gain, etc. settings, but can also introduce bias. It's almost always ideal to compare images taken during the same experiment.  Ideally the initial image pixel values are within a (0, 254) range. "Maxed out" pixel values (255, the highest possible value) cause loss of information - a range of intensities may have existed beyond the 255 value. This application uses the following method of normalization:

| new value = (original layer) - minimum value of the layer / (maximum value of the layer - minimum value of the layer) * 255

.. _resize:
  
Resize file(s)
------------------

This application resizes image(s) or video(s) using a resize factor, a constant that a frame's dimensions are multiplied by during the resize process. For input parameter resize factor, <1 indicates reducing resolution, >1 indicates "increasing" resolution. Decreasing resolution can help speed computational analysis of large files. In applications quantifying movement of cells, sometimes lower resolution is sufficient.  In applications quantifying changes in morphology or size, maintain the highest resolution possible.

| Artificially increasing resolution in post-processing oftentimes isn't useful. It is not possible to add information that the microscopy did not provide. In may lead to bias in morphological results by increasing changes in dimension.

.. _rotate:

Rotate file(s)
----------------

This application rotates image(s) or video(s) using a specified angle input parameter. All files are rotated to the same degree. 
Angle value units are degrees. An angle value >0 rotates the file counterclockwise and an angle value <0 rotates clockwise. This application is specially designed to maintain the original aspect ratio of the file such that the same µm-to-pixel ratio can be used during analysis. Rotating videos or images such that microfluidic channels are horizontal is crucial for applications that rely on left-right indexing, such as microchannel occlusion or velocity profiles. It is suggested for one-directional movement quantification, such as the deformability application. Rotating images has no affect on morphology measurements.

.. _vid to img:

Convert a video to an image sequence
----------------------------------------

Application converts a single video to a sequence of images. This script is useful for .avi files that must be presented to iCLOTS as a time series, like the accumulation/occlusion applications. Images are returned named with the video name plus a frame number. Up to five preceding zeros are used to number the frames sequentially. This will not work on files >99,999 frames, but iCLOTS cannot realistically handle that many images anyways. Images are saved as .png files to avoid unnecessary compression.
