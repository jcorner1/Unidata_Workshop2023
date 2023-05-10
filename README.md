<p>
    <img src="https://raw.githubusercontent.com/jcorner1/NIU_Masters/main/Masters_Thesis/NIU_Emblem.png" width="200" height="50" />
</p>

# 2023 Unidata Workshop - Methods on Verifying Convective Mode
This project is the result of a year and a half of work on my master's thesis at Northern Illinois University. The overarching goal of the project has been to develop a method of verifying convective mode witin the High-Resoultion Rapid Refresh (HRRR). 

## Data
There are a few different types of data used in this project. Firstly, the model data used is HRRR. These data can be found at this <a href="https://console.cloud.google.com/storage/browser/high-resolution-rapid-refresh;tab=objects?prefix=&forceOnObjectsSortingFiltering=false"> Google Repository</a>. Addtional information on HRRR including updates, microphysics schemes, and other facts can be found <a href="https://rapidrefresh.noaa.gov/hrrr/">here</a>. 

Since HRRR is being verified, an observational dataset is also needed. The data used here is the Multi-Radar/Multi-Sensor (MRMS) reflectivity product. These data can be found at the <a href="https://mesonet.agron.iastate.edu/GIS/rasters.php?rid=4"> Iowa State Mesonet Page</a>. Finally, a notebook to download the data and also remove unneeded products from the netCDF file can be found <a href="https://github.com/jcorner1/Unidata_Workshop2023/blob/main/Code/Data_download_change.ipynb">here</a>. The data is augmented to save storage space.

## Methods

### Machine Learning

### Method of Object-based Diagnostic Evaluation
The Method of Object-based Diagnostic Evaluation or MODE is a feature-based verification technique. Information on how to download and use MODE or other model evualtion tools can be found at the Developmental Testbed Center. Furthermore, the literature shows the many different uses of MODE.  

## Results

## Author Information
Jeremy Corner - M.S. Student &nbsp; &nbsp; &nbsp;  <a href="https://github.com/jcorner1">  Github </a> &nbsp; | &nbsp;<a href="https://twitter.com/JcornerWx">  Twitter </a> &nbsp; | &nbsp; <a href="mailto:jcorner1@niu.edu">  Email </a>


## Committee Members

Alex Haberlie - Advisor (Chair)  &nbsp; &nbsp; &nbsp;  <a href="http://www.svrimg.org">  SVRIMG </a> &nbsp; | &nbsp; <a href="https://github.com/ahaberlie">  Github </a> &nbsp; | &nbsp; <a href="https://twitter.com/alexhabe">  Twitter </a> &nbsp; | &nbsp; <a href="https://ahaberlie.github.io/">  Website </a> &nbsp; | &nbsp; <a href="https://scholar.google.com/citations?user=HvnxYVAAAAAJ">  Google Scholar </a> 

Victor Gensini - Member &nbsp; &nbsp; &nbsp;  <a href="https://github.com/vgensini">  Github </a> &nbsp; | &nbsp; <a href="https://twitter.com/gensiniwx?lang=en">  Twitter </a> &nbsp; | &nbsp; <a href="https://atlas.niu.edu/">  Website </a> &nbsp; | &nbsp; <a href="https://scholar.google.com/citations?user=qyLBZwkAAAAJ&hl">  Google Scholar </a>

Walker Ashley -  Member &nbsp; &nbsp; &nbsp;

Scott Collis - Collaborator &nbsp; &nbsp; &nbsp;
