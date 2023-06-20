## Description of the Azure services used for the High level architecture:
1. **Azure Blob Storage**: Using Azure Blob Storage to store the CSV files as the input data source.
    
   **Reason** : Azure Blob Storage organizes data in containers, which act as logical folders. Within a container, you can create individual blobs for 
                            each CSV file. Consider organizing your CSV files based on logical grouping, such as by date, data type, or application.
                              
 2. **Time Trigger and ETL**: 

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
    **Data Flow Sink:**  Configure the sink in the data flow to write the joined data to Azure Data Lake Storage.
      
4. **Azure Data Lake Storage:** Store the output data from the ETL process in Azure Data Lake Storage. This provides a scalable and secure storage solution for big 
                                data.
5. **Relational Database:** Use an Azure SQL Database or Azure Database for PostgreSQL as the relational database. Store your data in this database, which will 
                            serve as the source for extraction.
6. **Azure Data Factory (ADF):** Set up an Azure Data Factory pipeline to orchestrate the data movement and transformation. ADF provides data integration and ETL 
                                capabilities.
7. **Azure Data Lake Storage:** Use Azure Data Lake Storage as the intermediate storage to store the extracted data from the relational database and the filtered 
                                data before joining. Configure ADF to read data from the relational database and write it to Azure Data Lake Storage.
8. **Azure Databricks:** Utilize Azure Databricks as the compute engine for your data processing tasks. Within the ADF pipeline, you can use an Azure Databricks 
                         activity to trigger a Databricks notebook. The notebook can perform the necessary data filtering and join operations using Spark SQL or 
                         other relevant technologies.Within the ADF pipeline,then configure the output dataset to write the transformed data to ADLS
9. **Azure Data Lake Storage:** Use Azure Data Lake Storage to store the transformed data. Configure the output dataset of your ADF pipeline to write the 
                                transformed data to Azure Data Lake Storage.
10. **ADF:** Configure another ADF  pipeline to orchestrate the data movement to the Azure synapse analytics.   
11. **Azure Synapse Analytics (formerly SQL Data Warehouse):**  Azure Synapse Analytics includes a SQL engine that supports executing complex SQL queries. You can 
                 use the SQL language to perform the join operation on the ID columns of the input datasets. Write a SQL query that joins the data based on the ID 
                column and specifies the desired output columns.And stores it in the ADLS.
12. **Data Visualization and Dashboard:** To generate the dashboard, you can leverage Power BI or Azure Synapse Studio. Both tools provide visualization capabilities to create interactive dashboards and reports. You can connect to the destination Data Lake where the joined output data is stored and create visualizations based on the data. Power BI offers more advanced visualization features and a user-friendly interface, while Azure Synapse Studio provides integrated analytics capabilities within the Synapse Analytics workspace.    
      
## Overview of how these services fit into the architecture : 

Here's an overview of how these services fit into the architecture:

Use ADF to extract data from the relational database and load it into Azure Data Lake Storage.
Use ADF to trigger a data movement pipeline that reads the data from the Data Lake, applies the filter logic using Azure Databricks, and writes the filtered output back to the Data Lake.
Set up a separate data movement pipeline in ADF that reads the output from the other Data Lake, performs a join operation using Azure Synapse Analytics, and stores the joined result in the Data Lake.
         
