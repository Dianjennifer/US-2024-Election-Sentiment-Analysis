# Sentiment Analysis on 2024 U.S. Election
![image](https://github.com/Dianjennifer/US-2024-Election-Sentiment-Analysis/blob/main/Where%20do%20Harris%20and%20Trump%20stand%20on%20the%20key%20election%20issues_.jpeg)

## Table of Contents



## Introduction
Social media has become a powerful platform for political conversation, especially during major events like the 2024 U.S. presidential election. In this project, i analyzed the sentiment of social media mentions to explore how candidates like Kamala Harris, Donald Trump, and others are perceived by the public. By analyzing the tone—whether positive, negative, or neutral—of these mentions, we aim to understand how social media influences voter opinions and amplifies campaign messages. This analysis provides valuable insights into the emotional pulse of the electorate and the role of social media in shaping the election 

## Data Loading
For this analysis, the data was sourced from Kaggle, specifically the 2024 U.S. Election Sentiment on X (formerly Twitter) dataset. This dataset contains a collection of social media posts related to the 2024 U.S. presidential election, including mentions of key candidates and their associated sentiments. The dataset was obtained by downloading the files directly from Kaggle, a popular platform for datasets, competitions, and data science resources.

The dataset is divided into three parts: train, test, and validation, which include columns such as candidate name, sentiment, and timestamp. These files were loaded into R for further analysis. The data provides insights into how different candidates are discussed and perceived on social media, which is central to our sentiment analysis.

```r
train <- read.csv("C:/Users/PC/Downloads/archive/train.csv")
test <- read.csv("C:/Users/PC/Downloads/archive/test.csv")
val <- read.csv("C:/Users/PC/Downloads/archive/val.csv")

View(train)
View(test)
View(val)
```

## Data Cleaning

Before diving into the analysis, I cleaned and preprocessed the data. This involved handling missing values, converting the timestamp column into a proper datetime format, and standardizing the sentiment column into positive, negative, and neutral categories. I also ensured candidate names were consistent. These steps were essential to prepare the data for accurate sentiment analysis.

 * Combining the data.
The first step in the analysis was to combine the three separate datasets—train, test, and validation—into one comprehensive dataset. This allowed for a unified view of the social media mentions and their associated sentiments. I used the rbind() function in R to merge the datasets, ensuring that all relevant data was included for analysis. This combined dataset served as the foundation for the sentiment analysis.

```r
combined_data <- rbind(train, val, test)
View(combined_data)
```

 * Checking for missing values
```r
colSums(is.na(combined_data))
```
There were no missing values in the data.

 * Checking the column data type
```r
str(combined_data)
```
The time stamp column was formatted as text and therefore it was necessary to change it to datetime format.
```r
library(lubridate)
combined_data$timestamp <- ymd_hms(combined_data$timestamp)
str(combined_data$timestamp)
```
* Checking the distribution of numeric valuables

This helped in identifying patterns, outliers, and potential issues that could affect the analysis.
```r
hist(combined_data$likes, 
     main = "Distribution of Likes", 
     xlab = "Likes", 
     col = "orange", 
     border = "black", 
     breaks = 10)
```
