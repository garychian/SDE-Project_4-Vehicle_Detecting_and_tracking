# SDE-Project_4-Vehicle_Detection_and_tracking
## Overview
This project will implement traditional Computer Vision pipeline for object detection typically connsists of separate processing stages, for instance, **feature extrsction, spatial sampling and classification**

The purpose to use traditional method is we can understand what heppened during this process, instead of the balack box for Deep Neural networks. 

## Pipeline
* **Input**
	
	The project used training images from a combination of the [GTI vehicle image database](http://www.gti.ssr.upm.es/data/Vehicle_database.html) and [KITTI vision benchmark suite](http://www.cvlibs.net/datasets/kitti/). 


* **Feature Extraction**: Decide what features to use, color or gradient based features
	* Histogram of color

		Tally up the values for each color channel across the X-axis, but it can lead to false positive if it's the obly feature
	* HOG(Histogram of Oriented Gradients)

		HOG takes an image and breaks it down into a grid. Then, in each cell of the grid, every pixel has its gradient direction and magnitude computed. Those gradients are then into a histogram with a number of bins for possible directions, however gradients with larger magnitudes contribute more than smaller magnitudes. If you want to more details about HOG please refer to [here](https://www.youtube.com/watch?v=7S5qXET179I)

	> NOTE: the feature vector should normalized/standardized before being passed to the classifier. 

* **Choose and train a classifier**: We choose SVM this case
	* Linear SVM

	Now, we have a feature vector for image, we can train a clssifier on the training data. Linear SVM is excellent for classification, and also fast and accurate!  
* **Sliding window find vehicle**: record the central position of vehicle
* **Deal with over lapping window and false positive**

