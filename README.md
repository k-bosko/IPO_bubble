# IPO_bubble
scrape, clean and model IPO data with supervised ML

## Project description
Can we actually predict if an IPO will be successful? Which features increase this probability? This is what this project is about. It scrapes information about recent IPOs, combines it with stock data, cleans and munges the data and finally uses engineered features in a machine learning model to predict the offering success on its first day of trading.

**Disclosure:** This idea of this project started from my bad experience of investing into Lyft on its first day of trading in 2019. Lyft severely lost it's value after IPO and its price continues to fall. As a newbee in investing, I decided to explore the IPO market to better understand its dynamics in order to avoid similar mistakes in the future.

**Disclaimer:** This is a learning project to apply Data Science skills in Python. Thus, the insights from this project should not be taken as investment advice. 


## Results

**Step 1: NASDAQ scraper**

- scraped companies and their descriptions that went to IPO between 2010 and 2018 from NASDAQ site. 
- cleaned the list to get rid of companies that do not add value like:
    * those that were created specifically for mergers and acquisitions, 
    * those that are not traded on stock exchange but rather over-the-counter,
    * those that were created as ETFs or trusts (but not REITS). 

Result after data cleaning: 1600 companies in our IPO list down from initially 1866. 

**Step 2: Yahoo Finance scraper**

- scraped the stock info and company profile from Yahoo Finance for the IPO companies scraped in the previous step from NASDAQ site
- checked for duplicated tickers and company names
- checked for misalignment between company name and its ticker

Result: From 1533 IPO companies listed on Nasdaq, 703 companies were not scraped on Yahoo Finance. After checking the problem, it turned out that a lot of companies were defunct (merged/was acquired or otherwise delisted from the stock exchange) or the company was still funcational but changed the ticker.

**Step 3: Explorative Data Analysis** 

- explored created IPO dataset, in particular:
  * number of IPO companies by years
  * IPO offered price by sectors
  * number of IPO companies by the US states
  * number of IPOs by sector and industry
  * companies with the most & least employees growth
  * the highest paying CEOs of IPO companies by the decade in which they were born
  
**Step 4: Predictive modeling**

- engineered new features like:
    * Weekday of the IPO day
    * S&P500 change a week before IPO date to control for broad market conditions 
- split dataset into training/testing datasets 
- used SMOTE technique to deal with the imbalanced classes 
- normalized numerical features  
- trained GradientBoost, Logistic Regression, RandomForest and AdaBoost

## Conclusion
Offering success was defined here if the IPO price grew by more than $1 on its first day of trading.  

I used these features in my model:
- Price, Shares, Offer Amount, Employees, weekday of IPO from NASDAQ data
- sector, CEO_pay, CEO_born and SP_Week_Chg - from Yahoo Finance

I also calculated the total predicted gain from using the model and compare it to the actual gain (if all IPOs which prices went higher than $1 on the first day of trading were predicted correctly - practically impossible task) and the gain using the naive approach - i.e. when we invested in each and every IPO - buying it at opening price and selling at closing price.

Among the 4 models tested here, **Logistic Regression** performed the best (others are GradientBoost, RandomForest and AdaBoost). It has the highest f-beta score which maximizes precision - 0.63 (vs. GB=0.55, RF=0.58, Ada=0.59). The second best model which came close is AdaBoost. 

We also get the highest gains with the Logistic Regression - \$37.62 (vs GB=\$6.29, RF=\$3.13, Ada=\$9.21). 
It's not the highest possible amount that we could have earned. If we could have predicted all successful IPOs correctly, we would have earned \$85.99. Still using predictive modelling produced a much better outcome than naive approach, when investing in each and every IPO. In this case, the returns would have been negative or \$-34.09.

In general, all tested models performed much better than the naive approach and so could help avoid losing money by investing in unsuccessful IPOs.

