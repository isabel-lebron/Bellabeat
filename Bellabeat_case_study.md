Bellabeat - Google Data Analytics Case Study
================
Isabel Lebron
12/20/2021

<img src="bellabeat-logo.png"/> 

## Introduction to Bellabeat:

Bellabeat is a high-tech manufacturer of health-focused products for
women. Founders, Urška Sršen and Sando Mur, developed technology for
women to track their activity, sleep, stress, as well as reproductive
health, and designed this technology in a new and beautiful way. With
the goal to inspire women around the world, and empower them with
knowledge regarding their own health and habits, Bellabeat created a
product line including a variety of wellness products. These smart
wellness products include a Bellabeat Leaf wellness tracker, a
smartwatch, and a smart water bottle, all of which connect to the
Bellabeat app, which provides access to the user’s health data.

## Scenario:

With Bellabeat growing rapidly since its debut in 2013, they have the
potential to grow even further and become a larger part of the global
smart device market. With that being said, Urška Sršen, the cofounder
and Chief Creative Officer of Bellabeat believes that analyzing smart
device fitness data from other fitness-related devices, like FitBit,
could help foster new growth opportunities and ideas for the company. I
have been asked to focus on one of Bellabeat’s products and analyze
smart device data to gain insight into how consumers are using their
smart devices, which I will then use to help guide a new and improved
marketing strategy for the company.

<img src="bellabeat-time.png"/> 

### Step 1: Ask

#### Business Task:

As an aspiring junior data analyst, I have been tasked with providing
primary and secondary stakeholders with insightful recommendations for
one of Bellabeat’s wellness products. I will be analyzing smart device
user data in order to gain insight into how consumers are using their
non-Bellabeat smart devices, and then apply these insights to comparable
Bellabeat products such as the Bellabeat Time.  

#### Key Stakeholders:

**Primary stakeholders:**  

  - **Urška Sršen:** Bellabeat’s co-founder and Chief Creative
    Officer.  
  - **Sando Mur:** Mathematician and Bellabeat’s co-founder; a key
    member of the Bellabeat executive team.  

**Secondary Stakeholders:**  

  - **Bellabeat marketing analytics team:** The team I am “a part of”
    which includes data analysts responsible for collecting, analyzing,
    and reporting data that helps guide Bellabeat’s marketing
    strategy.  

#### Questions for the analysis:

1.  What are some trends in smart device usage?
2.  How could these trends apply to Bellabeat customers?
3.  How could these trends help influence Bellabeat marketing
    strategy?  

### Step 2: Prepare

#### About the data:

The dataset for this case study, FitBit Fitness Tracker Data, comes from
Kaggle and contains 18 .csv datasets in both long and wide formats. Each
dataset consists of in-depth metrics regarding users’ heart rate, sleep
monitoring, calories burned, and many more. The datasets were generated
by respondents to a survey via Amazon Mechanical Turk, distributed
between April 12th, 2016, and May 12th, 2016. Thirty eligible Fitbit
users consented to the submission of personal tracker data, including
minute-level output for physical activity, heart rate, and sleep
monitoring.

My analysis will be focusing on one product from Bellabeat’s product
line, Bellabeat Time. In addition to telling time, this product tracks
its users’ activity and sleep patterns and connects that information to
the Bellabeat app, which then shares insights with its owner. For my
analysis, I will be focusing on the following datasets:  

  - dailyActivity\_merged.csv, renamed DailyActivity.csv  
  - sleepDay\_merged.csv, renamed SleepDay.csv  
  - weightLogInfo\_merged.csv, renamed WeightLog.csv  

#### Limitations of the data

  - It is very likely that the 33 survey participants aren’t an actual
    representation off all FitBit users.
  - The data is old, from 2016, and it is possible trends of using
    wearable smart fitness devices have changed. Additionally, it is
    likely both FitBit and Bellabeat have released smart devices since
    2016.  

#### Data import / exploration

For my analysis, I will be using R Studio because of the many packages
and data visualization features available to clean and analyze the
data.  

##### Loading packages

``` r
library(tidyverse)
library(janitor)
library(dplyr)
library(lubridate)
library(ggplot2)
library(forcats)
```

  

##### Importing CSV files

Below we will be creating the data frames to be used for analysis.
First, we will make sure to set our working directory to where our CSV
files are located, then use the read\_csv() function to import them. We
will also be following typical naming conventions based on the names of
the CSV files.

``` r
DailyActivity <- read_csv("DailyActivity.csv")
SleepDay <- read_csv("SleepDay.csv")
WeightLog <- read_csv("WeightLog.csv")
```

  

#### Exploring our data

Below we will take a sneak peek at our data frames by using the str()
and colnames() functions. The head() function is used to get the first
few rows in the data frame, while colnames() retrieves the column names
in the data frame.

##### Daily Activity dataset

