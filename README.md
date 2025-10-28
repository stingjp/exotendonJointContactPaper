# Running with an Exotendon Reduces Compressive Knee Contact Force
Experimental data that was collected of runners with and without an exotendon while running at 2.67 m/s. The 3D musculoskeletal simulations utilize an optimal control problem implementation with OpenSim Moco.

## Publications
(pending)

## SimTK project


There are 2 data share repositories here 1) with raw/processed experimental data, and 2) the results directory and files used when executing the code in this repository (should only need this one).

The experimental data includes 5 subjects with the following types of data: 
- Motion capture
- Ground reaction forces

All the needed experimental data files are contained within the code results directory as well. 
Within each subject folder the subdirectories are:
- /welkexo - Exotendon running condition
- /welknatural - Natural running condition

Within each of these are subdirectories for each trial, or gait cycle simulated. Each of those then contains the results and files for the simulation of that gait cycle, /expdata (which contains all necessary experimental data and model files for that subject and gait cycle. 


## Github
https://github.com/stingjp/muscleEnergyModel/
Note: this is the main repository that I work from, and should contain all that is in this one. This reposotory has been trimmed down to the essentials in order to run this project. 

## Running the code
The majority of the codebase was written in python. The python files in the repository were written in python 3.10.14. The OpenSim version built for this project was 4.5.1 (commit: cf3ef35).

### Getting started: 
Clone the repository. 
Then create the results directory located at "../results/" from the main repository. This is where the directories from simtk should be located.   

### Running simulations
#### Within a subject-condition-trial: 


analyzeSubject.m allows you to run multiple simulations/analyses on a single subject. You can customize what simulations and analyses you would like to run. Options and desctiptions follow, starting with the ones used for paper results:
- torqueStateTrackGRFPrescribe.m -> formulates a torque-driven state tracking (IK results) problem with prescribed GRFs. 
- muscleStateTrackGRFPrescribe.m -> formulates a muscle-driven state tracking problem with prescribed GRFs. 
- muscleStateTrackGRFPrescribe_firstpass.m, muscleStateTrackGRFPrescribe_secondpass.m, muscleStateTrackGRFPrescribe_thirdpass.m -> these are the same as muscleStateTrackGRFPrescribe.m ^ but have approximate weights that may aid in generating progressively better results. The secondpass script is currently set up to provide the final results from the simulations. 

#### To batch process subjects, conditions, or trials, call runsubjects1.m (edit for which subjects, conditions, trials to run). This wrapper script can call analyzeSubject.m or analyzeSubject_setup.m - be sure to specify and set up that file for what analyses and simulations you would like to complete. 


#### Post-simulation analysis can be run after the simulation solves within the same analyzeSubject script. (below)
- analyzeMetabolicCost.m -> takes the simulated solution and performs all the metabolics computations. Computes average metaboalic cost of the gait cycle, stance and swing costs, as well as individual muscle costs. 
- computeIDFromResults.m -> takes the solution, and performs an analysis of the dynamics. It will return instances where reserves and residuals exceed the recommended thresholds by (Hicks et al. 2015). 
- computeKinematicDifferences.m -> takes the solution and plots differences between the dynamically consistent solution and the input kinematics. 
- computeMarkerRMSE.m -> takes the solution and computes the output marker trajectory RMSE from the experimental markers. 
- Throughout the analysis there is a data structure called 'Issues' that keeps track of any flags that the analysis files returns, and what they are. This is helpful when batching many simulations and subjects.


#### Other post-simulation analyses (often run on multiple subject results):

#### There are other scripts and code in the repository, but were not used in the final analysis of this project. Therefore, their functionality is not tested. 
