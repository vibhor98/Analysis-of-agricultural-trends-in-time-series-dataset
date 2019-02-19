# SocialCops Data Science Intern Challenge

## Challenge: Agriculture Commodities, Prices & Seasons
**Aim:** Your team is working on building a variety of insight packs to measure key trends in the Agriculture sector in India. You are presented with a data set around Agriculture and your aim is to understand trends in APMC (Agricultural produce market committee)/mandi price & quantity arrival data for different commodities in Maharashtra.

**Objectives:**

* Test and filter outliers.
* Understand price fluctuations accounting the seasonal effect
  * Detect seasonality type (multiplicative or additive) for each cluster of APMC and commodities
  * De-seasonalise prices for each commodity and APMC according to the detected seasonality type
* Compare prices in APMC/Mandi with MSP(Minimum Support Price)- raw and deseasonalised
* Flag set of APMC/mandis and commodities with highest price fluctuation across different commodities in each relevant season, and year.

## Dataset Description
The agriculture data contains two files in `./Mandi_Data` directory namely:
    
  * 'CMO_MSP_Mandi.csv' contains Minimum Support Prices (MSPs) and Crop Type for each commodity.
  * 'Monthly_data_cmo.csv' contains the APMC-wise monthly prices (min., max. and average) for each and every commodity.
    
**Variable description:**
* msprice- Minimum Support Price
* arrivals_in_qtl- Quantity arrival in market (in quintal)
* min_price- Minimum price charged per quintal
* max_price- Maximum price charged per quintal
* modal_price- Mode (Average) price charged per quintal

**Analysis file:** The complete analysis with description is in python notebook: `Analysis.ipynb`. 

## Methodology & Analysis

