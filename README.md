# Cyclistic-bike-share-analysis-case-study
By Christina Rozario
## **Introduction:**

This is a case study I had to do to get my google data analytic certification under Google Data Analytics Capstone: Complete a Case Study. This portfolio is representing my work for the case study. The data of the case study can be found in this [link](https://divvy-tripdata.s3.amazonaws.com/index.html). The name of the case study is Cyclistic bike-share analysis case study. Cyclistic is a fictional company. The original data is made available by Motivate International Inc.under this [license](https://ride.divvybikes.com/data-license-agreement). 

To answer the business questions I am going to follow the steps of the data analysis process: ask,prepare, process, analyze, share, and act. Each of these steps is going to have : 1.) Guiding questions 2.) Key tasks 3.) Deliverable 4.) Any other steps such as using tools and codes. I’m going to also present from the case study directly first the scenario and the characters & team below to answer the questions. 

### Scenario
You are a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, your team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights,
your team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve your recommendations, so they must be backed up with compelling data insights and professional data visualizations.

### Characters and teams
● Cyclistic: A bike-share program that features more than 5,800 bicycles and 600 docking stations. Cyclistic sets itself apart by also offering reclining bikes, hand tricycles, and cargo bikes, making bike-share more inclusive to people with disabilities and riders who can’t use a standard two-wheeled bike. The majority of riders opt for traditional bikes; about 8% of riders use the assistive options. Cyclistic users are more likely to ride for leisure, but about 30% use them to commute to work each day.

● Lily Moreno: The director of marketing and your manager. Moreno is responsible for the development of campaigns and initiatives to promote the bike-share program. These may include email, social media, and other channels.

● Cyclistic marketing analytics team: A team of data analysts who are responsible for collecting, analyzing, and reporting data that helps guide Cyclistic marketing strategy. You joined this team six months ago and have been busy learning about Cyclistic’s mission and business goals — as well as how you, as a junior data analyst, can help Cyclistic achieve them.

● Cyclistic executive team: The notoriously detail-oriented executive team will decide whether to approve the recommended marketing program.



## **Ask** 

My role: I am a junior data analyst working in the marketing analyst team at Cyclistic who is working to meet the goal that will be discussed below to create marketing strategies. 

Goal/the problem I am trying to solve/purpose: 
Creating marketing strategies to turn casual riders into annual members. 

Business Task/Question: 
“How do annual members and casual riders use Cyclistic bikes differently?”


## **Prepare**

