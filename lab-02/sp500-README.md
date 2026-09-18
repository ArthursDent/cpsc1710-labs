# The 10:30 Classifier

Open `sp500-classifier.html` directly in Chrome, Edge, or Firefox. No installation or API keys are required. Adjust the three sliders, inspect contributions, and try the easy, near-boundary, and conflicting examples.

## Question and labels

Using information available by 9:25 a.m. America/New_York, predict whether the S&P 500 index at 10:30 is above (1) or below (0) the previous trading day's close. Exact ties are excluded. This differs from predicting a positive return after buying at the open.

## Model

Logistic regression uses overnight futures return, the previous day's index return, and the previous day's VIX close. It standardizes inputs with training-only means and population standard deviations, then trains for 1,500 full-batch gradient steps with learning rate 0.08 and L2 coefficient 0.02 on weights (not the intercept). The class boundary is 0.5. The first 70% of ordered examples train the model; the remaining 30% evaluate it. No hyperparameter selection is performed. The futures-sign and always-above rules are comparison baselines. Brier score is mean squared error of predicted probability against the binary label.

## Data and limitations

The default 240 examples are reproducible synthetic data with an intentionally planted relationship between inputs and labels. Their performance says nothing about real markets. CSV import supports historical experiments; no historical data are bundled or fetched. CSV must be plain unquoted numeric data with the exact header shown in the page. Inputs are percentages in percentage points (0.5 means 0.5%). Futures return uses the same contract from the previous session's 4:00 p.m. ET quote to today's 9:25 a.m. ET quote. Account for contract rolls and use consistently sampled prices. Previous return uses the two preceding trading closes. Do not use today's VIX close or any post-cutoff feature. Input timestamps, holiday coverage, and provenance require independent verification.

Probabilities are not independently calibrated. Small test sets, repeated inspection of test results, regime changes, extrapolation, and data leakage can all mislead. A trading experiment needs a tradable instrument's entry/exit prices and transaction costs; this page does not calculate profit or recommend trades.

The assignment's Stop 3 says to avoid financial or other high-stakes decisions. This is an educational experiment; confirm topic suitability with your instructor before submitting it for that assignment.

## Development log

- Requested a classifier for the S&P 500 one hour after the open.
- Clarified that positive/negative is relative to the previous day's close.
- Built an interactive logistic-regression experiment with visible evidence, simulated examples, chronological evaluation, and optional CSV input.

## User testing still to do

Record your own observations for the three preset cases and ask a classmate to try the page. Do not claim synthetic results as market results. Write the assignment's personal reflection yourself.
