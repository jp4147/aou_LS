# aou_LS

This README provides a detailed workflow for analyzing genomic data and electronic health records (EHR) from the "All of Us" Research Program, specifically aimed at assessing the impact of Lynch Syndrome (LS) on cancer risk.

Requirements

•	Apply for Researcher Workbench access at https://workbench.researchallofus.org/login

•	Complete access for Registered Tier data
•	Complete the Controlled Tier Training
•	Use cloud-based Jupyter notebook platform provided by the All of Us Research Workbench
•	Pip install openpyxl (other necessary packages are already installed in the platform)
•	Run the Jupyter notebook codes as described in Step 1 through Step 6 to replicate the experimental results. Step 1 through Step 5 are extracting data from AOU database and Step 6 includes analysis using those data. We provided sample data to run “Step 6 Analysis – demo” locally outside the Workbench.

Step 1: High-Quality Variants 
•	Main node: 4CPUs, 26GB RAM, 1000 GB Disk
•	Workers (2): 4CPUs, 15GB RAM, 150GB Disk
•	Skip “mt.count()” which may take an hour 
•	Time and Cost: \$0.82/h, ~3min, \$0.041
-	Upload Variants: Begin by uploading Supplementary Table 1, which lists Mismatch Repair (MMR) pathogenic or likely pathogenic variants sourced from ClinVar.
-	Quality Check: Verify the quality of variants within the small callset derived from the "All of Us" genomic data against quality metrics (refer to Supplementary Figure 2 for details).
-	Save Genomic Data: Store the confirmed genomic data in a Hail matrix table for further analysis.

Step 2: LS Carrier Identification 
•	Main node: 4CPUs, 26GB RAM, 1000 GB Disk
•	Workers (2): 4CPUs, 15GB RAM, 150GB Disk
•	Time and Cost: \$0.82/h, ~150min, \$2.05
•	To make it faster, you may increase workers (e.g. 100 workers - $25/h) 
-	Load Genomic Data: Import the genomic data saved from Step 1.
-	Identify LS Carriers: Detect patients carrying variants in the MLH1, MSH2, MSH6, and PMS2 genes indicative of LS.
-	Record LS Carriers: Document identified LS carriers for subsequent analysis steps.

Step 3: Demographics 
•	Main node: 4CPUs, 26GB RAM, 1000 GB Disk
•	Workers (2): 4CPUs, 15GB RAM, 150GB Disk
•	Time and Cost: \$0.82/h, ~60min, \$0.82
-	Data Cleaning: Prepare the demographic data for patients included in the genomic dataset.
-	Annotation: Label patients as either LS carriers or non-LS carriers based on genomic data.
-	Save Demographics: Export cleaned and annotated demographic data to aou_demog.csv.

Step 4: Cancer conditions
•	Main node: 4CPUs, 26GB RAM, 1000 GB Disk
•	Workers (2): 4CPUs, 15GB RAM, 150GB Disk
•	Time and Cost: \$0.82/h, ~560min, \$7.65
-	Collect Medical Conditions: Retrieve medical condition information from EHR data for both LS carriers and non-LS carriers as determined in Step 3.
-	Cancer Diagnosis: Identify and record patients with a cancer diagnosis in pat_with_cancer.pickle. If you encounter an error while generating pat_with_cancer.pickle, try splitting the dataset into smaller portions and generate multiple files instead. This approach can help manage large datasets that might be causing the error due to size limitations.

Step 5: Family History 
•	Main node: 4CPUs, 26GB RAM, 1000 GB Disk
•	Workers (2): 4CPUs, 15GB RAM, 150GB Disk
•	Time and Cost: \$0.82/h, ~3min, \$0.041
-	Survey Data: Utilize survey data to identify carriers and non-carriers with a family history of LS cancer.
-	Record Family History: Save this information in LS_fam.csv and nonLS_fam.csv.

Step 6: Analysis 
•	Main node: 4CPUs, 26GB RAM, 1000 GB Disk
•	Workers (2): 4CPUs, 15GB RAM, 150GB Disk
•	Time and Cost: \$0.82/h, ~30min, \$0.41
-	Diagnosis Timing: Determine the age at first cancer diagnosis for both LS carriers and non-LS carriers.
-	Cumulative Probability Plot: Create a plot to visualize the cumulative probability of cancer diagnosis by age.
-	Disease-Free Survival (DFS): Estimate DFS stratified by genotype using a Kaplan-Meier survival plot.
-	Family History Impact: Analyze the influence of family history of LS cancer on DFS utilizing LS_fam.csv and nonLS_fam.csv created in Step 5.
