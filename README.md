# Analysis of preventive measures (COVID-19)


__Disclaimer:__  For informative purposes only. I am not a professional nor do I claim to be one.
This is a school project that I extended upon in my spare time. I hold no responsibility for data accuracy or data gathering. Full credit goes to good people that made these datasets, links bellow.

That said, dataset created is free for you to use.

---

## Project goal

Goal of this project was to create a dataset containing facts about countries and preventive measures taken by those countries during COVID-19 pandemic, so far. For these purposes, I combined data from multiple sources to create a simple data warehouse that stores data on new measures implemented for each day in the period from 01-01-2020 to 02-06-2020, for every country. With this I hope to provide a single dataset for analysing the effectiveness of preventive measures worldwide.

## Data sources

Data for this project was combined from 4 sources:

* [_ACAPS COVID-19: Government Measures Dataset_](https://data.humdata.org/dataset/acaps-covid19-government-measures-dataset) by *__The Humanitarian Data Exchange__* - puts together all the measures implemented by governments worldwide in response to the Coronavirus pandemic.

* [_COVID-19 Coronavirsu data_](https://data.europa.eu/euodp/en/data/dataset/covid-19-coronavirus-data/resource/55e8f966-d5c8-438e-85bc-c7a5a26f4863) by *__EU Open Data Portal__* - data on the geographic distribution of COVID-19 cases worldwide.

*  [_Oxford COVID-19 Government Response Tracker (OxCGRT_)](https://www.bsg.ox.ac.uk/research/research-projects/coronavirus-government-response-tracker) by *__Hale et. al.__* - systematically collects information on several different common policy responses that governments have taken to respond to the pandemic on 17 indicators such as school closures and travel restrictions.

* [_The World by Income and Region_](https://datatopics.worldbank.org/world-development-indicators/the-world-by-income-and-region.html) by *__The World Bank__* - The World Bank classifies economies for analytical purposes into four income groups: low, lower-middle, upper-middle, and high income.

---

## ETL

Image below shows ETL process. The transforming part was done with Python Pandas, code can be found in [ETL.ipynb](https://github.com/marzekan/covid-19-measures-analysis/blob/master/ETL.ipynb) document in this repo. Main part of ETL was to clean the data and to split it into meaningful tables.

![etlmodel](https://github.com/marzekan/covid-19-measures-analysis/blob/master/images/architecture.png)

## Data model

Data is modeled in the form of _star schema_ consisting of 4 dimension tables and 1 fact table. Dimension tables contain: country, date and measures data. There is also one junk dimension (_additional_dim_) that consists of additional data for each day that can be make use of in the future.

![datamodel](https://github.com/marzekan/covid-19-measures-analysis/blob/master/images/DW_ER_model.png)

## Data descriptions

### Country dimension (country_dim)

| Column name | Description |
|-------------|-------------|
|_country_name_| Name of the country.
|_country_code_| 3 letter code unique to every country.
|_geo_id_| Similar to country code.
|_continent_| Continent that country resides on. Similar (or same as) to _region_.
|_region_| Region of the world in which the country resides.
|_population_data_2018_| Population number (data from 2018.).
|_income_group_2020_| Income group of the country, as categorised by *_The World Bank_* (for the year 2020.).


### Measure dimension (measures_dim)

| Column name | Description |
|-------------|-------------|
|_measure_| Name of the measure. Categorical value, there is 34 of them.
|_measure_category_| Each measure is categorised in one of six categories.

### Fact table (measures_by_day_fact)

| Column name | Description |
|-------------|-------------|
|__additional_id_| Reference to _additional_dim_ table.
|_date_id_| Reference to *date_dim* table.
|_measure_id_| Reference to *measure_dim* table.
|_country_id_| Reference to *country_dim* table.
|_cases_| New cases for each day for each county.
|_deaths_| Number of death for each day for each country.
|_stringency_index_| Index developed by [OxCGRT](https://www.bsg.ox.ac.uk/research/research-projects/coronavirus-government-response-tracker), shows how strict government response measures are. For each day for each country.
|_government_response_index_| Index developed by [OxCGRT](https://www.bsg.ox.ac.uk/research/research-projects/coronavirus-government-response-tracker), shows government responsivnes for each day for each country.
|_containement_health_index_| Another index by OxCGRT, more info on [link](https://github.com/OxCGRT/covid-policy-tracker)
|_economic_support_index_| Another index by OxCGRT, more info on [link](https://github.com/OxCGRT/covid-policy-tracker)

