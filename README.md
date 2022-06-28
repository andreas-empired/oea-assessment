# OEA Hack Assessment

## Problem Statement

Confirming the scenario and problem statement to be addressed:

> *Green Valley Schools is an education system with 35,000 students in South State. School leaders want to see a report that shows them students’ attendance and learning outcomes (e.g., assessment scores, grades, or marks). School leaders would like to use this data to draw meaningful insights on patterns of student engagement and learning outcomes. Attendance is a key indicator of student engagement. They approached you to build an end-to-end solution for school leaders to use that addresses data ingestion, cleansing, preparation, analysis, and data visualization*

## Datasets

Test data sets used for extraction, ingestions & Visualisation.

### Batch 1

```
https://raw.githubusercontent.com/microsoft/OpenEduAnalytics/main/modules/Student_and_School_Data_Systems/test_data/_batch1/studentattendance.csv
https://raw.githubusercontent.com/microsoft/OpenEduAnalytics/main/modules/Student_and_School_Data_Systems/test_data/**batch1/studentdemographics.csv**
https://raw.githubusercontent.com/microsoft/OpenEduAnalytics/main/modules/Student_and_School_Data_Systems/test_data/**batch1/studentsectionmark.csv**
```

### Batch 2

```
https://raw.githubusercontent.com/microsoft/OpenEduAnalytics/main/modules/Student_and_School_Data_Systems/test_data/batch2/studentattendance.csv
https://raw.githubusercontent.com/microsoft/OpenEduAnalytics/main/modules/Student_and_School_Data_Systems/test_data/batch2/studentdemographics.csv
https://raw.githubusercontent.com/microsoft/OpenEduAnalytics/main/modules/Student_and_School_Data_Systems/test_data/batch2/studentsectionmark.csv
```

### Batch 3

```
https://raw.githubusercontent.com/andreas-empired/oea-assessment/main/batch3/studentattendance.csv
https://raw.githubusercontent.com/andreas-empired/oea-assessment/main/batch3/studentdemographics.csv
https://raw.githubusercontent.com/andreas-empired/oea-assessment/main/batch3/studentsectionmark.csv
```

## Landing and ingesting data

Our response includes a master pipeline which is an end-to-end pipeline, extracting data from an array of HTTP endpoints (see "Batch 1" dataset) followed by ingesting the extracted to Stage 2 persistence. 
The default batch category used is **Delta**. The pipeline however also provides capability to leverage both the **snapshot** and **incremental** data batch categories by passing it as a parameter. For each of the batch categories,
we've used existing implementation provided by the available OEA framework library.

<div><img src="https://raw.githubusercontent.com/andreas-empired/oea-images/main/pipeline_master.png" width="840"/></div>

## Landing Data

In response to the requirement:

> <font color="red">*1. Land csv data into stage1 data lake in OEA reference architecture [3 points]*</font>

Also referenced from the master pipeline, the solution contains a dedicated pipeline to isolate extraction of data from source only. The pipeline consumes datasets from the source system, and sinks the data to Stage 1 zone.

<div><img src="https://raw.githubusercontent.com/andreas-empired/oea-images/main/pipeline_extract_batch1.png" width="580"/></div>

Partioned by date:

<div><img src="https://raw.githubusercontent.com/andreas-empired/oea-images/main/storage_stage1.png" width="580"/></div>

## Ingesting Data

In response to the requirement:

> <font color="red">*2. Process that data into delta format into stage2 using the scripts included in the modules for the test data sets. All personal identifiable information must be pseudonymized [5 points]*</font>

Also referenced from the master pipeline, the solution contains a dedicated pipeline to isolate ingestion from from Stage 1 only. A key activity of the pipeline is the dependency of the **OEA_connector** Notebook, and the initialisation of the OEA framework to support 
base implementations, such as OEA documented batch category scope:

> Delta data is data that contains only changes that need to be incorporated. Processing this incoming data requires existing data to be updated and new data to be inserted (also referred to as upsert).

Indentiable information has been pseudonymized as per extension of **BaseOEAModule**:

* **hash** of student identifiers

<div><img src="https://raw.githubusercontent.com/andreas-empired/oea-images/main/module_greenvalleyschools.png" width="600"/></div>

Stage 1 data successfully extracted for batch 1, batch 2 and batch 3:

