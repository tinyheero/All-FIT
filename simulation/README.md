
## Description

We generated three datasets with each having 10,000 simulated sets of variants for known values of purity. Each simulated set consists of 20 to 100 variants, assuming the presence of at least one somatic heterozygous mutation. For the purpose of comparison, we also generate input files for ABSOLUTE, assuming one probe assigned to each segment of length 99 bp. The chromosome and start position are randomly chosen from the coding region in the genome. By using ploidy information from the simulation, segment mean of called 
CNV = `log ploidy/2 with base 2`. 
We provided maf file (list of SNV) and cnv file, which is complemented with copy-neutral region by assigning its segment mean = 0 and its number of probes = number of exons, to ABSOLUTE.<br/>

There are 8 different mutational models:
1. somatic, CNmut = 1, *Y* = 2 (sub-clonal mutation is simulated from this model)
2. somatic, LOH CNmut = 1, *Y* = 1
3. somatic, CNmut = 2, *Y* = 2
4. germline, CNmut = 1, *Y* = 2
5. germline, LOH CNmut = 1, *Y* = 1
6. germline, CNmut = 2, *Y* = 2
7. somatic, CNmut = ?, *Y* = ??
8. germline, CNmut = ?, *Y* = ??

CNmut - mutated allele's copy-number (*c*<sub>m</sub>)<br/>
*Y* - ploidy <br/>
? ranges from 1 to *Y* and ?? ranges from 3 to 8, representing high ploidy variants

## How to run simulation
Modifies script_directory in line 7 of `script/code_simulation/AddNeutralCNSegments.py` and `script/code_simulation/part1_generate_ABSOLUTE_input.py`
```
./script/run_simulation.sh 1 100
```
with first parameter being the dataset number and second parameter being number of simulated sets, giving the same dataset number will cause the folder to be overwriten.<br/>
To generate dataset with high percentage of sub-clonal mutations or high ploidy variants, replace `purity_simulation.py` on line 19 with `subclonal_purity_simulation.py` or `high_ploidy_purity_simulation.py` respectively.

## How to run scripts to estimate purity

### Command line to run ABSOLUTE
```
Rscript script/ABSOLUTE_command.r All-FIT/simulation/PuritySim_Set3/ABSOLUTE/ All-FIT/simulation/output_PuritySim_Set3/ABSOLUTE/ SIM_DATA_3.1
```
with first parameter being the input files directory, second parameter is the output files directory, and third parameter is the prefix for both input and output files.

### Command line to run All-FIT
```
python All-FIT.py -i All-FIT/simulation/PuritySim_Set3/our_method/SIM_DATA_3.1.xls -d All-FIT/simulation/output_PuritySim_Set3 -o SIM_DATA_3.1
```

## Simulated data used in paper
(within data.tar.bz2)<br/>
- PuritySim_Set3 - has equal probability of assigning each variant under 8 different mutational models, with slightly less than 1/4 chance of generating sub-clonal mutations.
- PuritySim_Set5 - has at least 25% of mutations being somatic, CNmut = 1, with slightly less than 2/3 chance of generating sub-clonal mutation; other mutational models share equal probability of being simulated.
- PuritySim_Set6 - allocated 100 sets for each percentage of high ploidy mutations, ranging from 0% of the variants (absence of high ploidy variants) to 99% (almost all variants in the sample are high ploidy changes).<br/>
There are also the respectively output folders for the above 3 different datasets.
- output_PuritySim_Set9 - final results from the subsampling simulations of PuritySim_Set3 on varying number of mutations.
- output_PuritySim_Set10 - final results from the subsampling simulations of PuritySim_Set3 on varying sequencing depth.

### Other scripts purpose
- removed_germline.py is used to remove germline variations from simulated datasets to provide input files in folder our_method-removed-germline (run this script at /PuritySim_Set/our_method/)
- check_accuracy_ignoreNearby.py, subclonal_check_accuracy_interval_ignoreNearby.py, and high_ploidy_check_accuracy_interval_ignoreNearby.py are used to generate accuracy of All-FIT with Pearson's correlation coefficient as well as Figures 4, 6, and S1-S2. (run these scripts at the directory of /output_PuritySim_Set/)
- estimating_error_num_mut_purity.py and estimating_error_alpha_purity.py are used to generate Figure 5. (the input files for these scripts are the results of running All-FIT on additional 2 subsampling simulations from PuritySim_Set3; remove_mut_simulation.py and shallow_seq_simulation.py are respectively the script to generate these permutated samples.)
- examples_running_All-FIT_ABSOLUTE_command.sh provides examples for running All-FIT and ABSOLUTE.

Due to patients' confidential issues , only simulated data is provided.
