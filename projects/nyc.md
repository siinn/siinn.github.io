---
layout: article
title: NYC Apartment Rent Prediction
---
# NYC Apartment Rent Prediction
> Prediction models are built for apartment rent in NYC using RandomForest and GradientBoosting regression. Two regression models are compared using statistical method. The dataset is collected by web scraping and cleaned using Pandas. Exploratory analysis is presented using Seaborn.

**Table of Contents:**
- Table of Contents
{:toc}

## Introduction
Housing is expansive in big cities like New York and Bay Area, and there are many factors to consider when renting or leasing an apartment. So it is important to come up with a good way to determine apartment rent based on its features such as location, size, and quality of the apartment.

Typically, this is done by human experience and intelligent. In this project, prediction models are built for apartment rent in NYC, and the performances of two regression models are studied.

## Data Collection
Jupyter notebook for this section is available in <boxed><a href="https://github.com/siinn/Data-Science-Portfolio/blob/master/NYC-Rental-Prediction/notebook/data_scrapping.ipynb">Data collection</a></boxed>.

Data are collected by web scraping from [Apartment.com](www.apartment.com), and Python <verbatim>requests</verbatim> and <verbatim>BeautifulSoup</verbatim> libraries are used for web scraping and parsing the data.


Because there are several layers of web pages before we can access the individual apartment listing, first the main page was parsed to obtain the list of urls of apartment listings in the region of interest. Then web pages for individual apartment listing are parsed to extract information about the units.

When making requests, minimum time interval of 0.2 or 1 second is applied in order to avoid conflict with the server policy.

The following information is collected from the website and stored as a DataFrame. Rent is used as a target variable, and others are used as features for regression models.

- Apartment complex name
- Number of bedrooms
- Number of bathrooms
- Size of unit (in sqft)
- Address
- Borough
- Lease length
- Number of bedroom, bathroom, sqft
- Tenant rating
- Pet policy (dog or cat allowed)
- Nearby school
- Built year
- Amenity such as laundry, fitness, business center, etc
- Rent

<br>

The raw output of data collection needs to be further processed as the data is not structured well. For example, number of bedrooms is stored as a string in various format such as "1 BA", "1", or "1Bedroom", and amenities are stored as a Python list of string. The un-cleaned DataFrame is shown in Table 1.

