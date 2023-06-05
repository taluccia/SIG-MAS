# SIG-MAS

# Overview

This repo contain scripts for pre and post processing of HUC data for the MAS project.

Pre processing takes original HUC data downloaded from the [USGS](https://apps.nationalmap.gov/downloader/) using a polygon that extended beyond the California Border. HUCs were then select by joining with RRK shapefile and  WIP shapefile, using the greatest overlap. 

The HUC shapefile is then fed into EE to identify HUCs with 10% treatable area and calculate the burn probability and percent WUI per HUC. In EE each HUC is then ranked by burn probability and percent WUI. 

HUCs are exported from EE to receive addition processing R. The ranking in EE started at zero, and are adjusted in R to start at 1. Additionally, for the SN RRK, which include the WIP regions, the HUCs are ranked again within each WIP region.

Finally, the HUCs are used to calculate the area of Herb, Shrub, Hardwood, and softwood from the treatable area layer in EE. This data is exportted from EE as a csv and then calculated as a percent in R.


# Description of Scripts

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

# Processes

1. Run SelectHucs50prctRRK.Rmd -> upload HucsRrkWip.shp output to EE assets
2. Run EE TreatableAreaBpWuiPercentRank -> output is HUCs by RRK
3. snHucTreat10WuiBpPrctRk_2023-05-29.csv -> read into RedoRankByWipForSN.Rmd -> output allHucsPrctRankWIP.shp to EE assets
4. 