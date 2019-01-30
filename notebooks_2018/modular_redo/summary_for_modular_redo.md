# Summary of Modular_Redo folder 
(written mostly before starting)

## Purpose
Most of the code before this point, July 2018, was in only one or two notebooks and most of the calculated data was between starting and finished was only kept in memory. As a result, it took a lot of time to jump back in to the project each time as a lot had to be re-run. This reorganization is meant to allow it to be easier to jump back in the project, work a little bit and then step away. It is a nights and weekend project, so this is a key characteristic to shoot for. The reorganization attempts to better enable smaller amounts of work through:
1. Writing more intermediate data structures to pickles or HDF5 files. 
2. Breaking things into more notebooks, based on tasks and/or after longer processing tasks are run to minimize running functions that take 5+ minutes to run before you can do any work.
3. Renaming things within notebooks to allow more jumping back and forth with less problematic rewriting.
4. Use Dask to speed up certain processes over straight Pandas.

## Likely Notebooks
Some notebooks are options. Those will be prefixed with (o). Mandetory ones will have (m).

- (o) Figure out what wells can be used based on presence or lack of tops and well curves
- (m) Load LAS files & restrict based on presence of tops and well curves
- (m) Find K nearest neighbors for each well. 
- (m) Create features
- (m) Machine learning 1: Model training
- (m) Machine learning 2: Inference, modeling part 2, inference 2, and scoring
- (o) Map results
- (o) Evaluate results of machine-learning
- (o) Explore features and alternative feature creation though UMAP and other visualizations techniques.

## Organization
- To make it clear what pickles or hdf5 files are created by which notebooks, the following practices are suggested. 
	- Files are written to the same folder as the notebook. 
	- Files may be read from the same directory of the notebook or a different location.
	- Files should have in their names something that sounds like the task and if necessary the notebook version. For example:
		- frKNN-NBvD_dfOf8KNNwith1KNN_vA might be the first version of pickle file with eight nearest neighbors and a column with the nearest neighbor produced from version D of the KNN notebook. 
	- To the extent the file name isn't clear, a markdown or json should be included in the same folder as the notebook that describes in detail which notebook created that file and what is included. 

## Conda Environment
Documented in environment.yml at root level of this repo. ( this is currently slightly out of date )


## Patterns to be encouraged
- Changes in what original features are used propogate throughout notebooks without having to change code, just fist notebook?
- Changes in which top is predicted propogate through notebooks without changing code, just first notebook?


## Flaws that shouldn't develop:
- Copying a notebook shouldn't result in two notebooks, each with slightly different code, that overright the same variable that is written to a file!
- Minimize "heavy" functions that need to be run before other code is changed by having those at the end of notebook!
- Minimize chance for accidental variable re-write and state loss within each notebook


## Eventual vision:
- Maximize percentage of code that just runs from one or a few calls while still maintaining the ability to edit how things run
- Take out anything, or at least as much as possible, that might be viewed as hardcoded, either names, variables, presence of fields, data structure, etc. 
- Be able to "just run" as well as mess with details as you want.
- Could be run as series of notebooks and change anything in the process, could as single notebook with visibility into code just without changing the code, could be run from command line without changing or knowing what is going on in the code.

## Changes needed to get to 'eventual vision':
- Need to change some of the variable names and code such that it is more generalized and not specific to top McMurray pick.
- Need to package more of the work into functions that can be both wrapped into a single big function run at the bottom of the notebook & functions that can be easily swapped out in said function at bottom of each notebook. 
- Eventually need to wrap larger functions into object oriented classes
- Need to consider what will happen when migrating back into single notebook or out of notebook model entirely.
	- Can these things still be packaged into calls made in sequence from an object oriented package? 
	- What documentation and tests will eventually need to be included? What problems might arise if people run this on different datasets with different characteristics?
- Things that are currently done by human that will need to be done by code to be able to run this from a single command include:
	- What curves are common enough to justify including them?
	- Which wells to exclude for data limitations?
	- How to fill gaps in curves?
	- Count the number of wells with each top present and return those wells in an array of some type
	- Get the wells where both necessary top and necessary curves are present
	- Let human override all of the steps above?
- Things that should be kept track of and not just be "in process" or "hard coded"
 1. paths to original input data not created during any of this process
     - wells
     - top names
     - known tops
     - location csv 
     - etc.
 2. paths to files saved/loaded as intermediate or final steps
     - intermediate dataframes, objects, CSVs, etc.
 3. configuration variables that reflect user choices for how the code is run
     - number and type of windows in rolling window functions
     - which original log curves to use as feature inputs
 4. names of columns that serve specific purposes
 5. names tied to configuration variables used to create said columns
 6. what columns were used in what dataframes
 7. data objects created by preceeding steps