``` r
head(DailyActivity)
```

    ## # A tibble: 6 × 15
    ##        Id ActivityDate TotalSteps TotalDistance TrackerDistance LoggedActivitie…
    ##     <dbl> <chr>             <dbl>         <dbl>           <dbl>            <dbl>
    ## 1  1.50e9 4/12/2016         13162          8.5             8.5                 0
    ## 2  1.50e9 4/13/2016         10735          6.97            6.97                0
    ## 3  1.50e9 4/14/2016         10460          6.74            6.74                0
    ## 4  1.50e9 4/15/2016          9762          6.28            6.28                0
    ## 5  1.50e9 4/16/2016         12669          8.16            8.16                0
    ## 6  1.50e9 4/17/2016          9705          6.48            6.48                0
    ## # … with 9 more variables: VeryActiveDistance <dbl>,
    ## #   ModeratelyActiveDistance <dbl>, LightActiveDistance <dbl>,
    ## #   SedentaryActiveDistance <dbl>, VeryActiveMinutes <dbl>,
    ## #   FairlyActiveMinutes <dbl>, LightlyActiveMinutes <dbl>,
    ## #   SedentaryMinutes <dbl>, Calories <dbl>

``` r
colnames(DailyActivity)
```

    ##  [1] "Id"                       "ActivityDate"            
    ##  [3] "TotalSteps"               "TotalDistance"           
    ##  [5] "TrackerDistance"          "LoggedActivitiesDistance"
    ##  [7] "VeryActiveDistance"       "ModeratelyActiveDistance"
    ##  [9] "LightActiveDistance"      "SedentaryActiveDistance" 
    ## [11] "VeryActiveMinutes"        "FairlyActiveMinutes"     
    ## [13] "LightlyActiveMinutes"     "SedentaryMinutes"        
    ## [15] "Calories"

##### Sleep dataset

``` r
head(SleepDay)
```

    ## # A tibble: 6 × 5
    ##           Id SleepDay          TotalSleepRecor… TotalMinutesAsle… TotalTimeInBed
    ##        <dbl> <chr>                        <dbl>             <dbl>          <dbl>
    ## 1 1503960366 4/12/2016 12:00:…                1               327            346
    ## 2 1503960366 4/13/2016 12:00:…                2               384            407
    ## 3 1503960366 4/15/2016 12:00:…                1               412            442
    ## 4 1503960366 4/16/2016 12:00:…                2               340            367
    ## 5 1503960366 4/17/2016 12:00:…                1               700            712
    ## 6 1503960366 4/19/2016 12:00:…                1               304            320

``` r
colnames(SleepDay)
```

    ## [1] "Id"                 "SleepDay"           "TotalSleepRecords" 
    ## [4] "TotalMinutesAsleep" "TotalTimeInBed"

##### Weight Log dataset

``` r
head(WeightLog)
```

    ## # A tibble: 6 × 8
    ##           Id Date      WeightKg WeightPounds   Fat   BMI IsManualReport    LogId
    ##        <dbl> <chr>        <dbl>        <dbl> <dbl> <dbl> <lgl>             <dbl>
    ## 1 1503960366 5/2/2016…     52.6         116.    22  22.6 TRUE            1.46e12
    ## 2 1503960366 5/3/2016…     52.6         116.    NA  22.6 TRUE            1.46e12
    ## 3 1927972279 4/13/201…    134.          294.    NA  47.5 FALSE           1.46e12
    ## 4 2873212765 4/21/201…     56.7         125.    NA  21.5 TRUE            1.46e12
    ## 5 2873212765 5/12/201…     57.3         126.    NA  21.7 TRUE            1.46e12
    ## 6 4319703577 4/17/201…     72.4         160.    25  27.5 TRUE            1.46e12

``` r
colnames(WeightLog)
```

    ## [1] "Id"             "Date"           "WeightKg"       "WeightPounds"  
    ## [5] "Fat"            "BMI"            "IsManualReport" "LogId"

### Step 3: Process

During this phase, we will be cleaning our data by removing any
duplicates or missing values from each dataset. Additionally, we will
transform data into the right type if it is incorrectly formatted.

#### Cleaning up:

##### Let’s start with the DailyActivity dataset

``` r
# Are there duplicates?
sum(duplicated(DailyActivity))
```

    ## [1] 0

``` r
# Are there nulls?
sum(is.na(DailyActivity))
```

    ## [1] 0

``` r
# Change name from ActivityDate to Date
names(DailyActivity)[names(DailyActivity) == "ActivityDate"] <- "Date"
# Change character format to date type
DailyActivity$Date <- as.Date(DailyActivity$Date, format = '%m/%d/%Y')
# Add day of the week column
DailyActivity$DayOfWeek <- wday(DailyActivity$Date, label = TRUE)
# Add total active minutes column
DailyActivity2 <- DailyActivity %>% 
  mutate(TotalActiveMinutes = VeryActiveMinutes + FairlyActiveMinutes + LightlyActiveMinutes)
# Check dataset
head(DailyActivity2)
```

    ## # A tibble: 6 × 17
    ##        Id Date       TotalSteps TotalDistance TrackerDistance LoggedActivitiesD…
    ##     <dbl> <date>          <dbl>         <dbl>           <dbl>              <dbl>
    ## 1  1.50e9 2016-04-12      13162          8.5             8.5                   0
    ## 2  1.50e9 2016-04-13      10735          6.97            6.97                  0
    ## 3  1.50e9 2016-04-14      10460          6.74            6.74                  0
    ## 4  1.50e9 2016-04-15       9762          6.28            6.28                  0
    ## 5  1.50e9 2016-04-16      12669          8.16            8.16                  0
    ## 6  1.50e9 2016-04-17       9705          6.48            6.48                  0
    ## # … with 11 more variables: VeryActiveDistance <dbl>,
    ## #   ModeratelyActiveDistance <dbl>, LightActiveDistance <dbl>,
    ## #   SedentaryActiveDistance <dbl>, VeryActiveMinutes <dbl>,
    ## #   FairlyActiveMinutes <dbl>, LightlyActiveMinutes <dbl>,
    ## #   SedentaryMinutes <dbl>, Calories <dbl>, DayOfWeek <ord>,
    ## #   TotalActiveMinutes <dbl>

