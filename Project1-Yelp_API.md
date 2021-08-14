# Yelp API Project


## Introduction 

We've learned how to query the Yelp API, analyze JSON data and create basic visualizations. It's time to put those skills to work in order to create a project of your own! Taking things a step further, you'll also independently explore how to perform pagination in order to retrieve a full results set from the Yelp API.

## Objectives

You will be able to: 

* Practice using functions to organize your code
* Use pagination to retrieve all results from an API query
* Practice parsing data returned from an API query
* Practice interpreting visualizations of a dataset

# Task: Query Yelp for All Businesses in a Category and Analyze the Results

![restaurant counter with pizza](images/restaurant_counter.jpg)

Photo by <a href="https://unsplash.com/@jordanmadrid?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Jordan Madrid</a> on <a href="/s/photos/pizza-restaurant?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>

## Overview

You've now worked with some API calls, but we have yet to see how to retrieve a more complete dataset in a programmatic manner. In this lab, you will write a query of businesses on Yelp, then use *pagination* to retrieve all possible results for that query. Then you will pre-process and analyze your data, leading to a presentation of your findings.

## Requirements

### 1. Make the Initial Request

Start by filling in your API key to make the initial request to the business search API. 

### 2. Add Pagination

Using loops and functions, collect the maximum number of results for your query from the API.

### 3. Prepare Data

Investigate the structure of the response you get back and extract the relevant information.

### 4. Perform Descriptive Analysis

Interpret visualizations related to the price range, average rating, and number of reviews for all query results.

### 5. Create Presentation Notebook

Edit this notebook or create a new one to showcase your work.

# 1. Make the Initial Request

## Querying

Start by making an initial request to the Yelp API. Your search must include at least 2 parameters: **term** and **location**. For example, you might search for pizza restaurants in NYC. The term and location is up to you but make the request below.

