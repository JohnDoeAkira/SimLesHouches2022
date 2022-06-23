# VIS simulation pipeline - ELViS

## Overview of the pipeline

### Inputs

All of the inputs are stored in the workdir. In production, the workdir is created by the IAL.

* **Simulation Request** : XML file
* **Mission Database** : XML file + associated files in the data directory
* **Input catalogs** (and SED libraries) : xml files and referenced FITS files
* **Instrument models** : files of various formats in the data directory 

### Processing elements

In VISproduction the pipeline is executed by the PipelineRunner following the instructions contained in the sim_ial_pipeline _PipScript_SIM_Splitted.py_ file.
Here we will execute each step manually.
The only difference is the automation.
* SimPlanner : Split the SimRequest by pointing 
* SimPlannerPointingProgramm : Split further by instruments
* EuclidTU : Merge the list of input catalogs into single star / galaxy catalog
* EuclidVisSplit : Split the processing by detectors (36 jobs)
* EuclidVisDetector : Run the actual image simulation
* EuclidVisCombine : Combine the 36 detectors into a single focal plane FITS file (not done here)

The corresponding scripts are (/sps/training/hudelot/VIS/workdir): 
* 0_SimPlanner.py
* 1_SimPlannerPointingProgramm.py
* 2_EuclidTU.py
* 3_EuclidVisSplit.py
* 4_EuclidVisDetector.py

Try to match these scripts with the functions of the pipeline description sim_ial_pipeline _PipScript_SIM_Splitted.py_.

## Running the pipeline

Copy the processing elements helper scripts in your workdir (+ SimRequest) : 

    cp /sps/training/hudelot/VIS/workdir/?_*py .
    rm SIM_REQ_C13_PILOT_3.xml
    cp /sps/training/hudelot/VIS/workdir/SIM_REQ_C13_PILOT_3.xml .

Read the scripts before running them to understand their interface (inputs / outputs).

### Step 1 : SimPlanner

    python3 0_SimPlanner.py >& SimPlanner.log
 
What files / directories have been created ? What is the content ?

Creates SimPlanner_pkg/pointing_input_files_list.json : the list of simulated pointings (one here)
* contains data/EUC_SIM_OUTPUT-EUCLID-018539_0A0369F9BC4B-0027892_20220613T221335.145257Z_VIS_C13_PILOT_R3.xml
  * contain the list of TASKS 
    * these tasks as xml files are stored in the data directory
    
What files / directories have been created ? What is the content ?

### Step 2 : SimPlannerPointingProgramm

    python3 1_SimPlannerPointingProgramm.py >& SimPlannerPointingProgramm.log

What files are created ?

Creates call_pointing.iterations.1.SimSplitter_pkg/VIS_input_pointing_list.json
* contains the list of VIS_Tasks

## Step 3 : True Universe

Merge the input catalogs

    python3 2_EuclidTU.py >& EuclidTU.log

What files / directories are created ?

Creates the output TU object :
* call_pointing.iterations.1.SimTU_pkg/outputTU.xml
    * create the TU catalogs (in data directory)
      * EUC_SIM_TUSTARCAT-18539_0A0369F9BC4B-0024553_20220613T225153.418788Z_v13_SC8_MAIN.fits
      * EUC_SIM_TUGALCAT-18539_0A0369F9BC4B-0024553_20220613T225155.306234Z_v3_SC8_Pilot.fits

## Step 4 : Euclid Vis Splitter

Split in jobs by detectors 

    python3 3_EuclidVisSplit.py >& EuclidVisSplit.log

What files / directories are created ?

Creates the ELViS config files for all detectors (myVISconf***)
* split the model files from FPA to CCD individal files (HOTPIX, BIAS, etc ...)
* split catalog files in CCDs

## Step 5 : Run the actual image simulation by detector 

Run the simulation. 

WARNING : runtime ~ 15mn ! Check the logs (tail EuclidVisDetector.log)

    python3 4_EuclidVisDetector.py >& EuclidVisDetector.log

Run the actual images simulation.

Where are the outputs ? What king of outputs do you get.
Open the FITS images with ds9.
