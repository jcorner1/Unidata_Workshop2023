<p>
    <img src="https://raw.githubusercontent.com/jcorner1/NIU_Masters/main/Masters_Thesis/NIU_Emblem.png" width="200" height="50" />
</p>

# Machine Learning to Declare Convective Mode in Future Climates
Future climates will likely lead to large-scale changes in atmospheric conditions and therefore affect convective weather. With these anticipated changes, convective mode is also expected to experience a substantial departure from normal. As convective mode correlates with natural hazards (i.e., hail, tornadoes, wind gusts), severity and magnitudes will also change in these hazards.

## Objectives 
The objectives within this section are not limited to the scope of a normal scientific paper but also apply open-science concepts to this work. These objectives are as follows:
- Explain these methods in this project clearly and explicitly state which portions of code are needed and which lines can be modified for a different user's purpose.
- Demonstrate how the frequency of convective modes will change in different future climate change scenarios.

## Data
These data are unique as they follow a couple of Representative Concentration Pathways (RCP; both the 4.5 and 8.5 scenarios are modeled in these data), which account for radiative forcing caused by increased levels of greenhouse gases within the atmosphere. RCPs are outlined in the <a href="https://www.ipcc.ch/report/ar5/syr/">Intergovernmental Panel on Climate Change 5th Assessment Report</a> addressing potential atmospheric responses to global climate change. The 4.5 scenario represents a peak in 2040 in carbon emissions and a steady decline till the end. RCP 4.5 is considered an intermediate pathway the most likely to happen in future climates. The 8.5 scenario indicates a worst-case situation with no decline in carbon emissions through 2100. 

<p>
    <img src="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Plots/All_forcing_agents_CO2_equivalent_concentration.svg.png?raw=true" width="600" height="320" />
</p>

Furthermore, a historic period of data from 1990 to 2005 is provided as a baseline to be used as a comparative tool of the same data type/format. To access these data, don't hesitate to contact Victor Gensini (information provided at the bottom of the page). For more information, please read the data paper <a href="https://link.springer.com/article/10.1007/s00382-022-06306-0">here</a>.

## Methods
During a discussion during the workshop, the idea of making these methods malleable or, to state it plainly... making it so this work is simple enough for a user so they can be changed/deleted/added as seen fit. For example, this work uses a CNN to classify convective mode within reflectivity images from model data. However, another person might want to use a CNN to classify different clouds in GOES-17 imagery. Therefore, these methods are explained to a length to allow a user with decent experience with Python but fairly low exposure to machine learning. This section explains the code provided and explicitly states which lines are essential and which can be changed. **Note:** not all lines in each notebook are explained as that would take up too much time and space. However, the most barebones code is explained with other less important aspects just being shown in the only notebooks. 

### Machine Learning
A deep-learning technique called a Convolutional Neural Network (CNN) is employed to classify convective mode. Input images centered on the particular convective object (i.e., a thunderstorm) are 136 by 136 pixels. The number of classes depends strictly on the unique labels the user assigns to images in the training dataset. The height and width can vary; however, they must remain consistent for each image in the dataset. There are many different methods to find a convective object, with one described later in this section. A traditional training/validation/testing split of 70/10/20 is done to ensure the model performs as best as possible. The percentages can be changed, but as stated before is a common practice in the machine-learning community. Normilazations are an important step when preprocessing data before use in a machine-learning algorithm. These methods divide the dBz value by 80 and set the data type as a float. This is done to convert values to a fraction (convert values to a number between 0 and 1). When working with simple RBG images, the values for the three color channels are divided by 255 to achieve this effect. 

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

#Normalize by 80 dBZ
x_train = x_train.astype("float32") / 80
x_test = x_test.astype("float32") / 80
x_val = x_val.astype("float32") / 80

