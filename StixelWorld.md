# Paper
* **Title**: The Stixel World: A Medium-Level Representation of Traffic scenes
* **Author**: Marius Cordts, Timo Rehfeld, Lukas Schneider, David Pfeiffer, Markus Enzweiler, Stefan Roth, Marc Pallefeys, Uwe Franke
* **Link**: https://doi.org/10.1016/j.imavis.2017.01.009
* **Tags**: Stereo-vision, Automotive vision, Intelligent vehicles, Scene understanding
* **Year**: 2017

# Summary
* What
     * Stixel World is a Medium Level represention of the data image representing an object as sticks like column-wise segmentation of image
     * Advantages:
        * It can be used for multiple applications like object detection, tracking, segmentation and mapping without needing to deal with the raw input each time
        * Compressed version of the original image (neither too generic nor to specific)
        * Using stixels as primitive element either decreases parsing time, increases accuracy or both
        * Less memory consumption since stixels being larger, but effective, version of image representation in contrast to the pixels
      
* How
    * The authors extend the existing Stixel work in the below aspects:
      * describe the Stixel model using [CRF](https://homepages.inf.ed.ac.uk/csutton/publications/crftut-fnt.pdf)-based     formulation.
      * introduction of additional factors in graphical model 
      * to learn all free parameters of the model using a structured support vector machine (S-SVM)
      * finally, they provide evaluation to analyze the influences of various components
    * Stixel world can be considered as a union of subsets of the properties of the following techniques:
      * road scene model - Gives eagle’s view of the street scene and scans the grids to identify the
        grids with occupant of interest
      * unsupervised bottom-up segmentation - partition on the basis on coherent color or texture
      * semantic segmentation - 2D appearance signals 3D depth information.
    * Segmentation problem of *w x h* image is broken down to *w/ws* individual segmentation problems.(*ws >= 1 and is fixed*)
    * ws is fixed to limit the number of pixels and chosen in advance to reduce computational complexity
    * The vertical extent of each Stixel is inferred in the model. Optional downscaling in the vertical direction to control the runtime. (*hs >= 1 and variable*)
    * Set labeling : 
      * Three core structural classes
          * Support : these Stixels are parallel to the ground plane at a constant height
          * Vertical : these Stixels are perpendicular to the ground plane at a constant distance
          * Sky : these Stixels are perpendicular to the ground plane at a infinite distance
      * Semantic classes
          * the core structural classes are further refined into semantic classes depending upon the application
    * Energy function
      * Describes an efficient energy minimization procedure called *Dynamic Programming* (DP)

* Results and impact of input signals:
   * Stixel model is able to retain 94% of the disparity and 85% of semantic accuracy with base parameterization
    * for smart down-sampling at equal compression rate it is 91.8% and 76.7% respectively
    * Model is evaluated considering single input modulity (amongst three) as well as with the combination of all
    * Results shows that adding color or semantic labels helps to improve depth accuracy
    * while adding color or depth improves semantic accuracy
    * color has minimal effect on the gain
    * observation tells that increased accuracy acompanies with increase in number of stixels
    * Cityscapes which has finer and more precise ground truths shows larger gap (54.1% as compared to 60.1%) 

# Rough Chapter-wise notes

* Introduction
  * Recent advancement in the vision technology has led to challenges in terms of processing power, energy consuption, packaging space and bandwidth.
  * Main concern is sharing computer resources
  * Stixel aims at representing the image at mid-level which can be used as a primitive element for multiple applications
  * Prior work: 
    * Limits the street structure in the vertical image direction
    * Top - sky, above supporting ground - objects. 
    * Less pronounced in horizontal direction
  * Proposed work:
    * Stixels - stick like column-wise image representation (like superpixels)
    * Solves enery minimization problem
    * Can be used for other applications as well
  * Extension of prior work:
    * building Stixel model using CRF-based formulation which is compliant with all previous stixel works
    * introduction of new factors that represent additional input modelities like color and pixel-level semantic labels
    * loss based training using structured support vector machine (S-SVM) to learn free parameters
 
* Related work
  * Stixel world is the union of subset of the properties of the following technologies:
    * Road scene models – Gives eagle’s view of the street scene and scans the grids to identify the
      grids with occupant of interest
    * Unsupervised bottom-up segmentation – partition on the basis on coherent color or texture
    * Semantic segmentation – 2D appearance signals 3D depth information

* Stixel Model
  * It is segmentation of an image into a thin stick like column-wise segments with class labels and depth model
    * Breaking down of w x h image
      * w/ws individual 1D segments (ws >= 1 and duce the computational complexity of the model refixed) 
      * ws is chosen in advance to rduce the computational complexity of the model
      * optional downscaling in the vertical direction further enables controlling the runtime
  * Stixel model is a medium-level representation. how?
    * each stixel provides an abstract representation of depth, physical extent and semantics more
      expressive than a pixel
    * Based on street scene model instead of bottom-up super-pixel methods
    * Individual objects represented by multiple stixels
  * Set labeling : 
    * Three core structural classes
        * Support : these Stixels are parallel to the ground plane at a constant height
        * Vertical : these Stixels are perpendicular to the ground plane at a constant distance
        * Sky : these Stixels are perpendicular to the ground plane at a infinite distance
    * Semantic classes
        * the core structural classes are further refined into semantic classes depending upon the application
  * Input Data is comprised of the following:
    * dense depth map
    * a color image
    * pixel-level label scores for each class
  * efficient energy minimization procedure - dynamic programming
  * model parameters learning - structural support vector machines (S-SVMs)
    
* Graphical Method
  * Explains the mathematical approch to minimize the energy function

* Inference
  * Dynamic programming
  * Algorithmic specifications
  * Approximation
  
* Parameter Learning
    

* Experiments
  * Datasets
    * KITTI: 
      * dense semantic labels and depth ground truth
      * 60 images, res - 0.5 MP each
      * entirely used for evaluation
    * KITTI'15: stereo challenge 
      * sparse disparity ground truth (no semantic ground truth) from velodyne HDL-64 laser scanner
      * 200 images
      * used for training
    * Cityscapes: 
      * dense annotations, stereo views available but ground truth disparities do not exists
      * 19 classes on 2975 training images and 500 images for validation
  * Metrics
    * Depth accuracy : the percentage of the disparity estimates that ars=e considered inliers
      * inlier: A disparity estimation with an absolute deviation of <= 3px and relative deviation of less than 5% when compared          with ground truth
    * Semantic performance : average Intersection-over-Union (IoU) over all classes
    * Number of stixels per image: measure of the complexity of the resulting representation
  * Benchmarking platform:
    * runtime is obtained as follows:
      * FPGA for SGM (Semi-global matching)
      * a GPU (NVIDIA Titan X) for the FCN
      * a CPU (Intel Xeon, 10 cores, 3GHz) for Stixel segmentation
  * Baseline:
    * pixel-level semantic input to differentiate three depth models analogus to structural classes
      * Grount pixels: the flat ground hypothesis is assigned with mean deviation
      * Verticle obstacles: the flat ground hypothesis is assigned with mean disparity
      * Sky: the flat ground hypothesis is assigned with zero disparity
   * Stixel Model:
    * Stixel parameters that influence the resulting number of Stixels and the computational time are:
      * the horizontal extent (downsampling factor) ws - controls discreatization
      * the vertical downscaling hs - quadratic influence on execution time
      * model complexity term
    * sweeping hs has the strongest influence on depth and semantic accuracy
    * ws and complexity has stronger influence on the number of Stixels
   * Results:
    * Stixel model is able to retain 94% of the disparity and 85% of semantic accuracy with base parameterization
    * for smart down-sampling at equal compression rate it is 91.8% and 76.7% respectively
   * Impact of Input cues:
    * Model is evaluated considering single input modulity (amongst three) as well as with the combination of all
    * Results shows that adding color or semantic labels helps to improve depth accuracy
    * while adding color or depth improves semantic accuracy
    * color has minimal effect on the gain
    * observation tells that increased accuracy acompanies with increase in number of stixels
    * Cityscapes which has finer and more precise ground truths shows larger gap (54.1% as compared to 60.1%) 
    
* Discussion and Conclusion
  * Presented successful medium-level representation witha  particular focus on automotive vision
  * Can be used as a primitive element for different applications
  * Model proves to be compact
  * Able to retain 94% disparity and 84% semantic accuracy
* Issues and future work:
  * with avancement in the vision technology, 3D points in real-world applications tend to increase to several millions
    * Stexil model can be used as a mid-level represtation of data to reduce the processing time on the data
  * finding the right set of parameters suitable for all application is a difficult task
    * automated parameter learning via structured support vector machine models-> limited success

* Glossary
  * Disparity: Disparity refers to the distance between two corresponding points in the left and right image of a stereo pair
  * Input cues: input signals
