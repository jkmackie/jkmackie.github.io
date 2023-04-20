---
title: "Oil Futures Trade Selection"
date: 2019-10-03T13:29:43-06:00
draft: false
---

### Rank Seasonal Oil Trades and Backtest Profit/Loss

***
###### ***Copyright © 2019 Justin Mackie. All Rights Reserved.  No one may distribute or create derivative works from my work without my written permission.  You are prohibited from using the code for commercial and/or business purposes.***
***

#### Background:
This project was built for [Commodity Analytics Group](https://commodityanalyticsgroup.com/).

Suppose we want to predict oil prices several months ahead.  Oil futures prices are daily time series data.  Each business day, there is one **settlement** price for the commodity.  Prices sort of repeat over the years in cyclical patterns called seasonality.  My model captures price patterns and computes the chance of the pattern repeating using historical prices.  The patterns correspond to commodity trades that are ranked best to worst.

The commodity illustrated in the model is Brent Crude, the key global price benchmark for Atlantic basin crude.  [Brent]( https://www.theice.com/products/219/Brent-Crude-Futures/specs)  is used to price the majority of Earth's internationally-traded crude supplies.

#### The Model:  :money_with_wings:
My statistical seasonality model selects the best long and short trades using historical data.  Then, cumulative trade Profit/Loss performance is tallied for the designated time period.  Model logic is built on Python and Pandas.  The Model reads and writes data from the SQLite3 database named **data.sqlite**.  Data visualizations are plotted with Seaborn and Matplotlib.  The model is presented in a Jupyter Notebook -- the **.ipynb** file.

***
#### Top 5 Long Trades by Contract Month -- Jan and Feb:
![Top 5 Long Trades](top5_long.PNG)

#### Brent Cumulative Profit-Loss for dozen selected trades:
![Cumulative PL](cum_pl_2018.PNG)
***

#### Model Details:
When picking a basket of financial trades (or a standalone trade), the odds a trade will move up or down is not the full picture.  Volatility and price path are relevant.  Volatility is important because a less volatile trade is better than a trade with wild price swings, all other things being equal.  Price path matters because as we go through time, a **cumulative total trade return** of $1 that never dips negative is better than a (larger) cumulative trade return of $1.10 that dips to ($2.00) and later ($1.00).  Only a highly risk-tolerant investor would prefer the latter pattern.

The model visualizes trade movement, both in dollar terms and normalized z-score.  The z-score is the move in standard deviations above/below the mean.  See below.

***
#### 1-3v4 Dollar Move -- 2011-2017:
![Dollar Move](1-3v4_dollar_move.PNG)

#### 1-3v4 Spread z-Normalized Move -- 2011-2017:
![z-move](1-3v4_z_move.PNG)
***

We can run the model on a different commodity like West Texas Intermediate (WTI) or RBOB Gasoline Futures.  We just need the data in a SQLite database!

The prices modeled are actually the price spread between two contracts for the designated calendar month.  Ex. 1-3v4 means the month three settlement price minus the month 4 settlement price for calendar month 1.  Terminology is bulleted below. Calculating the price spreads in a *time spread matrix* are outside our scope.  

Picking valid spreads is complicated by the fact that contracts expire.  We model spreads that are active for the whole calendar month (until any month-end expiration).  Here is a concrete example.  If the front contract is traded the whole month, but the back contract is traded only the last seven days, this is not a valid spread.

Why not model the spread between "Spread1" and "Spread2"?  I've modeled that too!  Commodity traders call them fly spreads.  In contrast, if there is one spread modeled, it is a spread model.

#### *Please note:  The model is private.  It was built for a commodity analytics startup called [CAG](https://commodityanalyticsgroup.com/).  Model available upon request.*

***
#### Trading Terminology:
* **Fit Years** - Years used in computing trade ranking stats.
* **Initial Price** - Price on Day before first day of month.
* **Last Price** - Last Price for day.
* **Roll Day** - Last day position is held.  For example, this may be a few days in advance of the expiration date.
* **Capital** - Position profit/loss.
* **Contract Month** - The month in which seller must deliver.  Ex. Delivery of one May-2019 Brent contract is physical delivery of 1,000 barrels of Brent in May.  The Ice Brent contract has the option to be cash settled.
* **Long Spread** - long front contract, short back contract.
* **Short Spread** - Short front contract, long back contract.
* **Front month** - first month in a spread.  Ex. 1 in a 1v3 spread.
* **Back month** - last  month in a spread.  Ex. 3 in a 1v3 spread.
* **calmonth_spread_label** - the contract months for the front and back contracts.  Ex 1-3v6 is calendar month 1 (Jan).  Front contract is month 3 (Mar) and back contract is month 6 (Jun).  The spread is the front price minus the back price.  That is the March minus June settlement price in this case.
* **z_diff** - z-score change between the month-start and month-end z-scores
* **Settles TABLE: settle** - Daily settlement price of product
* **Spreads TABLE: spread** - Settlement price difference for two different valid Contact Months on the same date.
* **Expiry TABLE: last_trade_date** - the last day the Contract Month (prompt_month) is available.  Ex. BRENT future for May-2019  expired 03-29-2019.
