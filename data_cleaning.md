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

#### What does my processed dataset contain?
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
    * If in January row, 1st column ride_id contains 0, it implies no missing values in ride_id column of January csv file.
    * If in February row, start_station_name contains say, 15600, it implies that in the Feb csv file, there are 15,600 missing values in station_name column.  
    
#### What's the point in calculating all these missing values in this manner?
Objective was to detect whether missing values are present in all columns or specific columns in all 12 files. The latter turned out to be true. All 12 files contained missing values only in the following columns- station names, station ids, end latitude and end longitude.

#### Removal of rows/columns with NA values:
1. Removed all columns with station names and station ids. [4 columns]  
Function used: year_dataset <- select(year_dataset,-c("start_station_name",                         "start_station_id", "end_station_name","end_station_id"))
2. Removed all rows with no end station latitude and longitude.  
Function used: 
Now the dataset contains no missing values.  
All NA values were removed with minimum data loss. 

#### Other Wrangling steps:
1. Remove all duplicate rows.  
Function used: sum(duplicated(year_dataset))  
if sum returned is 0, it implies no duplicate rows found.  

2. Check whether all values in ride_id column contain 16 characters or not.  
The ones with more or less than 16 will be discarded.  
id_length <- data.frame(nchar(year_dataset$ride_id))  
distinct(id_length)  

3. Rounding off latitude and longitude values.
  * This was a tricky part- rounding-off is not a good option for gps data, as more the decimal places, more accurate is the data.  
  [I did not round off grid values.]

#### After performing all these steps, the data will be ready for analysis. Phew!!