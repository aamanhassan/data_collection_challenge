# data_collection_challenge
module 11 challenge

# Scrape Titles and Preview Text from Mars News

 Import Splinter and BeautifulSoup
from splinter import Browser
from bs4 import BeautifulSoup as soup

browser = Browser('chrome')
# Visit the Mars news site
url = 'https://static.bc-edx.com/data/web/mars_news/index.html'
browser.visit(url)

# Scrape the Website

Create a Beautiful Soup object and use it to extract text elements from the website.

html = browser.html
news_soup = soup(html, 'html.parser')

# Extract all the text elements
text_elements= news_soup.find_all("div",class_='list_text')

# Create an empty list to store the dictionaries
new_items = []

# Loop through the text elements
# Extract the title and preview text from the elements
# Store each title and preview pair in a dictionary
# Add the dictionary to the list
for element in text_elements:
    title = element.find("div", class_='content_title').text
    preview = element.find("div", class_='article_teaser_body').text
    new_item= {
        "title":title,
        "preview":preview
    }

new_items

browser.quit()

# Part 2: Scrape and Analyze Mars Weather Data

# Import relevant libraries
from splinter import Browser
from selenium import webdriver
from bs4 import BeautifulSoup as soup
import matplotlib.pyplot as plt
import pandas as pd

browser = Browser('chrome')
url = "https://static.bc-edx.com/data/web/mars_facts/temperature.html"
browser.visit(url)

html= browser.html
html_soup= soup(html,'html.parser')

rows= html_soup.find_all('tr',class_='data-row')

# Create an empty list
list_of_rows = []
# Loop through the scraped data to create a list of rows
for row in rows:
    td= row.find_all('td')
    row = [col.text for col in td]
    list_of_rows.append(row)
    
# Create a Pandas DataFrame by using the list of rows and a list of the column names
df= pd.DataFrame(list_of_rows, columns= ['id','terrestrial_date','sol','ls','month','min_temp','pressure'])

# Confirm DataFrame was created successfully
df.head()

# Examine data type of each column
df.dtypes

# Change data types for data analysis
df.terrestrial_date = pd.to_datetime(df.terrestrial_date)
df.sol= df.sol.astype('int')
df.ls = df.ls.astype('int')
df.month= df.month.astype('int')
df.min_temp= df.min_temp.astype('float')
df.pressure = df.pressure.astype('float')

# Confirm type changes were successful by examining data types again
df.dtypes

# 1. How many months are there on Mars?
df["month"].value_counts().sort_index()

# 2. How many Martian days' worth of data are there?
df["sol"].nunique()

# 3. What is the average low temperature by month?
average_temp_by_month= df.groupby("month")["min_temp"].mean()
average_temp_by_month

# Plot the average temperature by month
average_temp_by_month.plot(kind='bar')
plt.ylabel('temp in C')
plt.xlabel('month')
plt.show()

# Identify the coldest and hottest months in Curiosity's location
average_temp_by_month.sort_values().plot(kind='bar')
plt.ylabel('temp in C')
plt.xlabel('month')
plt.show()

# 4. Average pressure by Martian month
pressure_by_month= df.groupby("month")["pressure"].mean()
pressure_by_month

# Plot the average pressure by month
pressure_by_month.sort_values().plot(kind='bar')
plt.ylabel('Atmospheric Pressure')
plt.xlabel('month')
plt.show()

# 5. How many terrestrial (earth) days are there in a Martian year?
df.min_temp.plot()
plt.ylabel('Min Temperature')
plt.xlabel('No of Terrestrial (earth) days')
plt.show()

# Analasis:
On average, the third month has the coldest minimum temperature on Mars, and the eighth month is the warmest. But it is always very cold there in human terms!
Atmospheric pressure is, on average, lowest in the sixth month and highest in the ninth.
The distance from peak to peak is roughly 1425-750, or 675 days. A year on Mars appears to be about 675 days from the plot. Internet search confirms that a Mars year is equivalent to 687 earth days.

# Write the data to a CSV
df.to_csv('mars_data.csv',index=False)

browser.quit()
Atmospheric pressure is, on average, lowest in the sixth month and highest in the ninth.
