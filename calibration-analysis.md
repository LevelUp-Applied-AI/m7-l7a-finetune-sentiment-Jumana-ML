# Module 7 Week A — Calibration Analysis

## Reliability Diagram Interpretation
The saved reliability diagram indicates that the fine-tuned DistilBERT model is generally **over-confident**. Most accuracy bars fall below the dashed identity line, meaning the model's self-reported confidence is higher than its empirical accuracy. 

For instance, in the **0.8–0.9** confidence bucket, the model shows an accuracy of approximately **0.75**, confirming that it overestimates its probability of being correct in this range. However, in the highest confidence bucket (**0.9–1.0**), the accuracy is close to **0.95**, showing better calibration when the model is extremely certain.

## Expected Calibration Error (ECE)
The calculated Expected Calibration Error (ECE) is **0.0522**. 

An ECE of ~5.2% is relatively low, suggesting the model is fairly trustworthy for general use. However, for a production environment where probability scores are used to trigger downstream actions (like automated customer support tickets), this gap means we cannot take the raw softmax probabilities as literal probabilities of correctness without slight adjustments.

## A Specific Calibration Pattern
A clear pattern of **over-confidence is observed in the middle-to-high confidence ranges (0.4 to 0.9)**. This pattern often arises when a model is trained on a dataset where certain classes (like Neutral reviews) have ambiguous boundaries. The model learns to favor specific features and assigns them high probability scores, even when the underlying sentiment is not distinct enough to justify such certainty.

## A Proposed Engineering Action
Based on these findings, I propose implementing **Temperature Scaling** as a post-processing step. By applying a learned scalar (temperature) to the logits before the softmax layer, we can "soften" the probability distribution. This would push the confidence scores lower to better align with the actual accuracy, effectively closing the 5.2% calibration gap and making the model's confidence scores more representative of reality.