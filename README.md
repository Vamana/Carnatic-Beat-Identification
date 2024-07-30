# Carnatic Beat Identification
### What _is_ that beat cycle?
 
## Introduction and Background
This project develops an ML/AI classifier that identifies the <b>Carnatic beat (taalam)</b> of a mridangam solo. <br/>
 - In the South Indian classical music genre (also called Carnatic music), the concept of rhythm is very well developed and sophisticated.
 - Every song/composition follows a certain beat cycle called taalam.
 - During a concert, the percussionist, who typically plays a hand drum called <b>mridangam</b>, performs a drum solo where he/she <b>improvises</b> on the taalam of the preceding song.
 - The percussion solo can last from 5 to 10 minutes to as much as 30 to 40 minutes.

## Project Goal
<b>Given a clip of a drum solo, identify the taalam (beat cycle) in which it is performed.</b> <br/>
- 5 popular taalam (beat cycle) types, thousands possible in principle.
- The scope of this project is restricted to 3 well-known taalams
- Aadi (8-beat cycle), mishra-chapu (7-beat cycle) and khanda-chapu (5-beat cycle)
- The 'Demo' directory contains two audio clips, of an 8-beat cycle and a 5-beat cycle. See (or hear) if you can identify which is which!

## Motivation and Business Case
- About <b>100 to 120 million</b> Carnatic music listeners worldwide.
- <b>“Search by beat”</b> functionality can be offered to users of music streaming services.
- <b>Education, Catalog and Archival:</b> A large number of recordings currently on tape (example All India Radio archives) can be digitized and automatically categorized by beat.
- <b>Curiosity/Exploration:</b> Can ML/AI techniques be brought to bear to identify and classify Carnatic beats? Can ML/AI approach human expertise?

## Data Sources
- Raw data is simply an audio clip - say in mp3 format.
- Numerous video and audio recordings of Carnatic music concerts, including the drum solo portion, are available on the internet. 
- <b>YouTube</b> (video) and <b>SangeethaPriya.org</b> (audio) host a large number of high-quality recordings. 
- Several private collections and an active aficionado community who swap recordings.

## Data Generation: Vectorize sound intensity
- Raw data (audio file) has to be transformed into a format suitable for analysis.
- <b>Idea:</b> generate a <b>series of time stamps corresponding to each drum beat.</b>
- A drum beat = <b>a sudden rise in sound intensity (a spike)</b> with respect to the background intensity.

## Sound Intensity Vectorization: Image Analysis
- Ideally, analyze the mp3 file and detect intensity spikes. But - requires mp3 file format expertise.
- Idea: Create an image of the sound intensity by taking the screenshot of the waveform in Audacity  (a popular open-source app for audio editing and recording). 
- Analyze the image to detect spikes in intensity (drum beats)

