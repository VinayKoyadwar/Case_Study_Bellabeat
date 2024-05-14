# Case_Study_Bellabeat
Bellabeat Case_Study with R Programming

**GOOGLE DATA ANALYTICS CAPSTONE PROJECT
CASE STUDY: BELLABEAT (RStudio)
DATA ANALYST: VINAY KOYADWAR
vinaykoyadwarcorp@gmail.com**

**INTRODUCTION**
This is a Bellabeat data analysis case study by the Google Data Analytics Capstone project. In this case study, I will perform many real-world tasks as a data analyst. To answer the key business questions, I will be following the steps of the data analysis process taught in the course: ask, prepare, process, analyze, share, and act.
SCENARIO
I am a junior data analyst working on the marketing analyst team at Bellabeat. Bellabeat is a high-tech manufacturer of health-focused products for women and a successful small company. The stakeholders believe that analyzing smart device fitness data could help unlock new growth opportunities for the company. I have been asked to focus on one of Bellabeat’s products and analyze smart device data to gain insight into how consumers are using their smart devices which will then help guide marketing strategy for the company.
PRODUCTS OF BELLABEAT 
Bellabeat app: The Bellabeat app provides users with health data related to their activity, sleep, stress, menstrual cycle, and mindfulness habits. This data can help users better understand their current habits and make healthy decisions. The Bellabeat app connects to their line of smart wellness products.
Leaf: Bellabeat’s classic wellness tracker can be worn as a bracelet, necklace, or clip. The Leaf Tracker connects to the Bellabeat app to track activity, sleep, and stress.
Time: This wellness watch combines the timeless look of a classic timepiece with smart technology to track user activity, sleep, and stress. The Time watch connects to the Bellabeat app to provide you with insights into your daily wellness.
Spring: This is a water bottle that tracks daily water intake using smart technology to ensure that you are appropriately hydrated throughout the day. The Spring bottle connects to the Bellabeat app to track your hydration levels.
Bellabeat membership: Bellabeat also offers a subscription-based membership program for users. Membership gives users 24/7 access to fully personalized guidance on nutrition, activity, sleep, health, beauty, and mindfulness based on their lifestyle and goals.
Overall, I am a working data analyst in a Bellabeat marketing analytics team responsible for collecting, analyzing, and reporting data that helps guide Bellabeat’s marketing strategy. The analysis will focus on:-
What are some trends in smart device usage?
How could these trends apply to Bellabeat customers?
How could these trends help influence Bellabeat's marketing strategy?


**ASK**

BUSINESS TASK
To analyze the usage of non-Bellabeat smart devices to gain insights into how the consumers use these smart devices and apply the insights to one of the Bellabeat products to help guide the marketing strategy for the company.

KEY STAKEHOLDERS
Urška Sršen: Bellabeat’s co-founder and Chief Creative Officer
Sando Mur: Mathematician and Bellabeat’s cofounder; key member of the Bellabeat executive team
Bellabeat marketing analytics team: A team of data analysts responsible for collecting, analyzing, and reporting data that helps guide Bellabeat’s marketing strategy.


**PREPARE**

DATASET: The dataset used in this analysis is Fitbit Fitness Tracker Data. The dataset is stored in the Kaggle public domain and is made available through Mobius. The data is organized in a long format.

DATA BIAS or CREDIBILITY(ROCCC): The dataset shows a sample selection bias as it does not reflect the overall population and we are not sure if the sample (30 users) is representative of the sample as a whole.  The dataset has data from two timelines; the first is from 12th March’16 to 11th April’16 and 12th April’16 to 12th May’16. Firstly the complete dataset is not current as well the data from 12th March’16 to 11th April’16 has null values and is missing important information for daily calories, intensities, and steps. So we will be using data from 12th April’16 to 12th May’16 for our analysis. Although the data is cited. To sum up, the data is original, reliable, and cited but is not comprehensive and complete.

DATA PRIVACY and INTEGRITY: This Kaggle data set contains a personal fitness tracker from thirty Fitbit users. Thirty eligible Fitbit users consented to the submission of personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring. It includes information about daily activity, steps, and heart rate that can be used to explore users’ habits. Variation between output represents the use of different types of Fitbit trackers and individual tracking behaviors/preferences.
SORT AND FILTERING DATA: Google spreadsheets are used to filter the data. In this based on observation, we got to know that dailySteps, dailyCalories, and dailyIntensities are a subset of dailyActivities.

