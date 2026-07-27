# Load an Additional Year of Data Using Spark SQL

Overview
This submodule demonstrates how to use Apache Spark in Cloudera AI (CAI / Workbench) to load an additional year of data (2008) into an Iceberg table. You will run PySpark code directly within your CAI Jupyter notebook session that reads data from a CSV file stored in AWS S3, filters the data for a specific year, and inserts it into the existing Iceberg table.  

The goal is to showcase Iceberg's multi-engine capabilities, specifically highlighting Spark in CAI for interactive data manipulation and Impala for querying the results in Cloudera Data Warehouse (CDW/Hue).  

## Prerequisites
Before running this submodule, ensure that:

You have launched a Jupyter Notebook session inside your Cloudera AI (CAI / Workbench) project with a Spark-enabled runtime/kernel.

Your session is configured with appropriate credentials to access AWS S3 cloud storage.

The Iceberg table (flights) was created in earlier steps using CDW/Hue (Hive).

Step-by-Step Guide
Step 1: Open Your CAI Jupyter Notebook
Open your active Jupyter Notebook session in CAI.

Either create a new notebook (.ipynb) or navigate to a new code cell in your current workbench notebook.

Step 2: Execute the PySpark Ingestion Code
In your Jupyter Notebook cell, run the following PySpark code. Make sure to replace <prefix> with your assigned user ID.

```Python
from pyspark.sql import SparkSession
import pyspark.sql.functions as F

#---------------------------------------------------
# CREATE / GET SPARK SESSION
#---------------------------------------------------
spark = SparkSession.builder \
    .appName('<prefix>-IcebergAdd2008') \
    .getOrCreate()

#-----------------------------------------------------------------------------------
# LOAD DATA (2008) FROM RAW CSV TABLE INTO ICEBERG TABLE
#-----------------------------------------------------------------------------------
print("JOB STARTED...")

# Run INSERT INTO Iceberg table from the raw CSV table
spark.sql("INSERT INTO <prefix>_airlines.flights SELECT * FROM <prefix>_airlines_csv.flights_csv WHERE year = 2008")

print("JOB COMPLETED.")

```
Note: Because you are running interactively in CAI, SparkSession is often automatically initialized as spark. Using .getOrCreate() ensures it leverages your active session gracefully.


Step 3: Monitor Execution in Notebook Output
Click Run (or press Shift + Enter) to execute the cell.

Watch the progress bar under the cell in your notebook interface.

Once finished, you will see the "JOB COMPLETED." output printed below the cell.

Step 4: Verify the Data in CDW/Hue
Once the PySpark execution completes, verify that the 2008 data has been added to the Iceberg table using Impala in CDW/Hue (or via a spark.sql(...) query in a new notebook cell).

In CDW/Hue, execute the following query to check the record count for 2008:
   ```
   SELECT year, count(*)
   FROM ${prefix}_airlines.flights
   GROUP BY year
   ORDER BY year desc;
   ```

2. You should see the records for the year 2008 along with data for the previous years.

   ![Query Results](../../images/64.png)

## Conclusion

This submodule demonstrated how to use Spark in Cloudera Data Engineering to load data into an Iceberg table. You have successfully inserted data for an additional year (2008) and verified the results using CDW/Hue. This process highlights Iceberg’s multi-engine capabilities, enabling both Spark-based data manipulation and Impala querying for analysis.

## Next Steps

Continue exploring more advanced functionality with Iceberg and Spark, or return to the main module for additional exercises and insights.
