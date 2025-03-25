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

### Step 1 - Download SAR data from the Alaska SAR Facility (ASF).
<br>
There are two options. 
1) Use ASF's vertex tool to identify and download Sentinel-1 GRD scenes for your region of interest. 
2) Use ASF's API tool to automate this process using a wkt polygon. An example is provided here: https://github.com/rcassotto/Sentinel-1-GRD-to-RTC-Pre-Processing/tree/main/ASF_API. 

### Step 2 - Pre-process Ground Range Detected (GRD) data to Radiometrically Terrain Corrected (RTC) Sigma0 images.
<br>
The _RTC_V3.py_ script will use SNAP to pre-process GRD to RTC Sigma0 geotiffs. Specifically, it applies an orbit correction, removes border noise, calibrates the data, applies a speckle filter, terrain corrects and generates Sigma0 geotiffs; it also removes ancillary products generated during the pre-processing steps. 

  #### Initial Setup
  Make the following changes prior to running this script for the first time: <br>
    1) Open the script _RTC_V3.py_ with a python editor. <br>
    2) Perform a search and replace for the following fields <br>
    <br> &emsp;&emsp;    - "usr/local/bin/gdal_translate" with the location of gdal_translate on your machine. <br>
    <br> &emsp;&emsp;    - baseSNAP: replace the current path with the location of where the SNAP gpt binary is located. <br>

  #### Executing Step 2
  1) Use a text editor to open the input file: _rtc_sample_inputs.txt_.
  2) Amend the inputs for your configuation <br>
       - _DEM_: DEM path and filename (optional); the program will default to SRTM if not specified. <br>
       - _grd_file_loc_: full path of the GRD zip files. <br>
       - _output_dir_: full path of the output directory for the output files. <br>
       - _pixsize_: desired output pixel size in meters. <br>
  3) Initiate the python script: **_python3 RTC_V3.py rtc_sample_inputs.txt_** <br>

### Step 3 - Download a permanent water geotiff.
<br>
The thresholding script will accept any geotiff to identify permanant water bodies. I use the Pekel et al's (2016) global surface water data product (https://global-surface-water.appspot.com/download).

### Step 4
1) Open the _SAR_Flood_Detection_v02.py_ module with a python editor. Change the path for gdal_dir on line 23 to the location on your local system. Save and exit.

2) Use a text editor to open the text input file (e.g. flood_detection_sample_inputs.txt) and modify to reflect your inputs. Save and exit.

3) Execute the algorithm via: _python3 SAR_Flood_Detection_v02.py flood_detection_sample_inputs.txt_.



# Citation
Jean-Francois Pekel, Andrew Cottam, Noel Gorelick, Alan S. Belward, High-resolution mapping of global surface water and its long-term changes. Nature 540, 418-422 (2016). (doi:10.1038/nature20584)
