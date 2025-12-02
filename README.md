*Incremental Data Copy Pipeline*

This project implements a configuration-driven incremental data ingestion pipeline in Azure Data Factory (ADF) that copies data from Azure SQL Database to Azure Data Lake Storage Gen2.

Incremental loading is achieved using a watermark value that is stored and maintained inside a configuration table.

*How the Pipeline Works*

    1.	Main Lookup reads all active rows from the configuration table (tblconfig).
    2.	Each configuration record is processed inside a ForEach loop.
    3.	Within the loop, a secondary Lookup retrieves the maximum value of the configured delta column from the source table.
    4.	The Copy Activity loads only records where:
      delta_column > last_processed_value (from tblconfig)
    5.	A Stored Procedure updates the Last Processed Value (LPV) in the configuration table after each load.
