# **Project "Prosffer"**




## **Business Idea**

A website that provides a general Overview for goods of different market brands in order to compare prices and get a wider overview of accessable goods.

## **Problem Statement**

- In today’s fast-paced, highly competitive marketplace, consumers are overwhelmed by the variety of goods, brands, and prices available across different stores and online platforms. With limited time and resources, customers often struggle to efficiently compare prices and quality, leading to suboptimal purchasing decisions. There is a lack of a centralized platform that provides a comprehensive, real-time comparison of goods from multiple brands and markets, making it difficult for users to identify the best value for their money.

- Examples:

    - In Germany we have different Supermarkets, such as Kaufland, Lidl, REWE, EDEKA, Aldi, etc, All of them have different type of products, brands, and special prices and promotions.

    - When people go shopping, there are so many different stores and brands selling the same kinds of things. It’s hard for them to know which store has the best price or which product is the best. Right now, people have to check lots of different websites or stores one by one to compare prices. This takes a lot of time and can be confusing. (Polina will continue)

    - Imagine your family wants to buy a big bag of rice, and different stores sell the same type of rice. One store sells it for $30, another for $25, and another for $20, but you don’t know which store has the lowest price without checking all the websites or visiting the stores. So, you have to spend a lot of time going through different websites, writing down the prices, and figuring out where to buy it for the best deal.

    - Wouldn’t it be much easier if there was one website that showed you all the prices from every store in one place? That way, you could quickly see which store has the cheapest rice, saving you time and money!




## **Solution Statement -> Business Mission Statement** 

- Our website will serve as a centralized platform that allows consumers to easily compare prices and features of goods from various brands across multiple markets. By aggregating real-time data on product availability, pricing, and user reviews, we will empower users to make informed purchasing decisions. Our intuitive interface will enable users to filter and sort products based on their preferences, ensuring they find the best value for their needs. With personalized recommendations and alertsfor price drops, our platform will enhance the shopping experience, ultimately saving users time and money.



---


> ### **What we need to be discussed**

- extract data from Supermarket websites
- focus on extraction from CSV-files (How to extract from PDF) ?
- database? (storing the fetched data, user, data analisys)
    - user
    - data analisys/processing
    - wishlist
    - frequently buy



> ### **Tools we need to use**
- **Python Packages | Libraries**

    - webscrap (BeautifulSoup, Scrapy)
    - Django
    - dj_rest_auth
    - requests
    - PostgreSQL
    - Panda
    - nampy
- **In details**

    1. **Web Development Framework:**
        - **Django:** A full-featured framework that comes with an ORM, admin interface, and built-in security features.

    2. **Web Scraping Tools**
        - **BeautifulSoup or Scrapy:** For gathering data (prices, descriptions, etc.) from various websites.
             -  **BeautifulSoup:** Easy-to-use library for smaller scraping tasks.
             -  **Scrapy:** A more powerful and scalable framework for scraping large datasets.
        - **Selenium:** For scraping websites that require user interaction (JavaScript-heavy websites).
    
    3. **Database Management**
        - **PostgreSQL:** For storing product data, price comparisons, and user information.
    
    4. **Frontend Technologies**
        - **HTML/CSS/JavaScript:** To create an interactive and user-friendly interface.
        - **React.js, Vue.js, or Angular.js**: Modern frontend libraries to build a dynamic, responsive user interface.
        - **Bootstrap** or **Tailwind CSS:** CSS frameworks to ensure your site looks good on all devices.
    
    5. **Data Processing and Analysis**
        - **Pandas:** For managing and manipulating the product and pricing data in Python.
        - **NumPy:** For more advanced numerical operations if needed.
        - **Matplotlib** or **Plotly:** For any visual comparison or charting features (if you want to show trends in pricing, for example).
    
    6. **Search and Filtering Tools**
        - **Elasticsearch**: For fast and powerful search capabilities across products and brands.
        - **Algolia:** A search-as-a-service solution that provides fast and easy product search functionality.
    
    7. **User Authentication and Security**
        - **Django's authentication system or Auth0:** For secure user sign-ups, logins, and authentication.
    
    8. **Caching and Performance Optimization**
        - **Redis** or **Memcached**: For caching frequently accessed data (like product prices) to improve site performance.
    
    9. **Automating Data Collection**
        - **Cron Jobs (on Linux-based systems)** to run scripts periodically.

> ### **Links to scrap**


- **Netto:**  https://www.netto-online.de/
- **Kaufland:** https://www.kaufland.de/
- **Aldie-Süd:**  https://www.aldi-sued.de/de/homepage.html
- **Alidie-Nord:** https://www.aldi-nord.de/
- **Edeka:** https://www.edeka24.de/
- **REWE:** https://shop.rewe.de/ (Terence Scrap this)
- **Lidl:** https://www.lidl.de/ (Terence Scrap this)

## **SCRAP CODE SOLUTION**

```py
from selenium import webdriver
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By
from bs4 import BeautifulSoup
import time

# Set up the Chrome WebDriver (adjust the path to your ChromeDriver)
service = Service('/home/dci-student/Desktop/DCI/Test/chromedriver')
driver = webdriver.Chrome(service=service)

# URL of the supermarket page you want to scrape
url = 'https://www.netto-online.de/konserven/c-N0122'
driver.get(url)

# Wait for the page to fully load (adjust sleep as needed or use explicit waits)
time.sleep(5)

# Pass the page content to BeautifulSoup
soup = BeautifulSoup(driver.page_source, 'html.parser')

# Now, scrape the product details as you would with BeautifulSoup
products = soup.find_all('a', class_='product__content')

for product in products:
    # Extract the product title
    title = product.find('div', class_='product__title')
    
    # Extract the price container
    price_container = product.find('div', class_='product__price__label')

    if title and price_container:
        # Extract integer part of the price
        integer_part = price_container.find('ins', class_='product__current-price').contents[0].strip()

        # Extract fractional part of the price (digits after the comma)
        fractional_part = price_container.find('span', class_='product__current-price--digits-after-comma').text.strip()

        # Combine integer and fractional parts to form full price
        price = integer_part + fractional_part

        # Print the product title and price
        print(f'Product: {title.text.strip()}, Price: {price}')

# Close the browser
driver.quit()
```

# **STEPS to get the CODE WORKING, is to download Chrome Driver**

To find the correct version of ChromeDriver for your installed version of Chrome, follow these steps:

Check Your Chrome Version:

Open Chrome.
Click on the three vertical dots (menu) in the top right corner.
Go to Help > About Google Chrome.
Note the version number (e.g., 115.0.5790.98).
Download ChromeDriver:

Visit the ChromeDriver download page.
Look for the version that matches your Chrome version. The ChromeDriver version should be the same as the major version number of your Chrome (e.g., if your Chrome is 115.x, download ChromeDriver 115.x).
Click on the version link, and download the appropriate file for your operating system (Windows, macOS, Linux).
Extract and Set Up ChromeDriver:

Extract the downloaded file.
Move the chromedriver executable to a directory in your system's PATH, or note its location for use in your automation scripts.