``` r
colnames(DailyActivity2)
```

    ##  [1] "Id"                       "Date"                    
    ##  [3] "TotalSteps"               "TotalDistance"           
    ##  [5] "TrackerDistance"          "LoggedActivitiesDistance"
    ##  [7] "VeryActiveDistance"       "ModeratelyActiveDistance"
    ##  [9] "LightActiveDistance"      "SedentaryActiveDistance" 
    ## [11] "VeryActiveMinutes"        "FairlyActiveMinutes"     
    ## [13] "LightlyActiveMinutes"     "SedentaryMinutes"        
    ## [15] "Calories"                 "DayOfWeek"               
    ## [17] "TotalActiveMinutes"

  

##### Next, let’s clean the SleepDay dataset

``` r
# Are there duplicates?
sum(duplicated(SleepDay))
```

    ## [1] 3

``` r
# Remove duplicates
SleepDay2<-distinct(SleepDay)
# Make sure duplicates were removed
sum(duplicated(SleepDay2))
```

    ## [1] 0

``` r
# Are there nulls?
sum(is.na(SleepDay2))
```

    ## [1] 0

``` r
# Change name from SleepDay to Date
names(SleepDay2)[names(SleepDay2) == "SleepDay"] <- "Date"
# Change character format to date type
SleepDay2$Date <- as.Date(SleepDay2$Date, format = '%m/%d/%Y')
# Add day of the week column
SleepDay2$DayOfWeek <- wday(SleepDay2$Date, label = TRUE)
# Check dataset
head(SleepDay2)
```

    ## # A tibble: 6 × 6
    ##         Id Date       TotalSleepRecor… TotalMinutesAsl… TotalTimeInBed DayOfWeek
    ##      <dbl> <date>                <dbl>            <dbl>          <dbl> <ord>    
    ## 1   1.50e9 2016-04-12                1              327            346 Tue      
    ## 2   1.50e9 2016-04-13                2              384            407 Wed      
    ## 3   1.50e9 2016-04-15                1              412            442 Fri      
    ## 4   1.50e9 2016-04-16                2              340            367 Sat      
    ## 5   1.50e9 2016-04-17                1              700            712 Sun      
    ## 6   1.50e9 2016-04-19                1              304            320 Tue

``` r
colnames(SleepDay2)
```

    ## [1] "Id"                 "Date"               "TotalSleepRecords" 
    ## [4] "TotalMinutesAsleep" "TotalTimeInBed"     "DayOfWeek"

  

##### Next, let’s clean the WeightLog dataset

``` r
# Are there duplicates?
sum(duplicated(WeightLog))
```

    ## [1] 0

``` r
# Are there nulls?
sum(is.na(WeightLog$WeightPounds))
```

    ## [1] 0

``` r
# Separate date and time
WeightLog2 <- WeightLog %>% 
  separate(Date, into=c("Date","Time"), sep=" ",remove =FALSE )
```

    ## Warning: Expected 2 pieces. Additional pieces discarded in 67 rows [1, 2, 3, 4,
    ## 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, ...].

``` r
# Change character format to date type
WeightLog2$Date <- as.Date(WeightLog2$Date, format = '%m/%d/%Y')
# Add day of the week column
WeightLog2$DayOfWeek <- wday(WeightLog2$Date, label = TRUE)
# Check dataset
head(WeightLog2)
```

    ## # A tibble: 6 × 10
    ##           Id WeightKg Date       Time    WeightPounds   Fat   BMI IsManualReport
    ##        <dbl>    <dbl> <date>     <chr>          <dbl> <dbl> <dbl> <lgl>         
    ## 1 1503960366     52.6 2016-05-02 11:59:…         116.    22  22.6 TRUE          
    ## 2 1503960366     52.6 2016-05-03 11:59:…         116.    NA  22.6 TRUE          
    ## 3 1927972279    134.  2016-04-13 1:08:52         294.    NA  47.5 FALSE         
    ## 4 2873212765     56.7 2016-04-21 11:59:…         125.    NA  21.5 TRUE          
    ## 5 2873212765     57.3 2016-05-12 11:59:…         126.    NA  21.7 TRUE          
    ## 6 4319703577     72.4 2016-04-17 11:59:…         160.    25  27.5 TRUE          
    ## # … with 2 more variables: LogId <dbl>, DayOfWeek <ord>

``` r
colnames(WeightLog2)
```

    ##  [1] "Id"             "WeightKg"       "Date"           "Time"          
    ##  [5] "WeightPounds"   "Fat"            "BMI"            "IsManualReport"
    ##  [9] "LogId"          "DayOfWeek"

  

Now that we have removed any duplicates and missing values in the data,
and correctly formatted the necessary data types, we can combine our
three data frames into one big data frame and then move on to analyzing
our data.  
  

##### Finally, merging our data

