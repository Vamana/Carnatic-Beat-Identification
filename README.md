# Carnatic Beat Identification
 This project develops an ML/AI classifier that identifies the Carnatic beat (taalam) of a mridangam solo
 
## Background:
In the South Indian classical music genre (also called **Carnatic** music), the concept of rhythm is very well developed and sophisticated. All songs follow a certain beat cycle called taalam. During a concert the percussionist, who typically plays a hand drum called mridangam, is given an opportunity to perform a drum solo, where he/she brings out the intricacies of the taalam of the preceding song. The solo can last from 5 to 10 minutes to as much as 30 to 40 minutes.

## The Project Goal:
Given a clip of the drum solo, identify the taalam (beat cycle) in which it is performed. <br/>

While there are 5 main taalam types that are commonly performed (and in principle thousands of possible taalams), I have restricted the scope of this project to **3 well-known** and often-used taalams: _aadi talam_ (8-beat cycle), _mishra-chapu talam_ (7 beat cycle) and _rupaka talam_ (3-beat cycle).

## Data Sources:
Fortunately, numerous video and audio recordings of Carnatic music concerts, including the drum solo portion, are available on the internet. In particular YouTube (video) and SangeethaPriya.org (audio) host thousands of high-quality recordings. So the _raw_ data (audio clips) is available.

## Data Generation: Vectorization
While the raw data is available, it has to be transformed into a form suitable for analysis. We do this by **_vectorizing_** the sound clip. <br/>

One could analyze the mp3 file directly and detect the timestamp of intensities. This would require a reasonably deep understanding of the mp3 format and some familiarity with some open-source (or free) software package that can be used to query the mp3 object for required parameters. In the interest of time, here we will generate the vectorization through an indirect path, as follows: <br/>
We will first create an **_image of the sound intensity_** by reading the mp3 file into **Audacity**, an open-source app for audio editing and recording, and taking a screenshot. We then use a custom-written image-analysis software (developed by me) to identify the intensity spikes corresponding to each drum beat. <br/>

#### Custom Image Analysis software
The idea is explained in the image below. At the end of the day, a vector of of time stamps is generated for each clip.

![Explainer](https://github.com/user-attachments/assets/c6597ff4-4fe2-4629-916a-9637869c68ea)

Here's a screen shot of the custom software at work (runs on Windows only). I call it **Spike**, because it detects spikes in the sound intensity.

![SignalDetectionApp](https://github.com/user-attachments/assets/c04272ee-47e9-4e4b-9aaf-54a245c0f365)

We remark here that there is some art, not just data science, behind the data generation. For example, what is the definition of a "spike" in sound intensity, and how does one separate it from the noise? How dependent is the vectorization on choice of base scan position? There was some trial and error and experimentation in developing the image analysis software. At a meta level, is there a better approach to vectorizing a sound clip to select out drum beats, other than intensity analysis?

## ML/AI Analysis Techniques:
The ML/AI question here is one of **classification**: given an audio clip, classify it as one of the 3 taalams.
We will try several classification algorithms (LogisticRegression, KNN, DecisionTree, SVM) to see which one gives the best results.


