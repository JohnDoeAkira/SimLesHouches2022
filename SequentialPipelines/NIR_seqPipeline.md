# NIR simulation pipeline - ELViS

## Overview of the pipeline

See [VIS pipeline](./VIS_seqPipeline.md). Same structure and inputs.

## Running the pipeline

Copy the processing elements helper scripts in your workdir : 

    cp /sps/training/hudelot/NIR/workdir/?_*py .

Read the scripts before running them to understand their interface (inputs / outputs).

### Step 1 : SimPlanner

    python3 0_SimPlanner.py >& SimPlanner.log

### Step 2 : SimPlannerPointingProgramm

    python3 1_SimPlannerPointingProgramm.py >& SimPlannerPointingProgramm.log

What files are created ? Differences with VIS ?

## Step 3 : True Universe

Merge the input catalogs

    python3 2_EuclidTU.py >& EuclidTU.log

## Step 4 : Euclid Vis Splitter

Split in jobs by detectors 

    python3 3_EuclidVisSplit.py >& EuclidVisSplit.log

What files / directories are created ?
Differences with VIS ?

## Step 5 : Run the actual image simulation by detector 

Run the simulation. 

WARNING : runtime ~ 15mn ! Check the logs (tail EuclidVisDetector.log)

    python3 4_EuclidVisDetector.py >& EuclidVisDetector.log

Where are the outputs ? What king of outputs do you get.

Check the results with ds9.