``` r
Final <- merge(merge(DailyActivity2, SleepDay2, by = c('Id', 'Date'), all = TRUE), WeightLog2, by = c('Id', 'Date'), all = TRUE)
head(Final)
```

    ##           Id       Date TotalSteps TotalDistance TrackerDistance
    ## 1 1503960366 2016-04-12      13162          8.50            8.50
    ## 2 1503960366 2016-04-13      10735          6.97            6.97
    ## 3 1503960366 2016-04-14      10460          6.74            6.74
    ## 4 1503960366 2016-04-15       9762          6.28            6.28
    ## 5 1503960366 2016-04-16      12669          8.16            8.16
    ## 6 1503960366 2016-04-17       9705          6.48            6.48
    ##   LoggedActivitiesDistance VeryActiveDistance ModeratelyActiveDistance
    ## 1                        0               1.88                     0.55
    ## 2                        0               1.57                     0.69
    ## 3                        0               2.44                     0.40
    ## 4                        0               2.14                     1.26
    ## 5                        0               2.71                     0.41
    ## 6                        0               3.19                     0.78
    ##   LightActiveDistance SedentaryActiveDistance VeryActiveMinutes
    ## 1                6.06                       0                25
    ## 2                4.71                       0                21
    ## 3                3.91                       0                30
    ## 4                2.83                       0                29
    ## 5                5.04                       0                36
    ## 6                2.51                       0                38
    ##   FairlyActiveMinutes LightlyActiveMinutes SedentaryMinutes Calories
    ## 1                  13                  328              728     1985
    ## 2                  19                  217              776     1797
    ## 3                  11                  181             1218     1776
    ## 4                  34                  209              726     1745
    ## 5                  10                  221              773     1863
    ## 6                  20                  164              539     1728
    ##   DayOfWeek.x TotalActiveMinutes TotalSleepRecords TotalMinutesAsleep
    ## 1         Tue                366                 1                327
    ## 2         Wed                257                 2                384
    ## 3         Thu                222                NA                 NA
    ## 4         Fri                272                 1                412
    ## 5         Sat                267                 2                340
    ## 6         Sun                222                 1                700
    ##   TotalTimeInBed DayOfWeek.y WeightKg Time WeightPounds Fat BMI IsManualReport
    ## 1            346         Tue       NA <NA>           NA  NA  NA             NA
    ## 2            407         Wed       NA <NA>           NA  NA  NA             NA
    ## 3             NA        <NA>       NA <NA>           NA  NA  NA             NA
    ## 4            442         Fri       NA <NA>           NA  NA  NA             NA
    ## 5            367         Sat       NA <NA>           NA  NA  NA             NA
    ## 6            712         Sun       NA <NA>           NA  NA  NA             NA
    ##   LogId DayOfWeek
    ## 1    NA      <NA>
    ## 2    NA      <NA>
    ## 3    NA      <NA>
    ## 4    NA      <NA>
    ## 5    NA      <NA>
    ## 6    NA      <NA>

  

### Step 4: Analyze

##### Let’s first take a look at the number of participants in each dataset

``` r
n_distinct(DailyActivity$Id)
```

    ## [1] 33

``` r
n_distinct(SleepDay$Id)
```

    ## [1] 24

``` r
n_distinct(WeightLog$Id)
```

    ## [1] 8

Using the n\_distinct() function we are able to determine the number of
unique values in the 3 datasets. Compared to the daily activity and
asleep data frames, the weight log data frame consists of a very low
number of participants. We will explore this more later on.  
  

##### Now let’s look at important statistics for each dataset

Using the summary() function, we are able to pull these important
statistics for each of the datasets.  
  

**Merged data frame**

``` r
Final %>% 
  select(TotalSteps, Calories,TotalActiveMinutes,TotalMinutesAsleep, BMI, IsManualReport ) %>% 
summary(Final)
```

    ##    TotalSteps       Calories    TotalActiveMinutes TotalMinutesAsleep
    ##  Min.   :    0   Min.   :   0   Min.   :  0.0      Min.   : 58.0     
    ##  1st Qu.: 3790   1st Qu.:1828   1st Qu.:146.8      1st Qu.:361.0     
    ##  Median : 7406   Median :2134   Median :247.0      Median :432.5     
    ##  Mean   : 7638   Mean   :2304   Mean   :227.5      Mean   :419.2     
    ##  3rd Qu.:10727   3rd Qu.:2793   3rd Qu.:317.2      3rd Qu.:490.0     
    ##  Max.   :36019   Max.   :4900   Max.   :552.0      Max.   :796.0     
    ##                                                    NA's   :530       
    ##       BMI        IsManualReport 
    ##  Min.   :21.45   Mode :logical  
    ##  1st Qu.:23.96   FALSE:26       
    ##  Median :24.39   TRUE :41       
    ##  Mean   :25.19   NA's :873      
    ##  3rd Qu.:25.56                  
    ##  Max.   :47.54                  
    ##  NA's   :873

**Notes**: Taking a look at some of these statistics we can gain some
insight about the FitBit users. According to NBC “The fitness goal of
10,000 steps a day is widely promoted, but a new study suggests that
logging even 7,000 daily steps may go a long way toward better health.”
From our summary statistics, we can see that on average, FitBit users
were reaching 7,638 steps a day, which seems like a good amount
according to NBC. We can take a look at more of these summary statistics
below.  
  

**DailyActivity data frame**

