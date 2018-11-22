---
layout: post
title:  "Udacity - Machine Learning for Trading - Part 1"
date:   2018-11-2
desc: "Study notes"
keywords: "Python, Data Science, Machine Learning, Trading, Stock Market, Udacity"
categories: [Machine Learning]
tags: [Machine Learning, Python, Data Science, Trading, Stock Market, Udacity]
icon: icon-html
---
# Introduction

Recently I have just started working with a group of aspiring data scientists on a project through [Method Data Science](http://methoddatascience.com/), where we are tasked with building a trading algorithm that recommends optimal timing fot trading stocks for our client company: Indicate - AI For Trading. However I am fairly new to this field so I decided to begin by taking [Machine Learning for Trading](https://classroom.udacity.com/courses/ud501) course offered by Udacity. The course started by introducing data processing with Pandas and Numpy, which I am already familiar with. So I begin the course from Lesson 5, which introduced some concepts new to me.

I find taking notes while watching video to be a great way of learning from MOOC, and since much coding are involved in this course but the code only appeared in video, I decided to take notes and post here as I go through the course. It will be great if my notes can help anyone interested in learning this field!

Now let's begin!

## Statistical Analysis of Time Series

* Rolling Statistics

	* Means over a time window (10 days, 20 days…)
	
	* `pandas.stats.moments.rolling_mean(values, window)`


* Bollinger Bands
	
	* Two standard deviations away from the Rolling mean (Low, Up)

	* Buy signal: drop below Low

	* Sell signal: go over Up


* Daily returns
	
	* How much does prices go up and down between days

	* Daily return for day t: daily_ret[t] = (price[t] / price[t-1]) - 1

	* Computing in Python:


```
	daily_returns[1:] = (df[1:] / df[:-1].values) - 1
	# df as prices for each day
	daily_returns.ix[0, :] = 0
```
* Cumulative returns

	* Cumulative returns from day 0 to day t: cumret[t] = (price[t] / price[0]) - 1

	* Figure out the code yourself :p

	
## Incomplete Data

* Pristine Data

	* Messy, not necessarily accurate

	* Not all stocks trade

* Why data goes missing - what can we do?

	* Company get acquired, so the stock disappeared

	* Broken lines: some companies sometimes trading sometimes not trading

	* What can we do: _Fill forward_ from last valid value. For starting dates, _Fill backward_


	<img src="{{ site.img_path }}/MLforTrading/part1-1.png" width="40%">

	* Reason to do this: Avoid associating with future

	* Code in Python: `pandas.DataFrame.fillna()`

	* Fill forward: `fillna(method='ffill', inplace=TRUE)`

	* Fill backward:  `fillna(method='bfill', inplace=TRUE)`

	* Be sure to run _fill forward_ first and then _fill backward_

## Histograms and scatter plots

* A closer look at daily returns - Histograms

	* Distribution of daily returns values: standard deviation, mean, Kurtosis

	* Kurtosis: How different the distribution is from Gaussian distribution

	* Positive Kurtosis: The distribution has more values on tails than Gaussian distribution - fat tails

	* Negative Kurtosis: The distribution has fewer values on tails than Gaussian distribution - skinny tails

	* Calculating Kurtosis: `df.kurtosis`

	* Plotting in Python:

```
	# Plot histogram
	daily_returns['SPY'].hist(bins=20) # Bins can be set or used default
	# Add mean and std to the plot
	mean = daily_returns['SPY'].mean()
	std = daily_returns['SPY'].std()
	plt.axvline(mean, color='w', linestyle='dashed', linewidth=2)
	plt.axvline(std, color='r', linestyle='dashed', linewidth=2)
	plt.axvline(-std, color='r', linestyle='dashed', linewidth=2)
	plt.show()

	# Plot two histogram together
	daily_returns['SPY'].hist(bins=20, label='SPY')
	daily_returns['XOM'].hist(bins=20, label='XOM')
	plt.legend(loc='upper right')
	plt.show()
```

* A closer look at daily returns - Scatterplots 

	* Concepts: Tl; Dr

	* Plot SPY vs XOM in Python
	
* Real world use of Kurtosis

	* Issue with assuming normal distribution: Ignoring kurtosis

	* That’s it??
	
## Sharpe ratio and other portfolio statistics

* Daily portfolio values

	* Example portfolio: 

```
start_val = 1000000
start_date = 2009-1-1
end_date = 2011-12-31
symbols = ['SPY', 'XOM', GOOG', 'GLD']
allocs = [0.4, 0.4, 0.1, 0.1]

```
* How to calculate the total value of portfolios day by day?

	1. Normalize each day’s prices to first day

	2. Multiply by  allocations to get allocated values

	3. Multiply by initial investment to get the real value of investment each day over time

	4. Sum each row across columns to get portfolio value of each day

![](Machine%20Learning%20for%20Trading%20-%20Part%201/Screen%20Shot%202018-11-06%20at%201.28.15%20PM.png)
* Portfolio statistics

	* Daily return of the first day is 0

	* Key statistics: 

		* cumulative return 

		* average daily return 

		* std of daily return

		* Sharpe Ratio

* Sharpe Ratio: risk adjusted return

	* All else being equal, lower risk / higher return is better

	* Risk free rate of return approximation:

		* LIBOR

		* 3 month T-bill

		* 0%

	* Computing: SR = E(Rp - Rf) / std(Rp - Rf) = mean(daily_return - daily_rf) / std(daily_return)

	* Rp: Return rate on the portfolio, Rf: Risk free rate of return

	* Sharpe Ratio is an annual measure, which can vary widely depending on how frequently you sample 

	* Make adjustment if not sampled annually: SR_annualized = K * SR

		* K = sqrt(number of samples per year)

		* daily K = sqrt(252), monthly K = sqrt(12), weekly K = sqrt(52)
		
## Optimizers: Building a parameterized model

* What is an optimizer? Basic common knowledge

* Minimization example: Basic common knowledge again

* Minimizer in Python using scipy: `scipy.optimize.minimize(f, Xguess, method, options)`

* Convex problems: Basic calculus

* Building a parameterized model: Building a basic linear regression model



