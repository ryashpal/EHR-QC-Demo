# EHR-QC-Demo

In this tutorial, we will demonstrate the utilization of EHR-QC for typical EHR data handling functions to prepare it for research purposes. This primarily includes standardizing both the schema and clinical concepts, along with data exploration and preprocessing activities.

Our target audience is data scientists working in biomedical or health domains, and also the clinicians. No prior knowledge is needed for conceptual understanding, however, for handson practice, basic knowledge on Python is required.

This tool requires Python, Postgres DB (only for standardising the schema - in this case converting to OMOP format), Docker (this utility can be set-up using Docker, but its optional), any EHR data in csv files

And, please dont forget to cite our paper if you find it useful in your pipelines.

Specifically, in this tutorial, we will be following this workflow illustrated in this diagram. We will start with importing the EHR data from CSV files. The imported data is standardised to OMOP schema which is then extracted for subsequent analysis. Given that the data is typically sparse, our initial preprocessing step involves the removal of low-coverage attributes (vertical) and observations (horizontal). The remaining missing values are imputed using the most suitable method for the dataset and its missing proportion. Finally, unsupervised removal of extreme values is performed, rendering the data ready for machine learning purposes. EHR-QC serves as a comprehensive solution, consolidating all the above actions into a single package.


The first step in this demo is to import the standard vocabulary. In this case, we are importing standard vocabulary files from a public repository called Athena. Before running this step, please make sure the paths of the vocabulary files are provided in the configurations file as shown. Once the configuration file is ready, running this command will import the concepts from athena files to database tables in a sequential manner. This step will take several minutes to complete, so I will stop the video and resume once its done.


The next step is to import the EHR data from csv files to the database. Similar to earlier steps, the paths of the csv files should be configured beforehand. Additionally, the column mapping information should also be provided to identify the attributes in the csv files. Once the configuration file is ready, running this command will import the EHR data from the csv files to the landing tables.


The necessary data from the landing tables is moved to staging tables in the next step. This will limit the amount of data that will be passed through the pipeline by eliminating the rows that are not required for the analysis.


Concept mapping will be performed next by executing this code. Concept mapping is performed to identify the standard vabulary concepts for the clinical terms present in the EHR. EHR-QC combines the outputs from syntactic and semantic mapping algorithms to provide the matching concepts in a fully automated manner. This is done by taking a majority vote followed by preferential selection of the mappings in the case of no concensus. We named this algorithm Majority Voting Plus (MVP). In our benchmarking experiments, the mappings obtained from MVP algorithm was found to be more semantically closer to the original concepts than the mappings obtained from manual curation by experts. The outputs from the MVP algorithm can be reviewed manually and corrections can be made if necessary. Upon finalisation of the mappings, the mapped concepts can be imported by running this command.


In the final step of standardisation, the entities obtained from the source data is transformed to standard OMOP structure or schema by executing this codeblock.


EHR-QC offers several functionalities to perform standard preprocessing tasks such as the extract of entities, obtain coverage information, impute missing data, detect and correct outliers, and generate a report summarising all the preprocessing activities.

Firstly, we start with concept coverage analysis which gives an indication of how pervasive an attribute is in the EHR. This will generate a csv file containing the percentage coverage at patient and episode levels. Obtaining this information will assist to select the attributes for downstream processing.

The selected attributes are extracted next which forms a csv file at the specified path. The next set of coverage reports provide the missing data count and percentage and also removes the low coverage columns and rows as they do not carry much information.

The remaining missing values are imputed using imputation functionality in EHR-QC. This function artificially simulates missingness of the same proportion as the input data to determine the best imputation technique. Further, the best performing technique will be employed to impute the missing values.

Finally, the outlier removal utlity of EHR-QC detects the outliers in an unsupervised manner without relying on rigid boundary rules. The proportion of outliers can be varied by changing the configuration settings.

All the preprocessing steps can be summarised by generating the preprocessing report. In this report, the distribution of data is drawn for each attribute initially, after imputation and post outlier removal respectively.
