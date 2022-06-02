# Goal
1. Scrape fashion products data from https://www.fordays.com.
2. Store the data in a hosted Postgres database.

# Scraping Strategy
### About the Website
There are generally **3 product categories**: Women, Men and Baby + Kids. Although there is a tab called "For All", it is a subset of all the products, so we had better scrape the 3 categories respectively for the whole website.

We can treat the website as **3 levels**: home page, product page and detail page. 
- **Home page**: 3 product genders
- **Product page**: overviews of proudcts in one category, like name and price
- **Detail page**: detailed information of one product, including size, color, inventory, description, fabric, etc

<img src="images/website pages.png" width="1000" height="200">

# Scraping Test and Data Storage
One page of "Men" products are scraped as a test. The data is stored in both **local database** and **AWS RDS**.

<img src="images/AWS RDS & Local DB.png" width="600" height="200">

### Database Structure
**Rows**: Each row represents **one sku**, which means a product with specific color and size. This allows us to track the invertory information for each sku. 

**Fields**:
```
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
