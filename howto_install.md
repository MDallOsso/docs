# HOW TO GET ANALYSIS CODE FOR CMS-HH-PD

# VERSION - MORIOND 2017
# CMSSW_8_0_25
# slc6_amd64_gcc530

# 0.
# CMS ENV:
export SCRAM_ARCH=slc6_amd64_gcc530
cmsrel CMSSW_8_0_25
cd CMSSW_8_0_25/src
cmsenv
mkdir Analysis
cd Analysis
cd ../


# NOTES: 
#       - if you plan to edit the code it is worth to create a fork and to clone that in your local area
#       to fork, open repo with browser and click on fork button
#       then clone from your repository (i.e. substitute 'cms-hh-pd' with your user name)
#       - before doing 'git cms-init', fork the cmssw repo. reply 'N' to the question that follows the command.
#       - it is possible to use alp dictionary without installing all ALPHA repository. 
#         download 'interface/alp_object' from alpha and save to 'src' in alp_analysis
#         include that file in the Event.h

# 1. 
# CODE FOR NTUPLES PRODUCTION/ CLASS DEFINITION
# ALPHA - follow instructions in readme
git@github.com:CMS-PD/ALPHA.git 
# alp_analysis
git clone git@github.com:cms-hh-pd/alp_analysis.git

# 2.
# ADDITIONAL SCRIPTS TO PRODUCE PLOTS
git@github.com:cms-hh-pd/alp_plots.git

# 3. 
# CODE TO RUN MVA - CREATE DATACARD/HISTS
git clone ssh://git@gitlab.cern.ch:7999/cms-hh-pd/hh2bbbb_limit.git

# environment setting needed for this repo:
mkdir -p /lustre/cmswork/$USER/tools/cmssw_extras
export PYTHONUSERBASE=/lustre/cmswork/$USER/tools/cmssw_extras
export PATH=/lustre/cmswork/$USER/tools/cmssw_extras/bin:$PATH
export PYTHONPATH=/lustre/cmswork/$USER/tools/cmssw_extras/lib/python2.7/site-packages:$PYTHONPATH
wget https://bootstrap.pypa.io/get-pip.py
python get-pip.py --user
pip install scipy --user --upgrade
pip install rootpy --user --upgrade
pip install root_numpy --user --upgrade
env HDF5_DIR=/lustre/cmswork/dcastrom/tools/src/hdf5-1.8.17-build pip install --user tables
pip install scikit-learn --user --upgrade
pip install xgboost --user --upgrade

#NOTE: Repeat export commands each time after cmsenv.

# 4. 
# CMS HIGGS COMBINE
# https://twiki.cern.ch/twiki/bin/viewauth/CMS/SWGuideHiggsAnalysisCombinedLimit#ROOT6_SLC6_release_CMSSW_7_4_X
# 7_4_7 needed
cd ../../../
export SCRAM_ARCH=slc6_amd64_gcc491
cmsrel CMSSW_7_4_7
cd CMSSW_7_4_7/src
cmsenv
git clone https://github.com/cms-analysis/HiggsAnalysis-CombinedLimit.git HiggsAnalysis/CombinedLimit
scram b -j 8


#################################

