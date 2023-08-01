# Upwork-Project-Books-Store-1000-Pages

```python
from selenium.webdriver.common.by import By
import pandas as pd
import numpy as np
import time
from selenium import webdriver
from bs4 import BeautifulSoup
import csv

driver = webdriver.Chrome()
page = 1
# Continue scraping until there are no more pages
while True:
    # Load the page
    driver.get(f'https://boardgamegeek.com/market?pageid={page}')
    time.sleep(np.random.uniform(1, 5))

    # Get the page source and create the BeautifulSoup object
    source = driver.page_source
    soup = BeautifulSoup(source, 'lxml')

    # Find all the book containers on the current page
    all_books = soup.find_all('div', class_='product-list ng-scope')

    # If no books are found, stop the loop
    if not all_books:
        print(f"No books found on page {page}. Stopping the loop.")
        continue

    # Open the CSV file in append mode
    with open('books.csv', 'a', newline='', encoding='utf-8') as csvfile:
        # Create a CSV writer object
        csv_writer = csv.writer(csvfile)

        # Iterate through each book in the current page and write its information to the CSV file
        for i in all_books:
            book_name = i.h3.text.strip()
            price = i.h4.text.strip()
            link = f'https://boardgamegeek.com/{i.a["href"]}'
            seller = i.find('a', class_='list-item-seller ng-binding').text.strip()
            csv_writer.writerow([book_name, price, link, seller])
    print(f"page number {page}")
    # Increment the page number for the next iteration
    page += 1

    # Add a random sleep to avoid being blocked by the website
    time.sleep(np.random.uniform(1, 20))

# Close the web driver
driver.quit()