``` r
DailyActivity2 %>% 
  select(TotalSteps, TotalDistance, LightlyActiveMinutes, FairlyActiveMinutes, VeryActiveMinutes, SedentaryMinutes, Calories) %>% 
  summary()
```

    ##    TotalSteps    TotalDistance    LightlyActiveMinutes FairlyActiveMinutes
    ##  Min.   :    0   Min.   : 0.000   Min.   :  0.0        Min.   :  0.00     
    ##  1st Qu.: 3790   1st Qu.: 2.620   1st Qu.:127.0        1st Qu.:  0.00     
    ##  Median : 7406   Median : 5.245   Median :199.0        Median :  6.00     
    ##  Mean   : 7638   Mean   : 5.490   Mean   :192.8        Mean   : 13.56     
    ##  3rd Qu.:10727   3rd Qu.: 7.713   3rd Qu.:264.0        3rd Qu.: 19.00     
    ##  Max.   :36019   Max.   :28.030   Max.   :518.0        Max.   :143.00     
    ##  VeryActiveMinutes SedentaryMinutes    Calories   
    ##  Min.   :  0.00    Min.   :   0.0   Min.   :   0  
    ##  1st Qu.:  0.00    1st Qu.: 729.8   1st Qu.:1828  
    ##  Median :  4.00    Median :1057.5   Median :2134  
    ##  Mean   : 21.16    Mean   : 991.2   Mean   :2304  
    ##  3rd Qu.: 32.00    3rd Qu.:1229.5   3rd Qu.:2793  
    ##  Max.   :210.00    Max.   :1440.0   Max.   :4900

**Notes:** As previously stated, the average amount of steps per day for
FitBit users is 7,638. While the 10,000 steps per day number have been
widely promoted over the years, studies from the National Institute of
Health have found that some people may benefit from starting with goals
of fewer than the common standard of 10,000 steps a day. In fact, death
rates declined with more steps taken each day until about 7,500 steps a
day, when the benefit leveled off. Therefore, with the average steps per
day being 7,638 for FitBit users, it seems as though they are on the
right track\! So it is important not to discourage users if they are
taking less than 10,000 steps per day. Additionally, FitBit users are
averaging 991.2 minutes of sedentary time a day, this is roughly 16.5
hours a day\! A new study suggests about 30-40 minutes per day of
building up a sweat should help to counteract the negative health impact
of sitting all day long. However, the 30-40 minutes is in regards to
sitting at a desk for 10 hours, and on average the FitBit user’s
sedentary time was 16.5 hours, so it is likely more than 40 minutes of
working up a sweat is going to be needed to counteract that time
sitting.  
  

**SleepDay data frame**

``` r
SleepDay2 %>% 
  select(TotalSleepRecords,TotalMinutesAsleep,TotalTimeInBed) %>% 
  summary()
```

    ##  TotalSleepRecords TotalMinutesAsleep TotalTimeInBed 
    ##  Min.   :1.00      Min.   : 58.0      Min.   : 61.0  
    ##  1st Qu.:1.00      1st Qu.:361.0      1st Qu.:403.8  
    ##  Median :1.00      Median :432.5      Median :463.0  
    ##  Mean   :1.12      Mean   :419.2      Mean   :458.5  
    ##  3rd Qu.:1.00      3rd Qu.:490.0      3rd Qu.:526.0  
    ##  Max.   :3.00      Max.   :796.0      Max.   :961.0

**Notes:** Taking a look at the summary of the sleep data frame, we can
see that on average, a FitBit user sleeps for 419.2 minutes which is
roughly 7 hours. This amount falls within the recommendations from the
CDC for how much sleep adults should get. However, taking a look at the
mean for total time in bed, we can see on average FitBit users spend
458.5 minutes in bed. This means that on average, participants are
spending 39.3 minutes awake in bed. According to an article from Health
Central, individuals should not spend any more than an hour awake in
bed. By avoiding this, you are helping to prevent a mental link being
formed between being in bed and being awake, which can essentially lead
to insomnia or other sleep issues.  
  

**WeightLog data frame**

``` r
WeightLog2 %>% 
  select(WeightPounds, BMI, IsManualReport) %>% 
  summary()
```

    ##   WeightPounds        BMI        IsManualReport 
    ##  Min.   :116.0   Min.   :21.45   Mode :logical  
    ##  1st Qu.:135.4   1st Qu.:23.96   FALSE:26       
    ##  Median :137.8   Median :24.39   TRUE :41       
    ##  Mean   :158.8   Mean   :25.19                  
    ##  3rd Qu.:187.5   3rd Qu.:25.56                  
    ##  Max.   :294.3   Max.   :47.54

**Notes:** Taking a look at the summary statistics from the weight log
dataset, the average BMI is 25.19. According to the CDC, a BMI above 25
is considered overweight, however, with very few participants logging
their weight, we are unable to make any conclusions regarding BMI. With
that being said, taking a look at the “IsManualReport” statistics, there
are twice the amount of manually reported entries in the WeightLog table
as non-manual entries. It could be beneficial to discover how dedicated
FitBit users are when it comes to logging their weight, so we will dive
deeper into this a little later.  
  

#### Analysis questions and answers:

##### Daily Activity