I am using the data from [https://divvy-tripdata.s3.amazonaws.com/index.html](https://divvy-tripdata.s3.amazonaws.com/index.html) which was provided by the case study. These are all csv files. I was instructed to download the previous 12 month’s data which is June 2021 to May 2022. The original data is made available by Motivate International Inc.under this [license](https://ride.divvybikes.com/data-license-agreement). Data has integrity since all the files have consistent columns and each column has the correct type of data. I be using Rstudio to do my all work since I be working with large amount of data. 

### Download data and store it

I be installing following packages:

### Installing packages

install.packages('tidyverse')

install.packages('lubridate')

install.packages('ggplot2')

install.packages('dplyr')

### load the packages

library('tidyverse')

library('lubridate')

library('ggplot2')

library('readr')


### Step 1: collecting data

tripdata_202106 <- read.csv("C:/Users/202106-divvy-tripdata.csv")

tripdata_202107 <- read.csv("C:/Users/202107-divvy-tripdata.csv")

tripdata_202108 <- read.csv("C:/Users/202108-divvy-tripdata.csv")

tripdata_202109 <- read.csv("C:/Users/202109-divvy-tripdata.csv")

tripdata_202110 <- read.csv("C:/Users/202110-divvy-tripdata.csv")

tripdata_202111 <- read.csv("C:/Users/202111-divvy-tripdata.csv")

tripdata_202112 <- read.csv("C:/Users/202112-divvy-tripdata.csv")

tripdata_202201 <- read.csv("C:/Users/202201-divvy-tripdata.csv")

tripdata_202202 <- read.csv("C:/Users/202202-divvy-tripdata.csv")

tripdata_202203 <- read.csv("C:/Users/202203-divvy-tripdata.csv")

tripdata_202204 <- read.csv("C:/Users/202204-divvy-tripdata.csv")

tripdata_202205 <- read.csv("C:/Users/202205-divvy-tripdata.csv")


## Process

Since the data is too big I am using Rstudio to process the data. In this process I am preparing the data for analysis. I am combining all 12 data sets into a single data frame.

### Step 2: Wrangle data and combining into a dataframe(into a single file)
Compare column names each of the files
While the names don't have to be in the same order, they DO need to match perfectly before we can use a command to join them into one file

colnames(tripdata_202106)
colnames(tripdata_202107)
colnames(tripdata_202108)
colnames(tripdata_202109)
colnames(tripdata_202110)
colnames(tripdata_202111)
colnames(tripdata_202112)
colnames(tripdata_202201)
colnames(tripdata_202202)
colnames(tripdata_202203)
colnames(tripdata_202204)
colnames(tripdata_202205)

### Inspect the dataframes and look for incongruencies

str(tripdata_202106)
str(tripdata_202107)
str(tripdata_202108)
str(tripdata_202109)
str(tripdata_202110)
str(tripdata_202111)
str(tripdata_202112)
str(tripdata_202201)
str(tripdata_202202)
str(tripdata_202203)
str(tripdata_202204)
str(tripdata_202205)

### Stack individual quarter's data frames into one big data frame
all_trips <- bind_rows(tripdata_202106, tripdata_202107, tripdata_202108, tripdata_202109,
tripdata_202110, tripdata_202111, tripdata_202112, tripdata_202201, tripdata_202202, tripdata_202203,
tripdata_202204, tripdata_202205)

### Sort and filter data:

Remove lat, long, fields as this data was dropped beginning in 2020
all_trips <- all_trips %>%  
  select(-c(start_lat, start_lng, end_lat, end_lng)) 
  
  ### Step 3: Clean up and add data to prepare for analysis         
         
 ### Inspect the new table that has been created

colnames(all_trips)  #List of column names
nrow(all_trips)  #How many rows are in data frame?
dim(all_trips)  #Dimensions of the data frame?
head(all_trips)  #See the first 6 rows of data frame.  Also tail(all_trips)
str(all_trips)  #See list of columns and data types (numeric, character, etc)
summary(all_trips)  #Statistical summary of data. Mainly for numericstable(all trips)
         
### There are a few problems we will need to fix:
 (1) The data can only be aggregated at the ride-level,which is too granular. We will want to add some additional columns of data -- such as day, month, year -- that provide additional opportunities to aggregate the data.
 (2) We will add "ride_length" to the entire dataframe for consistency.
 (3) There are some rides where trip duration shows up as negative, including several hundred rides where Divvy took bikes out of circulation for Quality Control reasons. We will want to delete these rides.

### 1.)
 Add columns that list the date, month, day, and year of each ride
 This will allow us to aggregate ride data for each month, day, or year ... before completing these operations we could only aggregate at the ride level

all_trips$date <- as.Date(all_trips$started_at) #The default format is yyyy-mm-dd
all_trips$month <- format(as.Date(all_trips$date), "%m")
all_trips$day <- format(as.Date(all_trips$date), "%d")
all_trips$year <- format(as.Date(all_trips$date), "%Y")
all_trips$day_of_week <- format(as.Date(all_trips$date), "%A")

### 2.)
 ### Add a "ride_length" calculation to all_trips (in seconds)

all_trips$ride_length <- difftime(all_trips$ended_at,all_trips$started_at)

### Inspect the structure of the columns
str(all_trips)

### 3):

### Cleaning data:

all_trips <- na.omit(all_trips) #remove rows with NA values
all_trips <- distinct(all_trips) #remove duplicate rows 
all_trips <- all_trips[!(all_trips$ride_length <=0),] #remove where ride_length is 0 or negative

### Create a new version of the dataframe (v2) since data is being removed

all_trips_v2 <- all_trips[!(all_trips$start_station_name == "HQ QR" | all_trips$ride_length<0),]

## Analyze

After I have processed the data during the process pharse I be using Rstudio to perform the analysis. 







