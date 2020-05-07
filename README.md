# SDE-Project_4-Vehicle_Detection_and_tracking
## Overview
This project will implement traditional Computer Vision pipeline for object detection typically connsists of separate processing stages, for instance, **feature extrsction, spatial sampling and classification**

The purpose to use traditional method is we can understand what heppened during this process, instead of the balack box for Deep Neural networks. 

![](https://github.com/garychian/SDE-Project_4-Vehicle_Detection_and_tracking/blob/master/project_video.gif)

## Pipeline
* **Input**
	
	The project used training images from a combination of the [GTI vehicle image database](http://www.gti.ssr.upm.es/data/Vehicle_database.html) and [KITTI vision benchmark suite](http://www.cvlibs.net/datasets/kitti/). Finally we implement the code in video stream, please see this [project video](https://github.com/garychian/SDE-Project_4-Vehicle_Detection_and_tracking/blob/master/project_video.mp4)


* **Feature Extraction**: Decide what features to use, color or gradient based features
	* Histogram of color

		Tally up the values for each color channel across the X-axis, but it can lead to false positive if it's the obly feature
	* HOG(Histogram of Oriented Gradients)

		HOG takes an image and breaks it down into a grid. Then, in each cell of the grid, every pixel has its gradient direction and magnitude computed. Those gradients are then into a histogram with a number of bins for possible directions, however gradients with larger magnitudes contribute more than smaller magnitudes. If you want to more details about HOG please refer to [here](https://www.youtube.com/watch?v=7S5qXET179I)

	> NOTE: the feature vector should normalized/standardized before being passed to the classifier. 

```python
# feature_extraction.py

# Define a function to return HOG features and visualization
def get_hog_features(img,orient,pix_per_cell,cell_per_block,vis= False,feature_vec=True):
	...
	return features

# Define a function to compute binned color feature 
def bin_spatial(img,size=(32,32)):
	....
	return features

# Define a function to compute color histogram features
def color_hist(img,nbins=32,bins_range=(0,256)):
	...
	return hist_features

# Define a function to extract featurers from a list of images
def extract_features(imgs, color_space='RGB', spatial_size=(32, 32),
                     hist_bins=32, orient=9,
                     pix_per_cell=8, cell_per_block=2, hog_channel=0,
                     spatial_feat=True, hist_feat=True, hog_feat=True):
                     ...
                     return features

```

```python
# detection_pipeline.py

# Define a function to extract features from a single image window, extract_feature() working on list of images
def single_img_features(imgs, color_space='RGB', spatial_size=(32, 32),
                     hist_bins=32, orient=9,
                     pix_per_cell=8, cell_per_block=2, hog_channel=0,
                     spatial_feat=True, hist_feat=True, hog_feat=True):
                     ....
                     return img_features 

# Define a function you will pass an image, and the list of windows to be searched
def search_window(img, windows, clf, scaler, color_space='RGB',
                   spatial_size=(32, 32), hist_bins=32,
                   hist_range=(0, 256), orient=9,
                   pix_per_cell=8, cell_per_block=2,
                   hog_channel=0, spatial_feat=True,
                   hist_feat=True, hog_feat=True):
					...
					return on_window

# Define a heat map 
def add_heat(heatmap,bbox_list):
	...
	return heatmap

# Define labels to the heatmap
def add_heat_labels(heatmap,bbox_list,labels):
	...
	return heatmap

def apply_threshold(heatmap, threshold):
	...
	return heatmap
```
* **Choose and train a classifier**: We choose SVM this case
	* Linear SVM
```python
# Vehicle_Classification.py

class Vehicle_Classification():
	def __init__(self):
	def train_classifier(self,learn_new_classifier=False,classifier_pickle='./detection_functions/svc_pickle.pickle', X_scaler_pickle='./detection_functions/X_scaler_pickle.pickle'):

```
	Now, we have a feature vector for image, we can train a clssifier on the training data. Linear SVM is excellent for classification, and also fast and accurate!  
* **Sliding window find vehicle**: record the central position of vehicle
* **Deal with over lapping window and false positive**
```python
# sliding_window.py

def slide_window(img_shape, x_start_stop=[None,None], y_start_stop=[None, None],
                 xy_window=(64, 64), xy_overlap=(0.5, 0.5)):
                 ....
                 return window_list

def return_labeled_windows(labels):
	...
	return bbox_collection

def perspective_widthnew_y,y_start_stop=[420, 720],bottom_width=360, top_width=32):
    new_width = int(((1. - ((y_start_stop[1] - new_y) / (y_start_stop[1] - y_start_stop[0]))) * (bottom_width - top_width)) + top_width)
    return int(new_width)
def slide_precheck(img_shape, y_start_stop=[440,720],xy_window_start=(440,440),xy_overlap=(0.0,0.8)):
	return shifted_collection
```


