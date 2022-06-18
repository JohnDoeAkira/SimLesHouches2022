# EXT simulation pipeline - Imagem

## Overview of the pipeline

See [VIS pipeline](./VIS_seqPipeline.md). Same structure and inputs.

## Running the pipeline

Copy the processing elements helper scripts in your workdir (+ SimRequest) : 

    cp /sps/training/hudelot/EXT/workdir/?_*py .
    cp /sps/training/hudelot/EXT/workdir/SIM_REQ_NIP_C13_PILOT_2.xml .

Also copy the Mission Data base containing the survey file (specific to EXT) : 

    cp /sps/training/hudelot/EXT/workdir/EUC_MDB_DEV_EXT-Cycle13-03032022.xml .

### Step 1 : SimPlanner

    python3 0_SimPlanner.py >& SimPlanner.log

### Step 2 : SimPlannerPointingProgramm

    python3 1_SimPlannerPointingProgramm.py >& SimPlannerPointingProgramm.log

What files are created ? Differences with VIS ?

## Step 3 : True Universe

Merge the input catalogs

    python3 2_EuclidTU.py >& EuclidTU.log

## Step 4 : Euclid EXT Splitter

Split in jobs by detectors 

    python3 3_EuclidExtSplit.py >& EuclidExtSplit.log

What files / directories are created ?
Differences with VIS ?

## Step 5 : Run the actual image simulation by detector 

Run the simulation. 

WARNING : runtime ~ 15mn ! Check the logs (tail EuclidNipDetector.log)

    python3 4_EuclidExtDetector.py >& EuclidExtDetector.log

Where are the outputs ? What king of outputs do you get.

Check the results with ds9.