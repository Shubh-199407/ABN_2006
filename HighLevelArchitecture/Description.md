# Description of the Azure services used for the High level architecture:
1. **_Azure Blob Storage_**: Using Azure Blob Storage to store the CSV files as the input data source.
    
   **Reason** : Azure Blob Storage organizes data in containers, which act as logical folders. Within a container, you can create individual blobs for 
                            each CSV file. Consider organizing your CSV files based on logical grouping, such as by date, data type, or application.
                              
 2. **_Time Trigger and ETL_**: 

    **- Azure Data Factory (ADF)**: Set up an ADF pipeline with a time-based trigger to schedule the ETL process.

    **- ADF Data Flows** : Utilize ADF Data Flows to perform the ETL operations, including removing PII columns from the CSV data as they provide a visual and code-                      free environment for building and executing data transformation logic.

    **Process**:

    ADF Data Flows provide a visual and code-free environment for building data transformation logic. It offers a range of data transformation activities that can 
    be used to manipulate, clean, and transform data. In specific case,we can utilize the following activities to remove PII columns:

    **Select:** The Select activity allows to choose specific columns from the CSV data that we want to keep, excluding the PII columns.It is possible to specify 
                 column names or use expressions to exclude columns containing sensitive information.

    **Derived Column**: The Derived Column activity enables you to create new columns or modify existing columns based on expressions. This activity can be used
                        remove PII values or mask sensitive information within specific columns.

    **Mapping Data Flow**: A Mapping Data Flow in ADF Data Flows provides a visual interface to build complex ETL logic using a combination of transformation 
                           activities. Activities like Filter, Conditional Split, or Alter Row can be used to filter out or replace PII values within the data. 
                                
