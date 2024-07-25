# Carnatic Beat Identification
 This project develops an ML/AI classifier that identifies the Carnatic beat (taalam) of a mridangam solo
 
## Background:
In the South Indian classical music genre (also called **Carnatic** music), the concept of rhythm is very well developed and sophisticated. All songs follow a certain beat cycle called taalam. During a concert the percussionist, who typically plays a hand drum called mridangam, is given an opportunity to perform a drum solo, where he/she brings out the intricacies of the taalam of the preceding song. The solo can last from 5 to 10 minutes to as much as 30 to 40 minutes.

## The Project Goal:
Given a clip of a drum solo, identify the taalam (beat cycle) in which it is performed. <br/>

While there are 5 main taalam types that are commonly performed (and in principle thousands of possible taalams), I have restricted the scope of this project to **3 well-known** and often-used taalams: _aadi talam_ (8-beat cycle), _mishra-chapu talam_ (7 beat cycle) and _khanda-chapu talam_ (5-beat cycle).

## Data Sources:
Fortunately, numerous video and audio recordings of Carnatic music concerts, including the drum solo portion, are available on the internet. In particular YouTube (video) and SangeethaPriya.org (audio) host thousands of high-quality recordings. So the _raw_ data (audio clips) is available.

## Data Generation: Vectorization
While the raw data is available, it has to be transformed into a form suitable for analysis. We do this by **_vectorizing_** the sound clip. <br/>
The vectorization idea pursued here is to generate a series of time stamps corresponding to each drum beat (strike). A drum beat's signature is a sudden rise in sound intensity (a spike) with respect to the background intensity.

One could ideally analyze the mp3 file directly and detect the timestamp of intensities. This would require a reasonably deep understanding of the mp3 format and some familiarity with some open-source (or free) software package that can be used to query the mp3 object for required parameters. In the interest of time, here we will generate the vectorization through an indirect path, as follows: <br/>
We will first create an **_image of the sound intensity_** by reading the mp3 file into **Audacity**, a popular open-source app for audio editing and recording, and taking a **screenshot** of the intensity waveform. We then use a custom image-analysis software (developed by me) to identify the intensity spikes corresponding to each drum beat. <br/>

#### Custom Image Analysis software
The idea is further explained in the image below, which is a screenshot of an audio clip viewed in Audacity.

![Explainer](https://github.com/user-attachments/assets/c6597ff4-4fe2-4629-916a-9637869c68ea)

Here's a screen shot of the custom software at work (runs on Windows only). I call it **Spike**, because it detects spikes in the sound intensity.

![SignalDetectionApp](https://github.com/user-attachments/assets/c04272ee-47e9-4e4b-9aaf-54a245c0f365)

At the end of the day, a vector of of time stamps is generated for each audio clip.

#### Data generation is underrated!
We remark here that there is some art behind the data generation. For example, what is the definition of a "spike" in sound intensity, and how does one separate it from background? How dependent is the vectorization on choice of base scan position? There was some trial and error and experimentation in developing the image analysis software. At a meta level, is there a better approach to vectorizing a sound clip to select out drum beats, other than intensity analysis? <br/>

One develops a genuine respect and admiration for those who collect high-quality, clean data - a role that is perhaps underappreciated when data can be easily downloaded with the click of a button.

## ML/AI Analysis Techniques:
The ML/AI question here is one of **classification**: given an audio clip, classify it as one of the 3 taalams.
We will try several classification algorithms (LogisticRegression, KNN, DecisionTree, SVM) to see which one gives the best results.

## Data:
Each data row corresponding to one audio clip consists of 250 time stamps (250 features), normalized to 1 (ie. if an intensity spike starts at the start of the clip, timestamp = 0; if a spike occurs at the end of the clip, timestamp = 1. The column "Beat" specifies the beat cycle of the audio clip (A or M or K). Here's a data sample of 5 rows:
![DataSample](https://github.com/user-attachments/assets/49f9169b-e18a-4b10-8a58-67d3f028f28e)

**Note:** <br/>
At the time of writing, we have only one data row of a Khanda Chapu (K) beat (others are being generated). The analysis below has been conducted with two classes of beats (A and M).

## Data Models
We built 5 models to classify the data (target = 'Beats'): LogisticRegression, DecisionTreeClassifier, KNeighborsClassifier, Support Vector Classifier (SVC) and LogisticRegressionCV.
Each model was trained on a training dataset and evaluated on a test dataset.

## Results
### Model Accuracy
Below are the accuracy results for each model:
![ModelAccuracy](https://github.com/user-attachments/assets/94ac6483-d97e-4231-813e-cac63bcbd5d7)

### Computation time
Here are the computation times (relative values are meaningful; absolute values will change as the dataset size increases):
![ComputeTime](https://github.com/user-attachments/assets/b3955fc7-88e6-44ea-bd1f-a748a93dd235)

## Evaluation
#### Model Accuracy
All models did fare better than random chance but most were not very impressive, scoring in the 60% to 70% range. The exception was <b>Support Vector Classifier (SVC)</b> which had a score of 87%. 

#### Computational performance
LogisticRegressionCV took the longest time by far. Our best accuracy performer, svc, was also the speediest.

#### Caveat
The above analysis was done with a _very small_ dataset! The results might change as more data is generated and brought into the analysis.

### Recommendations and Remarks
Based on the results of our Machine Learning models, considering both Model Accuracy and Computational Performance, we can recommend that <b>SVC</b> is a promising model to use to identify the beat cycle of a Carnatic drum solo. It is encouraging to see that the vectorization approach used here seems to be reasonable enough to pursue further. The results seem to validate the hope that ML/AI techniques can be successfully brought to bear on an esoteric topic like Carnatic beat identification.

### Next Steps
#### Data, data, data
There is an old adage that the 3 most important aspects in real estate are location, location, location. One could say that in an AI/ML project, the 3 most important aspects are data, data, data! Without a lot of high-quality data, the results of any analysis has to be taken with a grain of salt. We need **a lot** more data to analyze before we can confidently assert that we have built a reliable AI model to identify a Carnatic beat cycle.

#### Sound Clip Vectorization
At a meta level, is there a better approach to vectorizing a sound clip to select out drum beats, other than intensity analysis? Experts in the domain (music) might have some suggestions about how they discern different beat cycles, which might be translatable into a vectorization technique. 
 
#### Neural Nets
Since we have pretty high-dimensional data to begin with, a Neural Net approach might give much better results than the classic ML/AI regression techniques that we have applied here. This path is definitely worth exploring further.
