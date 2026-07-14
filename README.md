# ExplorerDiaries-IMD-Rainfall_data_analysis
Kerala Rainfall Analysis using IMD Gridded Data (2000–2020)

A Python workflow to download, clean, clip, and analyze 20 years of high-resolution gridded daily rainfall data from the India Meteorological Department (IMD), focused on the state of Kerala.

Overview

This notebook demonstrates an end-to-end pipeline for working with IMD's binary gridded rainfall datasets:


Download daily rainfall data (2000–2020) using imdlib
Convert the binary grid to an xarray Dataset for analysis
Clean the data by masking IMD's -999 nodata sentinel value
Clip the national grid to Kerala's boundary using a GADM administrative shapefile
Analyze spatial-average rainfall at daily and annual resolution


Data Sources

DataSourceGridded rainfall (0.25° × 0.25°)IMD Pune via imdlibKerala state boundaryGADM v4.1

Method Notes


IMD's raw grid stores nodata as -999 rather than a proper NaN/nodata flag, so it is explicitly masked out (rain.where(rain['rain'] != -999.)) before analysis.
The raster CRS is set to match the shapefile CRS (EPSG:4326) prior to clipping with rioxarray.
rio.clip(..., all_touched=True) is used so that boundary pixels touching Kerala are retained.
Spatial averaging is computed across the lat/lon dimensions to produce a single daily rainfall time series for the state, which is then aggregated to annual totals via groupby('time.year').


Tech Stack


imdlib — IMD data access
xarray — gridded/labelled array handling
rioxarray — raster CRS, clipping, geospatial ops
geopandas / shapely — vector boundary handling
matplotlib — visualization


Repository Structure

.
├── IMD_Data_Analysis.ipynb   # Main analysis notebook
└── README.md

Outputs


Raw vs. nodata-masked rainfall grids
Kerala-clipped rainfall overlay maps
Daily spatially-averaged rainfall time series (2000–2020)
Total annual rainfall trend for Kerala


Getting Started

bashpip install imdlib xarray geopandas rioxarray shapely matplotlib

Then open IMD_Data_Analysis.ipynb and run all cells. Note: the IMD download step (imd.get_data) can take a while for a 20-year range as it pulls year-wise binary files.

Possible Extensions


Extend to other states/districts or run at grid-cell level instead of a single spatial average
Add seasonal (monsoon vs. non-monsoon) breakdowns
Compute trend statistics (Mann-Kendall, Sen's slope) on the annual series