Further, we have installed and loaded the packages in RStudio; the packages are:
>"tidyverse"
>"ggplot2"
>"lubridate"
>"dplyr"
>"stringr"
>"janitor"

Then to explore the data structure, the below CSV files were loaded
>dailyActivity_merged.csv
>dailySteps_merged.csv
>sleepDay_merged.csv
>hourlySteps_merged.csv
>hourlyCalories_merged.csv
>hourlyIntensities_merged.csv
>weightLogInfo_merged.csv
>heartrate_seconds_merged.csv

Then we checked for the sample size for the following:
>n_distinct(daily_activity$Id)
>n_distinct(sleep$Id)
>n_distinct(hourly_steps$Id)
>n_distinct(weight$Id)

Here, the output shows that the sample size of weight data is only 8 which is too small and hence cannot be included in the analysis.
>[1] 33
>[1] 24
>[1] 33
>[1] 8

Now let's check and clean the duplicates:
>sum(duplicated(daily_activity))
>sum(duplicated(hourly_steps))
>sum(duplicated(sleep))

The output for the duplicate shows, that daily_activity and hourly_steps has zero duplicates while sleep has 3.
>[1] 0
>[1] 0
>[1] 3

To remove the duplicates:
>sleep <- unique(sleep)

Now that the data is clean and we have removed all the duplicates, let’s analyze it for further observations.


**PROCESS**

I am choosing RStudio to process the data. This is so, as RStudio is efficient in handling larger datasets. It is used for statistical analysis and provides an accessible language to organize, modify and clean data frames, and also create insightful visualizations.

**STEPS TO CLEAN THE DATA:**
First, we need to clean the data. We have observed that the naming of the variables is not as per the naming conventions. So we will change the column names and ensure the same format(in this case CamelCase to all lower) for all in order to easily merge the files. To do so:
>daily_activity <- rename_with(daily_activity, tolower)
>hourly_steps <- rename_with(hourly_steps, tolower)
>sleep <- rename_with(sleep, tolower)

To verify if variable names are changed to lowercase and in the same format:
>head(daily_activity)
>head(hourly_steps)
>head(daily_sleep)

Now that the variable names are in the same format, let's convert the date from strings to make it consistent in all. For that:
>daily_activity <- daily_activity%>%
>rename(date=activitydate)%>%
>mutate(date= as_date(date, format= "%m/%d/%Y"))
>sleep <- sleep %>%
>rename(date= sleepday) %>%
>mutate(date= as_date(date, format= "%m/%d/%Y  %I:%M:%S %p", tz= Sys.timezone()))
>hourly_steps <- hourly_steps %>% 
>rename(date_time= activityhour) %>% 
>mutate(date_time= as.POSIXct(date_time, format="%m/%d/%Y %I:%M:%S %p", tz= Sys.timezone()))
>head(daily_activity)
>head(sleep)
>head(hourly_steps)

Merging data frames, Merging daily_activity and sleep to activity_sleep as sleep has fewer observations than daily_activities and applying TRUE to all missing values

>activity_sleep <- merge(daily_activity, sleep, by= c("id","date"), all.x = TRUE)
>head(activity_sleep)

Now, that the data is cleaned, I can move to the next phase of data analysis which is ANALYZE phase.


**ANALYZE**

Let’s check how many observations are there in each data frame.
>nrow(daily_activity)
>nrow(sleep)
The output will be:
>[1] 940
>[1] 413
The daily_activity has 940 rows and sleep has 413 rows.

Now let’s check some summary statistics for each data frame we want to know about.

For the daily activity dataframe:
>daily_activity %>%
>select(totalsteps, totaldistance,sedentaryminutes) %>% 
>summary()

For the sleep data frame:
>sleep_day %>%
>select(TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed) %>%
>summary()

The output for this is as below:

