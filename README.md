# Road_Segmentation
This is the first in the series of repositories in development of the Smart City Planner research project I have been involved with for the past year.

The goal of this project is to develop a dynamic web application which will ease the process of city planning and development-tracking for the user with minimum effort spent by the user in repetitive processes. With the widespread development in the field of Artificial Intelligence, the aim here is to at least being able to reduce the repetitive processes such as spotting different features in an image to only a computational challenge.

This repository reflects a solution to road detection and road length calculation in images taken from a drone.

## The Application
* On the landing page the user can either upload his/her own images or to test the application the user can use the example test images provided

![](images/Picture1.png)

![](images/Picture2.png)
* Once uploaded it usually takes 3-5 minutes to process depending on the size of the image

![](images/Picture4.png)
* Once done processing the user can view the results by clicking the "View Results" button

![](images/Picture5.png)
* The result shows the output image with roads clearly marked along with the length of the road detected

![](images/Picture6.png)

## How does it work?
### The Dataset
* The dataset used were a set of 1000 images collected from drones, over various types of Indian settlements.
* Ground Truth was made by masking another blank layer onto the original image and marking each pixel which represented road.

![](images/dataset1.jpg)
* A CSV file with 28 features of each pixel was made for each image. The features being RGB values of the pixel itself along with each of the adjacent pixel. The last column being either 0 or 1 representing road pixel or non-road pixel.

![](images/dataset2.png)
* The CSV of 170 such images were combined to form one huge CSV as the training set.

### Machine Learning
* The application uses Random Forest algorithm to detect roads.
* Each pixel of the image goes through a pre-trained Random Forest perceptron, which predicts if the pixel belongs to the road class or the non-road class.
* Connected Component Analysis(CCA) is used to make clusters of
* Though slower than most deep learning techniques, this technique turned out to be much more accurate in the results.

### Length calculation
* The Ground Sample Distance(GSD) for each image is first calculated by extracting the "relative_altitude", "focal_length","sensor_height" and "sensor_width" from exif data of the image and then using the below formula.
![](images/gsd2.PNG)
* GSD tells us that how much distance(in cm) does each pixel dimension signify. Since pixels are square, minimum of height GSD and width GSD is considered to be the final GSD.
* Connected Component Analysis(CCA) is used to make clusters of pixels where the road was detected. A huge cluster is usually divided into multiple clusters.
* Hough transform is used to fit a mean line to each of these clusters. The width of the line kept at one pixel.
* Now to finally to calculate the length, the GSD is finally multiplied with the total number of pixels forming the mean lines.
* For testing the accuracy of the length, comparisons were made with the length of roads found out from Google Maps for the same area. This was tested for a huge set of images.
* The length turns out to be accurate up to an average variance of 5 meters


The target consumer for our application is municipal authorities and other urban planning committees which do not have access to latest satellite images of an area at their will.

It is an observation here that since drones do not fly very high, we usually get very high resolution image and thus in a general sense we have a lot of features to deal with.

SpaceNet challenge does it on satellite images all trained in USA, not meant to be used by municipal.
