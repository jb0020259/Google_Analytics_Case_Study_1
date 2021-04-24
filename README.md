## Google Analytics Professional Certificate Case Study 1 - Analyze the bike trips data

# ASK

### Business Task (Goal of the data analysis) - How do annual members and casual riders use Cyclistic bikes dierently?

Answer to the above question will help company understand the key differences in their customer behaviors and adapt a marketing strategy targetted towards casual members such that they can be converted into a annual members.

Below are the Key Stakeholders for the project - 
  Primary Stakeholder - Lily Moreno (Director of Marketing)
  Secondary Stakeholders - Cyclistic Marketing analytics team and Executive team

# PREPARE

Data source used for doing the analysis - [Cyclistic trip data](https://divvy-tripdata.s3.amazonaws.com/index.html)

Data is stored online on a S3 data store. Data is in the form of 12 different .zip files containing a CSV file each. One CSV file with trip data for each of the past 12 months.
Data is provided under this [license](https://www.divvybikes.com/data-license-agreement)

Data passess in the ROCCC test -
  It is Reliable & Original as the data is provided by the first party (where data was originated)
  It covers detailed trip data for the last 12 months including the location, date, time , customer type
  It is also the current (as it refer to the recent 12 months)
  It was not cited as the data was generated in house
  
It could not be proven that the data was unbiased as the data did not have more information on the participants (Age, Sex, Location, Physical Condition etc.)

So further analysis is done with the *Assumption* that the data generated was covering all participants without any specific bias.

# PROCESS

Since the data size is large, it was not suitable to use the Excel due to memory/time constraints. SQL was ruled out as it needed to setup a database and for doing the data visualization I would have to use a separate tool. 

Hence the choices of tool was between R and Tableau. While R is great for doing extensive data analysis, I found tableau much easier to process and analyze the data. Also the time I could save in writing the code for processing the data, I was able to use it in visualizing the data. In nutshell, Tableau Public edition was my choice for doing the data analysis.

Since, data was available in 12 separate CSV files, I had to create a union in Tableau so that I can do the analysis on the complete dataset.

After loading the data, I noticed that there are total 13 columns - ride_id,rideable_type,started_at,ended_at,start_station_name,start_station_id,end_station_name,end_station_id,start_lat,start_lng,end_lat,end_lng,member_casual

Since the data is about the bike trips, I had to first calculate the duration and the distance for each trip as it is not readily available.

Trip duration could be derived using the trip start and trip end time available in the data. However the format of those columns were in Date Time so I had to use `DATEDIFF('second',[Started At],[Ended At])` function in Tableau to first extract the time from the Date Time data and then find the trip duration in Seconds by subtracting start time from the end time.

After analyzing the trip duration data it was evident that some of trips were too long to be shown in the seconds unit. So I had to derive the trip duration in Hours and Minutes also using the DATEDIFF('hour',[Started At],[Ended At]) and `DATEDIFF('minute',[Started At],[Ended At])` functions.






  

  