The summary from both daily activity and sleep data frames shows that the mean steps daily by an individual are 7638 which is less than 10000(average steps to be healthy as suggested by the health organizations). 
Each user spends more than 16.52 hours(70% of the day) of his time sedentary or ideal.
Every user sleeps 7 hours a day.
Also, there is an exponential difference in the total steps and total distance from 1st Quarter to 3rd Quarter, however sleep records changed just a little bit.


**PLOTTING A FEW EXPLORATIONS**
1. The relationship between steps taken in a day and sedentary minutes

>ggplot(data=daily_activity, aes(x=totalsteps, y=sedentaryminutes)) + geom_point() + geom_smooth()+ labs(title = " Daily steps vs sleep") + annotate("text", x=30000, y=250, label="No Correlation")

>>>As we can see, there is little to no correlation between steps taken in a day and sedentary minutes.
Although the sedentary time is higher than the user takes steps daily to stay fit.

2. Let’s plot another exploration.
Relationship between minutes asleep and time in bed

>ggplot(data=sleep, aes(x=totalminutesasleep, y=totaltimeinbed)) + geom_point() + geom_smooth() + labs(title = " Minutes Asleep vs Time in Bed") + annotate("text", x=600, y=250, label="Directly Correlated")

>>>The data viz above shows that the minutes asleep vs time in bed are directly correlated to each other. More the time in bed, more the minutes asleep.
However, this still does not show the market segment we need to tap.

3.Steps taken vs Calories burned
>ggplot(data=daily_activity, aes(x=totalsteps, y=calories)) + geom_point() +  geom_smooth()+ labs(title = " Total Steps vs Calories >Burned") +  annotate("text", x=30000, y=1000, label="Directly Correlated")

>>>I can see the direct correlation between the total steps taken and the amount of calories burned.

