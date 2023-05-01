# SIG-MAS

# Overview

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

This script is filtering treatable vegetation types and printing those values for each RRK.

## RRKBoundary.Rmd

This was looking at the RRK Boundaries for a structure issue that is occurring EE, but is a non issue in R.