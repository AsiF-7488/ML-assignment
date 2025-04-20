# ML-assignment
1. Data Ingestion

Two datasets are loaded:
depth_df: LOB snapshots (20 levels deep for bid & ask).
trade_df: Aggregated trade data (buy/sell volumes, timestamps).
Timestamps are cleaned and converted to proper datetime format, truncating microseconds to 6 digits.


2. Feature Engineering

From depth_df and trade_df, the pipeline computes key microstructure features:

Mid Price: Average of best bid and best ask.
Spread: Difference between best ask and best bid.
Order Book Imbalance: Quantifies buy/sell pressure at top level.
Microprice: Price weighted by top-of-book quantity.
Log Returns: Based on mid price; used for volatility.
Rolling Volatility: Over 10 and 30-second windows.
Volume Imbalance: Net difference between buyer- and seller-initiated volume.
Cumulative Volume: Summed over recent intervals (10s, 30s).
This stage transforms raw LOB data into a structured, feature-rich representation of market dynamics.

3. GPU-Based Clustering with cuML

Features are standardized using StandardScaler (CPU).
Transformed to CuPy arrays and clustered using cuML.KMeans on GPU.
6 market regimes are identified based on unsupervised clustering.
The result is a new regime label for each timestamp.


4. Visualization

t-SNE (on CPU) projects high-dimensional data into 2D space, colored by cluster to visualize regime separation.
Time-Series Plot shows how regimes evolve over time on top of mid-price data.


5. Transition Matrix Analysis

A Markov-style transition matrix is computed from the cluster labels.
Each row i represents the probability of moving from regime i to another regime j.
This quantifies the persistence and shift between market regimes, useful for strategy design.
Insights and Applications
This pipeline helps in understanding market microstructure patterns, detecting shifts in liquidity or volatility, and clustering similar trading conditions.
Can be extended for real-time trading systems, volatility forecasting, or reinforcement learning environments.
