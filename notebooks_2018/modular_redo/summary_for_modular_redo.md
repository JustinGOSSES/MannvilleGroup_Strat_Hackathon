# Summary of Modular_Redo folder

## Purpose
Most of the code before this point, July 2018, was in only one or two notebooks and most of the calculated data was between starting and finished was only kept in memory. As a result, it took a lot of time to jump back in to the project each time as a lot had to be re-run. This reorganization is meant to allow it to be easier to jump back in the project, work a little bit and then step away. It is a nights and weekend project, so this is a key characteristic to shoot for. The reorganization attempts to better enable smaller amounts of work through:
1. Writing more intermediate data structures to pickles or HDF5 files. 
2. Breaking things into more notebooks, based on tasks and/or after longer processing tasks are run to minimize running functions that take 5+ minutes to run before you can do any work.
3. Renaming things within notebooks to allow more jumping back and forth with less problematic rewriting.
4. Use Dask to speed up certain processes over straight Pandas.

## Likely Notebooks
Some notebooks are options. Those will be prefixed with (o). Mandetory ones will have (m).

- (m) Find K nearest neighbors for each well. 
- (m) Load LAS files
- (m) Create features.
- (m) Machine learning
- (0) Map results.
- (0) Evaluate results of machine-learning.
- (0) Explore feature correlation and alternative feature creation though UMAP and other visualizations.

## Organization
- To make it clear what pickles or hdf5 files are created by which notebooks, the following practices are suggested. 
	- Files are written to the same folder as the notebook. 
	- Files may be read from the same directory of the notebook or a different location.
	- Files should have in their names something that sounds like the task and if necessary the notebook version. For example:
		- frKNN-NBvD_dfOf8KNNwith1KNN_vA might be the first version of pickle file with eight nearest neighbors and a column with the nearest neighbor produced from version D of the KNN notebook. 
	- To the extent the file name isn't clear, a markdown or json should be included in the same folder as the notebook that describes in detail which notebook created that file and what is included. 

## Conda Environment
Documented in environment.yml at root level of this repo. ( this is currently slightly out of date )

