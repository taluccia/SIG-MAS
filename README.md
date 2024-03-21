# SIG-MAS

# Overview

This repo contains scripts for pre and post processing of HUC data for the MAS project.

Pre processing takes original HUC data downloaded from the [USGS](https://apps.nationalmap.gov/downloader/) using a polygon that extended beyond the California Border. HUCs were then select by joining with RRK shapefile and included if 50% of the HUC overlaps with the RRK Region Boundary. 

The HUC shapefile is then fed into EE to identify HUCs with 10% treatable area and calculate the burn probability and percent WUI per HUC. In EE each HUC is then ranked by burn probability and percent WUI. These HUCs are then exported from EE to receive addition processing R.

The ranking in EE started at zero, and are adjusted in R to start at 1. Additionally, for the SN RRK, which include the WIP regions, the HUCs are ranked again within each WIP region.

We then added addtional properties to the HUCs in R for that will be used in the treatment scenarios. This includes...

Finally, the HUCs are used to calculate the area of Herb, Shrub, Hardwood, and softwood from the treatable area layer in EE. This data is exportted from EE as a csv and then calculated as a percent in R.




# Description of Scripts in script folder

## SelectHucs50prctRRK.Rmd

This script overlays the RRK region with the HUCs in California. The HUCs are assigned the RRK in which they have the largest overlap. The HUCs are then subset the the four RRK Regions of Interest for MAS.

## RedoRankByWipForSN.Rmd

This takes the export csv from EE, converts the ranks from 0-max to 1-max for Sierra Nevada Region. It then takes the SN and groups by WIP and recalculates the ranks for the HUC within each WIP. 

## SummarizePercentHST.Rmd

This script takes the CSV output from EE and calculates the percent treatable herb, shrub, hardwood, and softwood by RRK.

## PercentRankInspect.Rmd

This script was use to look into an issue with the data, which has been fixed.

## WHRVegTypeFilter.Rmd

This script is filtering treatable vegetation types and printing those values for each RRK. Those values are then used in EE.

## RRKBoundary.Rmd

This was looking at the RRK Boundaries for a structure issue that is occurring EE, but is a non issue in R.

# Data
*  [WIP Geographies](https://gis.data.cnra.ca.gov/datasets/6843fd5e35cf42e4a5c0c4fa548b1df8_0/explore?location=40.104257%2C-120.299466%2C6.00)
*  [HUC 12 boundaries](https://apps.nationalmap.gov/downloader/)

# Step by step Processes

1. In R, run `SelectHucs50prctRRK.Rmd` -> output HucsRrkWip.shp 
2. Upload HucsRrkWip.shp to EE assets
2. In EE, Run TreatableAreaBpWuiPercentRank -> output is HUCs by RRK (e.g., snHucTreat10WuiBpPrctRk_2023-05-29.csv) -> read into `RedoRankByRrkWip.Rmd` 
3.  `RedoRankByRrkWip.Rmd` -> output allHucsPrctRankWIP.shp -> upload to EE assets
4. Run EE `TreatableAreaBpWuiPercentRank` -> output is csv for each RRK Region
5. `TxHucNumbers.Rmd` -> takes output from EE adds properties for grouped percentiles, rank and percent rank for RFFC -> output `TxPrctRankRrkWipRffc.shp` 


# EE Scripts
* `TreatableAreaBpWuiPercentRank` --assigns properties to each HUC for treatable area, mean burn probability and proportion wui, then ranks the hucs within each RRK by burn probability and WUI

* `TxVegByHerbShrubHardSoftHucs` --creates treatable vegetation image, and calculates treatable area by Herb, shrub, Hardwood and Softwood. 



# EE Assets
Available in `projects/pyregence-ee/assets/mas/`
* `HerbShrubHardSoftImage.Rmd` --Raster classified as 1-4 for herb, shrub, hardwood, and softwood, treatable pixels

* `HucsRrkWip.Rmd` -- shp (feature collection) This is the starting point in EE. This file is generated in R and selectes all HUCs with the majority of their area within California and assigns their majority RRK and And for Sierra Nevada their majority WIP.

* `allHucsPrctRankWIP.Rmd` -- shp (feature collection); This is the second iteration that re-codes the ranks to start at 1 instead of zero, calculate percent rank, and assigns a rank for within the WIP.

* `TxPrctRankRrkWip.Rmd` --shp (feature collection) This takes the `allHucsPrctRankWIP` and adds additional properties for factor math in AFE.  

* `TxHucNumbers.Rmd`  --shp takes EE output and recombines separate RRK Regions into single shp. Adds properties for grouped percentiles, adds rank and percent rank for RFFC, which is calculated by multiplying the BP percent rank by the WUI percent WUI.   

# Spatial Data Downloads
Road File from https://www.census.gov/cgi-bin/geo/shapefiles/index.php?year=2016&layergroup=Roads

# NOTES:

## Weather
Central Coast Region [435], 
North Coast Region [774], 
Sierra Nevada Region [1148], 
South Coast Region [480]

Timesteps 0y and 5y: 2015-2024 (June-Oct)
Timestep 10y: 2025-2034 (June-Oct)
Timestep 20y: 2035-2044 (June-Oct)


