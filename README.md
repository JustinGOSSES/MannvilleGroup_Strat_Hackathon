# MannvilleGroup_Strat_Hackathon
- Team: Big Data > Big Lore
- Team members: <a href="https://github.com/JustinGOSSES">JustinGOSSES</a>,<a href="https://github.com/jazzskier">jazzskier</a>, <a href="https://github.com/GeophysicsPanda">GeophysicsPanda</a>, and <a href="https://github.com/dalide">dalide</a>.
- When: September 24th, 2017
- Where: <a href="http://stationhouston.com/">Station Houston</a>
- What: Agile Hackathon -> <a href="https://agilescientific.com/events/subsurface2018">event & sponsors</a>
- Thanks: to <a href="https://www.meetup.com/AWS-Houston/">AWS cloud services Houston</a> and <a href="https://sigopt.com/">Sigopt</a> for technical help and the other sponsors for feeding us so we didn't have to leave our keyboard

## Project Statement
Predict stratigraphic surfaces based on training on human-picked stratigraphic surfaces. Used 2000+ wells with Picks from the Mannville, including McMurray, in Alberta, Canada.

## Philosophy 
Instead of assuming there is a mathematical or pattern basis for stratigraphic surfaces that can be teased out of logs, focus on creating programatic features and operations that mimic the comparison-based observations that would have been done by a geologist.

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

## Key Jupyter Notebooks finished during Hackathon project

Final Data Prep & Machine Learning for the prediction finished by end of hackathon
https://github.com/JustinGOSSES/MannvilleGroup_Strat_Hackathon/blob/master/data_prep_wells_xgb.ipynb

Version of feature engineering work done during hackathon (but didn't get to include during hackathon)
https://github.com/JustinGOSSES/MannvilleGroup_Strat_Hackathon/blob/master/Feature_Brainstorm_Justin_vD-Copy1.ipynb

## Key Jupyter Notebooks Post Hackathon & Recent Results
Code development has moved to the `modular_redo` sub-folder. Things were made more modular to better enable short bits of work when time available. The notebooks are a bit messy but will clean up in near future.
https://github.com/JustinGOSSES/MannvilleGroup_Strat_Hackathon/tree/master/notebooks_2018/modular_redo]

#### Recent updates
The code runs faster and and mean absolute error is down from 90 to 15.03 and now 7+. Key approaches were:
1. Leverage knowledge from nearby wells.
2. Instead of distinguishing between 2 classes, pick and not pick, distinguish between 3 classes: (a) pick, (b) not pick but within 3 meters and (c) not pick and not within 3 meters of pick.
3. More features
4. Two steps: 
     A. First step is classification classes. Classes are groups based on distance from actual pick.  
     B. Second step uses the classification prediction and simply additive scoring of class predictions based across different size windows. In a scenario where there are two depths with a predicted class of 100, we decide between them by adding up all the class scores across different size windows above and below each depth. The higher score wins. We take this route as we assume the right depth will have more depths near it that look like the top pick and as such have higher classes predicted for them.
         B.2. We've also experimented with regression but this hasn't given any better results so far.
 

#### Distribution of Absolute Error in Test Portion of Dataset for Top McMurray Surface in Meters. 
Y-axis is number of picks in each bin, and X-axis is distance predicted pick is off from human-generated pick.
<img src="current_errors_TopMcMr_20181006.png"
     alt="image of current_errors_TopMcMr_20181006"
     style="float: left; margin-right: 25px;" />


## Future Work [also see issues]
7. Visualize probabilty of pick along well instead of just returning max probability prediction in each well. 
8. Generate average aggregate wells in different local areas for wells at different prediction levels. See if there are trends or if this helps to idenetify geologic meaningful features that correlate to many combined machine-learning model features. 
9. Explore methods to visualize weigtings of features on individual well basis using techniques similar to those learned in image-based deep-learning. 
10. Cluster wells using unsupervised learning and then see if clusters can be created that correlated with supervised prediction results. (initial trials with UMAP give encouraging results)

## Eventual Move of this Repository Contents to a Different Repository
The plan is that once things are winnowed down to a final approach, the resulting code will be moved the <a href="https://github.com/JustinGOSSES/StratPickSupML">StratPickSupML</a> repository will it will be cleaned into one or more modules and demo notebooks with less clutter of failed but possibly useful if reworked approaches.

## Help Wanted
This repo isn't particularly organized and there hasn't be a lot of time spent (actually no time spent) to make jumping in and helping out easy. That being said, there's no reason you couldn't just jump in an start improving things. The original group is working on this at a low level when we have time. There are a few issues that are enhancements that would be a good place to start.
