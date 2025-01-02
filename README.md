# Sentiment Analysis on 2024 U.S. Election
![image](https://github.com/Dianjennifer/US-2024-Election-Sentiment-Analysis/blob/main/Where%20do%20Harris%20and%20Trump%20stand%20on%20the%20key%20election%20issues_.jpeg)

## Disclamer
This analysis is intended for demonstration purposes only and does not represent a fully developed or operational project.

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
![image](https://github.com/Dianjennifer/US-2024-Election-Sentiment-Analysis/blob/main/Disribution%20of%20Likes.png)
```r
hist(combined_data$retweets, 
     main = "Distribution of Retweets", 
     xlab = "Retweets", 
     col = "purple",
     border = "black",
     breaks = 10)
```
![image](https://github.com/Dianjennifer/US-2024-Election-Sentiment-Analysis/blob/main/Distribution%20of%20Retweets)

* Text Normalization
To prepare the tweets for analysis, text normalization was performed to ensure consistency and reduce noise.
```r
library(dplyr)
library(tm)

clean_text <- function(text) {
  text <- tolower(text)
  text <- removePunctuation(text)
  text <- removeNumbers(text)
  text <- removeWords(text, stopwords("en"))
  text <- gsub("http\S+|www\S+", "", text) # Remove URLs
  return(text)}

combined_data <- combined_data %>%
  mutate(text_cleaned = sapply(tweet_text, clean_text))
```

## Exploratory Data Analysis
Exploratory Data Analysis (EDA) helps to understand the dataset’s key characteristics. I visualized and summarized key variables to uncover patterns, trends, and potential anomalies. Using tools like histograms and bar plots, EDA provided insights that guided the next steps of the analysis.

### Sentiment Distribution
The Sentiment Distribution step focuses on analyzing how the sentiment of social media posts is spread across different categories—positive, negative, and neutral. By examining the frequency of each sentiment category, I can understand the overall tone of the public’s opinion on the candidates. This analysis helps identify trends and biases in the data, offering valuable insights into how candidates are perceived by the public.
```r
# Inspecting the sentiment distribution using a bar plot
library(ggplot2)

# Create a bar plot of sentiment distribution
ggplot(combined_data, aes(x = sentiment, fill = sentiment)) +
  geom_bar() +
  labs(title = "Sentiment Distribution", x = "Sentiment", y = "Count") +
  scale_fill_manual(values = c("positive" = "lightgreen", "negative" = "lightcoral", "neutral" = "lightblue")) +
  theme_minimal()
```
![image](https://github.com/Dianjennifer/US-2024-Election-Sentiment-Analysis/blob/main/Sentiment%20Distribution.png)

* Sentiment Distribution by Party
By visualizing sentiment for each party, we can understand public perceptions and how they vary among political groups.
```r
ggplot(combined_data, aes(x = party, fill = sentiment)) +
  geom_bar(position = "dodge") +
  labs(title = "Sentiment by Party", x = "Party", y = "Count") +
  scale_fill_manual(values = c("positive" = "lightgreen", "negative" = "lightcoral", "neutral" = "lightblue")) +
  theme(axis.text.x = element_text(angle = 45, hjust = 1)) +
  theme_minimal()
```
![image](https://github.com/Dianjennifer/US-2024-Election-Sentiment-Analysis/blob/main/Sentiment%20by%20Party.png)

* Sentiment Distribution by Candidate
By analyzing the sentiment distribution for each candidate, we can uncover public perceptions and emotional reactions to their campaigns. This analysis provides insights into how different candidates resonate with voters and the tone of discussions surrounding them on social media.
```r
# Load required libraries
library(ggplot2)

# Create a stacked bar plot for sentiment distribution by candidate
ggplot(combined_data, aes(x = candidate, fill = sentiment)) +
  geom_bar(position = "stack") +
  labs(title = "Sentiment Distribution by Candidate",
       x = "Candidate",
       y = "Count") +
  scale_fill_manual(values = c("positive" = "lightgreen", "neutral" = "lightblue", "negative" = "lightcoral")) +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```
![image](https://github.com/Dianjennifer/US-2024-Election-Sentiment-Analysis/blob/main/Sentiment%20by%20Candidate.png)

## Engagemnt Analysis
It’s crucial to examine how the public engages with tweets or likes about candidates and political issues, and whether sentiment influences this engagement. By analyzing the connection between sentiment (positive, negative, or neutral) and engagement metrics, we can uncover which topics or candidates sparked more excitement, debate, or approval among the electorate.
```r
library(dplyr)
library(ggplot2)

# Summarize the engagement (retweets, likes) by sentiment
engagement_analysis <- combined_data %>%
  group_by(sentiment) %>%
  summarize(mean_retweets = mean(retweets, na.rm = TRUE),
            mean_likes = mean(likes, na.rm = TRUE))

# Ensure sentiment is a factor to control the order of bars
engagement_analysis$sentiment <- factor(engagement_analysis$sentiment, levels = c("positive", "neutral", "negative"))

# Visualize the average likes by sentiment
ggplot(engagement_analysis, aes(x = sentiment, y = mean_likes, fill = sentiment)) +
  geom_bar(stat = "identity") +
  labs(title = "Average Likes by Sentiment", x = "Sentiment", y = "Average Likes") +
  theme_minimal()
```
## Topic Analysis
By analyzing the frequency of words and sentiments across different candidates, political parties, and time periods, the goal is to uncover public opinion trends and identify what voters are most concerned about. This analysis will be supported by visualizations such as word clouds, helping to highlight important patterns and topics that emerge in election-related conversations.
```r

library(wordcloud)
library(RColorBrewer)
library(dplyr)

# Function to generate word cloud
generate_wordcloud <- function(text, title) {
  # Generate word cloud with a reduced number of words
  wordcloud(
    words = text,
    min.freq = 1,
    scale = c(3, 0.5), 
    random.order = FALSE,
    colors = brewer.pal(8, "Dark2"),
    max.words = 100 # Limit the number of words in the cloud
  )
  
  # Add title with smaller font size and without spacing issue
  title(main = title, cex.main = 0.8) 
}
# Generate word clouds for each sentiment
for (sentiment in c("positive", "neutral", "negative")) {
  # Subset the data
  subset <- combined_data %>% filter(sentiment == sentiment)
  
  # Generate the word cloud
  title_text <- paste("Word Cloud for", toupper(substr(sentiment, 1, 1)), substr(sentiment, 2, nchar(sentiment)), "Sentiment")
  generate_wordcloud(subset$tweet_text, title_text)
}
```
![image](https://github.com/Dianjennifer/US-2024-Election-Sentiment-Analysis/blob/main/Positive%20Sentiment.png)
![image](https://github.com/Dianjennifer/US-2024-Election-Sentiment-Analysis/blob/main/Neutral%20Sentiment.png)
![image](https://github.com/Dianjennifer/US-2024-Election-Sentiment-Analysis/blob/main/Negative%20Sentiment.png)


## Insights
Candidate Sentiment Dynamics . Social media sentiment for Donald Trump displayed significant polarization, with both strongly positive and strongly negative opinions dominating the discussion. Kamala Harris, on the other hand, exhibited a relatively neutral sentiment distribution, indicating less emotionally charged discourse overall.

Dominant Topics of Discussion . For both candidates, key issues like economic policy and healthcare reform were consistently mentioned.

## Limitataions
* Potential Biases in the Dataset
Demographic Representation: The dataset primarily contains social media mentions, which tend to be more representative of certain demographic groups, such as younger, more tech-savvy individuals. Consequently, the sentiment analysis may not fully capture the views of older populations, rural voters, or those with limited internet access. This skew could influence the overall sentiment trends, especially when considering the impact of social media on different voter segments.

* Limitations of Temporal Data
The dataset is limited to the time frame in which it was collected, meaning it might not capture shifting sentiments in real-time. If public opinion changes quickly, such as after a controversial statement or event, the dataset may not reflect the most current sentiment. This temporal lag can affect the accuracy of any conclusions drawn from the analysis.

