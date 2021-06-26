## Repository Info:

Repository Size: 72.2 MB

Code can be run using Visual Studio Code or Jupyter Notebook.  PgAdmin4 can be used for the SQL search queries.

Given a previously made data set analyzing Covid-19 positivity rates in Bexar County, the data was extracted, transformed with new data, and loaded into a new data base for future use.

![3](Images/3.png)

![4](Images/4.png)

![5](Images/5.png)

![2](Images/2.png)

# ETL Project: Bexar County Covid-19 by the numbers

## Extract

#### We used 2 datasets from the website: https://data.democratandchronicle.com/covid-19-vaccine-tracker/texas/bexar-county/48029/ and The Census Bureau CSV updated. All of our data was based on Bexar county. 

![Images/extract.png](Images/extract.png)

```python
# Path to chromedriver
#get_ipython().system('which chromedriver')

# Set the executable path and initialize the chrome browser in spliter
executable_path = {'executable_path': 'C:\Program Files (x86)\chromedriver.exe'}
browser = Browser('chrome', **executable_path)
```

```python

# ### Visit the Bexar Count vaccine Site
# Visit the Bexar county vaccine  site
url = 'https://data.democratandchronicle.com/covid-19-vaccine-tracker/texas/bexar-county/48029/'
browser.visit(url)
```

```python
# Retrive page with the request module
response = requests.get(url)

# Retrive page with the request module
response = requests.get(url)

# reults are returned as an iterable list
results = soup.find('td', class_="tbvar covr")

# Print all headlines
tds = soup.find_all('td')
# A blank list to hold the headlines
headlines = []
# Loop over td elements
for td in tds:
    # If td element has an anchor...
    if (td.a):
        # And the anchor has non-blank text...
        if (td.a.text):
            # Append the td to the list
            headlines.append(td)
```


## Transform

#### Our first steps in cleaning up the datasets involved updating the most recent vaccine information from all Texas counties and transfering it from HTML to Dataframe Pandas. 
#### We also pulled updated Census Bureau CSV to incorporate updated positive case count numbers in Bexar county

![Images/transform.png](Images/transform.png)

```python
# # Use Pandas to scrape data
tables = pd.read_html('https://data.democratandchronicle.com/covid-19-vaccine-tracker/texas/bexar-county/48029/')

# Second table
df = tables[2]
df
```

```python
Bexar_data = df.loc[df["Area"]=="Bexar County"]
Bexar_data
```

```python
My_data = Bexar_data["People Fully Vaccinated"]
str(My_data)
len("My_data")
```

```python
number_vaccinated = My_data[0:7]
number_vaccinated
```

```pyton
df.to_csv("Vaccine_Tracker.csv")

```



## Load

#### The last step was to transfer our final output into a Database. We created a database and respective tables to match the columns from the final Panda’s Data Frame using MYSQL and then connected to the database using SQLAlchemy and loaded the result. utilized PgAdmin

![Images/load.png](Images/load.png)

```SQl
CREATE TABLE vaccine_tracker (
 	Area VARCHAR,
	Population INT,
	Doses_Administered INT,
	People_vaccinated_with_one_dose INT,
	Fully_vaccinated INT	
);

Select *
From vaccine_tracker;
```

![Images/2.png](Images/2.PNG)

```SQL
Drop Table Bexar_updated_Covid_data;

CREATE TABLE Bexar_updated_Covid_data (
 	OBJECTID NUMERIC,
	ZIP_CODE NUMERIC,
	populationtotals_totpop_cy NUMERIC,
	Positive NUMERIC,
	CasesP100000 NUMERIC,
	ZipInt NUMERIC,
	ActiveCases NUMERIC,
	ActiveCaseRate NUMERIC,
	SHAPE_Length NUMERIC,
	SHAPE_Area NUMERIC

);

Select *
From Bexar_updated_Covid_data;
```

![Images/1.png](Images/1.PNG)

## Limitations

#### There were some limitations to our findings due to the data available. Census data only current to 2019 population from zip codes from project #1. 
