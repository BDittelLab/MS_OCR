
---
Author: Cody Gurski
Date archived: 20240122
---

# Background

We started this project as a means to analyze a large amount of flow cytometric data at once through dimensionality reduction, heatmaps, and graphs that display the composition of cluster contribution to the whole of the CD19+ B cell population in healthy controls (HC), Multiple sclerosis patients (MS), and Treatment delayed patients (TD). MS patients were treated with Ocrevus (OCR) starting after the collection of timepoint 0. Blood samples were collected from these patients at day 0, 1 month after first infusion, 3 months after infusion, 6 months after infusion, and 12 months after infusion. Normal course of treatment with OCR gives the patient a second dose two weeks from the first, and another full dose of OCR at 6 months post infusion. TD patients received their first infusion of OCR, but did not receive their 6 month dose. TD patients have gone 12-14 months since their first OCR infusion.

Flow cytometry data was concatenated in flowjo, where compensated scale values were exported as a CSV file. This CSV was imported into R where it was transformed and analysis performed.

# Analysis: 
## R
Figures 3, 4, 5, and supplemental figure 2 were created in the R script: Hu MS script for publication.RMD

The R script is written such that each major data transformation and figure are parts of a different chunk. Each chunk imports and exports all the necessary data to do the analysis within its own chunk such that all chunks will run independently of each other. 

- Files: 
	- data/BCG_scale_values3.csv
		- This csv is the raw data exported from FlowJo. In brief, all fcs files were pooled into a single FlowJo project, compensated, gated on CD19+ events, then concatenated into a single file. The compensated scale values were exported as a csv from the concatenated file. Some minor cleanup was performed on the file to make it acceptable to import into R. This file was imported in the second chunk only, where we inverse hyperbolic arcsin transformed the signal columns. After transformation, the data was summarized, and plotted to check for errors in compensation and transformation. The dataframe is then written to csv as BCG_fixed.csv
	- data/BCG_fixed.csv
		- This csv is the hyperbolic arcsin transformed version of the raw data scale values above. This csv was imported into the third chunk to perform dimensionality reduction and clustering. After clustering data was applied to this dataframe, it was saved as nocd19_recluster_all_res0.2.csv
	- data/nocd19_recluster_all_res0.2.csv
		- This csv is the transformed data with the clustering info. This csv is imported into all following chunks as "plot_data"
	- data/mean_values_per_cluster_by_marker.csv
		- This is an export of mean values of fluorescence for each signal marker and cluster. Created in chunk 6, these values were input into figure 4 as the Mean Relative Fluorescence (MRF)
	- data/TTID_samplespergroupdf.csv
		- This csv lists the number of patients per sample type and timepoint, used in chunk 7 to make figure 5A.
	- info/sessionInfo.txt
		- Contains session information with packages used and versions

Chunk 1: Library import
Chunk 2: Import raw data and transform
Chunk 3: Import transformed data and cluster
Chunk 4: Plot umaps with cluster groups for figure 3
Chunk 5: Plot heatmap for figure 4
Chunk 6: Create density plots that overlay the heatmap in figure 4
Chunk 7: Create bar plots for figure 5A and supplemental figure 2
Chunk 8: Create bar plots for figure 5B
Chunk 9: Output session info

## Python
Supplemental Figure 3 was created in Python using the SCCODA library to show which clusters are compositionally significantly different from each other per timepoint, and how many samples would be needed to assign significance for populations composing a smaller proportion of the whole.

- Files:
	- data/plot.data.fixed.2.csv
		- CSV containing the same transformed cluster data as in "nocd19_recluster_all_res0.2csv", but with id numbers assigned so this make work with the SCCODA library
	- data/louvain cluster by timepoint.csv
		- CSV that was assembled from the above run through the SCCODA library and manually filled in such that we could generate the final colored figure.
	- info/environment.yml
		- Contains environment information with packages used and their versions.


# Archival

This project is archived in BRIDATA at `smb://ads.versiti.org/bridata/DITB`

# File Hierarchy
    - data: contains all csv data files required to run the analysis
    - info: contains session and environment info for R and python
    - scripts: contains R and Python scripts used for analysis

# Environments
Output of R session info:

```
R version 4.3.2 (2023-10-31)
Platform: x86_64-pc-linux-gnu (64-bit)
Running under: Linux Mint 21.1

Matrix products: default
BLAS:   /usr/lib/x86_64-linux-gnu/blas/libblas.so.3.10.0 
LAPACK: /usr/lib/x86_64-linux-gnu/lapack/liblapack.so.3.10.0

locale:
 [1] LC_CTYPE=en_US.UTF-8       LC_NUMERIC=C               LC_TIME=en_US.UTF-8        LC_COLLATE=en_US.UTF-8     LC_MONETARY=en_US.UTF-8    LC_MESSAGES=en_US.UTF-8    LC_PAPER=en_US.UTF-8      
 [8] LC_NAME=C                  LC_ADDRESS=C               LC_TELEPHONE=C             LC_MEASUREMENT=en_US.UTF-8 LC_IDENTIFICATION=C       

time zone: America/Chicago
tzcode source: system (glibc)

attached base packages:
[1] stats     graphics  grDevices utils     datasets  methods   base     

loaded via a namespace (and not attached):
 [1] digest_0.6.31   R6_2.5.1        fastmap_1.1.1   xfun_0.39       Matrix_1.5-4.1  lattice_0.22-5  cachem_1.0.8    knitr_1.43      htmltools_0.5.5 rmarkdown_2.22  cli_3.6.1      
[12] sass_0.4.6      grid_4.3.2      jquerylib_0.1.4 compiler_4.3.2  rstudioapi_0.14 tools_4.3.2     uwot_0.1.14     bslib_0.5.0     evaluate_0.21   Rcpp_1.0.10     yaml_2.3.7     
[23] jsonlite_1.8.5  rlang_1.1.1    
```

- export of this info also found in 'info/sessionInfo.txt'


- Conda environment used for Python compositional analysis is found in 'info/environment.yml'




