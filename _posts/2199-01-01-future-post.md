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
Keeping a safe distance from the car in front of the driver is crucial to prevent a car accident since it gives drivers enough time to react to unexpected situations. Figure 1 explains the importance of a safe distance. However, accurately determining a safe distance can be a challenge for many drivers, as it varies depending on the speed of the vehicles. In addition, drivers need to pay attention to several aspects of the driving environment. To address this issue, in our project, we present a driving assistance tool that provides drivers with real-time safe distance information to ensure their road safety.

## 2. System design  
The overall system works in four steps, as illustrated in Figure 2. First, real-time video captured by an off-the-shelf camera is fed into the on-device component, specifically the Jetson Nano. We chose the Jetson Nano due to its similar hardware specifications to a mobile device, enabling realtime operation with mobile devices. Secondly, we operate an AI model on the Jetson Nano device to infer the depth information of the input video. Once the depth information is obtained, the outputs are post-processed to aggregate the depth information of the car in front. Finally, the system displays the aggregated depth information of the car in front which is a safe distance. This system allows real-time monitoring of the distance between the driver’s vehicle and the car in front.
