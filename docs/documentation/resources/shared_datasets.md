The directory `/data/datasets` is for depositing datasets that can be used by users of the IBU cluster. The data sharing has two main objectives:

1.  To save space on the cluster: Several people might work on the same data with different objectives, using different analysis pipelines.
2.  To have a centralized location for storing small datasets for pipeline testing or tutorials.

In order to keep this shared folder tidy, a few guidelines are explained here.

## Folder Structure

There is no set folder structure that has to be followed. Please consider the following:

* Each dataset should have a meaningful folder name.
* Inside the data folder, the README has to be placed and filled out (the template is provided at data/README\_TEMPLATE).

An example folder structure could look like the following:

```
/data/datasets  
|   GUIDELINES  
|   README\_TEMPLATE  
|   
└─── [dataset name]  
|    |  
│    │    README  
│    └─── Raw  
│          └─── p100  
│          |   └─── sample1\_L1\_R1\_001.fastq.gz  
│          |   └─── sample1\_L1\_R2\_001.fastq.gz  
│          |   └─── sample2\_L1\_R1\_001.fastq.gz  
│          |   └─── ...  
│          |  
│          └─── p1  
│               └─── sample1\_L1\_R1\_001.fastq.gz  
│               └─── sample1\_L1\_R2\_001.fastq.gz  
│               └─── sample2\_L1\_R1\_001.fastq.gz  
│          └─── ...  
|  
└─── other\_meaningful\_dataset\_name  
|      |  
|      |    README   
|      └───  ...  
|           └─── ...  
└─── ...
```

In this example unprocessed raw data is stored in a folder called `Raw`. Inside the `Raw` folder, there is a folder which indicates the percentage of original data stored. For example, p100 means that the files inside this folder contains all original reads. Similarily, p1 means that the original data has been subsampled to 1%. These datasets are helpful for pipeline testing or tutorials etc.

There might be several different use cases which may include the storage of some processed data files (e.g. `.bam` files). Please chose appropriate names for subfolders and make a README file inside the subfolder so that anyone can understand where the data came from and how it was processed.