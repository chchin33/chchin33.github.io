---
title: 'Saftey distance estimator app'
date: 2023-06-15
permalink: /posts/2012/08/blog-post-4/
tags:
  - cool posts
  - category1
  - category2
---

### The team

- [Seonghoon Yu](https://www.notion.so/CV-886e631607144ec1b49637e95864f37a?pvs=4): seonghoon@gm.gist.ac.kr
- [Chihyeon Jin](https://chchin33.github.io/): chjin1013@gm.gist.ac.kr
- [Seokwoo Jang](https://melodic-plantain-1e0.notion.site/Jang-Seok-Woo-Portfolio-513b52c79b8c4b4d80d21790274a480b): jangseokwoo@gm.gist.ac.kr
- [Donggyu Choi](https://www.notion.so/Donggyu-Choi-deb0b2a368e6465e9c0ebe31d207f452): donggyuchoi@gist.ac.kr

## 1. Problem definition  
Keeping a safe distance from the car in front of the driver is crucial to prevent a car accident since it gives drivers enough time to react to unexpected situations. Figure 1 explains the importance of a safe distance. However, accurately determining a safe distance can be a challenge for many drivers, as it varies depending on the speed of the vehicles. In addition, drivers need to pay attention to several aspects of the driving environment. To address this issue, in our project, we present a driving assistance tool that provides drivers with real-time safe distance information to ensure their road safety.

![Figure 1. Maintaining a safe distance prevents a car accident.](https://github.com/chchin33/chchin33.github.io/blob/Blog_branch/images/DE%20Figure%201.png?raw=true)  
**Figure 1. Maintaining a safe distance prevents a car accident.**

## 2. System design  
The overall system works in four steps, as illustrated in Figure 2. First, real-time video captured by an off-the-shelf camera is fed into the on-device component, specifically the Jetson Nano. We chose the Jetson Nano due to its similar hardware specifications to a mobile device, enabling realtime operation with mobile devices. Secondly, we operate an AI model on the Jetson Nano device to infer the depth information of the input video. Once the depth information is obtained, the outputs are post-processed to aggregate the depth information of the car in front. Finally, the system displays the aggregated depth information of the car in front which is a safe distance. This system allows real-time monitoring of the distance between the driver’s vehicle and the car in front.

![Figure 2. Illustration of our overall system.](https://github.com/chchin33/chchin33.github.io/blob/Blog_branch/images/DE%20Figure%202.png?raw=true)  
**Figure 2. Illustration of our overall system.**

## 3. Machine learning component  
To accurately identify the car in front and estimate its depth information, we need an AI model that simultaneously performs depth estimation and semantic segmentation. Segmentation is responsible for identifying the car in front, while depth estimation infers the depth of the identified car. To achieve this, we attach a depth estimation module to the existing SoTA semantic segmentation model [2], as shown in Figure 3. Then, we train our modified model on Cityscape [1] dataset which contains a comprehensive collection of segmentation mask and depth GT information in the road environment. This will help the model to adapt to the real-world road driving simulation.  

**Table 1. Performance results on the test set of Cityscape dataset [1].**  
![Table 1](https://github.com/chchin33/chchin33.github.io/blob/Blog_branch/images/Table%201.PNG?raw=true)


Through this training process, our model achieves impressive performance on the Cityscapes dataset on GPU system of Jetson Nano which is similar to the snapdragon 865 GPU. In Table 1, Our model shows a latency of 116 ms, 65.31 mIoU for segmentation accuracy, and 1.2337 rmse for depth estimation. These results demonstrate the effectiveness and accuracy of our trained AI model in identifying the car in front and providing reliable depth information. Using this depth information, we calculate a safe distance and display it in a real-time manner.

![Figure 3. Illustration of our modified model. We attach a depth estimation module to the SoTA segmentation model [2].](https://github.com/chchin33/chchin33.github.io/blob/Blog_branch/images/DE%20Figure%203.png?raw=true)  
**Figure 3. Illustration of our modified model. We attach a depth estimation module to the SoTA segmentation model [2].**

In addition, to train our model on two different task (depth estimation and segmentation), we design loss function as follows. CE is for segmentation and MAE is for depth estimation.

Combined Loss = CE + MAE  

$CE = -\sum_i{C_i log(s_i)}$  

$MAE = -\frac{1}{n}*\[ \sum_{j=1}^{n}{\lvert y_j - \hat{y_j}\rvert} \]$


## 4. System evaluation  
To validate and evaluate the effectiveness of our system, we execute our trained and modified model on On-Device (Jetson Nano) using two types of data. (1) Existing video data, and (2) Captured real-time video from the off-the-shelf camera. Since Jetson Nano has the similar as On-Device, The first setting allows us to check the performance and accuracy of our system in a controlled environment where we can closely monitor the input and output and gain insights into its capabilities. The second setting allows us to assess whether our system could be operated well in real-world situations where the real-time video is passed into our system.

![Figure 4. Qualitative results of ours in a first setting which is getting
the existing video data, not real-time video. Please note that this
result is not a post-processing version displaying a safe distance.](https://github.com/chchin33/chchin33.github.io/blob/Blog_branch/images/DE%20Figure%204.png?raw=true)  
**Figure 4. Qualitative results of ours in a first setting which is getting
the existing video data, not real-time video. Please note that this
result is not a post-processing version displaying a safe distance.**

![Figure 5. Qualitative results of ours in a second setting which is
operating on the real-time video from the camera.We drive Gwanju
with our system. If a front car is so close, we display warning
message instead of safe distance. Since this is our first prototype.](https://github.com/chchin33/chchin33.github.io/blob/Blog_branch/images/DE%20Figure%205.png?raw=true)  
**Figure 5. Qualitative results of ours in a second setting which is
operating on the real-time video from the camera.We drive Gwanju
with our system. If a front car is so close, we display warning
message instead of safe distance. Since this is our first prototype.**

Figure 4 shows the prediction of our system on Jetson Nano in the first setting which is inputting the existing video, not real-time video. Please note that this result is not a postprocessing version displaying a safe distance which demonstrates the next chapter. Since Jetson Nano is a lightweight device having similar hardware specs to a mobile device, we believe that our system has the same results when operating it on other On-Device. Figure 5 shows the prediction when our model is operated on a second setting. We actually drive Gwanju with our system. If the front car is so close, our system displays a warning message instead of safe distance. Since it is our prototype and not implemented. We will implement it in the future.


## 5. Application demonstration  
In order to provide more valuable and practical assistance to drivers, we have chosen to focus on real-time operation with an On-Device, rather than a web application as initially proposed. We believe that displaying a safe distance in realtime while driving can be highly beneficial for many drivers. Our future plan is to integrate our system with a black box as the off-the-shelf camera and display the safe distance on a mobile app in a real-time manner, providing drivers with immediate feedback.

## 6. Reflection
In this project, we have successfully implemented our system to run on the On-Device (Jetson Nano). As part of our future plans, we aim to connect the black box with the Jetson Nano to capture real-time video of the driving conditions.We intend to develop a mobile app that displays the safe distance in real time as a tool to assist the driver. To achieve this, we need to build our system which includes the mobile app and the black box. In addition, we will design a user-friendly interface for the app to make it convenient for the users. In our system, it is important to minimise distraction so that the driver’s view is not obstructed and can be easily glanced at without causing undue distraction. Therefore, designing a clear user interface that does not distract the driver is crucial for our system in the future.

## 7. Broader Impacts  
We explain (1) Intended use, (2) Unintended use and the associated harms, and (3) Design decisions to mitigate harms

### 7.1. Intented Use  
- Enhanced Road Safety: Our application is to promote road safety by providing drivers with real-time information about the safe distance between their vehicle and the car in front. This can help reduce the risk of accidents caused by insufficient following distance in this section.

### 7.2. Unintened use and the associated harms  
- There is a risk that some drivers may become overly reliant on the application and neglect other important aspects of safe driving, such as paying attention to road conditions and traffic signs. This could lead to complacency and an increased likelihood of accidents.  
- Distracted Driving: If the display of the safe distance is not designed with proper consideration for driver attention and distraction, it could potentially cause distraction and compromise the driver’s focus on the road.

### 7.3. Design Decisions to Mitigate Harms:  
- Clear User Interface: Our application design has prioritized developing a user interface that is intuitive, unobtrusive, and minimizes distraction. The display of the safe distance will be positioned in a manner that does not obstruct the driver’s view and can be easily glanced at without causing undue distraction.

## References
- [1] Marius Cordts, Mohamed Omran, Sebastian Ramos, Timo Rehfeld, Markus Enzweiler, Rodrigo Benenson, Uwe Franke, Stefan Roth, and Bernt Schiele. The cityscapes dataset for semantic urban scene understanding. In CVPR, 2016.
- [2] Wenqiang Zhang, Zilong Huang, Guozhong Luo, Tao Chen, Xinggang Wang, Wenyu Liu, Gang Yu, and Chunhua Shen. Topformer: Token pyramid transformer for mobile semantic segmentation. In CVPR, 2022.
