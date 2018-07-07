# Summary of Modular_Redo folder

## Purpose
Most of the code before this point, July 2018, was in only one or two notebooks, which resulted in it taking a lot of time to add in more work as a lot had to be re-run any time you got back into the projects. Some speed-up was achieved through pickles, more could be done. This re-working of code is an attempt to speed up / enable small amounts of work even further by:
1. Splitting up the tasks into small notebooks. 
2. Using hdf5 file formats to exchange certain things instead of just pickle files. We want to achieve easy exchange of information between notebooks even when different versions of Pandas are used (apprently it can be difficult to read pickles created with one version of pandas in another version of pandas across certain versions of Pandas).
3. Leverage Dask to do more tasks in parrallel. 

## Likely Notebooks
Some notebooks are options. Those will be prefixed with (o). Mandetory ones will have (m).

- (m) Find K nearest neighbors for each well. 
- (m) Load LAS files and create features.
- (m) Take in dataframe with created features and do machine learning
- (0) Map results of some dataframe.
- (0) Evaluate results of machine-learning.

## Organization
- To make it clear what pickles or hdf5 files are created by which notebooks, the following practices are suggested. 
	- Files are written to the same folder as the notebook. 
	- Files may be read from the same directory of the notebook or a different location.
	- Files should have in their names something that sounds like the task and if necessary the notebook version. For example:
		- frKNN-NBvD_dfOf8KNNwith1KNN_vA might be the first version of pickle file with eight nearest neighbors and a column with the nearest neighbor produced from version D of the KNN notebook. 
	- To the extent the file name isn't clear, a markdown or json should be included in the same folder as the notebook that describes in detail which notebook created that file and what is included. 

## Conda Environment
Documented in environment.yml at root level of this repo.