**1. What are some trends in smart device usage?**  
Taking a look at daily activity data, we can see that, on average,
FitBit users are reaching 7,638 steps a day. We can also see that, on
average, participants spend 227.5 minutes each day being active. These
total active minutes include a sum of the amount of time the user spends
participating in each level of activity: lightly active, moderately
active, and very active. On the other hand, FitBit users are also
averaging 991.2 minutes of sedentary time a day, which is roughly 16.5
hours\!  

**2. How could these trends apply to Bellabeat customers?**  
I believe these trends could help make sure Bellabeat users get enough
active time each day to counteract all the time they have spent
sedentary.  

**3. How could these trends help influence Bellabeat marketing
strategy?**  
As previously discussed, although 10,000 steps a day has been widely
promoted over the years by many fitness companies, studies show that
even 7,500 daily steps could lead towards better health. Focussing on
not discouraging Bellabeat’s customers, and potentially scaring them
with big numbers, Bellabeat can help make users feel more comfortable
and notify users throughout the day to get up and walk around and help
them reach their goals. In addition to that, with the amount of time
users spend sitting, I believe it could be beneficial for Bellabeat to
recognize when a user has been sedentary for a while and encourage them
to get up, even if it is for just a few minutes\!  
  

##### Sleep

**1. What are some trends in smart device usage?**  
In this particular study, we have sleep data for 24 of the 33
participants, meaning roughly 73 percent of the users wore their smart
devices to sleep. In addition, from the data, we can see that, on
average, the majority of the users got the CDC-recommended 7 hours of
sleep. In addition to hours asleep, taking a look at the time spent in
bed versus the time users were asleep, we see that participants are seen
spending, on average, 39.3 minutes awake in bed.  

**2. How could these trends apply to Bellabeat customers?**  
I believe these trends could help Bellabeat make sure their users are
getting the necessary amount of sleep, as well as helping to avoid
spending too much time in bed awake.  

**3. How could these trends help influence Bellabeat marketing
strategy?**  
While this is a relatively small study, and likely doesn’t accurately
represent the whole population of FitBit users, I think it is a safe
assumption to say not all users wear their device to sleep, most likely
due to comfortability. Bellabeat can focus on making their smart devices
not only comfortable to wear day-to-day, but also comfortable to wear
while asleep, and marketing that to potential customers.  
  

##### Weight Log

**1. What are some trends in smart device usage?**  
When it comes to smart device usage and users logging their weight, it
seems like this area needs some improvements. Just looking at overall
participants in the study, 8 of them used the weight log feature, that
is only 24 percent\! Additionally, of those 8 participants, 5 of them
manually reported their weights, and 3 had their weight automatically
reported using a smart scale.  

**2. How could these trends apply to Bellabeat customers?**  
I believe these trends show that Bellabeat customers should feel
comfortable reporting their weights and feeling empowered in doing so.
Creating a body-positive environment for Bellabeat customers is
extremely important\!  

**3. How could these trends help influence Bellabeat marketing
strategy?**  
As previously mentioned, creating a body-positive environment for
Bellabeat users could aid in the lack of users logging their weight.
Creating this environment in advertisements, on the website, and within
the app, can help with empowering our users to learn more about their
health. In addition, I believe using smart scales could be a great way
to help with the task of users manually reporting their weight.
Marketing smart scales and incorporating easy connectivity to the app
could lead to great success in increasing the number of users logging
their weight\!  

### Step 5: Share

I used the ggplot() function in order to create data visualizations that
show patterns and trends that I found to be important in the data
frames.  
  

#### Activity Visuals

**Let’s take a look at the relationship between the different types of
active minutes and total steps versus calories burned.**

``` r
# Relationship between very active minutes and calories burned
ggplot(data = DailyActivity2, aes(x=VeryActiveMinutes, y=Calories, color = Calories)) + 
  geom_point() +
  stat_smooth(method = lm) +
  labs(title = 'Very Active Minutes vs. Calories Burned', 
       x = "Very Active Minutes",y = "Calories Burned" )

# Relationship between fairly active minutes and calories burned
ggplot(data = DailyActivity2, aes(x=FairlyActiveMinutes, y=Calories, color = Calories)) + 
  geom_point() +
  stat_smooth(method = lm) +
  labs(title = 'Fairly Active Minutes vs. Calories Burned', 
       x = "Fairly Active Minutes",y = "Calories Burned" ) 

# Relationship between lightly active minutes and calories burned
ggplot(data = DailyActivity2, aes(x=LightlyActiveMinutes, y=Calories, color = Calories)) + 
  geom_point() +
  stat_smooth(method = lm) +
  labs(title = 'Lightly Active Minutes vs. Calories Burned', 
       x = "Lightly Active Minutes",y = "Calories Burned" )

# Relationship between total steps and calories burned
ggplot(data = DailyActivity2, aes(x=TotalSteps, y=Calories, color = Calories)) + 
  geom_point() +
  stat_smooth(method = lm) +
  labs(title = 'CTotal Steps vs. Calories Burned', 
       x = "Total Steps", y = "Calories Burned")
```

<img src="figures-side-1.png" width="50%" /><img src="figures-side-2.png" width="50%" /><img src="figures-side-3.png" width="50%" /><img src="figures-side-4.png" width="50%" /> 
 