### Section 1: Exploratory Data Analysis
* In MSP prices dataset, MSP prices for 10 commodities are missing.
* In monthly prices dataset, there are no missing enteries.
![Monthly prices box plot](https://github.com/vibhor98/SocialCops-Data-Science-Intern-Challenge/blob/master/Images/monthly_prices_boxplot%20.png)

The above **Box Plot** is for 'Bajri' commodity. It represents the average monthly prices for years: 2014, 2015 and 2016. The values in the rectangular box represent 1st, 2nd and 3rd Quartiles from bottom to up for every timestamp. The dark points away from the whiskers are outliers. For ex- In Mar 2016, the average price value (per quintal) is close to Rs. 6000 which is clearly an outlier.

Box Plot provides a very good visualization to find out the presence of outliers in the dataset.
![Mandi box plot](https://github.com/vibhor98/SocialCops-Data-Science-Intern-Challenge/blob/master/Images/mandi_boxplot.png)

The above box plot for MSPs (Minimum Support Prices) for various commodities evidents that there are no points outside the whiskers and hence, **no outliers** in the dataset: 'CMO_MSP_Mandi'.

### Section 2: Understanding price fluctuations accounting the seasonal effect

Since there are total **352 commodities** and **349 APMCs** in the dataset, we have taken a sample of 3 APMCs: Ahmednagar, Rahata and Kamkhed and again, sample of 3 commodities: Bajri, Onion and Capsicum. But all the methods are written in a generic way so that the analysis can be easily extended to more number of APMCs and commodities just by a single method call.

For each pair of commodity and APMC, we have done the following:

* Plot the monthly line graph for minimum, maximum and average prices of the commodity belonging to a given APMC to analyse the seasonality in the monthly prices. Below is the surge in the graph for Onion in Ahmednagar APMC.

![Onion Surge](https://github.com/vibhor98/SocialCops-Data-Science-Intern-Challenge/blob/master/Images/onion_surge.png)
The above plots clearly demonstrate a **peak-shaped curve (surge)** with peak around September' 2015 and rapid variations at specific time-frames. It is evident that the seasonality is yearly.

**Reason:** The main reason for this surge is **decreased supply**. Back in 2015, due to considerable **lower production of Onions**, supply had largely dropped. Consequently, the retail prices jumped. It had affected Onion prices in all the APMCs not only in Maharashtra but also in India as a whole.

* **Stationarity Test:** A time series is said to be stationary if its **statistical properties** such as mean and variance remain **constant over time**.
  * **Dickey Fuller Test:** It is one of the statistical tests for checking stationarity. Here, the null hypothesis is that the TS is non-stationary and alternate hypothesis is that the TS is stationary. The test results comprise of a Test Statistic and some Critical Values for difference confidence levels. If the **‘Test Statistic’ is less than the ‘Critical Value’**, we can reject the null hypothesis and say that the time series is stationary.
* De-seasonalizing the time series (making the TS stationary):
  * There are 2 major reasons behind non-stationarity of a TS:
    1. **Trend** – varying mean over time.
    2. **Seasonality** – variations at specific time-frames.
  * So, we need to remove both trends and seasonality to make TS stationary.
* Removing the seasonality from the time series:
  * **Differencing with lag 1:** One of the most common methods of dealing with both trend and seasonality is differencing. In this technique, we take the difference of the observation at a particular instant with that at the previous instant.
  * **STR Decomposition:** STR refers to Seasonality, Trend and Residual. In this approach, trend and seasonality are modeled separately from TS data and residual part of the series becomes stationary. Below is the STR decomposition for APMC: Jamkhed and Commodity: Bajri
  ![STR Decomposition](https://github.com/vibhor98/SocialCops-Data-Science-Intern-Challenge/blob/master/Images/STR_decomposition.png)
* Comparison of APMC-wise commodity prices (max., average and min.) with MSPs: Line plots are plotted to clearly demonstrate how MSPs deviate largely from the average prices.

### Section 3: Analyse price fluctuations across different commodities in each season and year

Since there are total **352 commodities** and **349 APMCs** in the dataset, we have taken a sample of 4 APMCs: Ahmednagar, Rahata, Jamkhed and Sangamner and again, sample of 4 commodities: Bajri, Onion, Capsicum and Soyabean. But all the methods are written in a generic way so that the analysis can be easily extended to more number of APMCs and commodities just by adding more commodities and APMCs in the list.

Firstly, we separated out the whole dataset year-wise (i.e. for 2014, 2015 and 2016) to do yearly analysis.
* No. of entries for year 2016: 28971
* No. of entries for year 2015: 25557
* No. of entries for year 2014: 7901

For each year in [2014, 2015, 2016], we do the following:
* Plot line plot for maximum, average and minimum prices of the commodity for that year.
* Take difference of maximum price and minimum price of the commodity to get the **Maximum Price Fluctuation** in that year.
* Plot 2 bar plots (APMCs v/s Price Fluctuations and Commodities v/s Price Fluctuations) to visually analyse the max. Price Fluctuations corresponding to an APMC and Commodity for each year.

![Price Fluctuations APMC-wise for 2015](https://github.com/vibhor98/SocialCops-Data-Science-Intern-Challenge/blob/master/Images/price_fluc_2015_apmc_barplot.png)

The above bar plot demonstrates the APMC-wise price fluctuations for 2015. It clearly shows that **Ahmednagar** APMC suffers from the highest price fluctuations of Rs. 3,800 per quintal.

![Price Fluctuations Commodity-wise for 2015](https://github.com/vibhor98/SocialCops-Data-Science-Intern-Challenge/blob/master/Images/price_fluc_2015_commodity_barplot.png)

The above bar plot demonstrates the Commodity-wise price fluctuations for 2015. It clearly shows that **Onion** Commodity suffers from the highest price fluctuations. It is quite an expected result because in 2015, due to **decreased production** and consequently, **very less supply of Onions** in the market especially during Sept and Oct' 2015, the **retail prices for Onion soared high** into the skies across different APMCs in the country. 

So, we can conclude that **APMC: Ahmednagar and Commodity: Onion suffer from the highest Price Fluctuations**.

## Results
![Price comparison](https://github.com/vibhor98/SocialCops-Data-Science-Intern-Challenge/blob/master/Images/price_comparison.png)

The above line plot demonstrates the variations in various prices (MSP, Average, Max. and Min.) of Bajri in Jamkhed APMC. It shows that for the selected commodity, the minimum prices set by the Government are quite low and the prices at which the commodities are being sold at APMCs are soaring high into the sky. The main reason for such a large price fluctuations between min. price set by the Government and actual price of the commodity can be: **Very high transportation cost**. Transportation Cost not only depends on the petrol/diesel prices but also depends on the geographical location of APMC. If the APMC is near to the farming lands, the cost is generally lower and very high in case of larger distances.

## Running the project
* Clone this repository by typing following command on the terminal:
  
  `git clone https://github.com/vibhor98/SocialCops-Data-Science-Intern-Challenge.git`

* Run: `pip install requirements.txt` to install all the required dependencies.
* Navigate to the directory containing `Analysis.ipynb` python notebook and run:
`jupyter notebook` to open the analysis file.