<table_title>Table 1. Un-cleaned DataFrame from Data Collection (Head only)</table_title>
<table_box>
  <table class="dataframe" style="width:2700px">
    <thead>
      <tr style="text-align: right;">
        <th></th>
        <th>amenity</th>
        <th>bathrooms</th>
        <th>bedrooms</th>
        <th>borough</th>
        <th>city</th>
        <th>deposit</th>
        <th>leaseLength</th>
        <th>name</th>
        <th>pet_policy</th>
        <th>postal_code</th>
        <th>property_info</th>
        <th>rating</th>
        <th>rent</th>
        <th>rental_title</th>
        <th>school</th>
        <th>sqft</th>
        <th>state</th>
        <th>street_address</th>
        <th>unit</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th>0</th>
        <td>[Bowling Alley, Cardio Fitness Room, Central A...</td>
        <td>1 BA</td>
        <td>Studio</td>
        <td>manhattan-ny</td>
        <td>New York</td>
        <td></td>
        <td>12 - 24 Month Lease</td>
        <td>Studio/1 Bathroom</td>
        <td>[\n\n\r\n                            Pets Nego...</td>
        <td>10018</td>
        <td>\nProperty Information\n\n•Built in 2017\n•598...</td>
        <td>5 star property</td>
        <td>$3,255</td>
        <td>555TEN, New York, NY</td>
        <td>[{'school_name': 'Public Elementary School', '...</td>
        <td>500 Sq Ft</td>
        <td>NY</td>
        <td>555 10th Ave</td>
        <td></td>
      </tr>
      <tr>
        <th>1</th>
        <td>[Bowling Alley, Cardio Fitness Room, Central A...</td>
        <td>1 BA</td>
        <td>Studio</td>
        <td>manhattan-ny</td>
        <td>New York</td>
        <td></td>
        <td>12 - 24 Month Lease</td>
        <td>Studio/1 Bathroom</td>
        <td>[\n\n\r\n                            Pets Nego...</td>
        <td>10018</td>
        <td>\nProperty Information\n\n•Built in 2017\n•598...</td>
        <td>5 star property</td>
        <td>$3,255</td>
        <td>555TEN, New York, NY</td>
        <td>[{'school_name': 'Public Elementary School', '...</td>
        <td></td>
        <td>NY</td>
        <td>555 10th Ave</td>
        <td>29C</td>
      </tr>
      <tr>
        <th>2</th>
        <td>[Bowling Alley, Cardio Fitness Room, Central A...</td>
        <td>1 BA</td>
        <td>Studio</td>
        <td>manhattan-ny</td>
        <td>New York</td>
        <td></td>
        <td>12 - 24 Month Lease</td>
        <td>Alcove Studio/1 Bathroom</td>
        <td>[\n\n\r\n                            Pets Nego...</td>
        <td>10018</td>
        <td>\nProperty Information\n\n•Built in 2017\n•598...</td>
        <td>5 star property</td>
        <td>$3,365</td>
        <td>555TEN, New York, NY</td>
        <td>[{'school_name': 'Public Elementary School', '...</td>
        <td>647 Sq Ft</td>
        <td>NY</td>
        <td>555 10th Ave</td>
        <td></td>
      </tr>
      <tr>
        <th>3</th>
        <td>[Bowling Alley, Cardio Fitness Room, Central A...</td>
        <td>1 BA</td>
        <td>Studio</td>
        <td>manhattan-ny</td>
        <td>New York</td>
        <td></td>
        <td>12 - 24 Month Lease</td>
        <td>Alcove Studio/1 Bathroom</td>
        <td>[\n\n\r\n                            Pets Nego...</td>
        <td>10018</td>
        <td>\nProperty Information\n\n•Built in 2017\n•598...</td>
        <td>5 star property</td>
        <td>$3,365</td>
        <td>555TEN, New York, NY</td>
        <td>[{'school_name': 'Public Elementary School', '...</td>
        <td></td>
        <td>NY</td>
        <td>555 10th Ave</td>
        <td>32H</td>
      </tr>
      <tr>
        <th>4</th>
        <td>[Bowling Alley, Cardio Fitness Room, Central A...</td>
        <td>1 BA</td>
        <td>1 BR</td>
        <td>manhattan-ny</td>
        <td>New York</td>
        <td></td>
        <td>12 - 24 Month Lease</td>
        <td>1 Bedroom/1 Bathroom</td>
        <td>[\n\n\r\n                            Pets Nego...</td>
        <td>10018</td>
        <td>\nProperty Information\n\n•Built in 2017\n•598...</td>
        <td>5 star property</td>
        <td>$4,415 - 5,050</td>
        <td>555TEN, New York, NY</td>
        <td>[{'school_name': 'Public Elementary School', '...</td>
        <td>550 Sq Ft</td>
        <td>NY</td>
        <td>555 10th Ave</td>
        <td></td>
      </tr>
    </tbody>
  </table>
</table_box>

## Data Cleaning
Jupyter notebook for this section is available in <boxed><a href="https://github.com/siinn/Data-Science-Portfolio/blob/master/NYC-Rental-Prediction/notebook/data_cleaning.ipynb">DATA CLEANING</a></boxed>.

Data is cleaned using <b>Pandas</b> so that the DataFrame is in appropriate format for <b>Scikit-learn</b> estimators. The following describes important data cleaning processes.

- String is converted to float ("1 Bathroom" to 1, "Studio" to 0, $3,255 to 3255, etc..)
- Address is converted to longitude and latitude
- Python list object is converted to dummy columns

<br>
After data cleaning, the DataFrame contains 1 target variable (rent), 9 numeric features (longitude, latitude, bathrooms, bedrooms, lease length, rating, sqft, built year, property size), and 70 binary features (as dummy columns). The cleaned DataFrame is shown in Table 2.

<table_title>Table 2. Cleaned DataFrame from Data Collection (Head only)</table_title>
<table_box>
  <table class="dataframe" style="width:12000px">
    <thead>
      <tr style="text-align: left;">
        <th></th>
        <th>longitude</th>
        <th>latitude</th>
        <th>bathrooms</th>
        <th>bedrooms</th>
        <th>borough</th>
        <th>city</th>
        <th>full_address</th>
        <th>leaseLength</th>
        <th>rating</th>
        <th>rent</th>
        <th>rental_title</th>
        <th>sqft</th>
        <th>state</th>
        <th>street_address</th>
        <th>pet_cat</th>
        <th>pet_dog</th>
        <th>built_year</th>
        <th>property_size</th>
        <th>amenity_air conditioning</th>
        <th>amenity_alcove</th>
        <th>amenity_balcony</th>
        <th>amenity_basketball</th>
        <th>amenity_bike storage</th>
        <th>amenity_business</th>
        <th>amenity_cable</th>
        <th>amenity_ceiling fans</th>
        <th>amenity_charging station</th>
        <th>amenity_clubhouse</th>
        <th>amenity_concierge</th>
        <th>amenity_controlled</th>
        <th>amenity_courtyard</th>
        <th>amenity_dishwasher</th>
        <th>amenity_disposal</th>
        <th>amenity_dry cleaning</th>
        <th>amenity_elevator</th>
        <th>amenity_fitness</th>
        <th>amenity_furnished</th>
        <th>amenity_game</th>
        <th>amenity_gated</th>
        <th>amenity_granite</th>
        <th>amenity_grill</th>
        <th>amenity_hardwood</th>
        <th>amenity_heat/hot water included</th>
        <th>amenity_heating</th>
        <th>amenity_high ceilings</th>
        <th>amenity_internet</th>
        <th>amenity_laundary facilities</th>
        <th>amenity_laundry service</th>
        <th>amenity_library</th>
        <th>amenity_lounge</th>
        <th>amenity_maid</th>
        <th>amenity_maintenance</th>
        <th>amenity_manager</th>
        <th>amenity_microwave</th>
        <th>amenity_movie</th>
        <th>amenity_online service</th>
        <th>amenity_oven</th>
        <th>amenity_package</th>
        <th>amenity_parking available</th>
        <th>amenity_parking garage</th>
        <th>amenity_parking security</th>
        <th>amenity_patio</th>
        <th>amenity_pet care</th>
        <th>amenity_pet play area</th>
        <th>amenity_picnic</th>
        <th>amenity_planned social activities</th>
        <th>amenity_playground</th>
        <th>amenity_pool</th>
        <th>amenity_private outdoor space</th>
        <th>amenity_recycling</th>
        <th>amenity_refrigerator</th>
        <th>amenity_renovated</th>
        <th>amenity_retail</th>
        <th>amenity_rooftop</th>
        <th>amenity_smoke free</th>
        <th>amenity_spa</th>
        <th>amenity_stainless</th>
        <th>amenity_storage space</th>
        <th>amenity_stove</th>
        <th>amenity_sundeck</th>
        <th>amenity_terrace</th>
        <th>amenity_tub</th>
        <th>amenity_views</th>
        <th>amenity_walk-in closets</th>
        <th>amenity_washer/dryer</th>
        <th>amenity_wheelchair</th>
        <th>amenity_wifi</th>
        <th>amenity_yoga studio</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th>0</th>
        <td>-73.995955</td>
        <td>40.759001</td>
        <td>1</td>
        <td>0</td>
        <td>Manhattan</td>
        <td>new york</td>
        <td>555 10th Ave, new york, NY</td>
        <td>24</td>
        <td>5</td>
        <td>3255.0</td>
        <td>555TEN</td>
        <td>500.0</td>
        <td>NY</td>
        <td>555 10th Ave</td>
        <td>1</td>
        <td>1</td>
        <td>2017</td>
        <td>598</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>1</td>
        <td>1</td>
      </tr>
      <tr>
        <th>1</th>
        <td>-73.995955</td>
        <td>40.759001</td>
        <td>1</td>
        <td>1</td>
        <td>Manhattan</td>
        <td>new york</td>
        <td>555 10th Ave, new york, NY</td>
        <td>24</td>
        <td>5</td>
        <td>4732.5</td>
        <td>555TEN</td>
        <td>550.0</td>
        <td>NY</td>
        <td>555 10th Ave</td>
        <td>1</td>
        <td>1</td>
        <td>2017</td>
        <td>598</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>1</td>
        <td>1</td>
      </tr>
      <tr>
        <th>2</th>
        <td>-73.995955</td>
        <td>40.759001</td>
        <td>2</td>
        <td>2</td>
        <td>Manhattan</td>
        <td>new york</td>
        <td>555 10th Ave, new york, NY</td>
        <td>24</td>
        <td>5</td>
        <td>6860.0</td>
        <td>555TEN</td>
        <td>967.0</td>
        <td>NY</td>
        <td>555 10th Ave</td>
        <td>1</td>
        <td>1</td>
        <td>2017</td>
        <td>598</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>1</td>
        <td>1</td>
      </tr>
      <tr>
        <th>3</th>
        <td>-73.995955</td>
        <td>40.759001</td>
        <td>3</td>
        <td>3</td>
        <td>Manhattan</td>
        <td>new york</td>
        <td>555 10th Ave, new york, NY</td>
        <td>24</td>
        <td>5</td>
        <td>9940.0</td>
        <td>555TEN</td>
        <td>1000.0</td>
        <td>NY</td>
        <td>555 10th Ave</td>
        <td>1</td>
        <td>1</td>
        <td>2017</td>
        <td>598</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>1</td>
        <td>1</td>
      </tr>
      <tr>
        <th>4</th>
        <td>-73.989890</td>
        <td>40.747661</td>
        <td>1</td>
        <td>0</td>
        <td>Manhattan</td>
        <td>new york</td>
        <td>100 W 31st St, new york, NY</td>
        <td>27</td>
        <td>5</td>
        <td>3456.5</td>
        <td>EOS</td>
        <td>516.0</td>
        <td>NY</td>
        <td>100 W 31st St</td>
        <td>1</td>
        <td>1</td>
        <td>2016</td>
        <td>375</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>1</td>
        <td>1</td>
        <td>1</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>1</td>
        <td>1</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
        <td>1</td>
        <td>0</td>
        <td>0</td>
        <td>0</td>
      </tr>
    </tbody>
  </table>
</table_box>

## Exploratory anlaysis
Jupyter notebook for this section is available in <boxed><a href="https://github.com/siinn/Data-Science-Portfolio/blob/master/NYC-Rental-Prediction/notebook/data_visualization.ipynb">VISUALIZATION</a></boxed>.

In this section, an exploratory analysis is performed in order to understand different features collected in the previous section. Also, correlation between features are explored to avoid any collinearity when building regression models.

### Number of bedrooms
First, number of bedrooms in apartments are plotted. The plots are separated into each borough. The plots are made with <verbatim>countplot</verbatim> from <b>Seaborn</b> library. It can be seen that 1 bedroom apartment is the most common type of apartment in Manhattan and Bronx, and other boroughs tend to have more bedrooms. The plot is not normalized.
<br><br>
<div align="center"><img src="/images/projects/nyc/visualization_1.png" width="500px"></div>

### Boroughs
Next, we count number of apartment listings in each borough. <verbatim>Countplot</verbatim> from <b>Seaborn</b> library is used in making the plot. The plot shows that we have roughly 650~800 apartment listings in all boroughs except Staten Island in which only ~100 apartment listings are available.
<div align="center"><img src="/images/projects/nyc/visualization_2.png" width="400px"></div>

### Relationship between rent and bedrooms
The relationship between two features, rent and number of bedrooms, is plotted using <verbatim>barplot</verbatim> from <b>Seaborn</b> library. The plot is separated into boroughs. The plot shows that rent is higher for the apartments with more number of bedrooms as expected. However, the rent difference between 3 to 5 bedrooms is not very large. The plot also shows that we have limited statistics for 6 bedroom apartments.

Also, it is noticeable that rent is significantly higher in Manhattan in comparison to other boroughs, and this is expected.
<div align="center"><img src="/images/projects/nyc/visualization_3.png" width="500px"></div>

### Relationship between rent and apartment rating
The relationship between rent and apartment rating is plotted using barplot. Apartment rating is an averaged review score from tenants, and the score ranges from 1 to 5. The plot is separated into boroughs. The plot shows that the average rent is slightly higher for the apartments with higher rating in Manhattan, Brooklyn, and Queens. Rent is less sensitive to the rating in Bronx and Staten Island. 

However, there are very few apartments with rating less than 3, suggesting that either tenants are generally satisfied with their apartments or the ratings on this website is biased.
<div align="center"><img src="/images/projects/nyc/visualization_4.png" width="500px"></div>

### Pet policy
Next, the average rent is compared between different pet policies. The left plot shows average rent of apartments prohibiting dogs (red) and apartments allowing dogs (blue). The plot shows that there is a significant difference in average rent between apartments depending on dog policy.

The right plot shows average rent of apartments prohibiting cats (red) and apartments allowing cats (blue). Again, there is a significant difference in average rent between apartments depending on cat policy.

<div align="center"><img src="/images/projects/nyc/visualization_5.png" width="400px"></div>

### Amenities
As explained in Data Cleaning section, amenities are stored as dummy variables. The average rent is compared between apartments with and without an amenity. Only selected amenities are plotted here as representative plots. They are:

- Washer/dryer in unit
- Business center
- Fitness center
- Furnished
- Gated
- Pet care service
- Yoga studio
- Package service
- Concierge

The plots shows that some amenities such as washer/dryer in unit, business center, or fitness center have large impact on rent. However, there are also amenities that do not have significant influence on rent, such as 'gated'.
<div align="center"><img src="/images/projects/nyc/visualization_6.png" width="100%"></div>

### Relationship between rent and apartment size (sqft)
The relationship between rent and apartment size is plotted using <verbatim>regplot</verbatim>. The plots are separated into 5 boroughs. The plots show that there is a positive correlation between rent and apartment size (sqft) as expected. This tendency is the strongest in Manhattan, followed by Brooklyn and Queens. In Bronx and Staten Island, apartment size does not have large impact on rent.
<div align="center"><img src="/images/projects/nyc/visualization_7.png" width="100%"></div>

### Relationship between rent and built year
The relationship between apartment built year and rent is plotted. Apartments with missing built year are filled with median built year of all apartments.

The plots show that apartments built later tend to have higher rent, but the correlation is not very strong. This can be partially explained by the large number of apartments with missing built year, shown as a straight line around 1990's. 

Also, low statistic in Bronx and Staten Island makes the feature harder to be useful in building regression models.
<div align="center"><img src="/images/projects/nyc/visualization_8.png" width="100%"></div>

### Relationship between rent and property size
The relationship between rent and property size, number of units in apartment complex, is plotted. Apartments with missing property size are filled with median property size of all apartments (~800).

The plots show that there is a weak correlation between rent and property size, suggesting that 'property size' isn't likely to be an important feature in building regression models.
<div align="center"><img src="/images/projects/nyc/visualization_9.png" width="100%"></div>

### Location and heatmap
Longitude and latitude are obtained from addresses using geocoder, and locations of apartments are plotted. The plot clearly shows the separation between boroughs and the rental listing density in each region.

In addition to the simple scatter plot of longitudes and latitudes, the apartment listing is plotted as a heat map overlaid with google map. The heat map shows that Manhattan and its nearby regions are the most populated as expected.
<div align="center"><img src="/images/projects/nyc_overlay.png" width="100%"></div>
<div align="center"><img src="/images/projects/nyc_heatmap.png" width="100%"></div>

### Correlation of continuous features
As the last step of the exploratory analysis, the correlations between numetic variables are plotted so that we can avoid collinearity between features when building regression models.

There are some features with slight correlation such as apartment size in sqft and number of bedrooms/bathrooms. However, the correlation doesn't seem strong, and since they are going to be most important features in building regression models, these features are kept.
<div align="center"><img src="/images/projects/nyc/visualization_11.png" width="100%"></div>

## Machine Learning (RandomForest and GradientBoosting)
Jupyter notebook for this section is available in <boxed><a href="https://github.com/siinn/Data-Science-Portfolio/blob/master/NYC-Rental-Prediction/notebook/regression_models.ipynb">MACHINE LEARNING</a></boxed>.

In this section, regression models are built for apartment rent in NYC using <b>scikit-learn</b> machine learning libraries.

First, a DataFrame containing features such as location, number of bedrooms and bathrooms, and amenities is retrieved. The samples are split into train and test dataset, and regression models are optimized using cross-validation.

Then the performance of two regression models, <b>RandomForest</b> and <b>GradientBoosting</b>, are compared.

### Data Preprocessing
Features and target are read from the DataFrame, and the dataset is split into train and test datasets.

The features are standardized ($$x\rightarrow \frac{x - \bar{x}}{\sigma}$$) using <b>Scikit StandardScaler</b>. It is important to note that when we standardize the features, we need to use the statistics from the train dataset and apply them to both train and test datasets in order prevent information leak from the test dataset to the train dataset.

### Grid Search
Grid search is performed to find the optimum set of parameters for RandomForest and GradientBoosting regressors. Cross-validation is used to evaluate the performance given a set of parameters, and the parameters that gives the best $$R^{2}$$ score is chosen.

However, often GridSearch results in over-fitting, and the parameters are further adjusted to avoid over-fitting the training samples. For this purpose, two flags are implemented to specify wheather GridSearch is performed for RandomForest and GradientBoosting regressor. If GridSearch is not performed, defaults sets of parameters are used for the estimators.

<br>
<code>
    GradientBoostingRegressor(alpha=0.9, criterion='friedman_mse', init=None,
    <br><span style="margin-left:10em">learning_rate=0.015, loss='ls', max_depth=3,
    <br><span style="margin-left:10em">max_features=None, max_leaf_nodes=None,
    <br><span style="margin-left:10em">min_impurity_decrease=0.0, min_impurity_split=None,
    <br><span style="margin-left:10em">min_samples_leaf=4, min_samples_split=2,
    <br><span style="margin-left:10em">min_weight_fraction_leaf=0.0, n_estimators=400,
    <br><span style="margin-left:10em">presort='auto', random_state=0, subsample=0.3, verbose=0,
    <br><span style="margin-left:10em">warm_start=False)
</code>             

### Learning curve

In order to evaluate the regression models with the given hyperparemteres, learning curves are produced by plotting training score and test score at varying sample sizes. Cross-validation is used for evaluating training and test scores, and $$R^{2}$$ score (1 - SSR/SST) is used for evaluation.

<div align="center"><img src="/images/projects/nyc/machine_learning_1.png" width="100%"></div>

In both RandomForest and GradientBoosting, the learning curves shows that training score and testing score become more close as the sample size increases, indicating that both regressors are not over-fitting data. However, relatively low $$R^{2}$$ scores suggest that the total variation in data is not fully explained by either of these models.

Also, due to the limited statistic, it is difficult to determine if more data would be helpful in building better estimator.

- if training and test score improve at larger samples size, we need to collect more data for better regression model.
- if training and test score don't improve at larger sample size, we need more complex model. i.e. adding more features.

### Cross-validated metrics
In addition to learning curves, the performance of estimators are evaluated using cross-validated metrics such as 'negative mean squared error' and $$R^{2}$$ score. The scores are calculated for RandomForest and GradientBoosting regressors using 10-fold cross-validation.

#### RandomForest cross-validated scores
<code>
(train) negative mean squared error = -1809887.691 (+/- 44710307.962)
    <br>(validation) negative mean squared error = -2859499.446 (+/- 618307765.210)
    <br>(train) R2 score = 0.635 (+/- 6.144)
    <br>(validation) R2 score = 0.640 (+/- 18.930)
</code>

#### GradientBoosting cross-validated scores
<code>
(train) negative mean squared error = -1998125.493 (+/- 54105804.087)
    <br>(validation) negative mean squared error = -2916320.856 (+/- 585746005.146)
    <br>(train) R2 score = 0.600 (+/- 8.496)
    <br>(validation) R2 score = 0.596 (+/- 18.821)
</code>

### Fitting models and making prediction
In the previous section, the hyperparameters for RandomForest and GradientBoosting are chosen by learning curves and cross-validated scores. Now, the models are trained using the training dataset, and predictions are made on training and test datasets.

After making predictions, $$R^{2}$$ and mean absolute percentage errors are evaluated for both training and test samples.

#### RandomForest test/train scores
<code>
(train) R2 score = 0.616
    <br>(train) mean absolute percentage error = 0.156
    <br>(test) R2 score = 0.674
    <br>(test) mean absolute percentage error = 0.168
</code>

#### GradientBoosting test/train scores
<code>
(train) R2 score = 0.572
    <br>(train) mean absolute percentage error = 0.148
    <br>(test) R2 score = 0.762
    <br>(test) mean absolute percentage error = 0.154
</code>


### Scatter Plot: Prediction vs Observed
In order to visualize how well the predictions from the regression models agree with observed value, the predictions are plotted against true values. The scatter plots show good linearity between predicted value and true value. GradientBoosting seems to provide better prediction, especially in the range between $5,000 and $7,500.
<div align="center"><img src="/images/projects/nyc/machine_learning_2.png" width="100%"></div>


### Histogram: Percentage Residual

The percentage residual of the prediction models are plotted with histograms. The percentage residual is defined by

Percentage residual = $$(\hat{y_{i}} - y_{i}) / y_{i}$$ where $$\hat{y}$$ represent predicted value.

The distributions of both RandomForest and GradientBoosting have mean of $$\approx$$ -0.05, suggesting that the models are slightly over-estimating rent. The standard deviation is slightly larger for the RandomForest method, but further test is needed to determine if two distributions are statistically different.
<div align="center"><img src="/images/projects/nyc/machine_learning_3.png" width="100%"></div>

<code>
    mean percentage residual = -0.0556839454032
    <br>mean percentage residual = -0.054551739349
    <br>std percentage residual = 0.26336286057
    <br>std percentage residual = 0.224177312376
</code>


### Scatter Plot: Residual vs Observed
Residuals, defined by predicted value - true value ($$\hat{y} - y$$), are plotted against observed $$y$$ for both RandomForest and GradientBoosting. Residuals from training sample and test sample are plotted separately.

- The residual distribution of RandomForest regression shows non-random, artificial structure, suggesting that the model isn't describing data very well.
- The residual distribution of GradientBoosting is slightly skewed, but we don't see artificial structure as in the RandomForest residual distribution. This also makes sense because from the percentage residual plots, we have learned that both models are slightly over-estimating the data (~5%).
<div align="center"><img src="/images/projects/nyc/machine_learning_4.png" width="100%"></div>

### Important Features
The features that contributed the most in building the regression models are obtained by <verbatim>feature_importances_</verbatim> method. Since there are more than 80 features considered in this analysis, only 10 most important features are plotted.

In RandomForest regression model, top five features dominated in building the model. In GradientBoosting regression model, more features contributed to the model, but 'number of bathrooms' made the most significant contribution to the model.
<div align="center"><img src="/images/projects/nyc/machine_learning_5.png" width="100%"></div>




## Conclusion
In this projects, two regression models, RandomForest and GradientBoosting, are considered for predicting apartment rent, and their performances are compared. Although two models provided a similar performance by $$R^{2}$$ score and percentage residual distribution, GradientBoosting seems to provide more reliable prediction of data because in GradientBoosting,

- Prediction vs Observed scatter plot seems to be more linear.
- Percentage residual has narrower distribution (smaller $$\sigma$$).
- Residual vs Observed scatter plot shows more natural, randomness.
- Relative to RandomForest, more features contributed to the model.

However, the prediction made by the GradientBoosting regression model is still limited for the following reasons.

- $$R^{2} \approx $$ 60% suggests that not all variance in data can be explained by these models.
- The large standard deviation in percentage residual ($$\sigma \approx$$ 0.22) suggests that the prediction will be off by more than 22% with probability of ~30%.

<br><br>
