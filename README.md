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

After I have processed the data during the process pharse I be using Rstudio to perform the analysis. I be conducting descriptive analysis, after that comparing members and causals. Next I be seeing the average ride time by each day for members vs causals and notice the days of the week are out of order. After that analyze ridership data by type and weekday. 

### Step 4: Conduct Descriptive analysis

### Descriptive analysis on ride_length (all figures in seconds)
mean(all_trips_v2$ride_length) #straight average (total ride length / rides)
median(all_trips_v2$ride_length) #midpoint number in the ascending array of ride lengths
max(all_trips_v2$ride_length) #longest ride
min(all_trips_v2$ride_length) #shortest ride
summary(all_trips_v2$ride_length) #condense the four lines above to one line using summary() on the specific attribute


### Compare members and casual users
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = mean)
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = median)
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = max)
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = min) 

### See the average ride time by each day for members vs casual users
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual + all_trips_v2$day_of_week, FUN = mean)

 all_trips_v2$member_casual all_trips_v2$day_of_week all_trips_v2$ride_length
 
1                      casual                   Friday           1726.9270 secs

2                      member                   Friday            766.9928 secs

3                      casual                   Monday           1831.8559 secs

4                      member                   Monday            758.7587 secs

5                      casual                 Saturday           2010.6898 secs

6                      member                 Saturday            877.4628 secs

7                      casual                   Sunday           2121.5550 secs

8                      member                   Sunday            887.9880 secs

9                      casual                 Thursday           1662.6278 secs

10                     member                 Thursday            746.6120 secs

11                     casual                  Tuesday           1574.6878 secs

12                     member                  Tuesday            736.7667 secs

13                     casual                Wednesday           1599.5752 secs

14                     member                Wednesday            738.6056 secs


### Notice that the days of the week are out of order. Let's fix that.
all_trips_v2$day_of_week <- ordered(all_trips_v2$day_of_week, levels=c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"))

### Now, let's run the average ride time by each day for members vs casual users

aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual + all_trips_v2$day_of_week, FUN = mean)

all_trips_v2$member_casual all_trips_v2$day_of_week all_trips_v2$ride_length

1                      casual                   Sunday           2121.5550 secs

2                      member                   Sunday            887.9880 secs

3                      casual                   Monday           1831.8559 secs

4                      member                   Monday            758.7587 secs

5                      casual                  Tuesday           1574.6878 secs

6                      member                  Tuesday            736.7667 secs

7                      casual                Wednesday           1599.5752 secs

8                      member                Wednesday            738.6056 secs

9                      casual                 Thursday           1662.6278 secs

10                     member                 Thursday            746.6120 secs

11                     casual                   Friday           1726.9270 secs

12                     member                   Friday            766.9928 secs

13                     casual                 Saturday           2010.6898 secs

14                     member                 Saturday            877.4628 secs

### analyze ridership data by type and weekday
all_trips_v2 %>% 
  mutate(weekday = wday(started_at, label = TRUE)) %>%  #creates weekday field using wday()
  group_by(member_casual, weekday) %>%  #groups by usertype and weekday
  summarise(number_of_rides = n()							#calculates the number of rides and average duration 
            ,average_duration = mean(ride_length)) %>% 		# calculates the average duration
  arrange(member_casual, weekday)								# sorts
  
  `summarise()` has grouped output by 'member_casual'. You can override using the
`.groups` argument.
  A tibble: 14 × 4
  Groups:   member_casual [2]
   member_casual weekday number_of_rides average_duration
   <chr>         <ord>             <int> <drtn>          
 1 casual        Sun              470098 2121.5550 secs  
 2 casual        Mon              302042 1831.8559 secs  
 3 casual        Tue              286986 1574.6878 secs  
 4 casual        Wed              285732 1599.5752 secs  
 5 casual        Thu              308584 1662.6278 secs  
 6 casual        Fri              359969 1726.9270 secs  
 7 casual        Sat              546090 2010.6898 secs  
 8 member        Sun              394650  887.9880 secs  
 9 member        Mon              466086  758.7587 secs  
10 member        Tue              524769  736.7667 secs  
11 member        Wed              512565  738.6056 secs  
12 member        Thu              501778  746.6120 secs  
13 member        Fri              459766  766.9928 secs  
14 member        Sat              441015  877.4628 secs  
> 
### Summary of my analysis:
  I have found that if I compare average ride time by each day for members vs casual users the casuals rode twice comparing the members in any day. Also if we compare week days vs weekend average duration of rides we can see that on weekends both causals and members rode more comparing the weekdays. The causals rode the highest on Sundays which is twice the time rides of the members. 

## Share
  
  I be sharing the visualization portion of my case study on this phase. I be using Rstudio and be sharing my thoughts on the visualization on here. 
  
  ### Let's visualize the number of rides by rider type

  all_trips_v2 %>% 
  mutate(weekday = wday(started_at, label = TRUE)) %>% 
  group_by(member_casual, weekday) %>% 
  summarise(number_of_rides = n()
            ,average_duration = mean(ride_length)) %>% 
  arrange(member_casual, weekday)  %>% 
  ggplot(aes(x = weekday, y = number_of_rides, fill = member_casual)) +
  geom_col(position = "dodge")
  
  `summarise()` has grouped output by 'member_casual'. You can override using the
`.groups` argument.
  
  ![unnamed-chuck-1-1](https://github.com/ChristinaRo12/Cyclistic-bike-share-analysis-case-study/blob/afccd5e308903098ded2de9ca32a2a7f84d828bc/Visualization%20the%20number%20of%20rides%20by%20ride%20types.png)
  
From this visualization we realize that members rode more on the weekdays compared to causal users. Also casuals rode more during the weekends.
  
### Let's create a visualization for average duration

  `summarise()` has grouped output by 'member_casual'. You can override using the
`.groups` argument.
  
  all_trips_v2 %>% 
  mutate(weekday = wday(started_at, label = TRUE)) %>% 
  group_by(member_casual, weekday) %>% 
  summarise(number_of_rides = n()
            ,average_duration = mean(ride_length)) %>% 
  arrange(member_casual, weekday)  %>% 
  ggplot(aes(x = weekday, y = average_duration, fill = member_casual)) +
  geom_col(position = "dodge")

![unnamed-chuck-2-1](https://raw.githubusercontent.com/ChristinaRo12/Cyclistic-bike-share-analysis-case-study/afccd5e308903098ded2de9ca32a2a7f84d828bc/Visualization%20for%20average%20duration.png)
  
From the average duration visualization we realize that on average casuals rode more than the members in any given day. During the weekends both the riders rode more compared to the weekdays. 

### Step 5: Export summary file for further analysis
  
 counts <- aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual + all_trips_v2$day_of_week, FUN = mean)
write.csv(counts, file = 'avg_ride_length.csv')

This is the summary file that can be saved on the desktop using this code. This is the [link](https://github.com/ChristinaRo12/Cyclistic-bike-share-analysis-case-study/blob/main/avg_ride_length.csv) to the csv file.
## Act
 
### The top three recommendation for the marketing according to my analysis

1.) Giving discounts to the members during the busy time of the week which is the weekend. This can make casual riders become members. 
           
2.) Promote a sense of community by having a campaign on how riding bikes changed lives and which might increase membership.
           
3.) Extending extra benefits to the members and giving week days more facilities. 
          