![Explainer](https://github.com/user-attachments/assets/78d0799c-c099-4918-9200-20f16d9d4f1a)

## Custom Image Analysis App
- Developed for this project (runs on Windows only). Scans the image to detect spikes.
- Generates a vector of timestamps of the spike locations, normalized to 1. Screenshot below.

![SignalDetectionApp](https://github.com/user-attachments/assets/c04272ee-47e9-4e4b-9aaf-54a245c0f365)

## Data Samples
- Each data row corresponds to one audio clip, approximately 1 to 1.5 minutes long.
- When vectorized, a clip produces about of 200 to 350 timestamps (features) 250 timestamps retained for each row, extra columns discarded. 
- Shorter clips are dropped during analysis. No way to fill in missing values!
- <b>Target = ‘Beats’</b> column. 3 classes of beat cycles: A (8-beat), K (5-beat) and M (7-beat)
- <b>117 data points</b> only. Very small dataset, generated manually. Well-balanced between classes.

![Dataframe](https://github.com/user-attachments/assets/71c22f70-abfd-4b05-b28a-c0b5e581dcdd)

## Exploratory Data Analysis - 1
- <b>Classification problem:</b> given an audio clip of a drum solo, which beat (class) does it correspond to?
- Preliminary analysis using 2 classes only to check feasibility of the approach. (computation details in Jupyter notebook _ASubbaraman-CarnaticBeatAnalysis-2.ipynb_)
- Several ‘classic’ classifiers were applied to the data, no hyperparameter tuning.

![ModelAccuracy_2Classes](https://github.com/user-attachments/assets/d02b3ec4-346b-4146-82a4-505c6b582e1d)

- RandomForest accuracy > 70%  (random guess = 50%)
- Reasonable results, given the small dataset.

## Exploratory Data Analysis - 2
Preliminary analysis using <b>2 classes</b> only to check the feasibility of using Neural Nets.
(computation details in Jupyter notebook _ASubbaraman-CarnaticBeatAnalysis_NN_2Classes.ipynb_)

![model_valid_accuracy_line_NN_2](https://github.com/user-attachments/assets/f573ec0f-8d8b-4f79-ab9e-8ea9531cb2ef)

- Neural Net accuracy approaches 80% (example model with 7 hidden layers, 30 neurons per layer). Reasonable results, given the small dataset.

![model_accuracy_vs_epoch_NN_2](https://github.com/user-attachments/assets/205fdb39-73e0-4359-8fd4-8433d0a6bbec)
- <b>Accuracy vs number of epochs</b> for a typical model. Dependence is already asymptotic. We will use epochs = 100 in the detailed analysis.

## Full Data Analysis - Classic Classifiers - 1
- Full analysis with 3 classes, deploying ‘classic’ ML/AI classifiers with hyperparameter tuning. (computation details in Jupyter notebook _ASubbaraman-CarnaticBeatAnalysis-3.ipynb_)
- Data was split into training (80%) and test (20%) data sets. 
- Model accuracy on the test data is shown below.

![ModelAccuracy-classic](https://github.com/user-attachments/assets/05669f01-63af-43c5-a8d7-e5379e428c03)

- <b>KNeighbors, RandomForest, AdaBoost:</b> accuracies ≅ 60%. (Random guess = 33%)
- <b>Hyperparameters </b>
- KNeighbors  : n_neighbors = 6
- RandomForest: n_trees = 4
- AdaBoost    : n_estimators = 200,  max_depth = 5
        
## Full Data Analysis - Classic Classifiers - 2
- Model computation time on the training data is shown below.

![ModelTime_classic](https://github.com/user-attachments/assets/d632685b-2edf-48e3-b66a-3f6f39d75aee)

- <b>KNeighbors,RandomForest:</b> Very speedy (and among most accurate from previous slide)
- <b>AdaBoost:</b> Somewhat slower 

## Full Data Analysis - Neural Nets
- Search performed over Sequential networks to optimize number of hidden layers and number of nodes (neurons) in each layer. All layers have the same number of nodes.
- (computation details in Jupyter notebook _ASubbaraman-CarnaticBeatAnalysis_NN_3Classes.ipynb_)
- Epochs kept fixed at 100 from earlier analysis of accuracy vs epochs.
- Accuracy on the test (validation) data are below.

![model_valid_accuracy_line_NN_3](https://github.com/user-attachments/assets/26feb6da-d3a5-48d9-afcc-5131bf8934dc)

- Best-performing models have: 4 to 8 hidden layers, 30 - 60 nodes
- Best performers are <b>50 to 66%</b> accurate (about twice the accuracy of random guess)

## Discussion of Results
- Classic ML/AI classification tools have an accuracy of about 60% (random guess = 33%)
- Neural networks have an accuracy of 50 to 66%.
- About twice as accurate as random guess.
- <b>Results are preliminary</b> due to the small dataset size and will most likely change as more data is generated.

#### Comparison with Human Performance
- Above ML/AI accuracy <b>probably already exceeds a lay listener’s performance.</b> Informal conversations indicate that a technical understanding of rhythm is quite hard for most people.
- Probably will not match a Carnatic percussion expert’s accuracy.

## Future Steps
- <b>Data, Data, Data!</b>
- Real estate adage: 3 most important aspects in real estate are location, location, location.
- For  an ML/AI project, one could say data, data, data. 
- Must increase the dataset size significantly, will require automated direct analysis of the mp3 audio file rather than manual image capture.
- Shout out to the often under-appreciated art of high-quality data collection - perhaps overlooked these days when data can be downloaded with the click of a button.

<b>How experts identify the beat:</b> Feedback from a couple of percussion experts: Each beat cycle has <b>signature rhythmic phrases.</b> Experts identify <b>rhythmic phrases embedded within a solo</b> to identify the beat cycle. <br/>
- This suggests that a filter-based neural network which filters (selects) rhythmic phrases, ie. a <b>Convolutional Neural Network</b> might be suitable for this task.