```
The images used as the input data for the machine-learning algorithm can be seen below. 

<p>
    <img src="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Plots/Single_img.png?raw=true" width="343" height="315" />
</p>

#### Data Augmentation
The sample size of the original dataset is around 6000 images. Therefore, some effort is put into bolstering the data's size and reducing overfitting in the model. For this project, a data augmentation method is employed by rotating the image around its centroid. This method is important as it preserves the thunderstorm in the center of the image. The figure below demonstrates this concept by showing nine augmented images of the previous figure. 

<p>
    <img src="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Plots/data_aug_img.png?raw=true" width="856" height="817" />
</p>

The code below demonstrates how to achieve this effect. 

```
from tensorflow.keras.preprocessing.image import ImageDataGenerator
datagen = ImageDataGenerator(rotation_range=55, zoom_range=[0.9,1.0], fill_mode="reflect")
```

#### Creating the CNN
First, the different layers, inputs, and other attributes must be set before inserting data into the model. 

```
model = keras.Sequential(
    [
        keras.Input(shape=(136, 136, 1)),
        layers.Conv2D(32, kernel_size=(3, 3), activation="relu"),
        layers.SpatialDropout2D(0.3),
        layers.MaxPooling2D(pool_size=(3, 3)),
        layers.Conv2D(64, kernel_size=(3, 3), activation="relu"),
        layers.SpatialDropout2D(0.3),
        layers.MaxPooling2D(pool_size=(3, 3)),
        layers.Conv2D(128, kernel_size=(3, 3), activation="relu"),
        layers.SpatialDropout2D(0.3),
        layers.MaxPooling2D(pool_size=(3, 3)),
        layers.Flatten(),
        layers.Dense(128, activation="relu"),
        layers.Dropout(0.6),
        layers.Dense(num_classes, activation="softmax"),
    ]
)

keras.utils.plot_model(model, show_shapes=True)
```
Next, data can be inserted into the model for training.  

```
epochs = 100
model.compile(loss="categorical_crossentropy", optimizer="adam", metrics=["accuracy"])
history = model.fit(datagen.flow(x_train, y_train, batch_size=32),
                                 epochs=epochs, validation_data=(x_val, y_val), workers=8)
```


Finally, the notebook demonstrated above can be found <a href="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Code/Train_CNN_Model.ipynb">here</a>. Also, an additional example using pngs instead of numpy files can be found here. That code is adapted heavily from this <a href="https://www.tensorflow.org/tutorials/load_data/images">example</a> from a tutorial from the TensorFlow website. Furthermore, the <a href="https://www.tensorflow.org">TensorFlow website</a> and its <a href="https://www.tensorflow.org/tutorials">tutorial page</a> can be highly beneficial in learning to work with CNNs and other deep-learning algorithms. 

### Gridded Data
These data are stored as netCDF files with variables such as reflectivity, updraft helicity, wind gusts, etc. Although this project is ongoing, the current work involves thresholding updraft helicity (75+) and reflectivity (50+). Grid points meeting both criteria are used as center points for storms. These center points will then later be used to generate images with the CNN to declare convective mode.

#### Improving Images
Following some feedback given during the poster session held on the first day of the Unidata Users Workshop, some effort was put into better conveying storm frequency than the plots shown in the <a href="https://drive.google.com/file/d/1vF86cTyBOierITyeEUdCA9pWz4J_llVX/view?usp=sharing">poster</a>. Firstly, those plots are hard to see the frequency as they are shown on the native WRF grid spacing of 3.75-km. Therefore, the data was coarsened using Xarray to increase the grid spacing to 75 km and easier to view while also showing more of a regional-scale frequency instead of a local scale. Next, the color bar is based on the maximum value for each season instead of the maximum value for all seasons. Therefore, updating the color bar to show the overall max is important, making it more intuitive to a person what season is more active. The below plot shows the activity for all seasons with these important aspects being used. These improvements are then used to show the data in the results portion upcoming. 

<p>
    <img src="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Plots/Storm_Reports_HIST_ALL.png?raw=true" width="744" height="459" />
</p>

## Results
Since the poster can only show so many results, this section rounds out the rest of the results not seen, as well as the figures shown on the poster. 

### Classification of Convective Mode
The convective mode could be declared using the aforementioned CNN for each image generated. However, first, we must address the training stages of the CNN model before proceeding to the next step. The below images show how the CNN model improved (or didn't) in accuracy with epoch. Overall, training CNN results in a maximum accuracy of around 90%.

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
- Use XAI techniques to identify how the CNN classifies convective mode.

Furthermore, there are also a lot of objectives to complete when updating this file. Some of these include:
- Adding explanations for a majority, if not all code blocks used in the various notebooks. 
- Clearly stating the objectives and conclusions of this project.


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