Now, let's add weekday_steps variable and assign days of the week 
>weekday_steps <- daily_activity %>% mutate(weekday = weekdays(date))
>weekday_steps$weekday <-ordered(weekday_steps$weekday, levels=c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", >"Sunday"))
>weekday_steps <-weekday_steps %>% group_by(weekday) %>% summarize (daily_steps = mean(totalsteps))
>head(weekday_steps)
>Steps taken by the user per day per week
>ggplot(weekday_steps, aes(weekday, daily_steps, fill = weekday)) +  geom_col() +  geom_hline(yintercept = 7500) +labs(title = "Daily >steps per day per week", x= "", y = "") + theme(axis.text.x = element_text(angle = 45,vjust = 0.5, hjust = 1))

Similar, to above, let's find out which day of the week the user sleeps the most

>weekday_sleep <- sleep %>%mutate(weekday = weekdays(date))

>weekday_sleep$weekday <-ordered(weekday_sleep$weekday, levels=c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", >"Sunday"))
>weekday_sleep <-weekday_sleep %>%group_by(weekday) %>%summarize (daily_sleep = mean(totalminutesasleep))
>head(weekday_sleep)

4. Steps taken by the user per day per week
>ggplot(weekday_sleep, aes(weekday, daily_sleep, fill = weekday)) +geom_col( ) +geom_hline(yintercept = 480) +labs(title = "Minutes >asleep per day per week", x= "", y = "") +theme(axis.text.x = element_text(angle = 45,vjust = 0.5, hjust = 1))

5. Let’s check the hourly steps per day for the users
>hourly_steps <- hourly_steps %>% separate(date_time, into= c("date","time"),sep = ' ')
>hourly_steps %>%group_by(time) %>%summarize(mean_steps = mean(steptotal)) %>% ggplot() +geom_col(mapping = aes(x=time, y = mean_steps, >fill = mean_steps)) + labs(title = "Hourly steps throughout the day", x="", y="") + theme(axis.text.x = element_text(angle = 90))

From the above visualization, we can conclude that the steps taken hourly are highest during 11:00 to 14:00 hours 17:00 to 19:00 hours in the day, which is most likely during lunch, while walking home, or going out after work.

Now let’s check the types of users based on the usage of smart devices.

Considering the data is of only 31 days, we are dividing the categories into 3 types:
> Highly Active User(more than 21 days)
> Moderately Active User (11 to 20 days)
> Least Active User (1 to 10 days)

Now let’s create a data frame by grouping ids, defining the days for each category as mentioned above
>device_usage <- activity_sleep %>%group_by(id) %>%summarize(days_used=sum(n())) %>% 
>                mutate(user_type= case_when(days_used >= 1 & days_used <= 10 ~ "least active user",
>                                            days_used >= 11 & days_used <= 20 ~ "moderately active user",
>                                            days_used >= 21 & days_used <= 31 ~ "highly active user",  ))
                                            
Convert it to percentage to better visualize the graph

>percent_usage <- device_usage %>% group_by(user_type) %>%summarise(total = n()) %>%
>mutate(totals = sum(total)) %>% group_by(user_type) %>% drop_na()%>% 
>summarise(total_percent = total / totals) %>% 
>mutate(labels = scales::percent(total_percent))

>percent_usage$user_type <- factor(percent_usage$user_type, levels=c("highly active user", "moderately active user", "least active >user"))

>head(percent_usage)

Let’s create a visualization, here the most convenient is pie chart.

>percent_usage %>% ggplot(aes(x = "",y = "labels", fill = user_type)) + 
>geom_bar(stat = "identity", width = 5)+ coord_polar("y", start=0) + theme_minimal() + 
>theme(axis.title.x= element_blank(),
>      axis.title.y = element_blank(), 
>      panel.border = element_blank(), 
>      panel.grid = element_blank(), 
>      axis.ticks = element_blank(),
>      axis.text.x = element_blank(),
>      plot.title = element_text(hjust = 0.5, size=14, face = "bold")) + 
>geom_text(aes(label = labels),position = position_stack(vjust = 0.5))+ 
>scale_fill_manual(values = c( "#4CB140","#EC7A08","#C9190B", "#000000"),
>                          labels = c("Highly Active User - 21 to 31 days", 
>                                      "Moderately Active User - 11 to 20 days",
>                                      "Least Active User- 1 to 10 days", "No Usage")) + 
>                          labs(title="Daily use of smart device")

Here, we can see that 78.8% of users are highly active users, 9% use the devices moderately and 3% are the least active users.


**SHARE**

KEY FINDINGS

Below are some key findings and observations based on the analysis and visualizations
There is a positive correlation between time in bed and time of sleep. 
There is no correlation between time in bed and steps taken, although the sedentary minutes are too high. This means users can reduce their sedentary minutes and a scope of increasing the steps taken per day.
Between the steps taken and calories burned, we can see a positive correlation, which can help motivate the users to take more steps to stay healthy.
Another observation is that the users are more active at the start and the end of the week, but a little less active in the midweek.
Also, the sleep time is more on weekends, mostly due to week offs in general for everyone.
One more observation is that the steps taken in a day are majorly from 11:00 to 14:00 hrs and 17:00 to 19:00 hrs. This opens up a scope for taking more steps, otherwise lesser than the standard set by CCD of 10000 steps/ day.
Among the active user, 78% use their devices all the time, while 9% use moderately and 3% use the least.


**ACT**
Bellabeat is a high-tech company that manufactures health-focused smart products. Sršen used her background as an artist to develop beautifully designed technology that informs and inspires women around the world. Collecting data on activity, sleep, stress, and reproductive health has allowed Bellabeat to empower women with knowledge about their health and habits.
Based on the mission and vision of the company, it should focus on the below objectives to improve the marketing strategies for future endeavors.
Fitbit does not provide any information or data on women's reproductive health. So the company can focus on marketing the product for overall health and habits and not just a fitness tracker.
With observations, we can see that the users are spending more sedentary time, so through marketing the company must encourage them to take more steps per day.
Bellabeat must run campaigns for the awareness of better health, on how many steps per day should be taken, and the benefits of it.
Bellabeat should encourage the users to utilize the time after 19:00 hrs for activities related to health.
Also, women are multitaskers, so Bellabeat must design a multifunctional device in such a way, that includes tracking with apps for music as it helps you focus more on activities, and emergency services(SOS) which ensures the safety of older women.
The company can also organize workshops or seminars on women's health, specifically on reproductive health for the exclusive users with membership to encourage more membership enrollments.
Also, the Bellabeat app must provide a personalized setting of goals, reward systems, and customized physical activities under the guidance of a professional.
