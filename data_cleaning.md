### Data Cleaning Process

#### Structure of the dataset:  
There are 12 csv files, one for each month of the year.  
They contain ride details about the bikes hired for commute.

1. ride_id
2. rideable_type- 3 categories: docked bikes, electric bikes & classic bikes.
3. start_time- date-time format
4. end_time- date-time format
5. start_station_name
6. end_station_name
7. start_station_id
8. end_station_id
9. start_lat- latitude of starting point
10. start_lng- longitude 
11. end_lat
12. end_lng
13. casual_member- 2 categories: members, casual (users without membership)

#### What does my processed dataset contains?
1. ride_id
2. bike_type- (renamed from rideable type)
3. start_time
4. end_time
5. duration (end_time - start_time)
6. start_lat
7. start_lng
8. end_lat
9. end_lng
10. user_type- (renamed from member_casual)

#### Steps taken for initial observation:
1. counted total NA values per column for each csv file and stored it in one dataframe each.  
Function used (e.g.): jan_na_count <- rbind(colSums(is.na(jan2021)))  
Where: 
  * jan_na_count- contains count of missing values in each column for January.
  * rbind- used for combining by rows. [cbind used for column bind]
  * is.na()- returns True in place of missing values and FALSE otherwise.
  * colSums()- used to calculate sum of all values in a column.
  * colSums(is.na())- returns total number of missing values in each column.  

2. Combined all such tables into 1 dataframe.  
Function used: total_na_count <- rbind(jan_na_count,feb_na_count,...)  

3. This total_na_count dataframe is a table with:
  * columns- all column names ride_id, start_time...etc.
  * rows- one row for each month- Jan, Feb,..so on.
  * values- the cell values contain total missing values for the given [row,column]
  For e.g.
    *If in January row, 1st column ride_type contains 0, it implies no missing values in ride_id column of January csv file.
    * If in February row, start_station_name contains say, 15600, it implies that in the Feb csv file, there are 15,600 missing values in station_name column.  
    
#### What's the point in calculating all these missing values in this manner?