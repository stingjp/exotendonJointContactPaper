# Running with an Exotendon Reduces Compressive Knee Contact Force
Experimental data that was collected of runners with and without an exotendon while running at 2.67 m/s. The 3D musculoskeletal simulations utilize an optimal control problem implementation with OpenSim Moco.

## Publications
(pending)

## SimTK project (data)

There are 2 data share repositories here 1) the results directory and files used when executing the code in this repository (only need this one, includes needed experimental data), and 2) additional raw and processed versions of other experimental data.  
1. "Exotendon simulation and experimental data": https://simtk.org/plugins/datashare/?group_id=3596
2. "Experimental raw data": https://simtk.org/plugins/datashare/index.php?group_id=2505


The data for this study includes 5 subjects with: 
- Motion capture
- Ground reaction forces
- Inverse kinematics and dynamics
- Scaled musculoskeletal models


All the needed experimental data files are contained within the code results directory as well. The downloaded folder should be located (from the code repo): '../testresults - Copy - Copy/results/'

Within each subject directory there are subdirectories for each condition (natural or exotendon running) and each trial (or gait cycle) simulated. Each of those then contains the results and files for the simulation of that gait cycle, /expdata (which contains all necessary experimental data and model files for that subject and gait cycle. 
i.e.
  - muscleEnergyModel/../testresults - Copy - Copy/results/welk005/welkexo/trial01 –> contains all the simulation files and results for Subject5, for the first exotendon trial including experimental reference data in a subdirectory /expdata.


## Github
Feel free to leave issues or comments if you run into issues or have questions! 

## Running the code
The majority of the codebase was written in python. The python files in the repository were written in python 3.10.14. The OpenSim version built for this project was 4.5.1 (commit: cf3ef35).

### Getting started: 
Clone the repository. 
Then create the results directory located at "../results/" from the main repository. This is where the directories from simtk should be located.   

### Running simulations

#### 1. runsubjects_py.py - Call from main repository.
This script is set up to be a wrapper where you can specify which subjects [line 190], conditions [line 191], and trials [line 192] you would like to simulate. The main call to simulate each combination is line 220, which calls analyzesubject() from simulationSetups.py.

#### 2. simulationSetups.py - support function to define your simulations.
This set of functions provide utility for various simulation types. The primary results come from the muscleStateTrackGRFPrescribe_thirdpass() function. This sets up a muscle-driven, kinematic tracking, GRF prescribed MocoTrack problem. The script is set up to pull all the relevant files from the (previously downloaded) simTK results directories, where it will utlimately save the simulation results as well.  

#### Post-simulation analysis can be run after the simulation solves within the same analyzeSubject script. (below)
- calcContactForces.py
- calcContactForces_hip.py
- calcContactForces_ankle.py  
These functions can be used to run the Joint Reaction Analysis (JRA) on the simulation results. These scripts are all similar to one another. They will will go through the subject results directories, run the JRA, then compile the results, and plot them for visualization. Paper plots were generated using these scripts. The scripts also generate visualizations of muscle activations, joint trajectories, and moments for both natural and exotendon running, so users can examine the quality of the results. The scripts can be adjusted to explore any subset of the subjects, conditions, and trials.  

#### Other post-simulation analyses (if you want to explore other aspects of the simulations)
- simulationSetups.py -> analyzeSubject_post() can be used to queue different analysis functions and batch process different subjects, similar to the main simulation call.
- simulationSetups.py -> cumulativeLoading() can be called to perform an analysis to gather data to explore more cumulative loading metrics during running.
- OsimUtilityfunctions.py contains a number of useful functions that can be used to gather gait cycle timings, plot inverse dynamics motions, plot simulated joint moments, get muscle forces, etc. (many of these are used throughout the previous scripts mentioned). 

#### There exists some other scripts and code in the repository that were not used in the final analysis of this project. Therefore, their functionality is not tested. 
