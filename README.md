# prophet-challenge
Module 8 Challenge
In this challenege we will find the closing price for stocks compared to the search trends of a website drop.
Step 1 Find Unusual Paaterns in Hourly Google Search Traffic
-------------------------------------------------------------------------------------------------------------
First we read the DataFrame from the csv file and see the last five and beginning 5 in the Search Trends.
We do info to gather more information about the columns. SLice the data to get everything for the month of 
May 2020. Plot our results.

In step 2 we get the sum total of all searches for the month of May 2020. Next we groupby the index year and 
index month to get the sum total of all the months within the years. We then find the median for all the months 
and after that we divide the may traffic by the monthly median traffic. 

Step 2 Mine Search Traffic Data for Seasonality
-------------------------------------------------------------------------------------------------------------
We plot our index hour from our groupby, take the mean and see the graph:

![alt text](/images/prophet1.PNG)

The next plot was to see our search trends data by the day using the isocalendar().day within the groupby call
and take the mean value from that:

![alt text](/images/prophet2.PNG)

The next graph would be for our weeks using isocalendar().week and take the mean value of that:

![alt text](/images/prophet3.PNG)

Step 3 Relate the Search Traffice to Stock Price Patterns
-------------------------------------------------------------------------------------------------------------
We now load a new csv file and this one is for the stock closing prices. We display the head() and tail() to 
see the first five observations and last five observations. We go ahead and print the plot() of out closing 
prices to see on the graph what the stock is doing: 

![alt text](/images/prophet4.PNG)

we now combine using the method of concat() the search trends DataFrame and the stock closing DataFrame we use axis=1 
to look at the rows and dropna() to drop any rows that have NaN values. We take that DataFrame and create a new one 
that searches the first half of the year for 2020. once we see the first half of the year we plot() the results
with two plots using .plot(subplots=True):

![alt text](/images/prophet5.PNG)

using this new concat_mercado dataFrame we then make a new column named 'Lagged Search Trends' which uses the 
search trends column and shifts it by 1 row or hour in our case. Next is to creat another column named
'Stock Volatility' which this one I had trouble with I used that was not the solution. The line below works
but that is for the absolute value and we were seeing everything in the tenths and not the thousandths.

concat_mercado['Stock Volatility'] = concat_mercado['close'].rolling(window=4).std()

This one is the correct solution: 

concat_mercado['Stock Volatility'] = concat_mercado['close'].pct_change().rolling(window=4).std()

Huge differnce between the two with relation to the decimal value. After we completed the previous steps we
then plot our results:

![alt text](/images/prophet6.PNG)

We then create the create 'Hourly Stock Return' where we will reference the close value pct_value() which
does all the heavy lifting for us. The last step is printint out a correlation between the columns and see how 
strong the correlation is. The answer was they were all weak correlations:

![alt text](/images/prophet7.PNG)

Step 4 Create a Time Series Model with Prophet
-------------------------------------------------------------------------------------------------------------
Too start we look at the search trends data frame columns, dtype=object. What we do next is reset the index
rename the columns and dropna() since we called the original DataFrame:

prophet_df = prophet_df.rename(columns={"Date": "ds", "Search Trends": "y"}).dropna()

We call the Prophet function and save it under model. we then model.fit to fit the time-series model.Then 
create a 2000 hour future prediction with:

future_mercado_trends = model.make_future_dataframe(periods=2000, freq='H')

We use model.predict to predict what the future outcomes would be. Plot the model with model.plot(prediction)
which give us two graphs: 

![alt text](/images/prophet8.PNG)

1[alt text](/images/prophet9.PNG)

Once we see the graphs we then take those values and print out the DataFrame:

![alt text](/images/prophet10.PNG)

We then set the index to ds to have the date_time back, print out a new DataFrame only using the 'yhat', 'yhat_lower'
'yhat_upper'. Then we print the last 2000 hours of our prediction and use the yhat columns to make our plot():

![alt text](/images/prophet11.PNG)

Then we get to our last plot(), we plot the forecast for the canada results in our fig=model.plot_components() and 
we get a series of four plots:

![alt text](/images/prophet12.PNG)
![alt text](/images/prophet13.PNG)