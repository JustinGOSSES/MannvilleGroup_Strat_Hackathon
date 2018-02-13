# MannvilleGroup_Strat_Hackathon
- Team: Big Data > Big Lore
- Team members: <a href="https://github.com/JustinGOSSES">JustinGOSSES</a>,<a href="https://github.com/jazzskier">jazzskier</a>, <a href="https://github.com/GeophysicsPanda">GeophysicsPanda</a>, and <a href="https://github.com/dalide">dalide</a>.
- When: September 24th, 2017
- Where: <a href="http://stationhouston.com/">Station Houston</a>
- What: Agile Hackathon -> <a href="https://agilescientific.com/events/subsurface2018">event & sponsors</a>
- Thanks: to <a href="https://www.meetup.com/AWS-Houston/">AWS cloud services Houston</a> and <a href="https://sigopt.com/">Sigopt</a> for technical help and the other sponsors for feeding us so we didn't have to leave our keyboard

## Project Statement
Predict stratigraphic surfaces based on training on human-picked stratigraphic surfaces. Used 2000+ wells with Picks from the Mannville, including McMurray, in Alberta, Canada.

## Project Summary
There has been studies that attempt to do similiar things for decades. A lot of them assume a mathematical pattern to stratigraphic surfaces and either don't train specifically on human-picked tops or do so lightly. We wanted to try as close a geologic approach (as opposed to mathematical or geophysical approach) as possible. What we managed to get done by the end of the hackathon is sorta a small scale first pass. 

Eventually, we want to get to the point where we've identified a large number of feature types that both have predictive value and can be tied back to geologist insight. There are a lot of observations happening visually (and therefore not consciously)  when a geologist looks at a well log and correlates it. We want to focus on engineering features that mimic these observations and the multitude of scales at which they occur.

In addition to automating correlation of nearby wells based on picks that already exist, which has value, we think this will help geologist have better discussions, and more quantitative discussions, about the basis of their correlation and why correlations might differ between geologists. You can imagine a regional area with two separate teams with different approaches to picking a top. You could use this to programmatically pick tops in area B like they are picked in area A and also the inverse. The differences in pick style then becomes easier to analyze with less additional work. 

## Datasets for Hackathon project

Report for Athabasca Oil Sands Data McMurray/Wabiskaw Oil Sands Deposit
http://ags.aer.ca/document/OFR/OFR_1994_14.PDF

Electronic data for Athabasca Oil Sands Data McMurray/Wabiskaw Oil Sands Deposit
http://ags.aer.ca/publications/SPE_006.html
Data is also in the repo folder: <a href="https://github.com/JustinGOSSES/MannvilleGroup_Strat_Hackathon/tree/master/SPE_006_originalData">SPE_006_originalData</a>

@dalide used the Alberta Geological Society's <a href="http://www1.aer.ca/GISConversionTools/conversion_tools.html">UWI conversion tool</a> to find <a href="https://github.com/JustinGOSSES/MannvilleGroup_Strat_Hackathon/blob/master/well_lat_lng.csv">lat/longs for each of the well UWIs</a>. These were then used to find each well's nearest neighbors as demonstrated in <a href="https://github.com/JustinGOSSES/MannvilleGroup_Strat_Hackathon/blob/master/notebooks_2018/mapmaking/Map_Exploration_v2-KDtree.ipynb">this</a> notebook. 

## Folder Re-Organization
On February 11th, 2018, @JustinGosses reorganized the folder to get a lot of the notebooks out of the top-level and into sub-folders as things were getting too crowded. This might cause the directory urls to some files to be incorrect. This will be the case for any notebook from the Hackathon or 2017. Fixing this problem will just require adding a ../ or ../../ to the front of the directory in most cases.

## Key Jupyter Notebooks for Hackathon project

Final Data Prep & Machine Learning for the prediction finished by end of hackathon
https://github.com/JustinGOSSES/MannvilleGroup_Strat_Hackathon/blob/master/data_prep_wells_xgb.ipynb

Version of feature engineering work done during hackathon (but didn't get to include during hackathon)
https://github.com/JustinGOSSES/MannvilleGroup_Strat_Hackathon/blob/master/Feature_Brainstorm_Justin_vD-Copy1.ipynb

## Key Jupyter Notebooks Post Hackathon

Latest feature engineering work (runs faster and less complication code with  15.56 RMSE)
https://github.com/JustinGOSSES/MannvilleGroup_Strat_Hackathon/blob/master/notebooks_2018/Test_RUN_2018_02/Prediction_with_KNN1info_2018-02-11_vB.ipynb
Key approaches were:
1. Leverage knowledge from nearby wells.
2. Instead of distinguishing between 2 classes, pick and not pick, distinguish between 3 class, pick, not pick but within 5 meters and not within 5 of pick.

## Future Work
1. Optimize code generative feature engineering code moving as much as possible to more efficient vector numpy
2. Shrink feature generation to more reasonable number
3. Explore different ways to pick included features that are more efficient
4. Continue deployment on GPU cloud instances with more computing power
5. Build upon early investigation for geographic similar well cross-correlation as 1st step before other feature engineering & modeling.
6. Explore other time series matching as pre-modeling step for additional feature generation or weighting. 
7. Visualize probabilty of pick along well instead of just returning max probability prediction in each well. 
8. Generate average aggregate wells in different local areas for wells at different prediction levels. See if there are trends or if this helps to idenetify geologic meaningful features that correlate to many combined machine-learning model features. 
9. Explore methods to visualize weigtings of features on individual well basis using techniques similar to those learned in image-based deep-learning. 
10. Cluster wells using unsupervised learning and then see if clusters can be created that correlated with supervised prediction results. 