<div><img src="https://raw.githubusercontent.com/andreas-empired/oea-images/main/storage_stage1_delta.png" width="680"/></div>

...and ingested to stage 2 **np**:

<div><img src="https://raw.githubusercontent.com/andreas-empired/oea-images/main/storage_stage2np_delta.png" width="580"/></div>

...and to stage 2 **p**:

<div><img src="https://raw.githubusercontent.com/andreas-empired/oea-images/main/storage_stage2p_delta.png" width="580"/></div>


## Lake Database

In response to the requirement:

> <font color="red">*3. Create a lake db in Synapse studio that creates a joint view of the data [3 points]*</font>

Based on batch 1, batch 2 and batch 3 the solution contains a lake db. The is based on parameter flags passed for any of the ingest pipelines:

<div><img src="https://raw.githubusercontent.com/andreas-empired/oea-images/main/pipeline_ingest.png" width="720"/></div>

Resulting in a lake db containing all data:

<div><img src="https://raw.githubusercontent.com/andreas-empired/oea-images/main/lake_database.png" width="720"/></div>

## Visualisation

In response to requirement:

> <font color="red">*4. Provide PowerBI visualizations that give insights to education system leaders for decision making and meaningful insights [5 points]*</font>

<font color="red">**TODO** (Piyush, Marc, Calvin)</font>

## Semantic Model

In response to requirement:

> <font color="red">*5. Create a basic semantic model defining the relationships that exist across the data sets [2 points]*</font>

The semantic data model and the key relationships within the data sets are referenced below

<div><img src=https://raw.githubusercontent.com/andreas-empired/oea-images/main/oea_semantic_data_model.png width="720"/></div>

Key relationships in the dataset

<div><img src=https://raw.githubusercontent.com/andreas-empired/oea-images/main/oea_data_model_relationships.png width="720"/></div>

## Data Dictionary

In response to requirement:

> *6. Provide a data dictionary [2 points]*

We have used Azure Purview to develop data dictionary. The collection has been setup  to point to the OEA Serverless Endpoint. 

<div><img src=https://raw.githubusercontent.com/andreas-empired/oea-images/main/oea_purview_collections.png width="720"/></div>

The scan performed on the data source resulted in listing of the assets

<div><img src=https://raw.githubusercontent.com/andreas-empired/oea-images/main/oea_purview_assets.png width="720"/></div>

The assets overview, details and schema related information is referenced in next section. The data dictionary information was enriched by adding the asset description, data elements definition etc. in Azure Purview.

<div><img src=https://raw.githubusercontent.com/andreas-empired/oea-images/main/studentattendance_lookup_overview.png width="720"/></div>

<div><img src=https://raw.githubusercontent.com/andreas-empired/oea-images/main/studentattendance_lookup_schema.png width="720"/></div>

<div><img src=https://raw.githubusercontent.com/andreas-empired/oea-images/main/studentattendance_pseudo_overview.png width="720"/></div>

<div><img src=https://raw.githubusercontent.com/andreas-empired/oea-images/main/studentattendance_pseudo_schema.png width="720"/></div>

<div><img src=https://raw.githubusercontent.com/andreas-empired/oea-images/main/studentdemographics_lookup_overview.png width="720"/></div>

<div><img src=https://raw.githubusercontent.com/andreas-empired/oea-images/main/studentdemographics_lookup_schema.png width="720"/></div>

<div><img src=https://raw.githubusercontent.com/andreas-empired/oea-images/main/studentdemographics_pseudo_overview.png width="720"/></div>

<div><img src=https://raw.githubusercontent.com/andreas-empired/oea-images/main/studentdemographics_pseudo_schema.png width="720"/></div>

<div><img src=https://raw.githubusercontent.com/andreas-empired/oea-images/main/studentsectionmark_lookup.png width="720"/></div>

<div><img src=https://raw.githubusercontent.com/andreas-empired/oea-images/main/studentsectionmark_lookup_schema.png width="720"/></div>

<div><img src=https://raw.githubusercontent.com/andreas-empired/oea-images/main/studentsectionmark_pseudo_overview.png width="720"/></div>

<div><img src=https://raw.githubusercontent.com/andreas-empired/oea-images/main/studentsectionmark_pseudo_schema.png width="720"/></div>
