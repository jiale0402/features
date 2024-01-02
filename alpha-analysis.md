1. Alpha#1: (rank(Ts_ArgMax(SignedPower(((returns < 0) ? stddev(returns, 20) : close), 2.), 5)) - 0.5)

This alpha seems to be used for the situation when returns are negative and we want to find the most volatile to least volatile stocks/commodity. The alpha starts off with a condition checking if the returns are negative for a commodity today. If they are, it calculates the rolling standard deviation of the past 20 days and then squares each value to get the variance. Then the alpha finds the highest variance in the last 5 days of our rolling variance time series by the function Ts_ArgMax. Then the alpha will rank all our concerned commodities based on their volatility/variance. Finally it subtracts - 0.5, probably to normalize the data.

In the case when returns are positive, the alpha just uses the closing price of the commodity, squares it, and then ranks and normalizes it, I'm unsure of the phenomenon this situation is trying to tackle as I don't know what the squared of closing price represents.

Numpy functions to use: 
np.std for standard deviation
np.power for squaring
np.nanmax for the ts_argmax part
np.argsort for ranking

2. Alpha#2: (-1 * correlation(rank(delta(log(volume), 2)), rank(((close - open) / open)), 6)) 

This alpha aims to find the correlation over a 6 day period between 2 factors:
a. percentage change in stock price
b. Logarithm of the 2 day volume difference of the stock
It ranks both these factors for the stock so they can be compared. Finally it multiplies the correlation by -1 to invert the sign which could be for many different reasons.

Numpy functions to use: 
np.argsort for ranking
np.log for getting the logarithm
np.corrcoeff for correlation
np.diff for delta


3. Alpha#3: (-1 * correlation(rank(open), rank(volume), 10))

This is very similar to #2, this alpha aims to find the correlation over a 10 day period between 2 factors:
a. Stock opening price
b. Stock volume
It ranks these factors and then finds the correlation which is finally inverted. It's trying to indicate a relationship about what happens when we have high/low price opening and how it impacts the volume.

Numpy functions to use: 
np.corrcoeff for correlation
np.argsort for ranking


4. Alpha#4: (-1 * Ts_Rank(rank(low), 9)) 

This alpha makes a time series ranking based on a stocks lowest price. It might be trying to spot undervalued stocks that could revert to a mean.
It firstly ranks stocks based on their lowest price they went to. Then it produces a time series over 9 days for all the stocks. It then multiplies the series by -1, which could be due to different reasons like a high positive value being undesirable in this strategy.

Numpy functions to use: 
np.argsort for ranking

6. Alpha#6: (-1 * correlation(open, volume, 10)) 

This alpha finds the correlation between th opening price and trading volume over 10 days and then inverts it. A positive correlation would show high interest in trading at higher prices with a higher trading volume and a negative correlation shows low interest to trade at high prices.

Numpy functions to use: 
np.corrcoeff for correlation

