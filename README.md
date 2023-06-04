<p>
    <img src="https://raw.githubusercontent.com/jcorner1/NIU_Masters/main/Masters_Thesis/NIU_Emblem.png" width="200" height="50" />
</p>

# Machine Learning to Declare Convective Mode
Future climates are likely to lead to large-scale changes in atmospheric conditions and therefore affect convective weather. With these anticpated changes, convective mode is also expected to expecience substaintial depatures from normal. As convective mode correlates with natural hazards (i.e., hail, tornadoes, wind gusts), severity and magnitudes will also change in these hazards. 

## Data
These data are unique as they follow a couple Representative Concentration Pathways (RCP; both the 4.5 and 8.5 scenarios are modeled in these data) which accounts for radiative forcing caused by increased levels of greenhouse gases within the atmosphere. RCPs are outlined in the <a href="https://www.ipcc.ch/report/ar5/syr/">Intergovernmental Panel on Climate Change 5th Assessment Report</a> addressing potential atmospheric responses to global climate change. The 4.5 scenario represents a peak in 2040 in carbon emmissions and then steady decline till the end. RCP 4.5 is considered an intermediate pathway the most likely to happen in future climates. The 8.5 scenario is indicative of a worst-case situation with no decline in carbon emmissions through 2100. Futhermore, a historic period of data from 1990 to 2005 is provided as baseline to be used as a compartive tool of same data type/format. For access to these data, please contact Victor Gensini (information provided at the bottom of the page). For more information, please read the data paper <a href="https://link.springer.com/article/10.1007/s00382-022-06306-0">here</a>. 

## Methods


### Machine Learning
A deep-learning technique called a Convolutional Neural Network (CNN) is employed to classify convective mode. Input images centered on the particular convective object (i.e., thunderstorm) are sized 136 by 136 pixels. A traditional training/validation/testing split of (will be listed later) is done to ensure the model performs as best as it can. Finally, a hundred epochs is used to adquately train the model.  A notebook on how the CNN was developed and trained can be found here. The below figure matches the images used to train the CNN. 

<p>
    <img src="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Plots/Single_img.png?raw=true" width="343" height="315" />
</p>

#### Data Augmentation
The sample size of the oringinal data is ... Therefore, some effort is put into bolstering the size of the data as well as reducing overfitting in the model. For this project, a method of data augmentation is employed by rotating the image arounds its centroid. This method is important as it preserves the thunderstorm in the center of the image. The figure below demostrates this concept by showing 9 augmented images of the previous figure. 

<p>
    <img src="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Plots/data_aug_img.png?raw=true" width="856" height="817" />
</p>

### Gridded Data

## Results
Since the poster can show so many results, this section can hopefully round out the rest of the results not seen as well as the figures shown on the poster. 

### Classification of Convective Mode
Using the aforementioned CNN, convective mode could be declared for each image generated. However, first we must address the training stages of the CNN model before proccedding to the next step.

#### Model Training

## Future Work


## Author Information
Jeremy Corner - M.S. Student &nbsp; &nbsp; &nbsp;  <a href="https://github.com/jcorner1">  Github </a> &nbsp; | &nbsp;<a href="https://twitter.com/JcornerWx">  Twitter </a> &nbsp; | &nbsp; <a href="mailto:jcorner1@niu.edu">  Email </a>


## Committee Members

Alex Haberlie - Advisor (Chair)  &nbsp; &nbsp; &nbsp;  <a href="http://www.svrimg.org">  SVRIMG </a> &nbsp; | &nbsp; <a href="https://github.com/ahaberlie">  Github </a> &nbsp; | &nbsp; <a href="https://twitter.com/alexhabe">  Twitter </a> &nbsp; | &nbsp; <a href="https://ahaberlie.github.io/">  Website </a> &nbsp; | &nbsp; <a href="https://scholar.google.com/citations?user=HvnxYVAAAAAJ">  Google Scholar </a> 

Victor Gensini - Member &nbsp; &nbsp; &nbsp;  <a href="https://github.com/vgensini">  Github </a> &nbsp; | &nbsp; <a href="https://twitter.com/gensiniwx?lang=en">  Twitter </a> &nbsp; | &nbsp; <a href="https://atlas.niu.edu/">  Website </a> &nbsp; | &nbsp; <a href="https://scholar.google.com/citations?user=qyLBZwkAAAAJ&hl">  Google Scholar </a>

Walker Ashley -  Member &nbsp; &nbsp; &nbsp; <a href="https://twitter.com/WalkerSAshley">  Twitter </a> &nbsp; | &nbsp; <a href="https://chubasco.niu.edu/"> Website </a> &nbsp; |  &nbsp; <a href="https://scholar.google.com/citations?user=SwhAm7IAAAAJ&hl">  Google Scholar </a>

Scott Collis - Collaborator &nbsp; &nbsp; &nbsp; <a href="https://github.com/scollis">  Github </a> &nbsp; | &nbsp; <a href="https://twitter.com/Cyclogenesis_au">  Twitter </a> &nbsp; | &nbsp; <a href="https://opensky.press/"> Website </a> &nbsp; |  &nbsp; <a href="https://scholar.google.com/citations?hl=en&user=eMCDQDIAAAAJ">  Google Scholar </a>
