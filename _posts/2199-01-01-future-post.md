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

- Seonghoon Yu:
- Chihyeon Jin:
- Seokwoo Jang:
- Donggyu Choi:

## 1. Problem definition  
Keeping a safe distance from the car in front of the driver is crucial to prevent a car accident since it gives drivers enough time to react to unexpected situations. Figure 1 explains the importance of a safe distance. However, accurately determining a safe distance can be a challenge for many drivers, as it varies depending on the speed of the vehicles. In addition, drivers need to pay attention to several aspects of the driving environment. To address this issue, in our project, we present a driving assistance tool that provides drivers with real-time safe distance information to ensure their road safety.

## 2. System design  
The overall system works in four steps, as illustrated in Figure 2. First, real-time video captured by an off-the-shelf camera is fed into the on-device component, specifically the Jetson Nano. We chose the Jetson Nano due to its similar hardware specifications to a mobile device, enabling real time operation with mobile devices. Secondly, we operate an AI model on the Jetson Nano device to infer the depth information of the input video. Once the depth information is obtained, the outputs are post-processed to aggregate the depth information of the car in front. Finally, the system displays the aggregated depth information of the car in front which is a safe distance. This system allows real-time monitoring of the distance between the driver’s vehicle and the car in front.

## 3. Machine learning component  
To accurately identify the car in front and estimate its depth information, we need an AI model that simultaneously performs depth estimation and semantic segmentation. Segmentation is responsible for identifying the car in front, while depth estimation infers the depth of the identified car. To achieve this, we attach a depth estimation module to the existing SoTA semantic segmentation model [2], as shown in Figure 3. Then, we train our modified model on Cityscape [1] dataset which contains a comprehensive collection of segmentation mask and depth GT information in the road environment. This will help the model to adapt to the real-world road driving simulation.  
Through this training process, our model achieves impressive performance on the Cityscapes dataset using the Sanpdragon 865 GPU, which is the same GPU system used in the Jetson Nano. In Table 1, Our model shows a latency of 116 ms, 65.31 mIoU for segmentation accuracy, and 1.2337rmse for depth estimation. These results demonstrate the effectiveness and accuracy of our trained AI model in identifying the car in front and providing reliable depth information. Using this depth information, we calculate a safe distance and display it in a real-time manner.

## 4. System evaluation  
To validate and evaluate the effectiveness of our system, we execute our trained and modified model on On-Device (Jetson Nano) using two types of data. (1) Existing video data, and (2) Captured real-time video from the off-the-shelf camera. Since Jetson Nano has the similar as On-Device, The first setting allows us to check the performance and accuracy of our system in a controlled environment where we can closely monitor the input and output and gain insights into its capabilities. The second setting allows us to assess whether our system could be operated well in real-world situations where the real-time video is passed into our system.  
Figure 4 shows the prediction of our system on Jetson Nano in the first setting which is inputting the existing video, not real-time video. Please note that this result is not a post processing version displaying a safe distance which demonstrates the next chapter. Since Jetson Nano is a lightweight device having similar hardware specs to a mobile device, we believe that our system has the same results when operating it on other On-Device. Figure 5 shows the prediction when our model is operated on a second setting. In real-time video, ours holds similar results with the first setting.

## 5. Application demonstration  
In order to provide more valuable and practical assistance to drivers, we have chosen to focus on real-time operation with an On-Device, rather than a web application as initially proposed. We believe that displaying a safe distance in real-time while driving can be highly beneficial for many drivers. Our future plan is to integrate our system with a black box as the off-the-shelf camera and display the safe distance on a mobile app in a real-time manner, providing drivers with immediate feedback.

## 6. Reflection
In this project, we have successfully implemented our system to run on the On-Device (Jetson Nano). As part of our future plans, we aim to connect the black box with the Jetson Nano to capture real-time video of the driving conditions. We intend to develop a mobile app that displays the safe distance in real time as a tool to assist the driver. To achieve this, we need to build our system which includes the mobile app and the black box. In addition, we will design a user-friendly interface for the app to make it convenient for the users. In our system, it is important to minimise distraction so that the driver’s view is not obstructed and can be easily glanced at without causing undue distraction. Therefore, designing a clear user interface that does not distract the driver is crucial for our system in the future.

## 7. Broader Impacts
