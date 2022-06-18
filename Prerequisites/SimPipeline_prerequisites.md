## Prerequisites

### Codes directory

First create a directory of your own for the cloned codes.
The working directory of the temporary account is */sps/training*. Then you need to create you own :

    mkdir /sps/training/<UserName>  , user whatever account username, your real name, nickname ...

Load the EDEN3.0 environment (Euclid env)

    export User_area=/sps/training/<UserName>/Projects/
    source /cvmfs/euclid-dev.in2p3.fr/CentOS7/EDEN-3.0/bin/activate


## The SIM pipeline

### Get the SIM pipeline repository

For those who have access to the official Euclid Gitlab, clone it locally (and checkout tag 1.0.14):

    mkdir /sps/training/<UserName>/Projects
	cd /sps/training/<UserName>/Projects
	git clone https://gitlab.euclid-sgs.uk/PF-SIM/sim_ial_pipelines.git sim_ial_pipeline
	git checkout 1.0.14

For the other, please copy it from the /sps/training/SIMressources :

    mkdir /sps/training/<UserName>/Projects
	cd /sps/training/<UserName>/Projects
    cp -r /sps/training/SIMressources/sim_ial_pipeline .

Look at the two main files of the pipeline :
* *SIM_IAL_Pipelines/auxdir/SIM_Pipelines/PkgDef_SIM.py* : description of the pipeline elements
* *SIM_IAL_Pipelines/auxdir/SIM_Pipelines/SIM_SplittedPipeline/PipScript_SIM_Splitted.py* : Sequential chaining of the pipeline

Identify the various parts in the PkgDef : SimPlanner, EuclidTU, Sim<VIS-NIP-NIS-EXT>Splitter, SIM<VIS-NIP-NIS-EXT>Detector
Study the chaining in the PipScript.

## Get the input data for the simulations

### Clone a PPO (Pipeline Processing Order) ... if you have a Euclid account

The easiest way to get all the necessary inputs for a simulation is to start with a PPO that has already been executed
(in any Euclid production, official SIM, custom of some EC member). They are all stored on the Euclid archive (EAS).

    # Download the PPO
    mkdir /sps/training/<UserName>/PPO; cd /sps/training/<UserName>/PPO
    python /sps/training/SIMressources/scripts/dataProductRetrieval_PV.py
        --username phudelot
        --password 'XXXXXX'
        --project TEST
        --data_product PipelineProcessingOrder                  # Data Model object type PPO
        --query="DataSetRelease=SIMVIS_CYCLE13_R4"              # SQL query for a specific PPO

Then you can "clone" the PPO workdir in your local directory (with links to all input products) using the PPO_workflow project.

    # Get the SDC-FR-TOOLS project
    cd /sps/training/<UserName>/Projects/
    git clone https://gitlab.euclid-sgs.uk/SDC-FR/sdc-fr-tools.git sdc-fr-tools/
    cd sdc-fr-tools/
    make purge; make; make install

    # Clone the PPO
    # input = /sps/training/PV_inputs is a cache directory with local input files. When PPO files are not present,
    # they are downloaded. Otherwise they are linked.
    mkdir /sps/training/<UserName>/clonedPPO; cd /sps/training/<UserName>/clonedPPO
    ERun PPO_workflow 1.8 CloneToLocal
    --ppo /sps/training/hudelot/ClonePPO/test_VIS_C13/PipelineProcessingOrder__0.xml
    --output /sps/training/hudelot/ClonePPO/ClonePPO_VIS_C13
    --easUser phudelot
    --easPwd 'XXXX'
    --sdc SDC-FR
    --input /sps/training/PV_inputs

### Copy the PPO workdir if you don't have a Euclid account

