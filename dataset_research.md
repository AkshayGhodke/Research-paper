# Datasets

* ImageNet 2012
* coco
* KITTI
* Open Images V4

# Open Images V4

  # Data Organization
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
      * 
