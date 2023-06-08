<p>
    <img src="https://raw.githubusercontent.com/jcorner1/NIU_Masters/main/Masters_Thesis/NIU_Emblem.png" width="200" height="50" />
</p>

# Machine Learning to Declare Convective Mode in Future Climates
Future climates are likely to lead to large-scale changes in atmospheric conditions and therefore affect convective weather. With these anticipated changes, convective mode is also expected to experience a substantial departure from normal. As convective mode correlates with natural hazards (i.e., hail, tornadoes, wind gusts), severity and magnitudes will also change in these hazards.

## Data
These data are unique as they follow a couple of Representative Concentration Pathways (RCP; both the 4.5 and 8.5 scenarios are modeled in these data) which accounts for radiative forcing caused by increased levels of greenhouse gases within the atmosphere. RCPs are outlined in the <a href="https://www.ipcc.ch/report/ar5/syr/">Intergovernmental Panel on Climate Change 5th Assessment Report</a> addressing potential atmospheric responses to global climate change. The 4.5 scenario represents a peak in 2040 in carbon emissions and then a steady decline till the end. RCP 4.5 is considered an intermediate pathway the most likely to happen in future climates. The 8.5 scenario is indicative of a worst-case situation with no decline in carbon emissions through 2100. 

<p>
    <img src="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Plots/All_forcing_agents_CO2_equivalent_concentration.svg.png?raw=true" width="600" height="320" />
</p>

Furthermore, a historic period of data from 1990 to 2005 is provided as a baseline to be used as a comparative tool of the same data type/format. For access to these data, please contact Victor Gensini (information provided at the bottom of the page). For more information, please read the data paper <a href="https://link.springer.com/article/10.1007/s00382-022-06306-0">here</a>.

## Methods
During a discussion during the workshop, the idea of making these methods malleable or to state it plainly... make it so this work is simple enough for a user so they can be changed/deleted/added as seen fit. For example, this work uses a CNN to classify convective mode within reflectivity images from model data. However, another person might want to use a CNN to classify different cloud regimes in GOES-17 imagery. Therefore, these methods are explained to a length to allow a user that has decent expercience with Python but might have fairly low expercience with machine learning. The goal of this section is to provide explanation on not only the code provided, but to explcity state which lines are essentail and which can be changed.

### Machine Learning
A deep-learning technique called a Convolutional Neural Network (CNN) is employed to classify convective mode. To use a CNN however, a person would first need to train the model. Input images centered on the particular convective object (i.e., thunderstorm) are sized 136 by 136 pixels. A traditional training/validation/testing split of (will be listed later) is done to ensure the model performs as best as it can.

```
#set input values
num_classes = 6
input_shape = (136, 136, 1)

#create the training, validation, testing splits
x_train = np.load("train_imgs_1996-2011.npy")
y_train = np.load("train_classes_1996-2011.npy")

x_val = np.load("validation_imgs_2012-2013.npy")
y_val = np.load("validation_classes_2012-2013.npy")

x_test = np.load("test_imgs_2014-2017.npy")
y_test = np.load("test_classes_2014-2017.npy")

y_train = keras.utils.to_categorical(y_train, num_classes)
y_val = keras.utils.to_categorical(y_val, num_classes)
y_test = keras.utils.to_categorical(y_test, num_classes)

```

Finally, a hundred epochs are used to adequately train the model. A notebook on how the CNN was developed and trained can be found here. The below figure matches the images used to train the CNN. 

<p>
    <img src="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Plots/Single_img.png?raw=true" width="343" height="315" />
</p>

#### Data Augmentation
The sample size of the original data is around 6000 images. Therefore, some effort is put into bolstering the size of the data as well as reducing overfitting in the model. For this project, a method of data augmentation is employed by rotating the image around its centroid. This method is important as it preserves the thunderstorm in the center of the image. The figure below demonstrates this concept by showing 9 augmented images of the previous figure. 

<p>
    <img src="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Plots/data_aug_img.png?raw=true" width="856" height="817" />
</p>

### Gridded Data
These data are stored as netCDF files with various variables such as reflectivity, updraft helicity, wind gusts, etc. Although this is an ongoing project, the current work involves thresholding both updraft helicity (75+) and reflectivity (50+). Grid points meeting both criteria are used as center points for storms. These center points will then later be used to generate images with the CNN to declare convective mode.

#### Improving Images
Following some feedback given during the poster session held on first day on of the Unidata Users Workshop, some effort was put into better conveying storm frequency than the plots shown in the <a href="https://drive.google.com/file/d/1vF86cTyBOierITyeEUdCA9pWz4J_llVX/view?usp=sharing">poster</a>. Firstly, those plots are hard to see the frequency as they are shown on the native WRF grid spacing of 3.75-km. Therefore, the data was coarsened using xarray to increase the grid spacing to 75-km and therefore easier to view while also showing more regional-scale frequency instead of local-scale. Next, the colorbar is based on the max value for each season as opposed to the max value for all the seasons. Therefore, it is important to update the colorbar to show the overall max, making it more intuitive to a person which season is more active. The below plot shows the activity for all seasons with these important aspects being used. These improvements are then used to show the data in the results portion upcoming. 

<p>
    <img src="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Plots/Storm_Reports_HIST_ALL.png?raw=true" width="744" height="459" />
</p>

## Results
Since the poster can show so many results, this section can hopefully round out the rest of the results not seen as well as the figures shown on the poster. 

