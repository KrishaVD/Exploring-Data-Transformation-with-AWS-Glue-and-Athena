# Exploring-Data-Transformation-with-AWS-Glue-and-Athena

Today, we're delving into an insightful journey with AWS Glue and Athena, utilising data from a random user API. 

In my previous post, we configured Kinesis streams to consume data from this API, and stored the collected data in an S3 bucket. Today, we're going to process and analyse this data using AWS Glue and Athena.

# AWS Glue: Cataloguing Our Data

Our first step is to organise our data. AWS Glue provides an excellent feature for this purpose: a crawler. The crawler scans our data and creates a schema, organising it in a structured manner.

To create a Glue Crawler:

1. Navigate to the AWS Glue service and select "Crawlers" in the left-hand navigation panel.
2. Click on "Add Crawler" and follow the instructions, ensuring it points to the correct S3 bucket containing the data from the Kinesis stream.
3. After setting up the crawler, run it. Upon completion, it will have created a table in a database that represents our data's schema.

# Athena: Querying Our Data

With our data catalogued, we turn to Athena to extract insights. Athena is an interactive query service that makes it easy to analyse data directly in S3 using standard SQL.

Let's determine how many records we have:

```sql
SELECT COUNT(*) 
FROM your_database.your_table;
```

Next, let's calculate the percentage of male and female records:

```sql
SELECT gender, COUNT(*)*100.0/(SELECT COUNT(*) FROM your_database.your_table) as percentage
FROM your_database.your_table
GROUP BY gender;
```

Now, let's categorise the records into age groups:

```sql
SELECT 
  CASE 
    WHEN age BETWEEN 21 AND 29 THEN '21-29'
    WHEN age BETWEEN 30 AND 39 THEN '30-39'
    WHEN age BETWEEN 40 AND 49 THEN '40-49'
    ELSE '50+'
  END as age_group,
  COUNT(*) as count
FROM your_database.your_table
GROUP BY age_group;
```

Please remember to replace `your_database` and `your_table` in the SQL queries with your actual database and table names. 


# AWS Glue: Transforming Our Data

After gaining insights from Athena, we return to AWS Glue to create a Glue job. This will transform our data from JSON format to CSV, enhancing compatibility with various applications.

To create a Glue job:

1. Navigate to AWS Glue and select "Jobs" in the left-hand navigation panel.
2. Click on "Add Job", follow the instructions, and point it to your source (the JSON files) and target (the location for your CSV files).
3. After setting up the job, run it. The job will transform your JSON data into CSV format and store it in your designated S3 bucket.

# Congratulations!

You've just utilised AWS Glue for data cataloguing and transformation, and Athena for data querying. You're now adept at handling cloud data!

Join us in our next post as we delve deeper into AWS data services. Until then, continue exploring, transforming, and extracting valuable insights from your data!

