## Schema inference and manual schema definition

Spark can automatically infer schemas...but sometimes it misinterprets data types, particularly with complex or ambiguous data. Manually defining a schema can ensure accurate data handling.
Manual schemas prevent Spark from guessing wrong types (e.g., reading numbers as strings.

## DataTypes in PySpark DataFrames

|Type|Example|Description|
|---|---|---|
|`IntegerType()`|42|Whole numbers|
|`LongType()`|100000|Large integers|
|`FloatType()`|3.14|Low-precision decimals|
|`DoubleType()`|12.567|High-precision decimals|
|`StringType()`|"Utkarsh"|Text values|

All come from pyspark.sql.types

## DataType Syntax Recap

``` python
from pyspark.sql.types import StructType, StructField, StringType, IntegerType  
schema = StructType([ StructField("emp_id", IntegerType(), True), StructField("emp_name", StringType(), True),    
StructField("salary", DoubleType(), True) ] )
#set the Schema 
df= spark.createdataframe(data,schema= schema)

```


`StructType()` → whole schema  
`StructField()` → each column definition

## Data Frames Selection and Filtering 
- Use .select() to choose specific columns 
- Use .filter () or .where() to filter data 
- Use .Sort to order by a collection of columns 

``` python
df.select("name","salary").filter(df.salary > 50000).show()
```
## Sorting & Dropping Missing Values

### Sorting

``` python
df.sort(df.age).show() 
df.orderBy(df.salary.desc()).show()
```

- `.sort()` → simple sorting
    
- `.orderBy()` → multi-column or ascending/descending control

### Handling Nulls

``` python
df.na.drop().show()                          
# Drop rows with any nulls         
df.na.fill({"city":"Unknown","salary":0}).show()  
# Replace missing values
```

✔ `.na.drop()` removes nulls  
✔ `.na.fill()` fills or imputes missing data

# Q) Schema writeout

We've loaded Schemas multiple ways now. So lets define a schema directly. We'll use a Data dictionary:

|Variable|Description|
|---|---|
|age|Individual age|
|education_num|Education by degree|
|marital_status|Marital status|
|occupation|Occupation|
|income|Categorical income|
```python
from pyspark.sql.types import StructType, StructField, IntegerType, StringType
# Fill in the schema with the columns you need from the exercise instructions
schema = StructType([StructField("age",IntegerType()),

                     StructField("education_num",IntegerType()),

                     StructField("marital_status",StringType()),

                     StructField("occupation",StringType()),

                     StructField("income",StringType())

                    ])
# Read in the CSV, using the schema you defined above

census_adult = spark.read.csv("adult_reduced_100.csv", sep=',', 
header=False, schema=schema)
# Print out the schema
census_adult.printSchema()
```
