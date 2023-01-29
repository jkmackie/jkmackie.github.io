---
title: "Transfer Learning with a One-Dimensional Signal"
date: 2022-10-04T14:11:14-06:00
draft: false
---

This project predicts pump cavitation using 16,000 Hz accelerometer data (example below).  This data can be automatically categorized by a model.

![accelerometer_data](signal_16000Hz.jpg)

Transfer learning reuses a model built for an old task as the starting point for a new task.  The pre-trained VGG16 Convolutional Neural Network (CNN) model was the starting point.  It categorizes images.

The original VGG16 was fine-tuned to categorize new images.  The new images were derived from an accelerometer signal from the pump.  

The bispectrum signal processing technique converts the signal to images.  Bispectrum measures the amount of phase coupling between signal components and signal power.  

See project below.

### [Transfer Learning with a One-Dimensional Signal](https://towardsdatascience.com/transfer-learning-with-a-one-dimensional-signal-76a0d543e9aa)

This article builds on statistical knowledge from the [signals article](https://medium.com/@mackiej/fourier-and-bispectral-analysis-of-signals-c7a71021b1c8).