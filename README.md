![bandicam 2024-11-10 20-51-01-811](https://github.com/user-attachments/assets/b8054892-9b51-43b5-89b5-c8ee8ae07659)
           
This image shows a Data Flow Task in an SSIS (SQL Server Integration Services) package named Package.dtsx, designed for loading a CSV file into a database. Below is a breakdown of each component and its functionality:

1-FF SRC Read Transaction: A Flat File Source that reads data from a CSV file. This component retrieves data from the specified file for further processing.

2-LKP subscriberID: A Lookup transformation that matches the subscriberID field in the incoming data with existing records in the database, likely for validation or enrichment purposes.

3-Derived Column (DER Get Subscriber ID): This component creates a derived column, potentially performing transformations or calculations to generate a new Subscriber ID field based on the incoming data.

4-OLEDB Dest Load Transaction: An OLE DB Destination where processed data is loaded into a SQL database table. This component receives data from the previous transformations and commits it to the destination database.

5-Error Handling:

Flat File Source Error Output: Handles any errors encountered during data reading from the flat file source and routes them to a designated error output (OLE DB Destination).
OLE DB Destination Error Output: Captures errors during data insertion into the OLE DB destination and routes them to the OLEDB Dest Error Destination output for error logging or further analysis.
Connection Managers: At the bottom of the image, the Connection Managers pane shows the connections used in the package:

Connections to various database servers and the flat file source are displayed, allowing SSIS to interact with data sources and destinations.
Overall, this SSIS Data Flow Task is configured to read, validate, transform, and load data from a CSV file into a database while handling errors at each critical stage.



![bandicam 2024-11-10 20-51-34-569](https://github.com/user-attachments/assets/f8d9a2c7-ecdd-4c7f-8aa9-7c9c926ed3b4)

In the Control Flow of the SSIS package, a Foreach Loop Container is implemented to manage the process of loading multiple files from a source location to a database. This container enables batch processing by iterating through each file in a specified directory. Here’s a breakdown of each component within this Control Flow setup:

Foreach Loop Container:

The main purpose of this container is to loop through all files in a specified directory. For each iteration, it picks up a single file, processes it through the data flow, and then manages post-processing steps for the file.
Within this loop, we have a sequence of tasks that manage the flow of each file from loading to its final destination.
DFT - Load CSV File to DB:

This is the Data Flow Task (DFT - Load CSV File to DB) that was shown in the previous image.
It reads data from each CSV file, applies lookups and transformations, and loads the processed data into a database table.
This data flow is responsible for handling the core functionality of data transformation and database insertion, including error handling and validation, as seen in the Data Flow Task configuration.
FST - Copy Files (on the right):

The File System Task (FST) - Copy Files is configured to create a backup of each processed file.
After each file is successfully loaded into the database, this task copies the file from its original location to a designated backup directory. This ensures that there is a copy of the original data for record-keeping or auditing purposes.
FST - Move Files (on the left):

The File System Task (FST) - Move Files is responsible for moving each processed file to an archive or a completed folder after it has been successfully copied and loaded into the database.
This helps maintain the source directory by ensuring only unprocessed files remain there and provides a clear separation between processed and unprocessed files.
Summary
This Control Flow setup, using the Foreach Loop Container, creates an efficient pipeline for processing multiple files. It iterates through each file, loads it into the database with necessary transformations, backs it up in a designated location, and finally moves it to an archive folder. The combination of data flow, file copying, and file moving ensures data integrity, processing accuracy, and organized file management throughout the ETL process.
