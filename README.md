# Movie Ratings Analysis

## **Prerequisites**

Before starting the assignment, ensure you have the following software installed and properly configured on your machine:

1. **Python 3.x**:
   - [Download and Install Python](https://www.python.org/downloads/)
   - Verify installation:
     ```bash
     python3 --version
     ```

2. **PySpark**:
   - Install using `pip`:
     ```bash
     pip install pyspark
     ```

3. **Apache Spark**:
   - Ensure Spark is installed. You can download it from the [Apache Spark Downloads](https://spark.apache.org/downloads.html) page.
   - Verify installation by running:
     ```bash
     spark-submit --version
     ```

4. **Docker & Docker Compose** (Optional):
   - If you prefer using Docker for setting up Spark, ensure Docker and Docker Compose are installed.
   - [Docker Installation Guide](https://docs.docker.com/get-docker/)
   - [Docker Compose Installation Guide](https://docs.docker.com/compose/install/)

## **Setup Instructions**

### **1. Project Structure**

Ensure your project directory follows the structure below:

```
MovieRatingsAnalysis/
├── input/
│   └── movie_ratings_data.csv
├── outputs/
│   ├── binge_watching_patterns.csv
│   ├──churn_risk_users.csv
│   └── movie_watching_trends.csv
├── src/
│   ├── task1_binge_watching_patterns.py
│   ├── task2_churn_risk_users.py
│   └── task3_movie_watching_trends.py
├── docker-compose.yml
└── README.md
```









- **input/**: Contains the `movie_ratings_data.csv` dataset.
- **outputs/**: Directory where the results of each task will be saved.
- **src/**: Contains the individual Python scripts for each task.
- **docker-compose.yml**: Docker Compose configuration file to set up Spark.
- **README.md**: Assignment instructions and guidelines.

### **2. Running the Analysis Tasks**

You can run the analysis tasks either locally or using Docker.

#### **a. Running Locally**

1. **Navigate to the Project Directory**:
   ```bash
   cd MovieRatingsAnalysis/
   ```

2. **Execute Each Task Using `spark-submit`**:
   ```bash
   spark-submit src/task1_binge_watching_patterns.py
   spark-submit src/task2_churn_risk_users.py
   spark-submit src/task3_movie_watching_trends.py
   ```

3. **Verify the Outputs**:
   Check the `outputs/` directory for the resulting files:
   ```bash
   ls outputs/
   ```
   You should see:
   - `binge_watching_patterns.txt`
   - `churn_risk_users.csv`
   - `movie_watching_trends.csv`

#### **b. Running with Docker (Optional)**

1. **Start the Spark Cluster**:
   ```bash
   docker-compose up -d
   ```

2. **Access the Spark Master Container**:
   ```bash
   docker exec -it spark-master bash
   ```

3. **Navigate to the Spark Directory**:
   ```bash
   cd /opt/bitnami/spark/
   ```

4. **Run Your PySpark Scripts Using `spark-submit`**:
   ```bash
   spark-submit src/task1_binge_watching_patterns.py
   spark-submit src/task2_churn_risk_users.py
   spark-submit src/task3_movie_watching_trends.py
   ```

5. **Exit the Container**:
   ```bash
   exit
   ```

6. **Verify the Outputs**:
   On your host machine, check the `outputs/` directory for the resulting files.

7. **Stop the Spark Cluster**:
   ```bash
   docker-compose down
   ```

## **Overview**

In this assignment, you will leverage Spark Structured APIs to analyze a dataset containing employee information from various departments within an organization. Your goal is to extract meaningful insights related to employee satisfaction, engagement, concerns, and job titles. This exercise is designed to enhance your data manipulation and analytical skills using Spark's powerful APIs.

## **Objectives**

By the end of this assignment, you should be able to:

1. **Data Loading and Preparation**: Import and preprocess data using Spark Structured APIs.
2. **Data Analysis**: Perform complex queries and transformations to address specific business questions.
3. **Insight Generation**: Derive actionable insights from the analyzed data.

## **Dataset**

## **Dataset: Advanced Movie Ratings & Streaming Trends**

You will work with a dataset containing information about **100+ users** who rated movies across various streaming platforms. The dataset includes the following columns:

| **Column Name**         | **Data Type**  | **Description** |
|-------------------------|---------------|----------------|
| **UserID**             | Integer       | Unique identifier for a user |
| **MovieID**            | Integer       | Unique identifier for a movie |
| **MovieTitle**         | String        | Name of the movie |
| **Genre**             | String        | Movie genre (e.g., Action, Comedy, Drama) |
| **Rating**            | Float         | User rating (1.0 to 5.0) |
| **ReviewCount**       | Integer       | Total reviews given by the user |
| **WatchedYear**       | Integer       | Year when the movie was watched |
| **UserLocation**      | String        | User's country |
| **AgeGroup**          | String        | Age category (Teen, Adult, Senior) |
| **StreamingPlatform** | String        | Platform where the movie was watched |
| **WatchTime**        | Integer       | Total watch time in minutes |
| **IsBingeWatched**    | Boolean       | True if the user watched 3+ movies in a day |
| **SubscriptionStatus** | String        | Subscription status (Active, Canceled) |

---



### **Sample Data**

Below is a snippet of the `movie_ratings_data.csv` to illustrate the data structure. Ensure your dataset contains at least 100 records for meaningful analysis.

```
UserID,MovieID,MovieTitle,Genre,Rating,ReviewCount,WatchedYear,UserLocation,AgeGroup,StreamingPlatform,WatchTime,IsBingeWatched,SubscriptionStatus
1,101,Inception,Sci-Fi,4.8,12,2022,US,Adult,Netflix,145,True,Active
2,102,Titanic,Romance,4.7,8,2021,UK,Adult,Amazon,195,False,Canceled
3,103,Avengers: Endgame,Action,4.5,15,2023,India,Teen,Disney+,180,True,Active
4,104,The Godfather,Crime,4.9,20,2020,US,Senior,Amazon,175,False,Active
5,105,Forrest Gump,Drama,4.8,10,2022,Canada,Adult,Netflix,130,True,Active
...
```

## **Assignment Tasks**

You are required to complete the following three analysis tasks using Spark Structured APIs. Ensure that your analysis is well-documented, with clear explanations and any relevant visualizations or summaries.

### **1. Identify Departments with High Satisfaction and Engagement**

**Objective:**

Determine which movies have an average watch time greater than 100 minutes and rank them based on user engagement.

**Tasks:**

- **Filter Movies**: Select movies that have been watched for more than 100 minutes on average.
- **Analyze Average Watch Time**: Compute the average watch time per user for each movie.
- **Identify Top Movies**: List movies where the average watch time is among the highest.


**Expected Outcome:**

A list of departments meeting the specified criteria, along with the corresponding percentages.

**Example Output:**

| Age Group   | Binge Watchers | Percentage |
|-------------|----------------|------------|
| Teen        | 195            | 45%        |
| Adult       | 145            | 38%        |

---

### **2. Identify Churn Risk Users**  

**Objective:**  

Find users who are **at risk of churn** by identifying those with **canceled subscriptions and low watch time (<100 minutes)**.

**Tasks:**  

- **Filter Users**: Select users who have `SubscriptionStatus = 'Canceled'`.  
- **Analyze Watch Time**: Identify users with `WatchTime < 100` minutes.  
- **Count At-Risk Users**: Compute the total number of such users.  

**Expected Outcome:**  

A count of users who **canceled their subscriptions and had low engagement**, highlighting **potential churn risks**.

**Example Output:**  


|Churn Risk Users                                  |	Total Users |
|--------------------------------------------------|--------------|
|Users with low watch time & canceled subscriptions|	350         |



---

### **3. Trend Analysis Over the Years**  

**Objective:**  

Analyze how **movie-watching trends** have changed over the years and find peak years for movie consumption.

**Tasks:**  

- **Group by Watched Year**: Count the number of movies watched in each year.  
- **Analyze Trends**: Identify patterns and compare year-over-year growth in movie consumption.  
- **Find Peak Years**: Highlight the years with the highest number of movies watched.  

**Expected Outcome:**  

A summary of **movie-watching trends** over the years, indicating peak years for streaming activity.

**Example Output:**  

| Watched Year | Movies watched |
|--------------|----------------|
| 2020         | 1200           |
| 2021         | 1500           |
| 2022         | 2100           |
| 2023         | 2800           |


---
### Challenges and Solutions
1. CSV Output Format
Challenge: Spark's write.csv() method creates a directory with part files, making it difficult to generate a single CSV file. Solution: Modified the write_output function to convert Spark DataFrames to pandas DataFrames and used to_csv() to directly output a single CSV file, eliminating the need for post-processing.

2. Data Type Handling
Challenge: Boolean values in the input data, particularly for the IsBingeWatched column, required special handling. Solution: Defined an explicit schema during data loading to ensure correct data type interpretation and filtered boolean values using col("IsBingeWatched") == True for compatibility with Spark SQL expressions.


3. Percentage Calculation and Rounding
Challenge: Accurate calculation and rounding of percentages for binge-watchers. Solution: Used Spark's mathematical functions for division and spark_round to ensure proper rounding to two decimal places. Combined binge-watcher and total user counts before calculating percentages for accurate results.

4. Resource Management
Challenge: Efficient resource utilization when processing large datasets. Solution: Managed resources by explicitly stopping the Spark session after job completion with spark.stop() and applied early filtering in the data processing pipeline to reduce the volume of data in subsequent steps.

## Conclusion
    This project showcases the power of PySpark's structured API for analyzing large-scale movie ratings data. By exploring binge-watching behavior, identifying churn risk users, and analyzing movie viewing trends, we gain valuable insights into user preferences and behavior. These insights can enhance content recommendation systems, inform retention strategies, and guide content acquisition decisions for streaming platforms.

         The code's modular design enables easy extension to accommodate additional analyses or adapt to changes in the data schema. Utilizing pandas for output writing streamlines the generation of readable CSV files from Spark DataFrames.

         By addressing the challenges outlined, we've developed a robust analytical solution that produces clean, actionable outputs while taking full advantage of Spark's distributed processing capabilities for scalable data analysis.


