# Term Project DE2


### Interpretation ###



### Data Sources ###
Gender Pay Gap from Eurostat: https://ec.europa.eu/eurostat/databrowser/view/earn_gr_gpgr2$DV_593/default/table?lang=en

GDP at Market Prices from Eurostat API: https://ec.europa.eu/eurostat/databrowser/view/tec00001/default/table?lang=en

Gender Ratio from Kaggle: https://www.kaggle.com/datasets/programmerrdai/gender-ratio-inequality

Country Codes from Kaggle: https://www.kaggle.com/datasets/andradaolteanu/iso-country-codes-global

Indicators considered
- Pay Gap in unadjusted form
- GDP per capita
- Gender ratio

### Knime Workflow ###
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














<img width="848" alt="Screenshot 2022-12-08 at 10 50 50" src="https://user-images.githubusercontent.com/113236007/206415570-bdbdeb3a-801d-4f10-b075-3c4571e25ecf.png">



