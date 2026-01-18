# klypto_ML_Task
# Nifty 50 Quant Trading Strategy (End-to-End)

This repo contains a complete algorithmic trading pipeline built for the **Nifty 50 Index**.

The goal was simple: take a standard trend-following strategy and make it smarter using Unsupervised Learning (Market Regimes) and Machine Learning (Signal Filtering)
# What This Project Does

Most strategies fail because they treat every market condition the same way. I built this system to:
1.  Identify the "Vibe" of the Market: Using Gaussian Mixture Models (GMM) to classify the market as *Uptrend*, *Downtrend*, or *Sideways*.
2.  Trade the Trend: A baseline EMA Crossover strategy works as the signal generator.
3.  **Filter the Noise:** A Random Forest classifier looks at volatility and basis to say "No" to bad trades before they happen.

 # The tools used
* Language: Python 
* Data Analysis: Pandas, NumPy
* Machine Learning: Scikit-learn (GMM, Random Forest)
* Visualization: Matplotlib, Seaborn


 # How Pipline Works (The Pipeline)

1. Data Processing
I used 5-minute interval data for **Nifty Spot** and **Nifty Futures**.
* Challenge: The timestamps didn't match perfectly.
* Result: Cleaned and merged datasets on an inner join, handled timezones, and used Forward Fill to plug small data gaps.

2. Feature Engineering
Instead of just using price, I engineered features to feed the models:
* Basis:The premium/discount between Futures and Spot prices (Sentiment indicator).
* Volatility: Rolling standard deviation (Risk indicator).
* Trend Strength: The spread between Fast (5) and Slow (15) EMAs.

3. Regime Detection (The "Unsupervised" Part) 
# The task was to use hmm but unfortunately due to error in my computer installation I used gmm for similar regime detection
I used *GMM (Gaussian Mixture Models)* to cluster market data into 3 hidden states.
 State 1: High Momentum (Uptrend)
 State 2: Negative Momentum (Downtrend)
 State 3: Low Volatility (Sideways/Choppy)

4. Strategy & Backtest
* Logic:Buy when EMA 5 > EMA 15 *only if* we are in a Bullish Regime. Sell when EMA 5 < EMA 15 *only if* we are in a Bearish Regime.
* Result: It worked, but had some false positives during regime transitions.

5. ML Enhancement 
I trained a **Random Forest Classifier** to look at the winners and losers from the backtest.
* It learned which conditions (high volatility + weak trend) usually lead to losses.
* Outcome: The model filters out low-confidence trades, significantly boosting the Win Rate.

# Key Results

* Regime Detection: Successfully separated trending moves from choppy noise.
* Outlier Analysis: Found that the biggest wins (>3 Sigma) happen during high-volatility expansions, not quiet periods.
* Performance: The ML filter reduced the total number of trades but drastically improved the quality of entries.



#Project Structure

 data                   # Contains the cleaned CSVs
notebooks               # Jupyter Notebooks with the code
images                  # Charts and graphs (PnL curves, Regime plots)
README.md               # This file
