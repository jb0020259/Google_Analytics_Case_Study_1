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

Hence the choices of tool was between R and Tableau. While R is great for doing extensive data analysis, I found tableau much easier to process and analyze the data. Also the time I could save in writing the code for processing the data, I was able to use it in visualizing the data. In nutshell, **Tableau Public edition** was my choice for doing the data analysis.

Since, data was available in 12 separate CSV files, I had to create a union in Tableau so that I can do the analysis on the complete dataset.

After loading the data, I noticed that there are total 13 columns - ride_id,rideable_type,started_at,ended_at,start_station_name,start_station_id,end_station_name,end_station_id,start_lat,start_lng,end_lat,end_lng,member_casual

Since the data is about the bike trips, I had to first calculate the duration and the distance for each trip as it is not readily available.

Trip duration could be derived using the trip start and trip end time available in the data. However the format of those columns were in Date Time so I had to use `DATEDIFF('second',[Started At],[Ended At])` function in Tableau to first extract the time from the Date Time data and then find the trip duration in Seconds by subtracting start time from the end time.

After analyzing the trip duration data it was evident that some of trips were too long to be shown in the seconds unit. So I had to derive the trip duration in Hours and Minutes also using the `DATEDIFF('hour',[Started At],[Ended At])` and `DATEDIFF('minute',[Started At],[Ended At])` functions.

Next calculation performed was trip distance. For this, I had to use the trip start and end latitude and longitude data. In Tableau, we can use `DISTANCE(MAKEPOINT([Start Lat],[Start Lng]),makepoint([End Lat],[End Lng]),"km")` function to calculate the distance between start and end location. We can also specify the distance unit which in my case was Kilometers.

When I take a look at the trip duration data, many trips had a duration in negative values. I had to filter out these values as a trip cannot have a *negative duration*.

I had also take a step to find out the week day from the date time using tableau function `datename("weekday",[Started At])`. This will help in analyzing the trip pattern during weekdays.

To analyze the trip duration distribution I had created separate bins using the below formula to add a calculated field- 

`if [Trip Duration (Minutes)] < 5 then "<5 Min" 
  elseif [Trip Duration (Minutes)] < 15 THEN "<15 Min" 
  elseif [Trip Duration (Minutes)] < 30 then "<30 Min" 
  elseif [Trip Duration (Minutes)] < 60 then "<1 Hr" 
  elseif [Trip Duration (Hour)] < 2 THEN "<2 Hr" 
  elseif [Trip Duration (Hour)] < 5 then "<5 Hr" 
  else ">5 Hr" 
  END`

After processing the data for analysis, it was time to start the analyze phase.

# Analyze

To answer the question - "How does member and casual users differ?" I had to compare their trip total duration, total distance, average duration and average distance.

After comparing these data it was surprising to note that even though the annual members were using the bikes more frequently (Total # of trip counts), it was the casual members who spent more time riding the bikes.

This fact, lead me to think that there could be two reasons for casual users to spend more time riding -
1. Either the distance is more
2. Or they are riding slower

To find the answer, I decided to find the average trip duration for casual users and member users, which turned out to be similar. This proved, that casual users were riding slower (or they were making frequent stops before they reached their destination). This is hypothetically possible if they are riding for the leisure as compared to the members who could be riding with some purpose or with some urgency.

A trend analysis showed that July-September saw the highest number of users taking bike rides gradually decreasing as we reached february month. However before we can confirm that this is a cyclical trend (occurring every year), we need more (older) data which is out of scope for the current study.

Another key question I attempted to solve was on which day and time of the week are users taking more rides? This analysis revealed that member users start riding earlier than the casual users who typically start in the evening around 6PM. Also it was evident that there was a clear trend in both types of user to use bikes more as they approach the weekend (peaking on the Saturday). This is another information which could be useful if we wish to run any targetted campaign or customer survey.

Apart from this, it was interesting find maximum trip duration, maximum trip distance, round trip calcuations (start and end location are same), preference for the bike types and the comparison of ride duration distribution. Most of the members take rides which is less than 15 minutes duration where as most of the casual users ride between 15 mins to 1 hour.

# Share

After analyzing the data in detail, it was the time to share the analysis in engaging way with the stakeholders. For this reason, I created a dashboard in the Tableau which combined each of the above analysis in a nice story telling manner.

Dashboard for the above analysis is available online on my Tableau public [profile](https://public.tableau.com/profile/jamesbond#!/vizhome/Case_Study_1_16190131371450/Dashboard1)

# Act

Based on the above data analysis, here are the 3 top recommendations to the stakeholder -
1. Schedule a marketing campaign on Saturday and Sunday between 10AM to 7PM where most of the casual members are using the service so that the campaign is effective.
2. Since it is now known that casual riders are riding for longer duration, adapt the marketing strategy such that it takes this finding into account. e.g. Company can highlight this finding to the casual customers to make them aware that they are already using the service for longer duration so why not benefit more by becoming the annual members. 
3. Since it can be seen that casual users are taking ride mainly for liesure, marketing team can come up with some annual membership offers around the leisure activities/locations so that casual users can be converted to annual members.
4. Company can anticipate the surge in demand during the month of July-September. However we need more data to confirm this trend.
5. Company must investigate why some of the station names/Id are missing from the data. Add the missing data for more accurate insights.
6. Company must investigate why some of the trip has duration in negative. Fix/improve the data collection as needed.
7. The fact that Docked bikes has the highest trip count is a matter of investigation. Is it that most customers are forgetting to end the trip or the docking process is not easy which makes the users leave the bikes without ending the trip.






  

  
