## Workflow Overview

1. Run FusionCatcher Analysis (fusioncatcher_run.sh):  
   - Processes a list of directories containing FASTQ files.  
   - Executes the FusionCatcher program to detect fusion genes.  
   - Saves the results in corresponding output directories.  

2. Align Reads to Fusion Transcript Junction Sequences:  
   - Takes fusion transcripts identified by FusionCatcher and maps supporting reads to the junction sequences using Bowtie2 as a custom reference.  
   - This step uses two different scripts to accommodate the naming conventions of specific datasets:  
     - bowtie2_alignment_mayo.py: Designed for the Mayo RNA sequencing study.  
     - bowtie2_alignment_hipsci.py: Designed for the HipSci RNA sequencing dataset.  
   - Steps performed by these scripts:
     - Align reads to indexed fusion transcripts using Bowtie2.  
     - Convert SAM to BAM files, sort, and index them.  
     - Extract reads for each fusion transcript.  
   - Outputs:
     - SAM/BAM files stored in structured directories.  
     - FASTA files for each fusion transcript and sample combination.  

3. Count Reads Mapped to Fusion Transcripts (bowtie2_counts.py):  
   - Counts the number of reads mapped to each fusion transcript for each sample using the FASTA files generated from Bowtie2 alignments.  
   - Outputs results as Counts_pivot_[date+time].txt.

4. Perform Coverage Analysis for WGS BAM Files (coverage_analysis.sh):  
   - Analyzes coverage in specified regions of HipSci WGS BAM files.  
   - Requires the following files:  
     - CSV file (hipsci-files_wgs_cram.csv) with metadata for cell lines.  
     - Optional text file (cell_lines.txt) listing specific cell lines to analyze.  
     - BED file (copy_regions.bed) specifying regions of interest.  

5. Calculate Read Count Ratios (calculate_ratios.py):  
   - Combines read counts from multiple VCF files generated by coverage_analysis.sh.  
   - Calculates ratios of read counts for specific regions and groups the results by region type.  

6. Perform SNP Genotyping (H1H2_SNP_genotyping.py):  
   - Performs SNP genotyping for H1 and H2 cell lines using SAMtools and BCFtools.  
   - Requires BAM files generated from coverage_analysis.sh.  

7. Construct Transcript Models (isoquant.py):  
   - Uses FASTA files containing PacBio reads supporting fusion transcripts to construct transcript models.  
   - Saves results in subdirectories corresponding to the input file names.

8. FusionCatcher Processing (FusionCatcher_processing.R):  
   - Takes outputs from all previous steps and combines them for visualization.  

9. Analyze Inter-Individual Variation in Fusion Transcript Expression (variable_fusion_expression.R):  
   - Focuses on fusion transcripts with inter-individual variation.  
   - Combines outputs from previous steps for targeted visualization.

---

## Repository Contents

### Fusion Gene Detection
- fusioncatcher_run.sh:  
  Runs FusionCatcher on a list of directories containing FASTQ files.  
  Outputs FusionCatcher results in structured directories.

### Bowtie2 Alignment and Read Processing
- bowtie2_alignment_mayo.py:  
  Aligns reads to fusion transcript junction sequences for the Mayo RNA sequencing dataset.

- bowtie2_alignment_hipsci.py:  
  Aligns reads to fusion transcript junction sequences for the HipSci RNA sequencing dataset.

- bowtie2_alignment_hipsci_controls.py:  
  Aligns reads to fusion transcript junction sequences for HipSci control samples.

- bowtie2_counts.py:  
  Counts the number of reads mapped to each fusion transcript for each sample.  
  Outputs the results in a pivot table format.

### Coverage Analysis and Genotyping
- coverage_analysis.sh:  
  Performs coverage analysis for WGS BAM files in specified regions.  
  Requires metadata and BED files.

- calculate_ratios.py:  
  Combines read counts from multiple VCF files to calculate ratios of read counts for specific regions.

- H1H2_SNP_genotyping.py:  
  Performs SNP genotyping for H1 and H2 cell lines using BAM files.

### Transcript Modeling and Visualization
- isoquant.py:  
  Constructs transcript models from PacBio reads supporting fusion transcripts.  

- FusionCatcher_processing.R:  
  Combines outputs from various scripts for comprehensive visualization.  

- variable_fusion_expression.R:  
  Focuses on visualizing fusion transcripts with inter-individual variation.
