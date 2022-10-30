# MySQL-to-GreenPlum-Data-Migration
This utility allows efficient table creation and data transfer from MySQL database platform to Greenplum. <br />

# Methods of Data Transfer
This python utility helps transfer a table data from MySQL to GP using two options, PXF Reader and GPFDist.
## <pre><code>1. PXF Reader:</pre></code>
MySQL to GP table data transfer by chunking and transferring the data in small parts to GP. When using PXF Reader, the utility 
creates chunks of the data based on date.
## <pre><code>2. GPFDist: </pre></code>
MySQL to GP table data transfer by saving the partitioned data in files and loading the files in GP. When using GPFDist, the utility 
creates separate files based on limited count (number of records) per file.<br />

# Design
![image](https://user-images.githubusercontent.com/26660037/198899676-e845ee9d-15a0-4674-8150-e2d54aab82f7.png)<br />

# Interface
This utility can easily be initiated using CMD. Upon running the utility, two options of data transfer would appear.<br /><br />
![image](https://user-images.githubusercontent.com/26660037/198899735-60190107-46c3-4ba4-97bd-d23115941ea6.png)<br />

# Requirements
* Python 3.7 and above<br />
<br />Library Dependencies:<br />
* Psycopg2<br />
* PyMSQL<br />

# Configurations:
* Log Table Creation
Execute the given create table statement in "S2S_LogTable - Create Table.txt" file to create MySQL logging table.
* Json Config File (in the zip attachment - configs.json):
### 1. MySQL (Source db) Credentials:
[DBCredentials][arch][host/user/password/db/port]
### 2. GP (Destination db) Credentials
[DBCredentials][gp][host/user/password/database/port] 
#### 3 PXF Reader Config (Fill only if using PXF for data transfer)
[PxfReader_Config][Table_name] - Name of the MySQL source Table that is to be transferred, without back quotes<br />
[PxfReader_Config][MysqlSchema] - Name of the MySQL schema where source Table is present<br />
[PxfReader_Config][MysqlDevSchema] - Name of the MySQL schema where your db user has create and drop rights<br />
[PxfReader_Config][GpSchema] - Name of the GP schema where MySQL source table need to be transferred<br />
[PxfReader_Config][MysqltoGpCon] - Name of the PXF connection provided by DBA<br />
[PxfReader_Config][DateColumn] - Date column in your MySQL source table based on which data chunks will be created<br />
[PxfReader_Config][DistributionColumn] - Comma separated columns on which distribution has to be done on gp final table<br />
[PxfReader_Config][MinDate] - Minimum date in your MySQL source table<br />
[PxfReader_Config][MaxDate] - Maximum date in your MySQL source table<br />
[PxfReader_Config][Pool_size] - We suggest 3, but can be changed as per your server resources<br />
[PxfReader_Config][Batch_size] - We suggest 100000, but can be changed as per your server resources<br />
[PxfReader_Config][Encoding] - Your table encoding, e.g UTF-8, UTF-16<br />
[PxfReader_Config][Interval] -<br />

* If total number of records in MySQL source table is less than 30 mill: the value in interval should be 0 
* if total number of records in MySQL source table is more than 30 mill: the value in interval should be number of days to process per chunk (the data is transferred and committed in chunks. we suggest around 30 mill records per chunk, add the number of days which roughly equate to 30 mill records)

### GPFDist Config (Fill only if using GPFDist for data transfer)
[GpfDist_Config][Table_name] - Name of the MySQL source Table that is to be transferred, without back quotes<br />
[GpfDist_Config][DevBoxIP] - IP of the Devbox where files will be created<br />
[GpfDist_Config][Port] - Port on which GPFDist will be running (it can be 8080)<br />
[GpfDist_Config][MysqlSchema] - Name of the MySQL schema where source Table is present<br />
[GpfDist_Config][MysqlDevSchema] - Name of the MySQL schema where your db user has create and drop rights<br />
[GpfDist_Config][GpSchema] - Name of the GP schema where MySQL source table need to be transferred<br />
[GpfDist_Config][FilePath] - Path of the directory where files will be created<br />
[GpfDist_Config][Total Count] - Total number of records in MySQL source table<br />
[GpfDist_Config][File_Count_Limit] - Limit of number of records per file<br />
[GpfDist_Config][Encoding] - Your table encoding, e.g UTF-8, UTF-16<br />
### DataMapping Updation in case of new datatypes incounter
The data types for the table creation in GP are mapped from MySQL to relevant GP data types. File Name: DataType_Mappings.txt<br />
The file contains the default mappings, but if you require to change a data type while moving the data from MySQL to GP, you may do so by changing the mapping of data type.<br />
Example: left hand side (MySQL datatype) = right hand side (GP datatype)<br /><br />
![image](https://user-images.githubusercontent.com/26660037/198900398-a0773dc9-dfb7-415c-a7b2-90a8f08b4dfd.png)<br />


# How to run the utility:
* Open cmd in the folder where you have extracted the zip file
* Run the command: python run.py

