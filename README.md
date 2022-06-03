# Goal
1. Scrape fashion products data from https://www.fordays.com.
2. Store the data in a hosted Postgres database.

# Scraping Strategy
### About the Website


We can treat the website as **3 levels**: home page, product page and detail page. 
- **Home page**: 3 product categories [Men, Women, Kids]
- **Product page**: overviews of proudcts in one category, like name and price
- **Detail page**: detailed information of one product, including size, color, description, fabric, etc

Although there is a tab called "For All", it is a subset of all the products, so we had better scrape the 3 categories respectively for the whole website.

<img src="images/website pages.png" width="800" height="160">

### Scraping Flow
The overall workflow is as follows. 

<img src="images/Scraping Flow.png" width="500" height="400">

For the web scraping part, both Selenium and bs4 are needed since there are some JavaScript codes in the website, some data is dynamic or hidden. We can scrape the source page first using Selenium, then load it into BeautifulSoup for further analysis. Each data record represents one sku, which means a product with specific color and size. This allows us to store the invertory information. 

For the data storage and update part, we can use [product_id, color, size] as primary key. Some update examples: product name change, price change, inventory change.

# Scraping Test
Here one page of "Men" products is scraped as a test. This page covers many special cases, e.g. products with a discount, products without colors or sizes, skus that are sold out, products lack of some information. Data is stored in both **local database** and **AWS RDS**.

<img src="images/AWS RDS & Local DB.png" width="600" height="200">

### Database Structure
**Rows**: Each row represents **one sku**, which means a product with specific color and size. This allows us to track the invertory information for each sku. 

**Fields**:
```
product_id (str)
display_name (str)
color (str)
size (str)
gender (str) [Men, Women or Baby + Kids]
sale_price (str) [actual sale price after discounts or promotions]
regular_price (str) [original price before discounts or promotions]
product_url (str)
description (str)
is_available (bool) [False if the product is sold out]
fit (str) [a brief description of product fit]
fabric (str) [a brief description of product fabric]
sustainability (str) [a brief description of product sustainability]
recycle (str) [a brief description of product recycle]
scrapped_date (date)
```
