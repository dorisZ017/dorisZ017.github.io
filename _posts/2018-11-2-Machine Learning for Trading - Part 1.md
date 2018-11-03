---
layout: post
title:  "Notes for Udacity - Machine Learning for Trading"
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

	<img src="{{ site.img_path }}/MLforTrading/part1-1.png" width="30%">

	* Reason to do this: Avoid associating with feature
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
* To be continued...