**Notes:** From the visuals above, we can see that the strongest
positive relationship occurs between very active minutes and total daily
calories burned. While this may be expected, it is still important to
see if our data is being accurately represented. Taking a look at the
relationship between fairly active minutes and calories burned, you can
still see a positive relationship, however, it is not as strong as the
first. With fairly active physical activity the participant still can
burn a good amount of calories. In addition, the relationship between
lightly active minutes and calories burned is significantly less
positive than very active and fairly active when it comes to calories
burned. This goes to show that the more vigorous physical activity the
participant did, the more calories they burned. So if the goal is to
lose weight and burn calories, it is important to incorporate more
vigorous physical activity into one’s day.

Finally, taking a look at the relationship between total steps and
calories burned, we see a relationship that one could expect. We clearly
see a positive correlation depicting the more steps taken, the more
calories are burned. So whether or not the goal is to lose weight,
increasing the amount of steps taken each day can help to stay active
and burn some calories\!  
  

#### Sleep Visuals

**First, I plotted the relationship between total minutes asleep and
total time in bed**

``` r
ggplot(data = SleepDay2, aes(x = TotalMinutesAsleep, y = TotalTimeInBed)) +
  geom_point() +
  stat_smooth(method = lm) +
  labs(title = "Total Minutes Asleep vs. Total Time In Bed")
```
<img src="unnamed-chunk-15-1.png"/> 

**Notes:** In the above visual, a positive relationship between total
minutes asleep and total time in bed is seen. For the most part, the
time users spent asleep and the time they spent in bed was very similar.
This means that users are not spending too much time awake in bed which
is important for their sleep health.  
  

**Next, I am going to plot the sleep distribution of the FitBit users to
see whether or not they get enough sleep according to CDC standards.**

``` r
SleepDay2 %>% 
  select(TotalMinutesAsleep) %>% 
  mutate(HoursAsleep = ifelse(TotalMinutesAsleep <= 420, 'Less than 7h',
                      ifelse(TotalMinutesAsleep <= 540, '7h to 9h',
                             'More than 9h'))) %>% 
  mutate(HoursAsleep = factor(HoursAsleep,
                              levels = c('Less than 7h', '7h to 9h','More than 9h'))) %>% 
  ggplot(aes(x = TotalMinutesAsleep, fill = HoursAsleep)) +
  geom_histogram(position = 'dodge', bins = 25) +
  theme_gray() +
   labs(title = "Sleep distribution",
       x = "Time asleep (minutes)",
       y = "Count")
```

<img src="unnamed-chunk-16-1.png"/> 
**Notes:** To get an idea about FitBit’s users’ sleep patterns, I
decided to plot their sleep distribution to see if whether or not they
get enough sleep according to CDC guidelines. By taking a look at the
visual, we can see that participants follow healthy sleep patterns,
getting between 6-9 hours of sleep per day, which is up to CDC’s
standards for getting enough rest.  
  

#### Weight Log Visuals

**Plotting Auto vs Manual reports of the weight data**

``` r
ReportType <- c("Manual Report", "Auto Report")
ReportTypeTF <- c(length(which(WeightLog2$IsManualReport == "TRUE")), length(which(WeightLog2$IsManualReport == "FALSE")))
ReportDF <- data.frame(ReportType, ReportTypeTF)

ggplot(data = ReportDF, aes(ReportType, ReportTypeTF)) + 
  geom_bar(stat = "identity", fill = "steelblue") +
  labs(title = "Types of Weight Log Methods", x = "Weight Log Methods", y = "Number of Records")
```

<img src="unnamed-chunk-17-1.png"/> 
**Notes:** Earlier when looking at summary statistics for the Weight Log
dataset, I decided it may be beneficial to see how dedicated FitBit
users are when it comes to logging their weight. Although there are only
8 participants in the Weight Log dataset, over half of the reports are
manual, 61.2 percent to be exact. This may mean that users need more
encouragement to report their weight, as they may not feel comfortable
in doing so.  
  

#### Calculating and plotting daily activity and sleep Statistics for each day of the week  

