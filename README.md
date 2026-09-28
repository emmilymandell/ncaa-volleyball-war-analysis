# NCAA Division I Volleyball Expected Points Added (EPA) & Wins Above Replacement (WAR) Model
An advanced analytics framework that adapts baseball and football analytical techniques (Expected Points & Wins Above Replacement) to NCAA Division I women’s volleyball play-by-play data.

### [View the Interactive Project Report Live Here](https://emmilymandell.github.io/ncaa-volleyball-war-analysis/)

## Project Overview

Traditional volleyball statistics, such as hitting percentage, total blocks, and digs, fail to capture in game context. Traditional metrics also suffer from position-dependent bias (e.g., setters receive few attack opportunities; middle blockers hit high-efficiency, low-volume sets), making it difficult to truly measure how much a player contributes to their team’s victories.

This project develops:

1. **Expected Points (EP) Model**: Evaluates the state of a rally/match to assign a expected point value to every event (serve, pass, set, attack, block, dig).

2. **Expected Points Added (EPA)**: Measures the net value contributed by an individual player or team on every touch.

3. **Wins Above Replacement (WAR)**: A comprehensive metric quantifying a player’s total contribution relative to a position-specific baseline replacement player.

## Methodology & Pipeline
### 1. Data Processing
* **Dataset**: NCAA Division I Women’s Volleyball play-by-play data.
* **Feature Engineering**: Tracks match state transitions, including score differential, server/receiver orientation, sub-state in rally, and play type sequence (Serve $\rightarrow$ Receive $\rightarrow$ Set $\rightarrow$ Attack $\rightarrow$ Block/Dig).

### 2. Expected Points (EP) Model
* **Model Choice**: Multi-variable Logistic Regression / XGBoost Classifier predicting the probability of winning the current point given the rally state ($S$).
* **Rally State Vectors**:
  $$\text{State } S = (\text{Point Differential}, \text{Serve/Receive Status}, \text{Rally Contact Number}, \text{System Quality})$$
* **Expected Value**:
  $$EP(S) = P(\text{Win Point} \mid S)$$

### 3. Expected Points Added (EPA)
For any touch changing the state from $S_{\text{start}}$ to $S_{\text{end}}$:
$$\Delta EP = EP(S_{\text{end}}) - EP(S_{\text{start}})$$
* **Terminal Events**:
  * Kills / Aces: $EP(S_{\text{end}}) = +1.0$
  * Attack Errors / Service Errors: $EP(S_{\text{end}}) = 0.0$

### 4. Wins Above Replacement (WAR)
* **Position Grouping**: Outside Hitter (OH), Opposite/Right Side (OPP), Middle Blocker (MB), Setter (S), and Libero/Defensive Specialist (L/DS).
* **Replacement Level**: Defined as the $20\text{th}$ percentile performance tier among qualified players at each position in NCAA Division I.
* **Conversion Factor**: Total Marginal EPA converted to wins via empirical points-to-wins relationship.

$$\text{WAR} = \frac{\text{Total EPA}_{\text{player}} - \text{Replacement EPA}_{\text{position}}}{\text{Points per Win}}$$

## Tech Stack & Analytical Ecosystem
* Language: R (v4.3+)
* Primary Libraries: 
  * `tidyverse` (Data manipulation, sequence parsing, and group-nested player aggregation)
  * `xgboost` / `mgcv` (Gradient boosting and GAMs for modeling rally winning probabilities and EP calibration)
  * `ggplot2` (Custom thematic visualizations, player EPA cards, and WAR distribution plots)
  * `scales` (Formatting plotting axes, probability thresholds, and percentage metrics)

## How to Reproduce this Analysis
To clone this repository and reproduce the statistical summaries locally:

1. Clone the repository:
   ```bash
   git clone [https://github.com/emmilymandell/ncaa-volleyball-war-analysis.git](https://github.com/emmilymandell/ncaa-volleyball-war-analysis.git)