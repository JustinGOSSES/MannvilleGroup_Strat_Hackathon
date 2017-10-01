# MannvilleGroup_Strat_Hackathon
- Team: Big Data > Big Lore
- Team members: <a href="https://github.com/JustinGOSSES">JustinGOSSES</a>,<a href="https://github.com/jazzskier">jazzskier</a>, <a href="https://github.com/GeophysicsPanda">GeophysicsPanda</a>, and <a href="https://github.com/dalide">dalide</a>.
- When: September 24th, 2017
- Where: <a href="http://stationhouston.com/">Station Houston</a>
- What: Agile Hackathon -> <a href="https://agilescientific.com/events/subsurface2018">event & sponsors</a>
- Thanks: to <a href="https://www.meetup.com/AWS-Houston/">AWS cloud services Houston</a> and <a href="https://sigopt.com/">Sigopt</a>

## Project Statement
Predict stratigraphic surfaces based on training on human-picked stratigraphic surfaces. Used 2000+ wells with Picks from the Mannville, including McMurray, in Alberta, Canada.

## Project Summary
There has been studies that attempt to do similiar things for decades. A lot of them assume a mathematical pattern to stratigraphic surfaces and either don't train specifically on human-picked tops or do so lightly. We wanted to try as close a geologic approach (as opposed to mathematical or geophysical approach) as possible. What we managed to get done by the end of the hackathon is sorta a small scale first pass. The second page would have been a larger scale first pass, where we generate many (50-500) dumb features and train on all of them using an algorithm, perhaps XGBoosted trees or similar, that does a good job of rapidly ignorning features that aren't doing a good job at prediction. 

## Datasets for Hackathon project

Report for Athabasca Oil Sands Data McMurray/Wabiskaw Oil Sands Deposit
http://ags.aer.ca/document/OFR/OFR_1994_14.PDF

Electronic data for Athabasca Oil Sands Data McMurray/Wabiskaw Oil Sands Deposit
http://ags.aer.ca/publications/SPE_006.html
Data is also in the repo folder: <a href="https://github.com/JustinGOSSES/MannvilleGroup_Strat_Hackathon/tree/master/SPE_006_originalData">SPE_006_originalData</a>

## Key Jupyter Notebooks for Hackathon project

Final Data Prep & Machine Learning for the prediction finished by end of hackathon
https://github.com/JustinGOSSES/MannvilleGroup_Strat_Hackathon/blob/master/data_prep_wells_xgb.ipynb

Version of feature engineering work done during hackathon (but didn't get to include during hackathon)
https://github.com/JustinGOSSES/MannvilleGroup_Strat_Hackathon/blob/master/Feature_Brainstorm_Justin_vD-Copy1.ipynb

latest feature engineering work (runs faster and less complication code)
https://github.com/JustinGOSSES/MannvilleGroup_Strat_Hackathon/blob/master/Feature_Brainstorm_Justin_vE_PH.ipynb

## Future Work
1. Optimize code generative feature engineering code moving as much as possible to more efficient vector numpy
   A. Features Type A) average value within different windows above, below, and around each depth point. 
   B. Features Type B) average value of a different number of max and min points within different size windows above, around, and below each depth point.
   C. Features Type C) find difference between a different number of max/min values in different size windows around each depth point.
2. Shrink feature generation to more reasonable number
3. Explore different ways to pick included features that are more efficient
4. Continue deployment on GPU cloud instances with more computing power
5. Use geopandas & folium to investigate geographic distribution of pick prediction error
6. Explore adding a feature for geographic similarity
7. Build upon early investigation for geographic similar well cross-correlation as 1st step before other feature engineering & modeling.
8. Explore other time series matching as pre-modeling step for additional feature generation or weighting. 
9. Visualize probabilty of pick along well instead of just returning max probability prediction in each well.
10. Explore prediction accuracy vs. original pick uncertainty level. Graph percent of picks within different depth cut-offs with different lines for different original uncertainty levels in picks. 
11. Generate average aggregate wells in different local areas for wells at different prediction levels. See if there are trends or if this helps to idenetify geologic meaningful features that correlate to many combined machine-learning model features. 
12. Explore methods to visualize weigtings of features on individual well basis using techniques similar to those learned in image-based deep-learning. 
13. Cluster wells using unsupervised learning and then see if clusters can be created that correlated with supervised prediction results. 
