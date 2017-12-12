COMMAND SEQUENCE TO EXECUTE ANALYSIS FLOW (skeleton):
======================================================

ALPHA:
0. python batch/submitLSFjobs.py -c python/HHAnalyzer.py -l HH -o /lustre/cmswork/hh/alpha_ntuples/v1_2017...
0b. cmsRun python/HHanalyzer.py

ALP_ANALYSIS:
1. Baseline Selection -> default tree + Hemisphere tree produced:
 python scripts/BaselineSelector.py -s data_moriond -o def_cmva -i v2_20170222
 python scripts/BaselineSelector.py -s signals -t -o def_cmva -i v2_20170606
 python scripts/BaselineSelector.py -s signals -t -o def_cmva -i v2_20170606 --jetCorr 0
 python scripts/BaselineSelector.py -s signals -t -o def_cmva -i v2_20170606 --jetCorr 1
 python scripts/BaselineSelector.py -s signals -t -o def_cmva -i v2_20170606 --jetCorr 2
 python scripts/BaselineSelector.py -s signals -t -o def_cmva -i v2_20170606 --jetCorr 3

2. Merge data and signal files:
 cd /lustre/cmswork/hh/alp_moriond_base/def_cmva/
 rm BTagCSVRun2016.root
 hadd BTagCSVRun2016.root BTagCSVRun2016*-v*.root

 rm HHTo4B_pangea.root
 hadd HHTo4B_pangea.root HHTo4B_*.root
 ** the same in the jetCorr folders.
 
3. Create mixed data looking at merged data sample:
 python scripts/MixingSelector.py -s Data -i def_cmva --comb appl

4. Baseline Selection on mixed data:
 python scripts/BaselineSelector.py -s Data -o def_cmva_mixed -i def_cmva/mixed_ntuples -m

hh2bbbb_limit:
5. Create symb link in a working folder (define folder names inside script):
 ./scripts/prepare_area.sh

6. Create h5 file with samples and variables needed:
 python scripts/create_h5.py -i foldername/

7. Train mva:
 python scripts/train_classifier.py -i foldername -o foldername

8. Check classifier:
 python scripts/classifier_report.py -j jsonpath -c sr_cut

8. Create datacard and hist for combine:
 python scripts/create_card.py -j jsonpath -o foldername --oname test -c 0.78

combine:
9. execute combine from CMSSW_7_4_7/src:
 combine -d  ../../CMSSW_8_0_25/src/Analysis/hh2bbbb_limit/datacardname  -M Asymptotic --run blind

Additional tool for limit:
- to scan bdt cut:
 ./scripts/run_cards.sh 
 (from combine area) ../../CMSSW_8_0_25/src/Analysis/hh2bbbb_limit/scripts/run_limits.sh
 ./scripts/read_log.sh

TO GET PLOTS
alp_plots:
0. to get plots before BDT from baseline selection output:
  python scripts/drawcomp_preBDT.py -c -n -o outfolder -w 0
1. to get plots after BDT from classifier_report output:
  python scripts/drawcomp_afterBDT.py -c -n -o outfolder -w 0 -b bdtname

#################################################







