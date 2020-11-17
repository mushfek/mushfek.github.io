---
layout: post
title: Sandman - An Intelligent Sleep Tracker
index: 4
description: Developed a sleep quality prediction system based on the features extracted from Heart-rate, Movement, and Circadian Rhythm data from the MEMS and PPG sensors of Apple Watch. Sleep stage detection was done using Random Forests and Neural Networks (Multilayer Perceptron) which achieved 90% accuracy and 60% specificity.
---

<h5><u>Abstract</u></h5>
Sleep disorder is one of the most less-noticed yet hazardous health issues that world is facing currently. 
Studies show that nearly 70% US citizens struggle with this problem. Sleep plays a vital role in brain function and recovery of the body. 
Getting inadequate sleep on a regular basis may lead to so many adverse short- and long-term health consequences. 
It is often associated with increased activity of the sympathetic nervous system and hypothalamic–pituitary–adrenal axis, metabolic effects, 
changes in circadian rhythms, and proinflammatory responses. Long-term consequences of sleep disruption in otherwise healthy 
individuals include hypertension, dyslipidemia, cardiovascular disease, weight-related issues, metabolic syndrome, type 2 diabetes mellitus, 
and colorectal cancer. Hence, sleep should be regarded with utmost priority for a healthy person. 
One of the most prominent reason behind this getting unnoticed until a very serious consequence is the complexity of the process of diagnosis. 
The gold standard for sleep measurement is the polysomnogram (PSG), which requires a sleep lab, sleep technician, and monitoring of multiple 
physiological parameter which are not available everywhere in the world as well as a costly process. 
This modern age of digitalisation and advent of wearable technologies, consumer marketed wearable devices might be used to design and develop 
efficient and affordable sleep tracking solutions. However, data collected by these devices are not easily obtainable which makes the process a 
little more complicated. In this project, we used a self-developed application inspired from the similar work works done by 
[The University of Michigan](https://github.com/ojwalch/sleep_accel) to collect raw heart-rate and acceleration data from Apple Watch, one of the most commonly used smartwatches. 
Using this data, we compared the contributions of multiple features (local standard deviation in heart rate, movements and circadian rhythm changes) to performance across several classifiers. Best performance was achieved using neural nets, though the differences across classifiers were generally small. For sleep-wake classification, our method scored 90% of epochs correctly, with 60% of true wake epochs (specificity) and 93% of true sleep epochs (sensitivity) scored correctly. Accuracy for differentiating wake, NREM sleep, and REM sleep was approximately 72% when all features were used. We generalized our results by testing the models trained on Apple Watch data using data from the Multi-ethnic Study of Atherosclerosis (MESA), and found that we were able to predict sleep with performance comparable to testing on our own dataset.

<h5><u>Experiments and Setup</u></h5>
At first, we studied fundamentals of human sleep physiology including how body’s natural responses change during sleeping and various stages of sleep. Sleep Research Society has a plethora of resources on this. We came to a research paper [2] published by the organisation which influenced and guided our work a lot. From those studies we worked up a theory to detect sleep stages and then using the time spent in each of the stages we derived to a scale of sleep quality.

Three types of features were considered as inputs to the classification algorithms tested: motion (activity counts, converted from raw acceleration in m/s2), heart rate, and the circadian clock. Every sample classified by the algorithms corresponds to a 30-second epoch scored during PSG. When classifying each 30-second epoch, features are cropped to a local window of 10 minutes around the scored epoch. 

<strong>Feature #1: Heart-rate</strong>

Heart rate was measured by PPG from the Apple Watch and returned in beats per minute sampled every several seconds. This signal was interpolated to have a value for every 1 second, 
smoothed and filtered to amplify periods of high change by convolving with a difference of Gaussian filter (σ1 = 120 seconds, σ2 = 600 seconds). Each individual was normalized by dividing by the 90th percentile in the absolute difference between each heart rate measurement and the mean heart rate over the sleep period. The standard deviation in the window around the scored epoch was used as the representative feature for heart rate. While this represents variation in heart rate, it is distinct from ECG-based definitions of heart rate variability. 

<strong>Feature #2: Motion</strong>

Acceleration was returned from the Apple Watch as three vec- tors representing acceleration in the x, y, and z directions, and a fourth, representing the timestamp of the measurement in seconds since January 1, 1970 (UNIX or epoch time). 
The acceleration in each direction was returned in units of g. 
In general, data were sampled at approximately 50 Hz.

Less than 3% of the total recording time was affected in this way, and interpolation was used to estimate counts during missing time points. Data from the first two subjects were included only after it was verified that doing so did not meaningfully change the results. 
To make trained classifiers backwards compatible with historical data collection methods, we converted our raw acceleration data to activity counts using MATLAB code available online and validated in the work of te Lindert and colleagues. The final activity count feature was arrived at by convolving the window with a Gaussian (σ = 50 seconds).

<strong>Feature #3: Circadian Rhythm</strong>

In this feature we tried to approximate the changing drive of the circadian clock to sleep over the course of the night. The clock-proxy feature was determined by two sep- arate ways. The first way was to use a fixed cosine wave, shifted relative to the time of recording start, which rose and fell over the course of the night. This way of computing the clock proxy term is attractive because it only requires the time of recording as an input. 

<strong>Training and Testing using MESA dataset</strong>

The work was divided into two teams: I used a Random Forest classifier and a Neural Net (Multilayer Perceptron) and another team went with a Logistic Regression and K-Nearest Neighbours classifiers. We used scikit-lean libraries for training the models using the MESA dataset (for training and testing) along with some data generated from Apple Watches of the developers and testers of the project.

<h5><u>Results</u></h5>
Our implementation differentiated sleep from wake with an accuracy of 90% and a specificity (true wake epochs scored correctly) of 60% (when at the 93% sensitivity threshold). Therefore, our results are similar to previously reported performance of actigraphy and in line with past work validating consumer wearable devices that estimate sleep with proprietary algorithms. Our contribution also considers the diversification of the test subjects where MESA dataset came from a demographics that is significantly different than the data generated from the devices from the developers and testers.

<h5><u>References</u></h5>
1.  [Brain Basics: Understanding Sleep](https://www.ninds.nih.gov/disorders/patient-caregiver-education/understanding-sleep)
2. Olivia Walch1,*, Yitong Huang2, Daniel Forger3 and Cathy Goldstein; Sleep stage prediction with raw acceleration and photoplethysmography heart rate data derived from a consumer wearable device 
3. Agarwal R, Gotman J. Digital tools in polysomnography. J Clin Neurophysiol 2002;19(April (2)):136–43.
