# National-Park-Wildfire-Risk
## GEOG5300 - Final Project
### April 26, 2026

## Introduction
The analysis aims to discover which U.S. National Parks are the most at-risk to wildfires, and if parks with high risk are spatially clustered. This information will help the U.S. National Park Services know which parks may need more wildfire precautions and where to focus wildfire prevention efforts during hot and dry seasons. Below are maps of the three regions of interest: National parks in the lower 48 states, National Parks in Alaska, and National Parks in Hawaii.

<img width="1344" height="960" alt="image" src="https://github.com/user-attachments/assets/c9f75cf3-ceb3-4fa8-a4b2-55d858e24f10" />
<img width="1344" height="960" alt="image" src="https://github.com/user-attachments/assets/45204aaf-bad8-4dd4-93c2-9df3daa49a52" />
<img width="1344" height="960" alt="image" src="https://github.com/user-attachments/assets/d2398955-f6cf-48fa-af80-f2bac684636d" />

## Data Overview
My data consists on two datasets: U.S. National Park Boundaries and United States Wildfire Hazard Potential. The US National Parks dataset is from U.S. Department of Transportation, Bureau of Transportation Statistics (BTS). It includes not only national parks, but also monuments, historic sites, memorials, etc, so there are a total of 437 observations. It has 21 variables that describe the name, location, park type, size, and creator/editor information. This dataset was created by the Land Resources Division in 1995 and was last updated by the National Parks Service at the beginning of 2026. This data is intended for GIS analysis, and not surveying or legal use. I have filtered this dataset to include only National Parks in the states, shrinking it to 61 observations. I have also split it into three pieces: Continental U.S., Alaska, and Hawaii, so that it matches the wildfire hazard potential dataset.
The wildfire hazard potential data is a classified raster in the form of three datasets: Alaska, continental U.S., and Hawaii. This data is the 2023 version, created from national datasets of annual burn probability and fire intensity from the USDA Forest Service. It uses data from LANDFIRE 2020 for the landscape conditions and wildland fuels. Because of this, landscape conditions are a few years outdated and any major changes in land cover type won’t be reflected in the data. This limits the generalization of this analysis for current and future years. This analysis will more accurately reflect which parks had the greatest risk of wildfires a few years ago.
## References
### U.S. National Park Boundaries
Bureau of Transportation Statistics. (2026). National parks [Data set]. U.S. Department of Transportation, National Transportation Atlas Database. Retrieved March 2, 2026, from https://geodata.bts.gov/datasets/usdot::national-parks/about

### Wildfire Hazard Potential
Dillon, Gregory K. 2023. Wildfire Hazard Potential for the United States (270-m), version 2023. 4th Edition. Updated 17 July 2024. Fort Collins, CO: Forest Service Research Data Archive. https://doi.org/10.2737/RDS-2015-0047-4.

## Visualizations
<img width="1344" height="960" alt="image" src="https://github.com/user-attachments/assets/d174cc9d-4564-4073-93b6-41319d5705cd" />
The Intermountain and Pacific West regions contain the highest count of national parks. This shows that the western United States contains a majority of the parks. This uneven distribution is important to keep in mind, as regions with more densely packed parks may naturally exhibit different spatial autocorrelation patterns than regions with very fewer parks.
<img width="1344" height="960" alt="image" src="https://github.com/user-attachments/assets/da145c9a-6c31-4013-a7ad-797318aa9f9f" />
The above plots show the wildfire risk classifications of the United States. This data will be used to analyze which national parks are most at risk.
<img width="1344" height="960" alt="image" src="https://github.com/user-attachments/assets/87da4292-78ef-40c6-8b61-92bc43306df2" />
This box and whisker plot shows how the area of national parks in square kilometers varies across regions of the United States. The Y-axis is on a log-scale for better visualization because parks sizes widely vary. Alaska’s parks are significantly larger than the majority of national parks.

## Analysis
I combined the two datasets and extracted the wildfire risk data for each national park, like a cookie cutter. I used a “majority” function, which assigns each national park to a single risk level – whichever risk level covers the most area. Once the majority wildfire risk was extracted for each park, I visualized the results using of series of color-coded maps.
<img width="1344" height="960" alt="image" src="https://github.com/user-attachments/assets/85e7f9c9-68ae-4093-9b25-070dacb4fe75" />
<img width="1344" height="960" alt="image" src="https://github.com/user-attachments/assets/1f7ccd31-a434-4afd-85fe-0a696e031023" />
<img width="1344" height="960" alt="image" src="https://github.com/user-attachments/assets/8839dd28-0471-4106-b875-9dfa2bd5ea8f" />
<img width="1344" height="960" alt="image" src="https://github.com/user-attachments/assets/28374832-a418-48a5-b846-e2785d57b0fa" />
There are three national parks with a majority of “High” or “Very High” wildfire risk level. They are:
1. Pinnacles - Very High 
2. Mesa Verde - High      
3. Zion - High

To determine if the wildfire risk across contiguous US National Parks exhibits a statistically significant spatial pattern, I conducted a Moran’s I test. Because the parks do not share physical borders, a k-Nearest Neighbors (kNN) spatial weights matrix was used, linking each park to its four closest neighbors.

### Moran I test under randomisation
data:  parks_cont$risk_level \
weights: park_weights 

Moran I statistic standard deviate = 1.8544, p-value = 0.03184 \
alternative hypothesis: greater \
sample estimates: \
Moran I statistic: 0.142132838 \
Expectation: -0.020000000       
Variance: 0.007644624
       
The test yielded a Moran’s I statistic of 0.142 and a p-value of 0.032. Because the p-value is less than the standard 0.05 threshold, we can reject the null hypothesis of spatial randomness. The positive Moran’s I statistic indicates statistically significant positive spatial autocorrelation. In other words, this test proves what we visually observe in the thematic maps: wildfire risk in US National Parks is geographically clustered, with high-risk parks significantly grouped together (mainly in the Western regions) rather than being randomly scattered across the country.
