# **Behavioral Cloning** 
---

**Behavioral Cloning Project**

The goals / steps of this project are the following:
* Use the simulator to collect data of good driving behavior
* Build, a convolution neural network in Keras that predicts steering angles from images
* Train and validate the model with a training and validation set
* Test that the model successfully drives around track one without leaving the road
* Summarize the results with a written report


[//]: # (Image References)

[image1]: ./examples/placeholder.png "Model Visualization"
[image2]: ./examples/placeholder.png "Grayscaling"
[image3]: ./examples/placeholder_small.png "Recovery Image"
[image4]: ./examples/placeholder_small.png "Recovery Image"
[image5]: ./examples/placeholder_small.png "Recovery Image"
[image6]: ./examples/placeholder_small.png "Normal Image"
[image7]: ./examples/placeholder_small.png "Flipped Image"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/432/view) individually and describe how I addressed each point in my implementation.  

---
### Files Submitted & Code Quality

#### 1. Submission includes all required files and can be used to run the simulator in autonomous mode

My project includes the following files:
* model.py containing the script to create and train the model
* drive.py for driving the car in autonomous mode
* model.h5 containing a trained convolution neural network 
* writeup.md summarizing the results

#### 2. Submission includes functional code
Using the Udacity provided simulator and my drive.py file, the car can be driven autonomously around the track by executing 
```sh
python drive.py model.h5
```
while having the simulator also running in parallel  

#### 3. Submission code is usable and readable

The model.py file contains the code for training and saving the convolution neural network. The file shows the pipeline I used for training and validating the model, and it contains comments to explain how the code works.

### Model Architecture and Training Strategy

#### 1. An appropriate model architecture has been employed

the model is the same model from Nvidiea as suggested in the course materials

#### 2. Attempts to reduce overfitting in the model

The model contains dropout layers in order to reduce overfitting (model.py lines 145). 
An early stopping callback was attached to the model montiroing validation loss.
The model was trained and validated on different data sets to ensure that the model was not overfitting (code line 128-129,157). 
The model was tested by running it through the simulator and ensuring that the vehicle could stay on the track.

#### 3. Model parameter tuning

The model used an adam optimizer, so the learning rate was not tuned manually (model.py line 154).

#### 4. Appropriate training data

Training data was chosen to keep the vehicle driving on the road. I used a combination of center lane driving, recovering from the left and right sides of the road ... 

For details about how I created the training data, see the next section. 

### Model Architecture and Training Strategy

#### 1. Solution Design Approach

The overall strategy for deriving a model architecture was to start where the Nvidia model suggested and tweak it till the final resources was acceptable.

to prepare the data the zone of intrest was cropped tp remove the sky and the car hood and just keep the lane lines (line 133).

then input data was averaged and stadnard divation maintained to give the network a better chance to converge.

In order to gauge how well the model was working, I split my image and steering angle data into a training and validation set. I found that my first model had a low mean squared error on the training set but a high mean squared error on the validation set. This implied that the model was overfitting. 

To combat the overfitting, I modified the model so that ...

Then I ... 

The final step was to run the simulator to see how well the car was driving around track one. There were a few spots where the vehicle fell off the track... to improve the driving behavior in these cases, I ....

At the end of the process, the vehicle is able to drive autonomously around the track without leaving the road.

#### 2. Final Model Architecture

The final model architecture as follows 

At first there was a 5*5 kernels with depth of 24 then 36 then 48 for the first 3 convolution layersusing 2*2 strides.(lines 138-140)
Followed by a 3*3 kernels with length 64 both(lines 141-142)

all the convolution layer were using valid padding and RELU activation functions..

then the output was flattened in order to prepare to the fully connected network(output of 8448 activation 4*33*64).

starting with 1164 neuron (activation relu) then a dropout of constant 0.65 (there was the option to put after the CNN because it had more paramter so it would allow for more redundancy, also if an overfitting issue was faced a variable dropout ratio was meant to be used which wasn't the case).

follwed by other 4 dense layers of 100,50,10 and the final output is only one value for the sterring

the drive.py was also modified to set the speed 15


#### 3. Creation of the Training Set & Training Process

The data used was the one provided by the course, in order to provide a more balanced data (versus mostly zero centered) the right and left pictures were also used.
the modifction value obtained by trigonometry didn't work, isntead a value of 0.2 was proven better by trial and error.
and for that the trials were all ín the training phase since some values provided better loss than other and hence 0.2 was chosen without testing on the simulator.

also to add more balance every image added was flipped, meaning for every recorded frame provided the network had 6 input samples(8036 augmented to 48216).
I didn't want to use any distoration to modify the images (adding shadows or shifting and tilting) since some peapers suggested that this will lead to under fitting, this this option was kept for later investigation 


I finally randomly shuffled the data set and put 20% of the data into a validation set(line 32). 

I used this training data for training the model. The validation set helped determine if the model was over or under fitting. The ideal number of epochs was not impotrant since early stoping was going to kill the training when overfitting begin to happen so a large value of 30 was chose
I used an adam optimizer so that manually training the learning rate wasn't necessary.
