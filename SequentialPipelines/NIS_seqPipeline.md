# NIS simulation pipeline - TIPS

## Overview of the pipeline

See [VIS pipeline](./VIS_seqPipeline.md). Same structure and inputs.

## Running the pipeline

Copy the processing elements helper scripts in your workdir : 

    cp /sps/training/hudelot/NIS/workdir/?_*py .

Read the scripts before running them to understand their interface (inputs / outputs).

### Step 1 : SimPlanner

    python3 0_SimPlanner.py >& SimPlanner.log

### Step 2 : SimPlannerPointingProgramm

    python3 1_SimPlannerPointingProgramm.py >& SimPlannerPointingProgramm.log

What files are created ? Differences with VIS ? NIP ?

## Step 3 : True Universe

Merge the input catalogs

    python3 2_EuclidTU.py >& EuclidTU.log

## Step 4 : Euclid NIS Splitter

Split in jobs by detectors 

    python3 3_EuclidNisSplit.py >& EuclidNisSplit.log

What files / directories are created ?
Differences with VIS / NIP ?

## Step 5 : Run the actual image simulation by detector 

Run the simulation. 

WARNING : runtime (too many) mn ! Check the logs (tail EuclidNisDetector.log)

    python3 4_EuclidNisDetector.py >& EuclidNisDetector.log

Where are the outputs ? What king of outputs do you get.

