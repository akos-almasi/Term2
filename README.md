# Term Project DE2
### Group members: ###
 - Shahana Ayobi
 - Alima Dzhanybaeva
 - Akos Almasi


### Interpretation ###



## Overview of Data Flow ##
### Data Sources ###
Gender Pay Gap from Eurostat: https://ec.europa.eu/eurostat/databrowser/view/earn_gr_gpgr2$DV_593/default/table?lang=en

GDP at Market Prices from Eurostat API: https://ec.europa.eu/eurostat/databrowser/view/tec00001/default/table?lang=en

Gender Ratio from Kaggle: https://www.kaggle.com/datasets/programmerrdai/gender-ratio-inequality

Country Codes from Kaggle: https://www.kaggle.com/datasets/andradaolteanu/iso-country-codes-global

Indicators considered:
- Pay Gap in unadjusted form
- GDP per capita
- Gender ratio

### Data collection ###
#### Eurostat ####
- Gender pay gap

The first dataset that was used in the project contains information on the gender pay gap for 30(?) European countries for 2011-2020. The original data has 7 variables: year, country, unit, field, flag, and ID. For this source we used NoSQL, and the csv file was imported to the platform by MongoDB Shell.    
[screenshot]

- GDP at Market Prices

To look into the relationship between gender pay gap and GDP, Eurostat API was used to access GDP at market prices for 30? European countries. The json data was acquired using data code (nama_10_gdp) in Eurostat Query Builder and filtered for year 2020, GDP at Market prices, and countries.  The data was then extracted using Postman.
[screenshot]
#### Kaggle ####
- Gender ratio

Further, we added the Gender ratio table in order to investigate whether the connection with the Pay Gap table is present. The original dataset has information on 35 countries for 2020. 

- Country Codes

We obtained the Alpha-2 country codes from Kaggle in order to access the GDP data via postman. The csv file was then loaded to MySQL for further analysis in Knime.


## Knime Workflow ##
#### MongoDB ####
In order to access NoSQL we added MongoDB Connector and to be able to read pay gap table we added MongoDB Reader node. We then transformed the obtained JSON to a Table and filtered the data to our needs, such as: leaving only 2020, keeping necessary columns, deleting missing values, etc. 
#### SQL ####
We used mysql workbench to access the gender ratio and the country codes csv files, and we used the mysql connector node to import it to knime, and we configured the Query Reader for 2020.  
#### Joiner ####
Using the joiner node, we first inner joined the gender gap and gender ratio tables by country4. Then, we used the second joiner to combine the obtained table with the country codes dataset by country.
#### Eurostat API ####
We configured the string manipulation node to access the GDP dataset, pasted the country codes to acquire GDP information for each country. We then used the get request node to access the JSON URL for each country.  We used the JSON Path node to extract the GDP at market prices, country names, country codes, year, and unit. We then performed data cleaning by removing unnecessary columns.

### Analytics on mongoDB ###
We wanted to visualize the average pay gap in each country, so we grouped by the country names column. Based on the bar chart we can see that the highest gap is in Latvia (21.3%), and the lowest is in Romania(4.7%) . 
Then we wanted to investigate this further so we checked what is the mean gap in each field. The result shows us that there is a negative pay gap in construction, which means women tend to earn more in this field, probably it's because there aren't many women in this occupation or most of the time they are in higher average salary positions compared to men who work on construction sites.

### Analytics on the Data Warehouse ###
We included the summary statistics table for the Data Warehouse
INCLUDE TABLE HERE

We took the log GDP for the following models:
First we ran a linear regression on pay gap conditioned on gender ratio and log GDP. We found out that if the female to male gender ratio increases by 1% the pay gap also increases by 1.2 %. Ln GDP is not significant probably because we are checking EU countries. Our adjusted R-squared value is low, so we would advise to add additional explanatory variable(s) to the model. 
INCLUDE REGRESSION EXPLANATION GRAPH, VIz
Since we found a significant relationship between gender ratio and pay gap, we wanted to visualize this correlation using a box plot. The plot confirms the regression result, the mean pay gap has an increasing trend meaning that as the proportion of women in the country rises the difference between average wage of men and women becomes bigger.  
BOX PLOT

Views:
Additionally, we created two table views that demonstrate the average pay gap and gender ratio by countries and by different fields. The user can specify the country displayed, the order of the observations based on the two indicators.
[TWO VIEWS]

### Conclusion ###

First, as we have seen from the visualizations for the Pay Gap table, in the fields where there are not that many women involved the gender pay gap can be even negative indicating that females in these professions earn more than men. However, in all other fields the pay gap is positive and this gap is the highest in the financial sector. Further, in order to prove our finding we ran linear regression on pay gap conditioned on gender ratio and log(GDP) and constructed a box plot. The previously observed pattern is also found in the obtained results, the gender ration and pay gap are indeed positively correlated. 









<img width="848" alt="Screenshot 2022-12-08 at 10 50 50" src="https://user-images.githubusercontent.com/113236007/206415570-bdbdeb3a-801d-4f10-b075-3c4571e25ecf.png">