``` r
# Average Active Minutes per day of week 
# Make sure we exclude zeros when calculating mean
DailyActivity2$TotalActiveMinutes[DailyActivity2$TotalActiveMinutes == 0] <- NA
# Split up the data frame for total active minutes by day
SplitMins <- split(DailyActivity2$TotalActiveMinutes, DailyActivity2$DayOfWeek)
# Find the average amount of active minutes for each day and round values
AvgMins <- sapply(SplitMins, mean, na.rm = TRUE)
AvgMins <- round(AvgMins, digits = 2)
# Create data frame
Day <- c("Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat")
MinsDF <- data.frame(as.factor(Day), AvgMins)
# Plot
ggplot(data = MinsDF, aes(x = fct_inorder(Day), y = AvgMins)) + 
  geom_bar(position= "dodge", stat = "identity", fill = "steelblue") +
  labs(title = "Average Active Minutes per Day of the Week", x = "Day of the Week", y = "Active Minutes") +
  geom_text(aes(label=AvgMins), vjust=1.6, color="white", size=3.5)

# Average Steps per day of week 
DailyActivity2$TotalSteps[DailyActivity2$TotalSteps == 0] <- NA
SplitSteps <- split(DailyActivity2$TotalSteps, DailyActivity2$DayOfWeek)
AvgSteps <- sapply(SplitSteps, mean, na.rm = TRUE)
AvgSteps <- round(AvgSteps, digits = 2)
StepsDF <- data.frame(Day, AvgSteps)
ggplot(data = StepsDF, aes(x = fct_inorder(Day), y = AvgSteps)) + 
  geom_bar(position= "dodge", stat = "identity", fill = "steelblue") +
  labs(title = "Average Steps per Day of the Week", x = "Day of the Week", y = "Steps") +
  geom_text(aes(label=AvgSteps), vjust=1.6, color="white", size=3)

# Average Burned Calories per day of week 
DailyActivity2$Calories[DailyActivity2$Calories == 0] <- NA
SplitCals <- split(DailyActivity2$Calories, DailyActivity2$DayOfWeek)
AvgCals <- sapply(SplitCals, mean, na.rm = TRUE)
AvgCals <- round(AvgCals, digits = 2)
CalsDF <- data.frame(Day, AvgCals)
ggplot(data = CalsDF, aes(x = fct_inorder(Day), y = AvgCals)) + 
  geom_bar(position= "dodge", stat = "identity", fill = "steelblue") +
  labs(title = "Average Burned Calories per Day of the Week", x = "Day of the Week", y = "Calories") +
  geom_text(aes(label=AvgCals), vjust=1.6, color="white", size=3)

# Average Hours of Sleep per Day of the Week
SleepDay2$TotalMinutesAsleep[SleepDay2$TotalMinutesAsleep == 0] <- NA
SplitSleep <- split(SleepDay2$TotalMinutesAsleep, SleepDay2$DayOfWeek)
AvgSleep <- sapply(SplitSleep, mean, na.rm = TRUE)
AvgSleepHours <- round(AvgSleep/60, digits = 2)
SleepDF <- data.frame(Day, AvgSleepHours)
ggplot(data = SleepDF, aes(x = fct_inorder(Day), y = AvgSleepHours)) + 
  geom_bar(position= "dodge", stat = "identity", fill = "steelblue") +
  labs(title = "Average Hours of Sleep per Day of the Week", x = "Day of the Week", y = "Hours of Sleep") +
  geom_text(aes(label=AvgSleepHours), vjust=1.6, color="white", size=3.5)
```

<img src="unnamed-chunk-18-1.png" width="50%" /><img src="unnamed-chunk-18-2.png" width="50%" /><img src="unnamed-chunk-18-3.png" width="50%" /><img src="unnamed-chunk-18-4.png" width="50%" />  
**Notes:** The visuals above show averages for some of the daily
activities for each day of the week. As we can see, the two days with
the most amount of physical activity, on average, were Tuesdays and
Saturdays. These two days have the highest average for active minutes,
total steps, and calories burned. Interestingly, we can see that it is
typical for both the highest and lowest averages to occur during the
weekend. Taking a look at the total steps visual, the average daily
steps was under the recommended amount of 10,000 for every day of the
week. However, as previously discussed, this number could be lowered to
7,500 which would then mean that the average daily steps for every day
reached that goal. We can also see that overall, the average amount of
calories burned for each day of the week does not vary much. Finally,
the recommended hours of sleep per night is 7 hours or more according to
the CDC. According to the sleep visual, on average, participants reached
that amount or are just shy of 7 hours.  

### Step 6: Act

#### Conclusions:

  - If the goal for users is to burn calories, they are more likely to
    do so by participating in very rigorous to fairly rigorous
    activities. They could also burn more calories by increasing their
    daily step count.
  - As far as a goal for the number of steps per day, it is very typical
    for that amount to be 10,000 steps. I believe it would be more
    beneficial to lower this number to 7,500 to be more realistic and to
    not overwhelm users. This is because taking 7,500 steps per day,
    generally has the same overall health effect as 10,000 steps, just
    fewer calories burned.
  - When it comes to the sleep data, on average, users reached 7 hours
    of sleep or were just shy of 7 hours. However, not all participants
    logged their sleep data, most likely due to not wearing the product
    to sleep.
  - We were able to see that more of the participants log their
    calories, while fewer users log their sleep data, and only a small
    amount of users are logging their weight. It is likely that users
    find wearing a watch at night uncomfortable as well as not feeling
    comfortable logging their weight.

#### Recommendations for Bellabeat:

  - I think it is very important for users to clarify what exactly it is
    that their goals are. If it is just to simply be more active, and
    not to lose weight, notifying users to get up and walk around a bit
    more could be helpful. On the other hand, if the goal of the user is
    to lose weight, it would be beneficial to notify users to stay on
    track and to burn the necessary calories to meet their goal.
  - I believe it is important for Bellabeat to let their users know that
    the more data they share, the more likely they will be able to set
    their own goals and reach them\!
  - When it comes to sleep health, notifying users if they have spent an
    hour in bed awake is important. Encouraging them to move to another
    location, rather than their bed, will help to prevent insomnia.
  - Recommending users to get at least 7 hours of sleep is also
    important for one’s sleep health. If users are getting less than the
    recommended 7 hours, encouraging them to try to get more sleep or
    going to bed at an earlier time could be beneficial.
  - Overall, in order to get more data on users’ sleep activity, I
    believe it is essential to make sure Bellabeat’s Time is comfortable
    to wear while sleeping.
  - Promoting body positivity, may help users to feel more comfortable
    inputting their weight more often. By empowering Bellabeat users,
    they could feel more safe inputting data about their weight into a
    database.
  - Overall, making the Bellabeat app more interactive than other
    typical fitness apps could be a great way to encourage user
    activity. Allowing them to add friends and share fun workouts could
    be a great way in doing so\!