### Classification of Convective Mode
Using the aforementioned CNN, the convective mode could be declared for each image generated. However, first, we must address the training stages of the CNN model before proceeding to the next step. The below images show how the CNN model improved (or didn't) in accuracy with epoch. Overall, training CNN results in a maximum accuracy of around 90%.

<p>
    <img src="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Plots/Model_perf_line.png?raw=true" width="609" height="380" />
</p>

Another classic machine learning product is the confusion matrix. An algorithm that is performing is easily identified in a confusion matrix as the bigger numbers should create a "line" with a slope of -1 (or top-left to bottom-right). As stated above, the CNN used in this project does a good job of correctly identifying storm mode. Therefore the confusion matrix below shows the desired effect talked about previously.

<p>
    <img src="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Plots/confusion_matrix.png?raw=true" width="479" height="363" />
</p>


### Gridded Storm Data
The gridded data results are broken up by season to better show not the spatial occurrence but also the temporal. The first plot below shows storm occurrences for the Winter months (December, January, and February). This shows some activity in the Gulf states, primarily in and around the Mississippi River Valley. 

<p>
    <img src="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Plots/COR_DJF_HIST_75UH50REF.png?raw=true" width="744" height="459" />
</p>

The next plot below shows the Spring months (March, April, and May). There is heavy and distinct activity present in the Southern and Central Great Plains as well as some activity in the Mississippi and Ohio River Valley. The Spring months also show the most activity of all of the seasons in terms of storm intensity. 

<p>
    <img src="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Plots/COR_MAM_HIST_75UH50REF.png?raw=true" width="744" height="459" />
</p>

The plot below shows activity for the Summer months (June, July, and August). This shows that activity migrates further north into the Northern Great Plains and Western Great Lake States. Although not as active as the Spring months, this season is most definitely comparable.

<p>
    <img src="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Plots/COR_JJA_HIST_75UH50REF.png?raw=true" width="744" height="459" />
</p>

Finally, the Fall months are shown below. This season shows a distinct drop off in terms of events but there is a cluster in the Northern Mississippi River Valley. 

<p>
    <img src="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Plots/COR_SON_HIST_75UH50REF.png?raw=true" width="744" height="459" />
</p>

## Future Work
- Use CNN created in this project to classify the convective mode for each storm identified using the center point algorithm.
- Create strom reports including location, intensity, other convective information, etc.

## Packages
This work would have not been possible if not for the development and maintence of many open-source Python packages. These packages and some of the commonly used functions are provided below:

- **Xarray** - A library normally used when working with gridded data such as netCDF and grib files. Some of the fuctions used is this work includes:
    - **Coarsen** - Makes data more coarse (i.e., lowers the resoultion or increases grid spacing).
    - **Where** - Find values of certain logic (greater than or equal to 40). Function also allows augmentation of data by performing basic math on the values meeting the criteria or setting it to a set number.  
- **TensorFlow** - A library that eases the development of workflows when using machine learning. TensorFlow is the most popularly used packages in the machine learning industry and some of the commonly used functions are listed next:
    - **Keras** - Important sublibrary housing different forms of deeper machine learning models. Most commonly used when working with nerual networks. 
- **Pandas** - A library used to working with tabular data such as csv. 
- **Numpy** - A library used when creating and changing data within an array. There are many simple functions used when "playing with the data", of which some are listed below:

## Author Information
Jeremy Corner - M.S. Student &nbsp; &nbsp; &nbsp;  <a href="https://github.com/jcorner1">Github</a> &nbsp; | &nbsp;<a href="https://twitter.com/JcornerWx">Twitter</a> &nbsp; | &nbsp; <a href="mailto:jcorner1@niu.edu">Email</a>


## Committee Members

Alex Haberlie - Advisor (Chair)  &nbsp; &nbsp; &nbsp;  <a href="http://www.svrimg.org">SVRIMG</a> &nbsp; | &nbsp; <a href="https://github.com/ahaberlie">Github</a> &nbsp; | &nbsp; <a href="https://twitter.com/alexhabe">Twitter</a> &nbsp; | &nbsp; <a href="https://ahaberlie.github.io/">Website</a> &nbsp; | &nbsp; <a href="https://scholar.google.com/citations?user=HvnxYVAAAAAJ">Google Scholar</a> 

Victor Gensini - Member &nbsp; &nbsp; &nbsp;  <a href="https://github.com/vgensini">Github</a> &nbsp; | &nbsp; <a href="https://twitter.com/gensiniwx?lang=en">Twitter</a> &nbsp; | &nbsp; <a href="https://atlas.niu.edu/">Website</a> &nbsp; | &nbsp; <a href="https://scholar.google.com/citations?user=qyLBZwkAAAAJ&hl">Google Scholar</a>

Walker Ashley -  Member &nbsp; &nbsp; &nbsp; <a href="https://twitter.com/WalkerSAshley">Twitter</a> &nbsp; | &nbsp; <a href="https://chubasco.niu.edu/">Website</a> &nbsp; |  &nbsp; <a href="https://scholar.google.com/citations?user=SwhAm7IAAAAJ&hl">Google Scholar</a>

Scott Collis - Collaborator &nbsp; &nbsp; &nbsp; <a href="https://github.com/scollis">Github</a> &nbsp; | &nbsp; <a href="https://twitter.com/Cyclogenesis_au">Twitter</a> &nbsp; | &nbsp; <a href="https://opensky.press/">Website</a> &nbsp; |  &nbsp; <a href="https://scholar.google.com/citations?hl=en&user=eMCDQDIAAAAJ">Google Scholar</a>
