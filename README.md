# Load required libraries
library(tidyverse)
library(lubridate)
library(ggplot2)

# Load data
train <- read_csv("train.csv")

# Process data
train$datetime <- ymd_hms(train$datetime)
train$hour <- hour(train$datetime)
train$day_of_week <- wday(train$datetime, label = TRUE)

# Group and summarize data
hourly_demand <- train %>%
  group_by(day_of_week, hour) %>%
  summarise(avg_demand = mean(count, na.rm = TRUE))

# Plot heatmap
ggplot(hourly_demand, aes(x = hour, y = day_of_week, fill = avg_demand)) +
  geom_tile(color = "white") +
  scale_fill_gradient(low = "white", high = "red") +
  labs(title = "Hourly Bike Sharing Demand",
       x = "Hour of Day",
       y = "Day of Week",
       fill = "Average Demand") +
  theme_minimal() +
  theme(text = element_text(size = 14))
