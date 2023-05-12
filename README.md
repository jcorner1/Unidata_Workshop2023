<p>
    <img src="https://raw.githubusercontent.com/jcorner1/NIU_Masters/main/Masters_Thesis/NIU_Emblem.png" width="200" height="50" />
</p>

# Methods on Verifying Convective Mode
This project is the result of a year and a half of work on my master's thesis at <a href="https://wcs.niu.edu/">Northern Illinois University</a>. The overarching goal of the project has been to develop a method of verifying convective mode witin the High-Resoultion Rapid Refresh (HRRR). 

## Data
There are a few different types of data used in this project. Firstly, the model data used is HRRR. These data can be found at this <a href="https://console.cloud.google.com/storage/browser/high-resolution-rapid-refresh;tab=objects?prefix=&forceOnObjectsSortingFiltering=false"> Google Repository</a>. Addtional information on HRRR including updates, microphysics schemes, and other facts can be found <a href="https://rapidrefresh.noaa.gov/hrrr/">here</a>. 

Since HRRR is being verified, an observational dataset is also needed. The data used here is the Multi-Radar/Multi-Sensor (MRMS) reflectivity product. These data can be found at the <a href="https://mesonet.agron.iastate.edu/GIS/rasters.php?rid=4"> Iowa State Mesonet Page</a>. Finally, a notebook to download the data and also remove unneeded products from the netCDF file can be found <a href="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Code/Data_download_change.ipynb">here</a>. The data is augmented to save storage space.

## Methods
Below are the methods used to generate the results from this project. A rough synopsis of the methods however is downloading the data mentioned previously and finding the thunderstorms (i.e., convective objects) in each. Next, a machine learning model is used to declare each convective object's storm mode. For this study, the storm modes we used were: (will list these later). Now, a verification technique can be used to determine the various bias associated with each storm mode in HRRR. 

### Convective Objects
Convective objects are the individual storms within the data. Objects are formed using a threshold of 30-dBZ with a convective threshold of 40-dBZ and an intense threshold of 50-dBZ. These objects are found using functions from <a href="https://scikit-image.org/">Scikit-Image</a>. To eliminate small, unwanted convective objects a threshold of 25 pixels for MRMS data and 8 pixels for HRRR. The pixel counts factor in the difference in the grid spacing of each dataset. Finally, the notebooks used to create these objects can be found here for HRRR and here for MRMS.

### Machine Learning


### Method of Object-based Diagnostic Evaluation
The Method of Object-based Diagnostic Evaluation or MODE is a feature-based verification technique. Information on how to download and use MODE or other <a href="https://dtcenter.org/community-code/model-evaluation-tools-met">Model Evaluation Tools</a> (MET) and METplus can be found at the <a href="https://dtcenter.org/">Developmental Testbed Center</a>. Furthermore, the literature shows the many different uses of MODE.  

MODE settings...

## Results
Since the poster can show so many results, this section can hopefully round out the rest of the results not seen as well as the figures shown on the poster. 

## Future Work
- Use a convolutional neural network algorithm to declare convective mode. 

## Author Information
Jeremy Corner - M.S. Student &nbsp; &nbsp; &nbsp;  <a href="https://github.com/jcorner1">  Github </a> &nbsp; | &nbsp;<a href="https://twitter.com/JcornerWx">  Twitter </a> &nbsp; | &nbsp; <a href="mailto:jcorner1@niu.edu">  Email </a>


## Committee Members

Alex Haberlie - Advisor (Chair)  &nbsp; &nbsp; &nbsp;  <a href="http://www.svrimg.org">  SVRIMG </a> &nbsp; | &nbsp; <a href="https://github.com/ahaberlie">  Github </a> &nbsp; | &nbsp; <a href="https://twitter.com/alexhabe">  Twitter </a> &nbsp; | &nbsp; <a href="https://ahaberlie.github.io/">  Website </a> &nbsp; | &nbsp; <a href="https://scholar.google.com/citations?user=HvnxYVAAAAAJ">  Google Scholar </a> 

Victor Gensini - Member &nbsp; &nbsp; &nbsp;  <a href="https://github.com/vgensini">  Github </a> &nbsp; | &nbsp; <a href="https://twitter.com/gensiniwx?lang=en">  Twitter </a> &nbsp; | &nbsp; <a href="https://atlas.niu.edu/">  Website </a> &nbsp; | &nbsp; <a href="https://scholar.google.com/citations?user=qyLBZwkAAAAJ&hl">  Google Scholar </a>

Walker Ashley -  Member &nbsp; &nbsp; &nbsp; <a href="https://twitter.com/WalkerSAshley">  Twitter </a> &nbsp; | &nbsp; <a href="https://chubasco.niu.edu/"> Website </a> &nbsp; |  &nbsp; <a href="https://scholar.google.com/citations?user=SwhAm7IAAAAJ&hl">  Google Scholar </a>

Scott Collis - Collaborator &nbsp; &nbsp; &nbsp; <a href="https://github.com/scollis">  Github </a> &nbsp; | &nbsp; <a href="https://twitter.com/Cyclogenesis_au">  Twitter </a> &nbsp; | &nbsp; <a href="https://opensky.press/"> Website </a> &nbsp; |  &nbsp; <a href="https://scholar.google.com/citations?hl=en&user=eMCDQDIAAAAJ">  Google Scholar </a>
