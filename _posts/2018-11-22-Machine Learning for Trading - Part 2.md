---
layout: post
title:  "Udacity - Machine Learning for Trading - Part 2"
date:   2018-11-22
desc: "Study notes"
keywords: "Python, Data Science, Machine Learning, Trading, Stock Market, Udacity"
categories: [Machine Learning]
tags: [Machine Learning, Python, Data Science, Trading, Stock Market, Udacity]
icon: icon-html
---
# Introduction

This is the second part of my notes for Udacity - Machine Learning for Trading course. You can find the first part [here](https://dorisz017.github.io/machine%20learning/2018/11/02/Machine-Learning-for-Trading-Part-1.html). The content are great for those who are new to trading in that it gives a overall introduction to the domain and I have learned a lot from it.

# Machine Learning for Trading - Part 2

## So you want to be a hedge fund manager?

* Type of funds

	* ETF: Exchange traded funds

		* Buy / Sell like stocks

		* Baskets of stocks

		* Transparent

		* Extremely liquid (easy to trade, large volume)

	* Mutual Fund

		* Buy / Sell at end of day

		* Quarterly disclosure

		* Less transparent

	* Hedge Fund

		* Sell / Sell by agreement

		* No disclosure

		* Not transparent

* Liquidity and capitalization

	* Large “cap”: number of shares * price

* Incentives for fund managers: How are they compensated?

	* Assets Under Management: the total amount of money being managed by the fund.

	* ETF: Expense Ratio 0.001% ~ 1.00%

	* Mutual Funds: Expense Ratio 0.5% ~ 3.00%

	* Hedge Funds: “Two and Twenty”: 2% of AUM + 20% of profit

* Hedge fund goals and metrics

	* Goals

		* Beat a benchmark

		* Absolute return (Long / short term)

	* Metrics

		* Cumulative return

		* Volatility

		* Risk / Reward: Sharpe Ratio

* The computing inside a hedge fund

* 
![](Machine%20Learning%20for%20Trading%20-%20Part%202/Screen%20Shot%202018-11-06%20at%204.32.59%20PM.png)

![](Machine%20Learning%20for%20Trading%20-%20Part%202/Screen%20Shot%202018-11-06%20at%208.52.08%20PM.png)

## Market Mechanics

* What is in an order?

	* Buy or Sell

	* Symbol: The identifier of stock / ETF

	* Number of shares

	* Limit or Market: Have limitation on price or Accept the current market price

	* Price: Need to be specified if it is a Limit order

* The order book

	* Each exchange keeps an order book of buy and sell for each stock {BID/ASK, Price, Size}

	* Order of Buy - BID on order book

	* Order of Sell - ASK on order book

	* Shares are sold or bought by prices of orders (High to low or Low to high)

* How orders get to the exchange

	* Buyer sends an order -> Broker finds the exchange with best price from order book -> Order executed -> Buyer confirm

	* Buyer and seller send orders -> Broker can make the transaction internally without involving exchanges (According to law, the buyer and seller must get the price at least as good as they would have got from exchanges)

	* A buyer sends an order to a broker, another buyer sends an order to another broker: go through Dark Pool

* How hedge funds exploit market mechanics

	* You placed buy order  from 2500 miles away from the exchange (takes 12ms to transfer information), a hedge fund’s computer is located 100 meters away from exchange (0.3ms)

	* Order book exploit:

		1. Hedge fund observes the order book

		2. HF buys stock

		3. You click “Buy”

		4. Meanwhile price goes up

		5. Hedge fund sells you stock

	* Geographic Arbitrage: Buy from New York, and then sell at London

* Additional order types

	* Exchanges only execute Buy or Sell, Market or Limitt

	* Broker order types: 

		* Stop loss: Sell when stock drops below certain prices

		* Stop gain: Sell when stock rises over certain prices

		* Trailing stop: Adjusted selling price

		* Most important: Sell short: Sell a stock short when you believe its will go down

* Mechanics of short selling

	* EX. IBM is selling at $100; Joe has 100 shares of IBM, and he can lend some shares; Lisa wants to buy IBM; You don’t own IBM shares, but you want to sell IBM short.

	* Entry: You can borrow 100 shares from Joe, and then sell them to Lisa. Lisa gives you $100 * 100 = $10,000. Now you have $10,000 in your account and you owe Joe 100 stocks

	* Exit: Now IBM is selling at $90, and Joe wants his shares back. You buy 100 shares with $90 * 100 = $9000 from Nate, who own the shares and give them back to Joe. Now you have $1000 in your account.

	* Your broker makes these happen for you

	* What can go wrong? If IBM stock goes up instead of going down, you lose money.

## What is a company worth?

* Why company value matters

	* Sell when stock price is higher than true value, and buy when stock price is lower than true value

	* Intrinsic value: how much money will I get in the future for owning stock

	* Book value: the assets the company owns

	* Market cap: Stock market value = number of shares * price, at how much you can buy the company

* The value of a future dollar

	* Present Value P.V., Future Value F.V., Interest Rate IR,

	* P.V. = F.V. / (1 + IR)^i

	* Intrinsic Value = Dividend / Discount rate

## The Capital Assets Pricing Model (CAPM)

* The market portfolio

	* Markets

	* Cap weighted: Wi = mktcap(i) / sum(mktcap)

	* Sectors: Energy, Technology…

* The CAPM equation

	* Ri(t) = Bi * Rm(t) + Ai(t)

		* Ri(t) : Return of an individual stock on a day t

		* Rm(t): Return on the market

		* Ai(t), alpha: residual, E(Ai(t)) = 0

	* Stock prices are most significantly influenced by market

* CAPM vs Active management

	* Passive (CAPM): Buy index and hold; alpha is random, E(alpha) = 0

	* Active: Pick stocks (Overweight / Underweight); alpha can be predicted

* CAPM for portfolios

	* Rp(t) = sum( Wi * (Bi * Rm(t) + Ai(t) ) ); Bp = sum( Wi * Bi); sum(Wi) = 1

	* CAPM: Rp(t) = Bp * Rm(t) + Ap(t)

	* Active: Rp(t) = Bp * Rm(t)  + sum(Wi * Ai(t))

* Implications of CAPM

	* Only way to beat market is to choose B (Beta)

	* Choose high B in up markets and low B in down market

	* Efficient Market Hypothesis says you can’t predict the market - You can’t beat market if CAPM is true

* Arbitrage Pricing Theory

![](Machine%20Learning%20for%20Trading%20-%20Part%202/Screen%20Shot%202018-11-08%20at%205.12.42%20PM.png)

## How hedge funds use CAPM

* Two stock scenario

	* Predict A +1% over market, beta(A) = 1.0

	* Predict B -1% below market beta(B) = 2.0

* Two stock CAPM math - Stock a and Stock b

	* Rp = sum( Wi * (Bi * Rm + Ai) = (WaBa + WbBb) * Rm

	* Wa = 0.5, Wb = -0.5, Ba = 1.0, Bb = 2.0, Aa = 1%, Ab = -1%

	* Rp = -0.5 * Rm + 1%

	* 1%: Market information; -0.5Rm: Market risk - Can’t control

	* Make -0.5Rm equal to 0: Bp = 0 = WaBa + BbBb

	* Example:
![](Machine%20Learning%20for%20Trading%20-%20Part%202/Screen%20Shot%202018-11-09%20at%201.52.34%20PM.png)
* How does it work?

	* Wa = 0.66, Wb = -0.33, Rm = 10%

	* Rp = WaBaRm + WaAa + WbBbRm + WbAb = 1%

	* Eliminating market risk, we get 1% of return

* Summary

	* Assuming

		* Information Alpha(i)

		* Beta i

	* CAPM enables minimizing market risk - Beta(p) = 0 and find Wi
	 
## Technical Analysis

* Characteristics of Technical Analysis (in contrast with Fundamental Value)

	* Looks at historical prices and volume only

	* Computes statistics called indicators

	* Indicators are heuristics - they can work!

	* Not considering the value of the company

* When is Technical Analysis effective?

	* Individual indicators weak

	* Combinations stronger

	* Look for contrasts (stock vs market)

	* Shorter time periods

	* As trading horizon become longer (milliseconds -> days -> years), decision speed goes down, decision complexity goes up; Fundamental Value goes up, Technical Value goes down

	* Computers excel at short trading horizon

* A few indicators

	* Momentum: Price change over time = price[t] / price[t-n] - 1

	* Simple moving average SMA : a proxy for underlying value

	* Bollinger Bands BB: Two std over and below SMA BB = price[t] - SMA[t] / 2std[t]

		* Cross from over Band to inside Band: Sell signal

		* Cross from below band to inside Band: Buy signal

* Normalization: scale data to [-1, 1]: normed = (values - mean) / values.std()

## Dealing with data

* How data is aggregated

	* A tick: a successful transaction

	* Data of price and volume is consolidated in day-by-day or minute-by-minute chunks

	* For each chunk, get open price, high price, close price and volumn

* Stock splits: When price is too high, company splits one share to several shares

	* Problem: Sell signal occurs but there is actually no drop in price

	* Solution: Use adjusted close price to account for these changes: divide past price by number of splits

![](Machine%20Learning%20for%20Trading%20-%20Part%202/Screen%20Shot%202018-11-09%20at%204.57.02%20PM.png)
* Dividends

	* Company pays dividends regularly to share owners, which can affect stock price

	* EX. Company will pay a dividend of $1 for each share. Now each share worths $100. What will happen to the stock price on the days before and after the dividend is paid?

	* On the day before dividend, the price will rise by $1, on the next day, the price will drop by $1

	* Adjust for dividends: Before the dividend is paid, adjust the price by the proportion of dividend. In this case, adjust all history price by 1% down

	*  Adjusted close price for same day under different time frames can be different

* Survivor bias: Select stocks based on today’s SP500, while 68% stocks died between 2007 and 2009

	* Survivor Bias Free data
	
## Efficient Market Hypothesis

* EMH Assumptions

	* Large number of investors

	* New information arrives randomly

	* Prices adjust quickly

	* Prices reflect all available information

* Origin of information

	* Price/Volume

	* Fundamental

	* Exogenous

	* Company insiders

* 3 Forms of the EMH

	* Weak: Future prices cannot be predicted by analyzing historical prices

	* Semi-strong: Prices adjust rapidly to new public information

	* Strong: Prices reflect all information public and private
	
## The Fundamental Law of active portfolio management

* Grinold’s Fundamental Law

	* Performance

	* Skill

	* Breadth

	* performance = skill * sqrt(breadth)

	* IR = IC * sqrt(BR)

* The coin flipping casino

	* Flip coins instead of stocks

	* The coin is biased - like Alpha 0.51 heads

	* Uncertainty is like Beta

	* Bet N coins. Win - now have 2N coins; Lose - now have 0

	* Casino: 1000 tables, 1000 tokens, game runs in parallel

	* Two extreme cases: Betting all 1000 tokens on one table (Single bet) vs betting 1 token on each table (Multi bet)

	* Expected reward of betting 1000 tokens on one table = expected reward of betting 1 token on each table = $20

	* Risk

		* Risk of losing it all: 0.49 vs 0.49^1000

		* Standard deviation of reward: 31.62 vs 1

	* Sharpe Ratio of Multi bet = 20 = Sharpe ratio of Single bet * sqrt(Number of Multi bet) = 0.63 * sqrt(1000), which is equivalent of performance = skill * sqrt(breadth)

	* Lessons

		* Higher Alpha generates higher Sharpe Ratio

		* More execution opportunities provides higher Sharpe Ratio

		* Sharpe ratio grows as sqrt(Breadth)

	* IR, IC and Breadth

		* Bp - Beta(p); Rm - r(m); Ap - Alpha(p)

		* Rp(t) = BpRm(t) + Ap(t); BpRm(t) - Market; Ap(t) - Skill

		* Information Ratio IR = mean(Ap(t)) / std(Ap(t))

		* IC: Information Coefficient (correlation of forecasts to returns)

		* BR: Breadth - number of trading opportunities per year

	* The Fundamental Law

		* IR = IC * sqrt(BR) 

		* IR: performance; IC: skill; BR: breadth

I'm not going to go through the rest of the course since I have got what I need from it, but content still seems to be great and everyone is encouraged to learn!