If you do not have an official Euclid account you can copy this directory (warning ... it's big !)
The final directory tree is :

    .
    ├── PipelineProcessingOrder__0.xml
    ├── PR_script.sh
    ├── sdc-fr-local.properties
    └── workdir
        ├── data
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2171_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2171_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2172_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2172_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2174_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2174_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2175_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2175_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2247_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2247_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2253_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2253_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2256_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2256_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2257_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2257_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2258_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2258_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2259_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2259_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2260_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2260_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2261_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2261_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2262_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2262_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2263_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2263_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2264_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2264_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2265_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2265_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2266_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2266_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2267_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2267_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2268_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2268_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2269_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2269_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2270_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2270_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2271_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2271_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2289_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2289_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2292_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2292_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2432_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2432_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2434_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2434_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2435_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2435_v3.xml
        ├── EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2440_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_HIGHZ-1.10.13-2021-02-18_HPIX_5_NEST_2440_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2171_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2171_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2172_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2172_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2174_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2174_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2175_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2175_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2247_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2247_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2253_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2253_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2256_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2256_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2257_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2257_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2258_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2258_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2259_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2259_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2260_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2260_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2261_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2261_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2262_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2262_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2263_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2263_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2264_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2264_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2265_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2265_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2266_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2266_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2267_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2267_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2268_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2268_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2269_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2269_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2270_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2270_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2271_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2271_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2289_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2289_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2292_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2292_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2432_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2432_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2434_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2434_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2435_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2435_v3.xml
        ├── EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2440_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_QSO-1.10.12-2021-02-18_HPIX_5_NEST_2440_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2171_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2171_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2172_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2172_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2174_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2174_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2175_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2175_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2247_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2247_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2253_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2253_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2256_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2256_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2257_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2257_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2258_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2258_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2259_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2259_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2260_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2260_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2261_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2261_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2262_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2262_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2263_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2263_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2264_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2264_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2265_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2265_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2266_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2266_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2267_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2267_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2268_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2268_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2269_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2269_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2270_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2270_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2271_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2271_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2289_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2289_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2292_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2292_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2432_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2432_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2434_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2434_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2435_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2435_v3.xml
        ├── EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2440_v3.xml -> /sps/training/PV_inputs/EUC-GALCAT_STANDARD-1.10.11-2021-02-17_HPIX_5_NEST_2440_v3.xml
        ├── EUC_MDB_MISSIONCONFIGURATION-DEV_2022-04-12T12:00:00.00Z_00.xml -> /sps/training/PV_inputs/EUC_MDB_MISSIONCONFIGURATION-DEV_2022-04-12T12:00:00.00Z_00.xml
        ├── galcat.json
        ├── params.dat
        ├── SIM_REQ_C13_PILOT_3.xml -> /sps/training/PV_inputs/SIM_REQ_C13_PILOT_3.xml
        ├── starcat.json
        ├── STARCAT-RA227.49-DEC31.00-v13_SC8_MAIN.xml -> /sps/training/PV_inputs/STARCAT-RA227.49-DEC31.00-v13_SC8_MAIN.xml
        ├── STARCAT-RA227.53-DEC29.00-v13_SC8_MAIN.xml -> /sps/training/PV_inputs/STARCAT-RA227.53-DEC29.00-v13_SC8_MAIN.xml
        ├── STARCAT-RA229.81-DEC29.00-v13_SC8_MAIN.xml -> /sps/training/PV_inputs/STARCAT-RA229.81-DEC29.00-v13_SC8_MAIN.xml
        ├── STARCAT-RA229.83-DEC31.00-v13_SC8_MAIN.xml -> /sps/training/PV_inputs/STARCAT-RA229.83-DEC31.00-v13_SC8_MAIN.xml
        ├── STARCAT-RA230.08-DEC27.00-v13_SC8_MAIN.xml -> /sps/training/PV_inputs/STARCAT-RA230.08-DEC27.00-v13_SC8_MAIN.xml
        ├── STARCAT-RA230.13-DEC33.00-v13_SC8_MAIN.xml -> /sps/training/PV_inputs/STARCAT-RA230.13-DEC33.00-v13_SC8_MAIN.xml
        ├── STARCAT-RA230.73-DEC35.00-v13_SC8_MAIN.xml -> /sps/training/PV_inputs/STARCAT-RA230.73-DEC35.00-v13_SC8_MAIN.xml
        ├── STARCAT-RA232.10-DEC29.00-v13_SC8_MAIN.xml -> /sps/training/PV_inputs/STARCAT-RA232.10-DEC29.00-v13_SC8_MAIN.xml
        ├── STARCAT-RA232.16-DEC31.00-v13_SC8_MAIN.xml -> /sps/training/PV_inputs/STARCAT-RA232.16-DEC31.00-v13_SC8_MAIN.xml
        ├── STARCAT-RA232.32-DEC27.00-v13_SC8_MAIN.xml -> /sps/training/PV_inputs/STARCAT-RA232.32-DEC27.00-v13_SC8_MAIN.xml
        ├── STARCAT-RA232.51-DEC33.00-v13_SC8_MAIN.xml -> /sps/training/PV_inputs/STARCAT-RA232.51-DEC33.00-v13_SC8_MAIN.xml
        ├── STARCAT-RA233.17-DEC35.00-v13_SC8_MAIN.xml -> /sps/training/PV_inputs/STARCAT-RA233.17-DEC35.00-v13_SC8_MAIN.xml
        ├── STARCAT-RA234.39-DEC29.00-v13_SC8_MAIN.xml -> /sps/training/PV_inputs/STARCAT-RA234.39-DEC29.00-v13_SC8_MAIN.xml
        ├── STARCAT-RA234.49-DEC31.00-v13_SC8_MAIN.xml -> /sps/training/PV_inputs/STARCAT-RA234.49-DEC31.00-v13_SC8_MAIN.xml
        └── STARCAT-RA234.90-DEC33.00-v13_SC8_MAIN.xml -> /sps/training/PV_inputs/STARCAT-RA234.90-DEC33.00-v13_SC8_MAIN.xml

The data directory is not expanded. It's too large. It contains all the input data referenced in the Mission Data Base.

Try to identify the input elements of the simulations :
* input catalogs (stars and galaxies)
* input Mission Data Base object
* Simulation Request (SimRequest)






