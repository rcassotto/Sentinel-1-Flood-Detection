# Sentinel-1-Flood-Detection
Python based tools to detect flood indundation through bimodal thresholding.  The tool uses GRD-derived observations to produce binary flood maps.  It incorporates a geotiff of water extent from the Global Surface Water data set (https://global-surface-water.appspot.com/download) by Pekel et al, (2016) to delineate permanent water bodies from ephemeral flooded regions. 

## Initial Setup 
Download or clone the repository to your local machine. 

It is highly recommended to create a dedicated python environment to operate in. Many python package tools are available; I use anaconda (https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).

Once a dedicated environment is created and activated, install the necessary python dependencies (see below).

Once the python environment is configured, install additional non-python dependencies before running the workflow. 

### Python Dependencies
geopandas<br> matplotlib<br> numpy<br> rasterio<br> scipy<br> shapely<br> simplekml<br> skimage<br>

### Non-Python Dependencies
Two additional dependecies are required to run the workflow; both are open sourced. 
- gdal.
- The SeNtinel Application Platform (SNAP).
- EarthData account (it's free!)

  #### _GDAL_
  The Geospatial Data Abstraction Library is called within the python framework. **_If you installed gdal with python in the   evironment above, you can copy the full path to the installed binary for gdal_translate and paste it where indicated in the python scripts below. You do not have to install gdal seperately_.** 

  #### _SNAP_
  ESA's SNAP program is required to pre-process the SLC and GRD data to coherence and sigma0 images, respectively. Follow the instructions on ESA's website to install SNAP (https://step.esa.int/main/download/snap-download/). Once installed, note the full path of the gpt tool. You will need to enter it in the necessary scripts identified below.
  
  _Note: A SNAP update may be necessary, even after a fresh install. <br> E.g._
       **_/local_path_to_snap/snap/bin/snap --nosplash --nogui --modules --update-all_**


## Detailed workflow

### Step 1 - Download SAR data from the Alaska SAR Facility (ASF)
<br>
There are two options. 
1) Use ASF's vertex tool to identify and download Sentinel-1 GRD scenes for your region of interest. 
2) Use ASF's API tool to automate this process using a wkt polygon. An example is provided [here](https://github.com/rcassotto/Sentinel-1-GRD-to-RTC-Pre-Processing/tree/main/ASF_API). 


# Citation
Jean-Francois Pekel, Andrew Cottam, Noel Gorelick, Alan S. Belward, High-resolution mapping of global surface water and its long-term changes. Nature 540, 418-422 (2016). (doi:10.1038/nature20584)
