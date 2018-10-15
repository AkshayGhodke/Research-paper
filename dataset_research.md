# Datasets

* ImageNet 2012
* coco
* KITTI
* Open Images V4

# Open Images V4

  * Data Organization
    * Image-level labels
         * Classification:

           |      | Train |	Validation |	Test |	# Classes 	| # Trainable Classes |
           |---        |---|---|---|---|----|
           | Images                   |	9,011,219| 	41,620 |	125,436| 	- |	- |
           | Machine-Generated Labels |	78,977,695 |	512,093 |	1,545,835 |	7,870 |	4,764 |
           | Human-Verified Labels |	27,894,289|551,390 |1,667,399 | 19794 | 7186|

         * [19995](https://storage.googleapis.com/openimages/2018_04/classes.txt) distinct classes
         * Classes are qualified to be trainable if it has 100+ positive human-verified set
         * [7186](https://storage.googleapis.com/openimages/2018_04/classes-trainable.txt) trainable classes

    * Bounding Boxes
         * Classification:

           || Train |	Validation |	Test |	# Classes |	
           |---                       |---|---|---|---|
           | Images |	1,743,042 |	41,620 |	125,436 |		- |	
           | Boxes |	14,610,229 |	204,621 |	625,282 |	600 |

         * 1.74 M - human-verified  image-level labels
         * Focused on specific lables -> Eg., image labels {car, limousine, screwdriver} annotated as {limousine, screwdriver}
         * On an Avg., 8.4 boxed objects per image
         * For [human body parts and the clas "Mammal"](https://storage.googleapis.com/openimages/2018_04/class-ids-human-body-parts-and-mammal.txt), bounding box for only [95335](https://storage.googleapis.com/openimages/2018_04/train/train-image-ids-with-human-parts-and-mammal-boxes.txt) images have been drawn (1327596 boxes on 95335 images)
         * [Semantic hierarchy](https://storage.googleapis.com/openimages/2018_04/bbox_labels_600_hierarchy_visualizer/circle.html) for validation and test datasets
         * In all splits (train, val, test), annotators also marked a set of attributes for each box, e.g. indicating whether that object is occluded
         
  * Data Formats
    * Boxes (csv):
         * Example,
         
           |ImageID|Source|LabelName|Confidence|XMin|XMax|YMin|YMax|IsOccluded|IsTruncated|IsGroupOf|IsDepiction|IsInside|
           |-------|------|---------|----------|----|----|----|----|----------|-----------|---------|-----------|--------|
           |000026e7ee790996|freeform|/m/07j7r|1|0.071905|0.145346|0.206591|0.391306|0|1|1|0|0|
           
           * **ImageID**: the image this box lives in
           * **Source**: indicates how the box was made:
             * **freeform** and **xclick** are manually drawn boxes
             * **activemil** are boxes produced using an enhanced version of the method [1]. These are human verified to be accurate at IoU>0.7.
           * **LabelName**: the MID of the object class this box belongs to.
           * **Confidence**: a dummy value, always 1.
           * **XMin, XMax, YMin, YMax**: coordinates of the box, in normalized image coordinates. XMin is in [0,1], where 0 is the leftmost pixel, and 1 is the rightmost pixel in the image. Y coordinates go from the top pixel (0) to the bottom pixel (1).
           * **IsOccluded**: Indicates that the object is occluded by another object in the image.
           * **IsTruncated**: Indicates that the object extends beyond the boundary of the image.
           * **IsGroupOf**: Indicates that the box spans a group of objects (e.g., a bed of flowers or a crowd of people). We asked        annotators to use this tag for cases with more than 5 instances which are heavily occluding each other and are physically touching.
           * **IsDepiction**: Indicates that the object is a depiction (e.g., a cartoon or drawing of the object, not a real physical instance).
           * **IsInside**: Indicates a picture taken from the inside of the object (e.g., a car interior or inside of a building).
    * Image Labels (csv): 
         * Example,
         
           |ImageID|Source|LabelName|Confidence|
           |-------|------|---------|----------|
           |000026e7ee790996|verification|/m/04hgtk|0|
         * **Source**: indicates how the annotation was created:

           * **verification** are labels verified by in-house annotators at Google.
           * **crowdsource-verification** are labels verified from the Crowdsource app.
           * **machine** are machine-generated labels.

         * **Confidence**: Labels that are human-verified to be present in an image have confidence = 1 (positive labels). Labels that are human-verified to be absent from an image have confidence = 0 (negative labels). Machine-generated labels have fractional confidences, generally >= 0.5. The higher the confidence, the smaller the chance for the label to be a false positive.
         
         
     * Class Names: 
       * Example, 
         /m/0pc9,Alphorn
         /m/0pckp,Robin
         /m/0pcm_,Larch
         /m/0pcq81q,Soccer player
        
        * Follows standard CSV escaping rules. Eg., /m/03hgsf0,"Lemon, lime and bitters"
        
      * Image IDs
        * Example,
           |ImageID|Subset|OriginalURL|OriginalLandingURL|License|AuthorProfileURL|Author|Title|OriginalSize|OriginalMD5|Thumbnail300KURL|Rotation|
          |----|----|-----|-----|------|---|-----|------|------|---------|----------|----|
          |000060e3121c7305|train|https://c1.staticflickr.com/5/4129/5215831864_46f356962f_o.jpg | https://www.flickr.com/photos/brokentaco/5215831864 | https://creativecommons.org/licenses/by/2.0/ | https://www.flickr.com/people/brokentaco/ | David | 28 Nov 2010 Our new house | 211079 | 0Sad+xMj2ttXM1U8meEJ0A== | https://c1.staticflickr.com/5/4129/5215831864_ee4e8c6535_z.jpg | 0|
      