Use the `requests` library ([documentation here](https://requests.readthedocs.io/en/master/user/quickstart/#make-a-request)).

You'll also need an API key from Yelp. If you haven't done this already, go to the Yelp [Manage App page](https://www.yelp.com/developers/v3/manage_app) and create a new app (after making an account if you haven't already).


```python
# Replace None with appropriate code

# Import the requests library
import json
import requests

# Get this from the "Manage App" page. Make sure you set them
# back to None before pushing this to GitHub, since otherwise
# your credentials will be compromised
def get_keys(path):
    with open(path) as f:
        return json.load(f)
    
#keys = get_keys(hidden)
api_key = keys['api_key']

# These can be whatever you want! But the solution uses "pizza"
# and "New York NY" if you want to compare your work directly
term = 'pizza'
location = 'New York NY'

# Set up params for request
url = "https://api.yelp.com/v3/businesses/search"
headers = {
    "Authorization": "Bearer {}".format(api_key)
}
url_params = {
    "term": term.replace(" ", "+"),
    "location": location.replace(" ", "+")
}

# Make the request using requests.get, passing in
# url, headers=headers, and params=url_params
response = requests.get(url, headers=headers, params=url_params)

# Confirm we got a 200 response
response
```




    <Response [200]>




```python
# Run this cell without changes

# Get the response body in JSON format
response_json = response.json()
# View the keys
response_json.keys()
```




    dict_keys(['businesses', 'total', 'region'])



## Extracting Data

Now, retrieve the value associated with the `'businesses'` key, and inspect its contents.


```python
# Replace None with appropriate code

# Retrieve the value from response_json
businesses = response.json()['businesses']

# View number of records
print(f"There are {len(businesses)} businesses in this response")

# View the first 2 records
businesses[:2]
```

    There are 20 businesses in this response
    




    [{'id': 'zj8Lq1T8KIC5zwFief15jg',
      'alias': 'prince-street-pizza-new-york-2',
      'name': 'Prince Street Pizza',
      'image_url': 'https://s3-media3.fl.yelpcdn.com/bphoto/ZAukOyv530w4KjOHC5YY1w/o.jpg',
      'is_closed': False,
      'url': 'https://www.yelp.com/biz/prince-street-pizza-new-york-2?adjust_creative=ur9Wljdns4RF6XMXurfFSA&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=ur9Wljdns4RF6XMXurfFSA',
      'review_count': 3936,
      'categories': [{'alias': 'pizza', 'title': 'Pizza'},
       {'alias': 'italian', 'title': 'Italian'}],
      'rating': 4.5,
      'coordinates': {'latitude': 40.72308755605564,
       'longitude': -73.99453001177575},
      'transactions': ['pickup', 'delivery'],
      'price': '$',
      'location': {'address1': '27 Prince St',
       'address2': None,
       'address3': '',
       'city': 'New York',
       'zip_code': '10012',
       'country': 'US',
       'state': 'NY',
       'display_address': ['27 Prince St', 'New York, NY 10012']},
      'phone': '+12129664100',
      'display_phone': '(212) 966-4100',
      'distance': 1961.8771417367063},
     {'id': 'ysqgdbSrezXgVwER2kQWKA',
      'alias': 'julianas-brooklyn-3',
      'name': "Juliana's",
      'image_url': 'https://s3-media2.fl.yelpcdn.com/bphoto/clscwgOF9_Ecq-Rwsq7jyQ/o.jpg',
      'is_closed': False,
      'url': 'https://www.yelp.com/biz/julianas-brooklyn-3?adjust_creative=ur9Wljdns4RF6XMXurfFSA&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=ur9Wljdns4RF6XMXurfFSA',
      'review_count': 2339,
      'categories': [{'alias': 'pizza', 'title': 'Pizza'}],
      'rating': 4.5,
      'coordinates': {'latitude': 40.70274718768062,
       'longitude': -73.99343490196397},
      'transactions': ['delivery'],
      'price': '$$',
      'location': {'address1': '19 Old Fulton St',
       'address2': '',
       'address3': '',
       'city': 'Brooklyn',
       'zip_code': '11201',
       'country': 'US',
       'state': 'NY',
       'display_address': ['19 Old Fulton St', 'Brooklyn, NY 11201']},
      'phone': '+17185966700',
      'display_phone': '(718) 596-6700',
      'distance': 308.56984360837544}]



# 2. Add Pagination

Now that you are able to get one set of responses, known as a **page**, let's figure out how to request as many pages as possible.

## Technical Details

Returning to the Yelp API, the [documentation](https://www.yelp.com/developers/documentation/v3/business_search) also provides us details regarding the **API limits**. These often include details about the number of requests a user is allowed to make within a specified time limit and the maximum number of results to be returned. In this case, we are told that any request has a **maximum of 50 results per request** and defaults to 20. Furthermore, any search will be limited to a **total of 1000 results**. To retrieve all 1000 of these results, we would have to page through the results piece by piece, retrieving 50 at a time. Processes such as these are often referred to as pagination.

Also, be mindful of the **API** ***rate*** **limits**. You can only make **5000 requests per day** and are also can make requests too fast. Start prototyping small before running a loop that could be faulty. You can also use `time.sleep(n)` to add delays. For more details see https://www.yelp.com/developers/documentation/v3/rate_limiting.

In this lab, you will define a search and then paginate over the results to retrieve all of the results. You'll then parse these responses as a list of dictionaries (for further exploration) and create a map using Folium to visualize the results geographically.

## Determining the Total

Depending on the number of total results for your query, you will either retrieve all of the results, or just the first 1000 (if there are more than 1000 total).

We can find the total number of results using the `"total"` key:


```python
# Run this cell without changes
response_json['total']
```




    11800



(This is specific to the implementation of the Yelp API. Some APIs will just tell you that there are more pages, or will tell you the number of pages total, rather than the total number of results. If you're not sure, always check the documentation.)

In the cell below, assign the variable `total` to either the value shown above (if it is less than 1000), or 1000.


```python
# Replace None with appropriate code
total = None
if response_json['total'] < 1000:
    total = response_json['total']
else:
    total = 1000
total
```




    1000



### Calculating the Offsets

The documentation states in the parameters section:

> **Name**: `limit`, **Type:** int, **Description:** Optional. Number of business results to return. By default, it will return 20. Maximum is 50.

> **Name**: `offset`, **Type:** int, **Description:** Optional. Offset the list of returned business results by this amount.

So, to get the most results with the fewest API calls we want to set a limit of 50 every time. If, say, we wanted to get 210 total results, that would mean:

1. Offset of `0` (first 50 records)
2. Offset of `50` (second 50 records)
3. Offset of `100` (third 50 records)
4. Offset of `150` (fourth 50 records)
5. Offset of `200` (final 10 records)

In the cell below, create a function `get_offsets` that takes in a total and returns a list of offsets for that total. You can assume that there is a limit of 50 every time.

*Hint: you can use `range` ([documentation here](https://docs.python.org/3.3/library/stdtypes.html?highlight=range#range)) to do this in one line of code. Just make the returned result is a list.*


```python
# Replace None with appropriate code
def get_offsets(total):
    offsets = []
    for offset in range(0, total, 50): 
        offsets.append(offset)      
    return offsets
```


```python
get_offsets(210)

```




    [0, 50, 100, 150, 200]



Check that your function works below:


```python
# Run this cell without changes

assert get_offsets(200) == [0, 50, 100, 150]
assert get_offsets(210) == [0, 50, 100, 150, 200]
```

### Putting It All Together

Recall that the following variable has already been declared for you:


```python
# Run this cell without changes
url_params
```




    {'term': 'pizza', 'location': 'New+York+NY'}



We'll go ahead and also specify that the limit should be 50 every time:


```python
# Run this cell without changes
url_params["limit"] = 50
```

In order to modify the offset, you'll need to add it to `url_params` with the key `"offset"` and whatever value is needed.

In the cell below, write code that:

* Creates an empty list for the full prepared dataset
* Loops over all of the offsets from `get_offsets` and makes an API call each time with the specified offset
* Extends the full dataset list with each query's dataset


```python
# Replace None with appropriate code

# Create an empty list for the full prepared dataset
full_dataset = []

for offset in get_offsets(total):
    # Add or update the "offset" key-value pair in url_params
    url_params['offset'] = offset
    
    # Make the query and get the response
    response = requests.get(url, headers=headers, params=url_params)
    
    # Get the response body in JSON format
    response_json = response.json()
    
    # Get the list of businesses from the response_json
    businesses = response.json()['businesses']
    
    # Extend full_dataset with this list (don't append, or you'll get
    # a list of lists instead of a flat list)
    full_dataset.extend(businesses)

# Check the length of the full dataset. It will be up to `total`,
# potentially less if there were missing values
len(full_dataset)
```




    1000



This code may take up to a few minutes to run.

If you get an error trying to get the response body in JSON format, try adding `time.sleep(1)` right after the `requests.get` line, so your code will sleep for 1 second between each API call.

# 3. Prepare Data

Now that we have all of our data, let's prepare it for analysis. It can be helpful to start this process by inspecting the raw data.


```python
# Run this cell without changes

# View the first 2 records
full_dataset[:2]
```




    [{'id': 'zj8Lq1T8KIC5zwFief15jg',
      'alias': 'prince-street-pizza-new-york-2',
      'name': 'Prince Street Pizza',
      'image_url': 'https://s3-media3.fl.yelpcdn.com/bphoto/ZAukOyv530w4KjOHC5YY1w/o.jpg',
      'is_closed': False,
      'url': 'https://www.yelp.com/biz/prince-street-pizza-new-york-2?adjust_creative=ur9Wljdns4RF6XMXurfFSA&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=ur9Wljdns4RF6XMXurfFSA',
      'review_count': 3936,
      'categories': [{'alias': 'pizza', 'title': 'Pizza'},
       {'alias': 'italian', 'title': 'Italian'}],
      'rating': 4.5,
      'coordinates': {'latitude': 40.72308755605564,
       'longitude': -73.99453001177575},
      'transactions': ['pickup', 'delivery'],
      'price': '$',
      'location': {'address1': '27 Prince St',
       'address2': None,
       'address3': '',
       'city': 'New York',
       'zip_code': '10012',
       'country': 'US',
       'state': 'NY',
       'display_address': ['27 Prince St', 'New York, NY 10012']},
      'phone': '+12129664100',
      'display_phone': '(212) 966-4100',
      'distance': 1961.8771417367063},
     {'id': 'ysqgdbSrezXgVwER2kQWKA',
      'alias': 'julianas-brooklyn-3',
      'name': "Juliana's",
      'image_url': 'https://s3-media2.fl.yelpcdn.com/bphoto/clscwgOF9_Ecq-Rwsq7jyQ/o.jpg',
      'is_closed': False,
      'url': 'https://www.yelp.com/biz/julianas-brooklyn-3?adjust_creative=ur9Wljdns4RF6XMXurfFSA&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_search&utm_source=ur9Wljdns4RF6XMXurfFSA',
      'review_count': 2339,
      'categories': [{'alias': 'pizza', 'title': 'Pizza'}],
      'rating': 4.5,
      'coordinates': {'latitude': 40.70274718768062,
       'longitude': -73.99343490196397},
      'transactions': ['delivery'],
      'price': '$$',
      'location': {'address1': '19 Old Fulton St',
       'address2': '',
       'address3': '',
       'city': 'Brooklyn',
       'zip_code': '11201',
       'country': 'US',
       'state': 'NY',
       'display_address': ['19 Old Fulton St', 'Brooklyn, NY 11201']},
      'phone': '+17185966700',
      'display_phone': '(718) 596-6700',
      'distance': 308.56984360837544}]



Write a function `prepare_data` that takes in a list of dictionaries like `businesses` and returns a copy that has been prepared for analysis:

1. The `coordinates` key-value pair has been converted into two separate key-value pairs, `latitude` and `longitude`
2. All other key-value pairs except for `name`, `review_count`, `rating`, and `price` have been dropped
3. All dictionaries missing one of the relevant keys or containing null values have been dropped

In other words, the final keys for each dictionary should be `name`, `review_count`, `rating`, `price`, `latitude`, and `longitude`.

Complete the function in the cell below:


```python
# Replace None with appropriate code

def prepare_data(data_list):
    results = []  
    for business_data in data_list:   
        
        # Make a new dictionary to hold prepared data for this business
        prepared_data = {}        

        # Extract name, review_count, rating, and price key-value pairs
        # from business_data and add to prepared_data
        # If a key is not present in business_data, add it to prepared_data
        # with an associated value of None
        
        for key, value in business_data.items():
            key_list = ['name','review_count','rating','price']
            for key in key_list:
                prepared_data[key] = business_data.get(key)
                

# Found a more efficient way than coding for each key so below code not used

# Was not clear on the instruction:
# 'If a key is not present in business_data, 
# add it to prepared_data with an associated value of None'
# because the filter below removes businesses with values of None anyway
# so added default value of string 'None' to return businesses

#                 prepared_data['name'] = business_data.get('name','None')
#                 prepared_data['review_count'] = business_data.get('review_count','None')
#                 prepared_data['rating'] = business_data.get('rating','None')
#                 prepared_data['price'] = business_data.get('price','None')


                
        # Parse and add latitude and longitude columns
        prepared_data['latitude'] = business_data['coordinates']['latitude']
        prepared_data['longitude'] = business_data['coordinates']['longitude']
                
        # Add to list if all values are present
        if all(prepared_data.values()): 
            results.append(prepared_data) # this removes businesses where key has no value 
            # eg 'price' key does not exist for some businesses
    
    return results

# Test out function
# Adeline removed limiting index range
# Original line: prepared_businesses = prepare_data(full_dataset [:5])
prepared_businesses = prepare_data(full_dataset)
prepared_businesses[:5]
```




    [{'name': 'Prince Street Pizza',
      'review_count': 3936,
      'rating': 4.5,
      'price': '$',
      'latitude': 40.72308755605564,
      'longitude': -73.99453001177575},
     {'name': "Juliana's",
      'review_count': 2339,
      'rating': 4.5,
      'price': '$$',
      'latitude': 40.70274718768062,
      'longitude': -73.99343490196397},
     {'name': "Lombardi's Pizza",
      'review_count': 6179,
      'rating': 4.0,
      'price': '$$',
      'latitude': 40.7215934960083,
      'longitude': -73.9955956044561},
     {'name': 'Lucali',
      'review_count': 1697,
      'rating': 4.0,
      'price': '$$',
      'latitude': 40.6818,
      'longitude': -74.00024},
     {'name': 'Rubirosa',
      'review_count': 2421,
      'rating': 4.5,
      'price': '$$',
      'latitude': 40.722766,
      'longitude': -73.996233}]



Check that your function created the correct keys:


```python
# Run this cell without changes

assert sorted(list(prepared_businesses[0].keys())) == ['latitude', 'longitude', 'name', 'price', 'rating', 'review_count']
```

The following code will differ depending on your query, but there may be fewer results in the prepared list than in the full dataset (if any of them were missing data):


```python
# Run this cell without changes
print("Original:", len(full_dataset))
print("Prepared:", len(prepared_businesses)) 
# Fewer values because excluding businesses with keys that had no value associated
```

    Original: 1000
    Prepared: 789
    

Great! Now let's create a DataFrame to hold our data - this will make our cleaning and analysis easier.


```python
# Replace None with appropriate code

# Import pandas
import pandas as pd

# Create DataFrame from prepared business data
business_df = pd.DataFrame(prepared_businesses)

# Inspect the DataFrame
business_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>review_count</th>
      <th>rating</th>
      <th>price</th>
      <th>latitude</th>
      <th>longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Prince Street Pizza</td>
      <td>3936</td>
      <td>4.5</td>
      <td>$</td>
      <td>40.723088</td>
      <td>-73.994530</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Juliana's</td>
      <td>2339</td>
      <td>4.5</td>
      <td>$$</td>
      <td>40.702747</td>
      <td>-73.993435</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Lombardi's Pizza</td>
      <td>6179</td>
      <td>4.0</td>
      <td>$$</td>
      <td>40.721593</td>
      <td>-73.995596</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Lucali</td>
      <td>1697</td>
      <td>4.0</td>
      <td>$$</td>
      <td>40.681800</td>
      <td>-74.000240</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rubirosa</td>
      <td>2421</td>
      <td>4.5</td>
      <td>$$</td>
      <td>40.722766</td>
      <td>-73.996233</td>
    </tr>
  </tbody>
</table>
</div>



To make analysis of prices easier, let's convert `price` to a numeric value indicating the number of dollar signs.


```python
# Replace None with appropriate code

# Convert price to numeric  
business_df['price'] = (business_df['price']).str.len()
business_df['price'] = pd.to_numeric(business_df['price'])
business_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>name</th>
      <th>review_count</th>
      <th>rating</th>
      <th>price</th>
      <th>latitude</th>
      <th>longitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Prince Street Pizza</td>
      <td>3936</td>
      <td>4.5</td>
      <td>1</td>
      <td>40.723088</td>
      <td>-73.994530</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Juliana's</td>
      <td>2339</td>
      <td>4.5</td>
      <td>2</td>
      <td>40.702747</td>
      <td>-73.993435</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Lombardi's Pizza</td>
      <td>6179</td>
      <td>4.0</td>
      <td>2</td>
      <td>40.721593</td>
      <td>-73.995596</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Lucali</td>
      <td>1697</td>
      <td>4.0</td>
      <td>2</td>
      <td>40.681800</td>
      <td>-74.000240</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rubirosa</td>
      <td>2421</td>
      <td>4.5</td>
      <td>2</td>
      <td>40.722766</td>
      <td>-73.996233</td>
    </tr>
  </tbody>
</table>
</div>



# 4. Perform Descriptive Analysis

## Descriptive Statistics

Take the businesses from the previous question and do an initial descriptive analysis. Calculate summary statistics for the review counts, average rating, and price.


```python
# Replace None with appropriate code

# Calculate summary statistics for the review counts, average rating, and price

# business_df[['review_count','rating','price']].describe()

business_df.agg({'review_count':['mean','median','std'],
                'rating':['mean','median','std'],
                'price':['mean','median','std']})
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>review_count</th>
      <th>rating</th>
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>mean</th>
      <td>449.547529</td>
      <td>3.904943</td>
      <td>1.648923</td>
    </tr>
    <tr>
      <th>median</th>
      <td>239.000000</td>
      <td>4.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>std</th>
      <td>745.356983</td>
      <td>0.454630</td>
      <td>0.624938</td>
    </tr>
  </tbody>
</table>
</div>



Describe the results displayed above and interpret them in the context of your query. (Your answer may differ from the solution branch depending on your query.)


```python
# Replace None with appropriate text
"""
Review Count has a mean that is much larger than the median. 
The large standard deviation also indicates that there are data points that are very far from the mean. 
The median would be the best measure of central tendency here since the mean is more sensitive to outliers.
The results show that most pizza restaurants have a smaller number of reviews, and only a few outlier restaurants 
with very large numbers of reviews.

Rating has its mean and median being very close, and the standard deviation is very small.
This means that most pizza restaurant ratings were 4.0, with the majority of ratings falling between the range 
of 3.5 and 4.5. 

Price is negatively skewed, with the mean being smaller and nearly a standard deviation less than the median.
Most pizza restaurants are priced '$$' or cheaper. 
"""
```




    "\nReview Count has a mean that is much larger than the median. \nThe large standard deviation also indicates that there are data points that are very far from the mean. \nThe median would be the best measure of central tendency here since the mean is more sensitive to outliers.\nThe results show that most pizza restaurants have a smaller number of reviews, and only a few outlier restaurants \nwith very large numbers of reviews.\n\nRating has its mean and median being very close, and the standard deviation is very small.\nThis means that most pizza restaurant ratings were 4.0, with the majority of ratings falling between the range \nof 3.5 and 4.5. \n\nPrice is negatively skewed, with the mean being smaller and nearly a standard deviation less than the median.\nMost pizza restaurants are priced '$$' or cheaper. \n"



## Histograms

Create histograms for the review counts, average rating, and price.


```python
# Replace None with your code
import matplotlib.pyplot as plt
%matplotlib inline

fig, (ax1, ax2, ax3) = plt.subplots(ncols=3, figsize=(16, 5))

# Plot distribution of number of reviews per business
all_review_counts = ax1.hist(business_df['review_count'])

# Plot distribution of ratings across businesses
all_ratings = ax2.hist(business_df['rating'])

# Plot distribution of prices across businesses
all_prices = ax3.hist(business_df['price'])
```


    
![png](output_41_0.png)
    


Describe the distributions displayed above and interpret them in the context of your query. (Your answer may differ from the solution branch depending on your query.)


```python
# Replace None with appropriate text
"""
Review Count is a positively skewed distribution, where the mean tails to the right because of the few outlier restaurants 
with very large numbers of reviews. This aligns with the previous assessment using mean, median and standard deviation.
The results show that more than 750 out of the 789 pizza restaurants have 2000 reviews or less.

Rating appears to be a more normalised, symmetrical distribution as expected from the analysis above that the mean 
and median are very close, and the standard deviation is very small.
The histogram shows that more than half (400 out of 789) of the pizza restaurant ratings was 4.0, with the majority of 
approximately 700 out of 789 ratings falling between the range of 3.5 and 4.5. 

Price is negatively skewed, with the mean being smaller and nearly a standard deviation less than the median.
Almost 750 out of 789 pizza restaurants were priced between '$' and '$$', with only a few expensive '$$$$' restaurants. 
"""
```




    "\nReview Count is a positively skewed distribution, where the mean tails to the right because of the few outlier restaurants \nwith very large numbers of reviews. This aligns with the previous assessment using mean, median and standard deviation.\nThe results show that more than 750 out of the 789 pizza restaurants have 2000 reviews or less.\n\nRating appears to be a more normalised, symmetrical distribution as expected from the analysis above that the mean \nand median are very close, and the standard deviation is very small.\nThe histogram shows that more than half (400 out of 789) of the pizza restaurant ratings was 4.0, with the majority of \napproximately 700 out of 789 ratings falling between the range of 3.5 and 4.5. \n\nPrice is negatively skewed, with the mean being smaller and nearly a standard deviation less than the median.\nAlmost 750 out of 789 pizza restaurants were priced between '$' and '$$', with only a few expensive '$$$$' restaurants. \n"



## Ratings vs. Price

Create a visualization showing the relationship between rating and price. You can do this a few different ways - one option could be to show the average price at each rating using a bar chart.


```python
price_rating = business_df.groupby('rating')['price'].mean()
price_rating
```




    rating
    2.0    1.500000
    2.5    1.333333
    3.0    1.355556
    3.5    1.486339
    4.0    1.763636
    4.5    1.662420
    5.0    1.800000
    Name: price, dtype: float64




```python
# Replace None with your code

# Calculate average price for each rating
price_rating = business_df.groupby('rating')['price'].mean()

# Plot results

# Create the plot
fig, ax = plt.subplots()

# Plot both sets of data
ax.plot(price_rating)

# Add labels for x and y axes
ax.set_xlabel('Rating')
ax.set_ylabel('Average Price')

# Add a title for the plot
ax.set_title('Average Price by Rating')
```




    Text(0.5, 1.0, 'Average Price by Rating')




    
![png](output_46_1.png)
    


Is a higher price associated with a higher rating? (No need for any additional math/statistics, just interpret what you see in the plot.)


```python
# Replace None with appropriate text
"""
Yes, a higher priced pizza generally received a higher rating. 
The cheapest pizzas received a rating of 2.5 whilst the more expensive pizzas recieved a rating of 4.0 or higher.
"""
```




    '\nYes, a higher priced pizza generally received a higher rating. \nThe cheapest pizzas received a rating of 2.5 whilst the more expensive pizzas recieved a rating of 4.0 or higher.\n'



## Ratings vs Review Counts

Finally, let's look at ratings vs. review counts. You can analyze this relationship similarly to ratings vs. price.


```python
# Replace None with your code

# Calculate average review count for each rating
rating_review_count = business_df.groupby('rating').agg({'review_count': 'mean'})

# Plot results
fig, ax = plt.subplots()
ax.bar(x = list(rating_review_count.index), height = list(rating_review_count['review_count']), width = 0.3)
ax.set_xlabel("Average Rating")
ax.set_ylabel("Average Review Count")
```




    Text(0, 0.5, 'Average Review Count')




    
![png](output_50_1.png)
    


Is a higher number of reviews associated with a higher rating?


```python
# Replace None with appropriate text
"""
No, a high number of reviews did not necessarily mean the pizza restaurant received a higher rating.
Whilst the restaurants who received a 4.0 rating from an average of 600 reviews, the restaurants who received a rating 
of 2.5 and 5.0 had a similar review count average. If the number of reviews was linked to rating, the rating of 5.0 would 
have a higher number of reviews. 
"""
```




    '\nNo, a high number of reviews did not necessarily mean the pizza restaurant received a higher rating.\nWhilst the restaurants who received a 4.0 rating from an average of 600 reviews, the restaurants who received a rating \nof 2.5 and 5.0 had a similar review count average. If the number of reviews was linked to rating, the rating of 5.0 would \nhave a higher number of reviews. \n'



## Level Up: Create a Folium Map

Make a map using Folium of the businesses you retrieved. Be sure to also add popups to the markers giving some basic information such as name, rating and price.

You can center the map around the latitude and longitude of the first item in your dataset.


```python
full_dataset[17].get('price','None')
```




    'None'




```python
# Replace None with appropriate code

# Import the library
import folium

# Set up center latitude and longitude
centre_lat = full_dataset[0]['coordinates']['latitude']
centre_long = full_dataset[0]['coordinates']['longitude']

# Initialize map with center lat and long
yelp_map = folium.Map([centre_lat, centre_long], zoom_start=12)

# Adjust this limit to see more or fewer businesses
limit = 20

for business in full_dataset[:limit]:
    # Extract information about business
    lat = business['coordinates']['latitude']
    long = business['coordinates']['longitude']
    # Baby Luc's [17], Frankie and Vali's [12] are examples of names needing to be fixed for char error in popup
    name = business['name'].replace("’","'") 
    rating = business.get('rating')
    price = business.get('price')
    details = "Name:{} \n Price: {} \n Rating:{}".format(name,price,rating)
    
    # Create popup with relevant details
    popup = folium.Popup(details)
    
    # Create marker with relevant lat/long and popup
    marker = folium.Marker(location=[lat, long], popup=popup)
    
    marker.add_to(yelp_map)
    
yelp_map
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe src="about:blank" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" data-html=PCFET0NUWVBFIGh0bWw+CjxoZWFkPiAgICAKICAgIDxtZXRhIGh0dHAtZXF1aXY9ImNvbnRlbnQtdHlwZSIgY29udGVudD0idGV4dC9odG1sOyBjaGFyc2V0PVVURi04IiAvPgogICAgCiAgICAgICAgPHNjcmlwdD4KICAgICAgICAgICAgTF9OT19UT1VDSCA9IGZhbHNlOwogICAgICAgICAgICBMX0RJU0FCTEVfM0QgPSBmYWxzZTsKICAgICAgICA8L3NjcmlwdD4KICAgIAogICAgPHNjcmlwdCBzcmM9Imh0dHBzOi8vY2RuLmpzZGVsaXZyLm5ldC9ucG0vbGVhZmxldEAxLjYuMC9kaXN0L2xlYWZsZXQuanMiPjwvc2NyaXB0PgogICAgPHNjcmlwdCBzcmM9Imh0dHBzOi8vY29kZS5qcXVlcnkuY29tL2pxdWVyeS0xLjEyLjQubWluLmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9qcy9ib290c3RyYXAubWluLmpzIj48L3NjcmlwdD4KICAgIDxzY3JpcHQgc3JjPSJodHRwczovL2NkbmpzLmNsb3VkZmxhcmUuY29tL2FqYXgvbGlicy9MZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy8yLjAuMi9sZWFmbGV0LmF3ZXNvbWUtbWFya2Vycy5qcyI+PC9zY3JpcHQ+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vY2RuLmpzZGVsaXZyLm5ldC9ucG0vbGVhZmxldEAxLjYuMC9kaXN0L2xlYWZsZXQuY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vbWF4Y2RuLmJvb3RzdHJhcGNkbi5jb20vYm9vdHN0cmFwLzMuMi4wL2Nzcy9ib290c3RyYXAubWluLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL21heGNkbi5ib290c3RyYXBjZG4uY29tL2Jvb3RzdHJhcC8zLjIuMC9jc3MvYm9vdHN0cmFwLXRoZW1lLm1pbi5jc3MiLz4KICAgIDxsaW5rIHJlbD0ic3R5bGVzaGVldCIgaHJlZj0iaHR0cHM6Ly9tYXhjZG4uYm9vdHN0cmFwY2RuLmNvbS9mb250LWF3ZXNvbWUvNC42LjMvY3NzL2ZvbnQtYXdlc29tZS5taW4uY3NzIi8+CiAgICA8bGluayByZWw9InN0eWxlc2hlZXQiIGhyZWY9Imh0dHBzOi8vY2RuanMuY2xvdWRmbGFyZS5jb20vYWpheC9saWJzL0xlYWZsZXQuYXdlc29tZS1tYXJrZXJzLzIuMC4yL2xlYWZsZXQuYXdlc29tZS1tYXJrZXJzLmNzcyIvPgogICAgPGxpbmsgcmVsPSJzdHlsZXNoZWV0IiBocmVmPSJodHRwczovL3Jhd2Nkbi5naXRoYWNrLmNvbS9weXRob24tdmlzdWFsaXphdGlvbi9mb2xpdW0vbWFzdGVyL2ZvbGl1bS90ZW1wbGF0ZXMvbGVhZmxldC5hd2Vzb21lLnJvdGF0ZS5jc3MiLz4KICAgIDxzdHlsZT5odG1sLCBib2R5IHt3aWR0aDogMTAwJTtoZWlnaHQ6IDEwMCU7bWFyZ2luOiAwO3BhZGRpbmc6IDA7fTwvc3R5bGU+CiAgICA8c3R5bGU+I21hcCB7cG9zaXRpb246YWJzb2x1dGU7dG9wOjA7Ym90dG9tOjA7cmlnaHQ6MDtsZWZ0OjA7fTwvc3R5bGU+CiAgICAKICAgICAgICAgICAgPG1ldGEgbmFtZT0idmlld3BvcnQiIGNvbnRlbnQ9IndpZHRoPWRldmljZS13aWR0aCwKICAgICAgICAgICAgICAgIGluaXRpYWwtc2NhbGU9MS4wLCBtYXhpbXVtLXNjYWxlPTEuMCwgdXNlci1zY2FsYWJsZT1ubyIgLz4KICAgICAgICAgICAgPHN0eWxlPgogICAgICAgICAgICAgICAgI21hcF8xOWE3N2M3YmIzYmY0NzI5OWI4YjQwODYzYjYzZGQ5NSB7CiAgICAgICAgICAgICAgICAgICAgcG9zaXRpb246IHJlbGF0aXZlOwogICAgICAgICAgICAgICAgICAgIHdpZHRoOiAxMDAuMCU7CiAgICAgICAgICAgICAgICAgICAgaGVpZ2h0OiAxMDAuMCU7CiAgICAgICAgICAgICAgICAgICAgbGVmdDogMC4wJTsKICAgICAgICAgICAgICAgICAgICB0b3A6IDAuMCU7CiAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgIDwvc3R5bGU+CiAgICAgICAgCjwvaGVhZD4KPGJvZHk+ICAgIAogICAgCiAgICAgICAgICAgIDxkaXYgY2xhc3M9ImZvbGl1bS1tYXAiIGlkPSJtYXBfMTlhNzdjN2JiM2JmNDcyOTliOGI0MDg2M2I2M2RkOTUiID48L2Rpdj4KICAgICAgICAKPC9ib2R5Pgo8c2NyaXB0PiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFwXzE5YTc3YzdiYjNiZjQ3Mjk5YjhiNDA4NjNiNjNkZDk1ID0gTC5tYXAoCiAgICAgICAgICAgICAgICAibWFwXzE5YTc3YzdiYjNiZjQ3Mjk5YjhiNDA4NjNiNjNkZDk1IiwKICAgICAgICAgICAgICAgIHsKICAgICAgICAgICAgICAgICAgICBjZW50ZXI6IFs0MC43MjMwODc1NTYwNTU2NCwgLTczLjk5NDUzMDAxMTc3NTc1XSwKICAgICAgICAgICAgICAgICAgICBjcnM6IEwuQ1JTLkVQU0czODU3LAogICAgICAgICAgICAgICAgICAgIHpvb206IDEyLAogICAgICAgICAgICAgICAgICAgIHpvb21Db250cm9sOiB0cnVlLAogICAgICAgICAgICAgICAgICAgIHByZWZlckNhbnZhczogZmFsc2UsCiAgICAgICAgICAgICAgICB9CiAgICAgICAgICAgICk7CgogICAgICAgICAgICAKCiAgICAgICAgCiAgICAKICAgICAgICAgICAgdmFyIHRpbGVfbGF5ZXJfMTUzMTQ4MGU4YWM5NDliNTkzYTU2YWYxNGNlZTk2ZDUgPSBMLnRpbGVMYXllcigKICAgICAgICAgICAgICAgICJodHRwczovL3tzfS50aWxlLm9wZW5zdHJlZXRtYXAub3JnL3t6fS97eH0ve3l9LnBuZyIsCiAgICAgICAgICAgICAgICB7ImF0dHJpYnV0aW9uIjogIkRhdGEgYnkgXHUwMDI2Y29weTsgXHUwMDNjYSBocmVmPVwiaHR0cDovL29wZW5zdHJlZXRtYXAub3JnXCJcdTAwM2VPcGVuU3RyZWV0TWFwXHUwMDNjL2FcdTAwM2UsIHVuZGVyIFx1MDAzY2EgaHJlZj1cImh0dHA6Ly93d3cub3BlbnN0cmVldG1hcC5vcmcvY29weXJpZ2h0XCJcdTAwM2VPRGJMXHUwMDNjL2FcdTAwM2UuIiwgImRldGVjdFJldGluYSI6IGZhbHNlLCAibWF4TmF0aXZlWm9vbSI6IDE4LCAibWF4Wm9vbSI6IDE4LCAibWluWm9vbSI6IDAsICJub1dyYXAiOiBmYWxzZSwgIm9wYWNpdHkiOiAxLCAic3ViZG9tYWlucyI6ICJhYmMiLCAidG1zIjogZmFsc2V9CiAgICAgICAgICAgICkuYWRkVG8obWFwXzE5YTc3YzdiYjNiZjQ3Mjk5YjhiNDA4NjNiNjNkZDk1KTsKICAgICAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2M2ZmNhNDM3OWMxOTRkNjNiMWEzNTNhYTJkOTg1YTgyID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzIzMDg3NTU2MDU1NjQsIC03My45OTQ1MzAwMTE3NzU3NV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF8xOWE3N2M3YmIzYmY0NzI5OWI4YjQwODYzYjYzZGQ5NSk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfM2FmMjkzZmQyYjk3NDJiNzkyMjllMDk5YWZkOTBiMGYgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzA2Y2I4MTFjYzFjNDRkOTdhNTY5MmEzZGNjZWEzZjY4ID0gJChgPGRpdiBpZD0iaHRtbF8wNmNiODExY2MxYzQ0ZDk3YTU2OTJhM2RjY2VhM2Y2OCIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TmFtZTpQcmluY2UgU3RyZWV0IFBpenphICAgUHJpY2U6ICQgICBSYXRpbmc6NC41PC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzNhZjI5M2ZkMmI5NzQyYjc5MjI5ZTA5OWFmZDkwYjBmLnNldENvbnRlbnQoaHRtbF8wNmNiODExY2MxYzQ0ZDk3YTU2OTJhM2RjY2VhM2Y2OCk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9jNmZjYTQzNzljMTk0ZDYzYjFhMzUzYWEyZDk4NWE4Mi5iaW5kUG9wdXAocG9wdXBfM2FmMjkzZmQyYjk3NDJiNzkyMjllMDk5YWZkOTBiMGYpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfMjY1YzZmNWFkNjQzNDc1Y2FmZjI5MWE1MDM3MGVkMTUgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43MDI3NDcxODc2ODA2MiwgLTczLjk5MzQzNDkwMTk2Mzk3XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwXzE5YTc3YzdiYjNiZjQ3Mjk5YjhiNDA4NjNiNjNkZDk1KTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9kOWU5MDE1OWJhNzM0YTNjYjU5NWYwMDRiNTY0NDUxNiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfM2M5OGRmMjc1M2NlNGVmNWI1Mjg4N2MyZjMwNDA1NmMgPSAkKGA8ZGl2IGlkPSJodG1sXzNjOThkZjI3NTNjZTRlZjViNTI4ODdjMmYzMDQwNTZjIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5OYW1lOkp1bGlhbmEncyAgIFByaWNlOiAkJCAgIFJhdGluZzo0LjU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfZDllOTAxNTliYTczNGEzY2I1OTVmMDA0YjU2NDQ1MTYuc2V0Q29udGVudChodG1sXzNjOThkZjI3NTNjZTRlZjViNTI4ODdjMmYzMDQwNTZjKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzI2NWM2ZjVhZDY0MzQ3NWNhZmYyOTFhNTAzNzBlZDE1LmJpbmRQb3B1cChwb3B1cF9kOWU5MDE1OWJhNzM0YTNjYjU5NWYwMDRiNTY0NDUxNikKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl9kZTM3YTI5MzJkOGQ0Zjc5YmFkMTI4ZTNhMzQ2NjBhYiA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcyMTU5MzQ5NjAwODMsIC03My45OTU1OTU2MDQ0NTYxXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwXzE5YTc3YzdiYjNiZjQ3Mjk5YjhiNDA4NjNiNjNkZDk1KTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF80MTYzNjQ1NTI1NmE0NDUxYmNkYTAwYzZmOTBiNWViMyA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfZGEzMzUxYjZmOTc1NDkzYmFhZGM4MTc3OGIzZjYzYTAgPSAkKGA8ZGl2IGlkPSJodG1sX2RhMzM1MWI2Zjk3NTQ5M2JhYWRjODE3NzhiM2Y2M2EwIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5OYW1lOkxvbWJhcmRpJ3MgUGl6emEgICBQcmljZTogJCQgICBSYXRpbmc6NC4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzQxNjM2NDU1MjU2YTQ0NTFiY2RhMDBjNmY5MGI1ZWIzLnNldENvbnRlbnQoaHRtbF9kYTMzNTFiNmY5NzU0OTNiYWFkYzgxNzc4YjNmNjNhMCk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl9kZTM3YTI5MzJkOGQ0Zjc5YmFkMTI4ZTNhMzQ2NjBhYi5iaW5kUG9wdXAocG9wdXBfNDE2MzY0NTUyNTZhNDQ1MWJjZGEwMGM2ZjkwYjVlYjMpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfNWRkYjZhZjU1OGIxNDgzMjlhMjg5Zjc2NWNjMjAzOTkgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC42ODE4LCAtNzQuMDAwMjRdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfMTlhNzdjN2JiM2JmNDcyOTliOGI0MDg2M2I2M2RkOTUpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzczZDRkZWEyMjI2YzQ4ZTdhOTFjM2I3YjM2YjYyZDZhID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF80OTZjYzdhZWRhYzA0NDYzYjQ1NGVjMGE3N2UxOTc4NyA9ICQoYDxkaXYgaWQ9Imh0bWxfNDk2Y2M3YWVkYWMwNDQ2M2I0NTRlYzBhNzdlMTk3ODciIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5hbWU6THVjYWxpICAgUHJpY2U6ICQkICAgUmF0aW5nOjQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF83M2Q0ZGVhMjIyNmM0OGU3YTkxYzNiN2IzNmI2MmQ2YS5zZXRDb250ZW50KGh0bWxfNDk2Y2M3YWVkYWMwNDQ2M2I0NTRlYzBhNzdlMTk3ODcpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNWRkYjZhZjU1OGIxNDgzMjlhMjg5Zjc2NWNjMjAzOTkuYmluZFBvcHVwKHBvcHVwXzczZDRkZWEyMjI2YzQ4ZTdhOTFjM2I3YjM2YjYyZDZhKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2NmNzFjMGM1ZjgzYjRmNTRiMzVlYTkxOWJmMjBiOWE4ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzIyNzY2LCAtNzMuOTk2MjMzXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwXzE5YTc3YzdiYjNiZjQ3Mjk5YjhiNDA4NjNiNjNkZDk1KTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8wNWIxMWMyYTFmNTQ0ZDNmOGQwNmUzZmVkNDRkMzk3ZiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNWIyYmYyZDA3ZTEyNGY3MzgxZTg5YjEyZDRjNjcwZTIgPSAkKGA8ZGl2IGlkPSJodG1sXzViMmJmMmQwN2UxMjRmNzM4MWU4OWIxMmQ0YzY3MGUyIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5OYW1lOlJ1Ymlyb3NhICAgUHJpY2U6ICQkICAgUmF0aW5nOjQuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8wNWIxMWMyYTFmNTQ0ZDNmOGQwNmUzZmVkNDRkMzk3Zi5zZXRDb250ZW50KGh0bWxfNWIyYmYyZDA3ZTEyNGY3MzgxZTg5YjEyZDRjNjcwZTIpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfY2Y3MWMwYzVmODNiNGY1NGIzNWVhOTE5YmYyMGI5YTguYmluZFBvcHVwKHBvcHVwXzA1YjExYzJhMWY1NDRkM2Y4ZDA2ZTNmZWQ0NGQzOTdmKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzRiNGNiM2YyYzQ3MTQ2OTliZWE4ODAwNzFjMzAzMGQ0ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzMwNTU1MjAzNTUxOTgsIC03NC4wMDIxMTYzMjM3NzkyOV0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF8xOWE3N2M3YmIzYmY0NzI5OWI4YjQwODYzYjYzZGQ5NSk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfNzY4NzAxYTllNzg4NGQyNWE2OTA0MjliMTRlNWRlYTQgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2Q3Y2E0ZjA5MGI2MDQzMjM5MWJiODc4YjIxMjk2ZDVjID0gJChgPGRpdiBpZD0iaHRtbF9kN2NhNGYwOTBiNjA0MzIzOTFiYjg3OGIyMTI5NmQ1YyIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TmFtZTpKb2UncyBQaXp6YSAgIFByaWNlOiAkICAgUmF0aW5nOjQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF83Njg3MDFhOWU3ODg0ZDI1YTY5MDQyOWIxNGU1ZGVhNC5zZXRDb250ZW50KGh0bWxfZDdjYTRmMDkwYjYwNDMyMzkxYmI4NzhiMjEyOTZkNWMpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfNGI0Y2IzZjJjNDcxNDY5OWJlYTg4MDA3MWMzMDMwZDQuYmluZFBvcHVwKHBvcHVwXzc2ODcwMWE5ZTc4ODRkMjVhNjkwNDI5YjE0ZTVkZWE0KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2NhZTM3NmRjYWQzZTQyOGY4M2M4MmQ0NTEyMDdhZGVmID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzI5NTQ2LCAtNzMuOTU4NTY4XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwXzE5YTc3YzdiYjNiZjQ3Mjk5YjhiNDA4NjNiNjNkZDk1KTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF85ZDkyNzZkNTM3M2U0ZGQ2OGE3YWFmMTU4M2ZiMDgyMiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfMTQ0Yjg5MWRjM2ZmNDVmOGFhOTNhODVlZmNkYTMxMDAgPSAkKGA8ZGl2IGlkPSJodG1sXzE0NGI4OTFkYzNmZjQ1ZjhhYTkzYTg1ZWZjZGEzMTAwIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5OYW1lOlBhdWxpZSBHZWUncyAgIFByaWNlOiAkJCAgIFJhdGluZzo0LjU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfOWQ5Mjc2ZDUzNzNlNGRkNjhhN2FhZjE1ODNmYjA4MjIuc2V0Q29udGVudChodG1sXzE0NGI4OTFkYzNmZjQ1ZjhhYTkzYTg1ZWZjZGEzMTAwKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyX2NhZTM3NmRjYWQzZTQyOGY4M2M4MmQ0NTEyMDdhZGVmLmJpbmRQb3B1cChwb3B1cF85ZDkyNzZkNTM3M2U0ZGQ2OGE3YWFmMTU4M2ZiMDgyMikKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl83NTU3YzdhNzUyNzM0ZmNmODI5MjkwZTY4MTIwOTExMCA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcxMTYyLCAtNzMuOTU3ODNdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfMTlhNzdjN2JiM2JmNDcyOTliOGI0MDg2M2I2M2RkOTUpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzJjZDA3MWZhOWVhNzQ1ZTY5ODljOTJiYjRiYTNmNTAyID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9jYTA2NjM2ZGZkYjY0NjYzYWJmNGEwMjk0MDljNmUwZCA9ICQoYDxkaXYgaWQ9Imh0bWxfY2EwNjYzNmRmZGI2NDY2M2FiZjRhMDI5NDA5YzZlMGQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5hbWU6TCdpbmR1c3RyaWUgUGl6emVyaWEgICBQcmljZTogJCAgIFJhdGluZzo0LjU8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfMmNkMDcxZmE5ZWE3NDVlNjk4OWM5MmJiNGJhM2Y1MDIuc2V0Q29udGVudChodG1sX2NhMDY2MzZkZmRiNjQ2NjNhYmY0YTAyOTQwOWM2ZTBkKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzc1NTdjN2E3NTI3MzRmY2Y4MjkyOTBlNjgxMjA5MTEwLmJpbmRQb3B1cChwb3B1cF8yY2QwNzFmYTllYTc0NWU2OTg5YzkyYmI0YmEzZjUwMikKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl8wMGE1OTMwMmVjMDI0NTU4ODUzZTMzZTNlY2QxMzVhYyA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjcwNDkzLCAtNzMuOTMzOTldLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfMTlhNzdjN2JiM2JmNDcyOTliOGI0MDg2M2I2M2RkOTUpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2FlNzViMWFjNmNlNTRlZDVhOGRkNWU1YWJjMWQ4NzdjID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF8wM2Y0ZmE1N2QwNmU0YzFlYmU2NDc2ZWFlYTA5ZmEwMiA9ICQoYDxkaXYgaWQ9Imh0bWxfMDNmNGZhNTdkMDZlNGMxZWJlNjQ3NmVhZWEwOWZhMDIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5hbWU6Um9iZXJ0YSdzICAgUHJpY2U6ICQkICAgUmF0aW5nOjQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9hZTc1YjFhYzZjZTU0ZWQ1YThkZDVlNWFiYzFkODc3Yy5zZXRDb250ZW50KGh0bWxfMDNmNGZhNTdkMDZlNGMxZWJlNjQ3NmVhZWEwOWZhMDIpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfMDBhNTkzMDJlYzAyNDU1ODg1M2UzM2UzZWNkMTM1YWMuYmluZFBvcHVwKHBvcHVwX2FlNzViMWFjNmNlNTRlZDVhOGRkNWU1YWJjMWQ4NzdjKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2Q3YTRmNmU2NzM3YzQzMTViY2Y0NmZjMDk2YmIwMTcyID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzMxNTgsIC03NC4wMDMzMl0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF8xOWE3N2M3YmIzYmY0NzI5OWI4YjQwODYzYjYzZGQ5NSk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfNTAyN2ZkYmZjNDAxNGNjZjg2NWMzNDA4OTJjZjFkZjQgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzJkNDljZjgzZWVjMTQ3NDU5NTJkMGVhOTZjMzE3MGI2ID0gJChgPGRpdiBpZD0iaHRtbF8yZDQ5Y2Y4M2VlYzE0NzQ1OTUyZDBlYTk2YzMxNzBiNiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TmFtZTpKb2huJ3Mgb2YgQmxlZWNrZXIgU3RyZWV0ICAgUHJpY2U6ICQkICAgUmF0aW5nOjQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF81MDI3ZmRiZmM0MDE0Y2NmODY1YzM0MDg5MmNmMWRmNC5zZXRDb250ZW50KGh0bWxfMmQ0OWNmODNlZWMxNDc0NTk1MmQwZWE5NmMzMTcwYjYpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfZDdhNGY2ZTY3MzdjNDMxNWJjZjQ2ZmMwOTZiYjAxNzIuYmluZFBvcHVwKHBvcHVwXzUwMjdmZGJmYzQwMTRjY2Y4NjVjMzQwODkyY2YxZGY0KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2U3NjYzN2Q5ZDg2YTQ0MmNiZTQ4MmU1OGU2M2NkM2JhID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzAyNTgzLCAtNzMuOTkzMjQxM10sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF8xOWE3N2M3YmIzYmY0NzI5OWI4YjQwODYzYjYzZGQ5NSk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfY2M1ODhiNzlkNzk2NDYyZTgzZDUwZDdiMGJhYmUzNWYgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzE2MDZlNDgzN2MzMjRlNTZhM2VhZDc4MGY3ZTBjMTliID0gJChgPGRpdiBpZD0iaHRtbF8xNjA2ZTQ4MzdjMzI0ZTU2YTNlYWQ3ODBmN2UwYzE5YiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TmFtZTpHcmltYWxkaSdzIFBpenplcmlhICAgUHJpY2U6ICQkICAgUmF0aW5nOjMuNTwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9jYzU4OGI3OWQ3OTY0NjJlODNkNTBkN2IwYmFiZTM1Zi5zZXRDb250ZW50KGh0bWxfMTYwNmU0ODM3YzMyNGU1NmEzZWFkNzgwZjdlMGMxOWIpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfZTc2NjM3ZDlkODZhNDQyY2JlNDgyZTU4ZTYzY2QzYmEuYmluZFBvcHVwKHBvcHVwX2NjNTg4Yjc5ZDc5NjQ2MmU4M2Q1MGQ3YjBiYWJlMzVmKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzZhNjAzZDMwMGJkZDQ2OTRhMjQ0N2IwYTBkYjZmNTY0ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNjg3NTIsIC03My45NTQ0Ml0sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF8xOWE3N2M3YmIzYmY0NzI5OWI4YjQwODYzYjYzZGQ5NSk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfYjY2ZGE4MjU4YzBkNDIyNmI1Zjc3NTg4NjRkZTc2ZGYgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sX2M2N2Y5YjFlM2I5ZDQ4NjQ4NTkwYmFkZWJiZmZhYWViID0gJChgPGRpdiBpZD0iaHRtbF9jNjdmOWIxZTNiOWQ0ODY0ODU5MGJhZGViYmZmYWFlYiIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TmFtZTpGcmFua2llIGFuZCBWYWxpJ3MgICBQcmljZTogTm9uZSAgIFJhdGluZzo1LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfYjY2ZGE4MjU4YzBkNDIyNmI1Zjc3NTg4NjRkZTc2ZGYuc2V0Q29udGVudChodG1sX2M2N2Y5YjFlM2I5ZDQ4NjQ4NTkwYmFkZWJiZmZhYWViKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzZhNjAzZDMwMGJkZDQ2OTRhMjQ0N2IwYTBkYjZmNTY0LmJpbmRQb3B1cChwb3B1cF9iNjZkYTgyNThjMGQ0MjI2YjVmNzc1ODg2NGRlNzZkZikKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl8zMjE0ZTJjYTIxYzI0MjZhYTg5YmM2NmJmOGZjN2FiYSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjYzNDc0MSwgLTc0LjAyODQyNTldLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfMTlhNzdjN2JiM2JmNDcyOTliOGI0MDg2M2I2M2RkOTUpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzZmYzQ2MzA0YzA1YjQwMjM4MjA1MzAzMzUzNjJjNjhkID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF83ODY0NDhiYzZmM2M0MWZjODZiNWM1ZGVhM2FjYjkxYyA9ICQoYDxkaXYgaWQ9Imh0bWxfNzg2NDQ4YmM2ZjNjNDFmYzg2YjVjNWRlYTNhY2I5MWMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5hbWU6TG9tYmFyZG8ncyBvZiBCYXkgUmlkZ2UgICBQcmljZTogJCQgICBSYXRpbmc6NS4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzZmYzQ2MzA0YzA1YjQwMjM4MjA1MzAzMzUzNjJjNjhkLnNldENvbnRlbnQoaHRtbF83ODY0NDhiYzZmM2M0MWZjODZiNWM1ZGVhM2FjYjkxYyk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl8zMjE0ZTJjYTIxYzI0MjZhYTg5YmM2NmJmOGZjN2FiYS5iaW5kUG9wdXAocG9wdXBfNmZjNDYzMDRjMDViNDAyMzgyMDUzMDMzNTM2MmM2OGQpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfMTYwODMzOThlOWViNDRhMjg5NzljYTllZDJkYTFhN2UgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC42MjUwOTMsIC03My45NjE1MzFdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfMTlhNzdjN2JiM2JmNDcyOTliOGI0MDg2M2I2M2RkOTUpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwX2NhMWYyNDMwYzU2MTQxNzY4OTgzZTUwOWJhNzAyNzVjID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF8xOGZlOGU0NTYxYmM0ZjA3YmUyM2RlNWE0NWZmODEwMiA9ICQoYDxkaXYgaWQ9Imh0bWxfMThmZThlNDU2MWJjNGYwN2JlMjNkZTVhNDVmZjgxMDIiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5hbWU6RGkgRmFyYSBQaXp6YSAgIFByaWNlOiAkJCAgIFJhdGluZzo0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfY2ExZjI0MzBjNTYxNDE3Njg5ODNlNTA5YmE3MDI3NWMuc2V0Q29udGVudChodG1sXzE4ZmU4ZTQ1NjFiYzRmMDdiZTIzZGU1YTQ1ZmY4MTAyKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzE2MDgzMzk4ZTllYjQ0YTI4OTc5Y2E5ZWQyZGExYTdlLmJpbmRQb3B1cChwb3B1cF9jYTFmMjQzMGM1NjE0MTc2ODk4M2U1MDliYTcwMjc1YykKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl85ODQwNDIyMzBkYjQ0Yzc3YjY0ZjJmYTQ1MDc3N2Y1ZSA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjczMzMxLCAtNzMuOTg3NjNdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfMTlhNzdjN2JiM2JmNDcyOTliOGI0MDg2M2I2M2RkOTUpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzI5ZGVlMDQ3OTdjNjQwMzFiMGEzMTE4N2M0MTlmODU4ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9iZjc4YWE1MzA3MmQ0MmU2YmM5NDRlMzVhYTU5Y2IzMyA9ICQoYDxkaXYgaWQ9Imh0bWxfYmY3OGFhNTMwNzJkNDJlNmJjOTQ0ZTM1YWE1OWNiMzMiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5hbWU6Sm9lJ3MgUGl6emEgICBQcmljZTogJCAgIFJhdGluZzo0LjA8L2Rpdj5gKVswXTsKICAgICAgICAgICAgcG9wdXBfMjlkZWUwNDc5N2M2NDAzMWIwYTMxMTg3YzQxOWY4NTguc2V0Q29udGVudChodG1sX2JmNzhhYTUzMDcyZDQyZTZiYzk0NGUzNWFhNTljYjMzKTsKICAgICAgICAKCiAgICAgICAgbWFya2VyXzk4NDA0MjIzMGRiNDRjNzdiNjRmMmZhNDUwNzc3ZjVlLmJpbmRQb3B1cChwb3B1cF8yOWRlZTA0Nzk3YzY0MDMxYjBhMzExODdjNDE5Zjg1OCkKICAgICAgICA7CgogICAgICAgIAogICAgCiAgICAKICAgICAgICAgICAgdmFyIG1hcmtlcl8wOWNlZGIzNGY4Mzk0YTRiYWE1ZDAxMDcxNjBjNTgzYiA9IEwubWFya2VyKAogICAgICAgICAgICAgICAgWzQwLjczMjA2MjcwMTY1MTIsIC03NC4wMDM2NTUyMjcwMTM3XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwXzE5YTc3YzdiYjNiZjQ3Mjk5YjhiNDA4NjNiNjNkZDk1KTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF8yMjhlNmE3ZjBmODk0NDU0OTJlNTc0YjQxNWYzYzFjYyA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfZTJiZTIxMGI0NTRmNDgwYjg4NzEwZWJjZjlkMGQyZGIgPSAkKGA8ZGl2IGlkPSJodG1sX2UyYmUyMTBiNDU0ZjQ4MGI4ODcxMGViY2Y5ZDBkMmRiIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5OYW1lOkJsZWVja2VyIFN0cmVldCBQaXp6YSAgIFByaWNlOiAkICAgUmF0aW5nOjQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8yMjhlNmE3ZjBmODk0NDU0OTJlNTc0YjQxNWYzYzFjYy5zZXRDb250ZW50KGh0bWxfZTJiZTIxMGI0NTRmNDgwYjg4NzEwZWJjZjlkMGQyZGIpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfMDljZWRiMzRmODM5NGE0YmFhNWQwMTA3MTYwYzU4M2IuYmluZFBvcHVwKHBvcHVwXzIyOGU2YTdmMGY4OTQ0NTQ5MmU1NzRiNDE1ZjNjMWNjKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzFiN2NhNjNkY2EwMTRmN2ZhN2Y3ZTE0MDEwMWU1YzcyID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNjgwMTAwNjM0NTAxMTcsIC03My45OTY4NDI3MDU5NjQzM10sCiAgICAgICAgICAgICAgICB7fQogICAgICAgICAgICApLmFkZFRvKG1hcF8xOWE3N2M3YmIzYmY0NzI5OWI4YjQwODYzYjYzZGQ5NSk7CiAgICAgICAgCiAgICAKICAgICAgICB2YXIgcG9wdXBfMGQxMWRlZmFlOTFhNDg4Zjg4ZDVmZjc3OTdlZTg3MGUgPSBMLnBvcHVwKHsibWF4V2lkdGgiOiAiMTAwJSJ9KTsKCiAgICAgICAgCiAgICAgICAgICAgIHZhciBodG1sXzNkZjMyNzU2YTY4MzQ2MDY5NjYwYWEwM2Y1MGYzN2NhID0gJChgPGRpdiBpZD0iaHRtbF8zZGYzMjc1NmE2ODM0NjA2OTY2MGFhMDNmNTBmMzdjYSIgc3R5bGU9IndpZHRoOiAxMDAuMCU7IGhlaWdodDogMTAwLjAlOyI+TmFtZTpCYWJ5IEx1YydzICAgUHJpY2U6IE5vbmUgICBSYXRpbmc6My4wPC9kaXY+YClbMF07CiAgICAgICAgICAgIHBvcHVwXzBkMTFkZWZhZTkxYTQ4OGY4OGQ1ZmY3Nzk3ZWU4NzBlLnNldENvbnRlbnQoaHRtbF8zZGYzMjc1NmE2ODM0NjA2OTY2MGFhMDNmNTBmMzdjYSk7CiAgICAgICAgCgogICAgICAgIG1hcmtlcl8xYjdjYTYzZGNhMDE0ZjdmYTdmN2UxNDAxMDFlNWM3Mi5iaW5kUG9wdXAocG9wdXBfMGQxMWRlZmFlOTFhNDg4Zjg4ZDVmZjc3OTdlZTg3MGUpCiAgICAgICAgOwoKICAgICAgICAKICAgIAogICAgCiAgICAgICAgICAgIHZhciBtYXJrZXJfMGFkNzdmOGZjYTRhNDY1ZGFkZDc3MjdkODUwNTBmOWYgPSBMLm1hcmtlcigKICAgICAgICAgICAgICAgIFs0MC43MTYxODc4ODU0NjA1NywgLTczLjk2NjUxNzU0MzIzNjY5XSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwXzE5YTc3YzdiYjNiZjQ3Mjk5YjhiNDA4NjNiNjNkZDk1KTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF9iYzZiN2U1MTAzNjc0M2M4OTE5NzNmZWZmODA2YTgyYSA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfNWIzMzA3NTAxM2U4NDFkNjlhMGYzMGMyMzBmMzcxMGUgPSAkKGA8ZGl2IGlkPSJodG1sXzViMzMwNzUwMTNlODQxZDY5YTBmMzBjMjMwZjM3MTBlIiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5OYW1lOlJvYmVydGEncyAgIFByaWNlOiBOb25lICAgUmF0aW5nOjQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF9iYzZiN2U1MTAzNjc0M2M4OTE5NzNmZWZmODA2YTgyYS5zZXRDb250ZW50KGh0bWxfNWIzMzA3NTAxM2U4NDFkNjlhMGYzMGMyMzBmMzcxMGUpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfMGFkNzdmOGZjYTRhNDY1ZGFkZDc3MjdkODUwNTBmOWYuYmluZFBvcHVwKHBvcHVwX2JjNmI3ZTUxMDM2NzQzYzg5MTk3M2ZlZmY4MDZhODJhKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyX2IxNmNjMmYyZjkzNDRjNGI4YjIwMmNmMjk5YzA5ZTY4ID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzI4MTk5LCAtNzMuOTg1MTgyXSwKICAgICAgICAgICAgICAgIHt9CiAgICAgICAgICAgICkuYWRkVG8obWFwXzE5YTc3YzdiYjNiZjQ3Mjk5YjhiNDA4NjNiNjNkZDk1KTsKICAgICAgICAKICAgIAogICAgICAgIHZhciBwb3B1cF82OTE2ZDA0NjJhMmY0ZjMxYTU1ZmRiODU2Yzg5Y2QzMiA9IEwucG9wdXAoeyJtYXhXaWR0aCI6ICIxMDAlIn0pOwoKICAgICAgICAKICAgICAgICAgICAgdmFyIGh0bWxfOTNhNGZlMGJjYWFmNDlkZjg4NTU4YmZjOTJmMDBjYjcgPSAkKGA8ZGl2IGlkPSJodG1sXzkzYTRmZTBiY2FhZjQ5ZGY4ODU1OGJmYzkyZjAwY2I3IiBzdHlsZT0id2lkdGg6IDEwMC4wJTsgaGVpZ2h0OiAxMDAuMCU7Ij5OYW1lOkVhc3QgVmlsbGFnZSBQaXp6YSAgIFByaWNlOiAkICAgUmF0aW5nOjQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF82OTE2ZDA0NjJhMmY0ZjMxYTU1ZmRiODU2Yzg5Y2QzMi5zZXRDb250ZW50KGh0bWxfOTNhNGZlMGJjYWFmNDlkZjg4NTU4YmZjOTJmMDBjYjcpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfYjE2Y2MyZjJmOTM0NGM0YjhiMjAyY2YyOTljMDllNjguYmluZFBvcHVwKHBvcHVwXzY5MTZkMDQ2MmEyZjRmMzFhNTVmZGI4NTZjODljZDMyKQogICAgICAgIDsKCiAgICAgICAgCiAgICAKICAgIAogICAgICAgICAgICB2YXIgbWFya2VyXzk3NGZjOTBlYTZiZjQxMDU4YjA2NWY5OTY3NTVjNDRkID0gTC5tYXJrZXIoCiAgICAgICAgICAgICAgICBbNDAuNzQ0NTg4MiwgLTczLjk4NDc5MzZdLAogICAgICAgICAgICAgICAge30KICAgICAgICAgICAgKS5hZGRUbyhtYXBfMTlhNzdjN2JiM2JmNDcyOTliOGI0MDg2M2I2M2RkOTUpOwogICAgICAgIAogICAgCiAgICAgICAgdmFyIHBvcHVwXzM4NGQyOGIwNDIzOTQzMTJiODRkODI2YmZiYTRjYzQ0ID0gTC5wb3B1cCh7Im1heFdpZHRoIjogIjEwMCUifSk7CgogICAgICAgIAogICAgICAgICAgICB2YXIgaHRtbF9kMTA3OTUxZjQ5MmM0OWE2OWE2OWZiZDM2N2JjN2E4NCA9ICQoYDxkaXYgaWQ9Imh0bWxfZDEwNzk1MWY0OTJjNDlhNjlhNjlmYmQzNjdiYzdhODQiIHN0eWxlPSJ3aWR0aDogMTAwLjAlOyBoZWlnaHQ6IDEwMC4wJTsiPk5hbWU6TWFydGEgICBQcmljZTogJCQkICAgUmF0aW5nOjQuMDwvZGl2PmApWzBdOwogICAgICAgICAgICBwb3B1cF8zODRkMjhiMDQyMzk0MzEyYjg0ZDgyNmJmYmE0Y2M0NC5zZXRDb250ZW50KGh0bWxfZDEwNzk1MWY0OTJjNDlhNjlhNjlmYmQzNjdiYzdhODQpOwogICAgICAgIAoKICAgICAgICBtYXJrZXJfOTc0ZmM5MGVhNmJmNDEwNThiMDY1Zjk5Njc1NWM0NGQuYmluZFBvcHVwKHBvcHVwXzM4NGQyOGIwNDIzOTQzMTJiODRkODI2YmZiYTRjYzQ0KQogICAgICAgIDsKCiAgICAgICAgCiAgICAKPC9zY3JpcHQ+ onload="this.contentDocument.open();this.contentDocument.write(atob(this.getAttribute('data-html')));this.contentDocument.close();" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>



# 5. Create Presentation Notebook

Now that you've completed your project, let's put it into an easily presentable format so you can add it to your portfolio. To do this, we recommend completing the following steps outside of this notebook.

1. Create a new GitHub repository for your project.
2. Save a copy of this notebook into your local repository.
3. Edit the text and images in the notebook to present your project and help someone else understand it.
4. Run your notebook from start to finish, then **delete your API key** and save it.
5. Create a README.md file in your repository with a brief summary of your project.
6. Push your updated repository to GitHub to share with your instructor and employers!

# Level Up: Project Enhancements

After completing the project, you could consider the following enhancements if you have time:

* Save a cleaned version of your dataset in your repository, then modify the notebook to load and use that dataset for the analysis. This will allow others to replicate your analysis and avoid running their own API queries.
* Use `seaborn` to improve the styling of your visualizations
* Explain the implications of your findings for business owners or internal Yelp staff
* Repeat the project for another business category or location, and compare the results

# Summary

Nice work! In this lab, you've made multiple API calls to Yelp in order to paginate through a results set, prepared your data for analysis, and then conducted and visualized descriptive analyses. Well done!
