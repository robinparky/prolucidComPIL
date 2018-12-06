# ProLuCIDComPIL


### We modified original Prolucid search engine to be compatible to ComPIL metaproteomics data analysis.


### File formats

MS2 and SQT are plaintext file formats detailed in the following publication:

[McDonald, W. H. et al. MS1, MS2, and SQT-three unified, compact, and easily parsed file formats for the storage of shotgun proteomic spectra and identifications. Rapid Commun. Mass Spectrom. 18, 2162–2168 (2004).](http://dx.doi.org/10.1002/rcm.1603)

MS2 files can be generated from instrument RAW files using a tool such as  [RawConverter](http://fields.scripps.edu/rawconv/)

#### Input file format

Blazmass takes an MS2 file as input. MS2 files contain MS/MS precursor ion, charge, and fragment information:

```
MS2 Format
S       000040  000040  960.22797
I       RetTime 0.25
I       PrecursorInt    6606.3
I       IonInjectionTime        150.000
I       ActivationType  HCD
I       PrecursorFile   MSMS_sample.ms1
I       PrecursorScan   34
I       InstrumentType  FTMS
Z       4       3837.89004
109.4537 168.2 0
111.1992 175.5 0
112.6070 188.2 0
136.0749 575.7 0
143.1249 190.1 0
152.1059 178.3 0
...
```

#### Output file format

Blazmass outputs search results in the SQT file format, which contains unfiltered proteomic scoring information, including the best scoring peptide matches for each scan, parent proteins for each matched peptide, and other search-related information.

```
SQT Format
S       10210   [information for scan #10210]
M       1       [best scoring peptide match]
L       [parent protein for peptide match 1]
L       [parent protein for peptide match 1]
L       [parent protein for peptide match 1]
M       2       [second-best scoring peptide match]
L       [parent protein for peptide match 2]
L       [parent protein for peptide match 2]
...
```
----
### Running locally

*tested on CentOS 6, 7, Linux Mint/Ubuntu 15.10*

**Requirements**

* Java 1.8 (Oracle or OpenJDK)
* [MongoDB 3.0+](http://www.mongodb.org/)
    * MongoDB databases can be running locally (`localhost`), remotely as a single node (typically using TCP port 27017), or sharded behind a `mongos` process (typically port 27018)
* Databases
    * see metaproteomics repository for [build_compil](https://bitbucket.org/sulab/metaproteomics)
    * or see [here](https://hpccloud.scripps.edu/index.php/s/55zVzx1QVaxstqe) for demo databases

# Instructions

----

### Configuration Of build_compil:

1. go to build_compil
2. edit ex/python/multiprocess_JSON_import.py
    * Change "HOST" and "PORT" variables to match your mongodb configuration

3. edit create_compil
    1. Change variable "FASTADB"  so that it is assigned to "${ORIGFASTADB%.*}"_renumbered."${ORIGFASTADB##*.}"
    2. Change "MONGO_HOST" and "MONGO_PORT" to match your mongodb configuration
        * Examples
            * "HOST = localhost"
            * "PORT = 27017" 
4. edit blazmass.params
    * Change "mongoDB_URI" parameter to match your mongodb configuration
        * Example: "mongodb://localhost:27017"



### To upload Fasta file to MongoDB

* create_compil is current located in build_compil
1. Go to  build_compil
2. Update the blazmass.params if needed	
3. run "create_compil path/to/fasta/file database_name collection_name"
        Example: 
	        "create_compil ~/testFasta/*.fasta testDB testColl"

*ComPIL/MongoDB integration by [Sandip Chatterjee](http://www.scripps.edu/wolan) & [Greg Stupp](http://sulab.org/)*