- How to pass information across classes without making bad assumptions:
    Assumptions:
        - User will only want to use data from preceeding step in that function and not from elsewhere
        - Data from preceeding step was created in way expected
        - Object variables won't get too large in size if we just continuous pass full object from last step into full object in this step.
    Possible organization to follow:
            - Only pass what is needed not full objects. In other words pass as arguments, `object.key` not `object` and then reference `object.key`. 
            - Try to not hard-code in default arguments. 
            - Try to keep anything that might be a configuration variable stored in a configuration variable object such that it can be easily found and printed with eventual results long after that function has been run. The user doesn't have to "keep track" of too much.
            
-------------------------

## Original inputs:
- LAS files for each well
- Text file of pick depths for multiple surfaces in each well
- CSV file of lat/long for each well

## Column Types:

### Index columns not used as training or targets but mainly used for keeping track of things
- UWI
- Site 
- depth 
- etc.


### Feature Columns or columns used to generate other columns that are features
- Original unchanged columns from logs that are common enough to be included
	- GR
	- ILD
	- NPHI 	
	- DPHI
- Original columns from data sources that weren't LAS files
	- Pick depth for that well
	- Location information
	- Thickness calculated in nearby wells
	- Depth of predicted top using only thickness information from nearby wells
	- Distance between depth of row in question and prediction from neighbor thickness
	- Pick quality of nearsest neighbor
	- Average pick quality in all nearest neighbors
	- Distance betweeen depth of row in question and prediction from all nearest neighbor thicknesses
	- [NOT YET DONE] various regiona locations tied to GDE noted at 1 or 0 in multiple columns
- Programmatically generated colummns without normalization
	- For each of the original unchanged colums above:
		- And for various window depths
			- Around & Mean within window
			- Above & Mean within 
			- Around & Min within window
			- Above & Min within window
			- Around & Max within window
			- Above & nLargest within window
			- Around & nLargest within window
			- [NOT YET DONE] need to do all of above but in below dirction too!!!
		
- Programmatically generated columns with normalization done only within each well
	- [NOT YET DONE]:
			- For each well, normalize GR from 0 to 1 as new column (might do columns other than GR?)
			- For windows of given size at distances at or away from row instance, find the average of N highest normalized GR values
- Programmatically generated columns with normalization across all wells
	- [NOT YET DONE] well thickness
	- 

### Target or label columns: (different ways to do this)
- Yes/no for whether a row is a specific top
- formation labels along each row
- At pick, near pick, or far away from a specific pick. 
- Nothing or pick name if row is closest row to that pick depth

## Author provided variables in first notebook! and continued throughout!
- Name of top to be predicted
- Name of other top to use for known neighbor well thickness calculations
- Original LAS curves that are common enough to be used as features (others disgarded)
- Names of LAS curves to be used for each type of calculated features

## Approximate Steps:
- Load log curves
- Figure out which log curves are there often enough to be used
- Load tops and figure out which ones are there often enough to use
- Picks tops & logs that need to be present to use a well, Limt the wells to those in that group
- Create unique ID for each row
- Create column to be used for training/test split
- Load picks as dataframe.
- Find nearest neighbors for each well using only wells in training set for traing, and all wells for test wells. Calculate thickness of top in question to other top below or above of nearsest neighbor and nearest 7 neighbors, max, min, and variance in thickness. Add as features.
- Create features
- Add in nearest neighbor features
- Train models - classification models that result in class of at pick, near pick, or away from pick
- Model Inference
- Training or picking step 2, in which regression, or simple  process of finding median value, in order to place a score for each row. Max score is the predicted pick. Other higher scores used to represent uncertainty of pick... not sure the best way to go about that yet. 
- Scoring and visualization of error 

## List of things that will likely vary with different datasets or different approaches

- Which top you're trying to predict for
- Whether you use information about predictions for other tops to inform the prediction for another top
- Whether you need wells in your training and prediction set to have a base or top already there to use neighbor thickness as a variable
- Which curves are used
- Whether all wells must have those curves
- Whether different curve names that represent a similar curve type can be used as the same curve
- Number of wells
- Degree of petrophysics used beforehand as preprocessing
- What columns are normalizes to a 0-1 scale and whether normalization is done at well level or across all wells.
- To what degree you include geographic regions are a feature
- To what degree you use well to well differences between neighbors as features




