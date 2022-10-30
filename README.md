# MySQL-to-GreenPlum-Data-Migration
This utility allows efficient table creation and data transfer from MySQL database platform to Greenplum. 

# Methods of Data Transfer
This python utility helps transfer a table data from MySQL to GP using two options, PXF Reader and GPFDist.
1. PXF Reader: 
MySQL to GP table data transfer by chunking and transferring the data in small parts to GP. When using PXF Reader, the utility 
creates chunks of the data based on date.
2. GPFDist: 
MySQL to GP table data transfer by saving the partitioned data in files and loading the files in GP. When using GPFDist, the utility 
creates separate files based on limited count (number of records) per file.

