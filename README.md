# Sentiment Analysis on 2024 U.S. Election
![image](https://github.com/Dianjennifer/US-2024-Election-Sentiment-Analysis/blob/main/Where%20do%20Harris%20and%20Trump%20stand%20on%20the%20key%20election%20issues_.jpeg)

## Table of Contents



7. ## Introduction
Social media has become a powerful platform for political conversation, especially during major events like the 2024 U.S. presidential election. In this project, i analyzed the sentiment of social media mentions to explore how candidates like Kamala Harris, Donald Trump, and others are perceived by the public. By analyzing the tone—whether positive, negative, or neutral—of these mentions, we aim to understand how social media influences voter opinions and amplifies campaign messages. This analysis provides valuable insights into the emotional pulse of the electorate and the role of social media in shaping the election 

## Data Loading
For this analysis, the data was sourced from Kaggle, specifically the 2024 U.S. Election Sentiment on X (formerly Twitter) dataset. This dataset contains a collection of social media posts related to the 2024 U.S. presidential election, including mentions of key candidates and their associated sentiments. The dataset was obtained by downloading the files directly from Kaggle, a popular platform for datasets, competitions, and data science resources.

The dataset is divided into three parts: train, test, and validation, which include columns such as candidate name, sentiment, and timestamp. These files were loaded into R for further analysis. The data provides insights into how different candidates are discussed and perceived on social media, which is central to our sentiment analysis.

```sql
train <- read.csv("C:/Users/PC/Downloads/archive/train.csv")
test <- read.csv("C:/Users/PC/Downloads/archive/test.csv")
val <- read.csv("C:/Users/PC/Downloads/archive/val.csv")

View(train)
View(test)
View(val)
```
