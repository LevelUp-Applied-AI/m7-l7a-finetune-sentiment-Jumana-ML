# Module 7 Week A — Lab Evaluation Report

## Dataset
The model was fine-tuned on the **AARSynth app reviews** dataset, containing **7,472 examples**. The dataset is divided into three sentiment classes: Negative (0), Neutral (1), and Positive (2). I used an 80/20 split, resulting in **5,977 training examples** and **1,495 test examples**.

## Model and hyperparameters
- **Backbone:** `distilbert-base-uncased`
- **Number of labels:** 3
- **Learning rate:** `5e-5`
- **Epochs:** `2`
- **Batch size:** `8`
- **Max_length:** `128`
- **Seed:** `42`
- **Training time (wall-clock):** Approximately **72 minutes** (4331 seconds) on local CPU.

## Metrics on the test split

### Aggregate:
| Metric | Value |
|---|---|
| Accuracy | 0.6067 |
| Macro-F1 | 0.6125 |

### Per class:
| Class | F1 | Precision | Recall |
|---|---|---|---|
| Negative | 0.697 | 0.727 | 0.669 |
| Neutral  | 0.519 | 0.437 | 0.637 |
| Positive | 0.621 | 0.768 | 0.521 |

## Confusion matrix
| | Pred: Negative | Pred: Neutral | Pred: Positive |
|---|---|---|---|
| **True: Negative** | 334 | 153 | 12 |
| **True: Neutral** | 96 | 295 | 72 |
| **True: Positive** | 29 | 226 | 278 |

*Interpretation:* The model struggles significantly with distinguishing between **Neutral** and **Positive** classes. The largest error source is 226 Positive reviews being misclassified as Neutral.

## Three qualitative error examples

### 1. Mixed Sentiment (Positive as Neutral)
- **Sentence:** "good, but slow workflow."
- **Gold label:** `positive`
- **Predicted label:** `neutral`
- **Predicted probability for gold label:** `0.2297`
- **Analysis:** The sentence contains both a positive word ("good") and a negative/neutral constraint ("slow workflow"). The model likely gave more weight to the latter part of the sentence, leading to a neutral prediction.

### 2. Semantic Ambiguity (Neutral as Positive)
- **Sentence:** "nice app to use with friends"
- **Gold label:** `neutral`
- **Predicted label:** `positive`
- **Predicted probability for gold label:** `0.2685`
- **Analysis:** Words like "nice" and "friends" carry strong positive connotations. The model interpreted the semantic tone correctly as positive, but the gold label in the dataset was neutral, creating a mismatch.

### 3. Spelling/Noise Error (Neutral as Negative)
- **Sentence:** "everthing is tought before its use"
- **Gold label:** `neutral`
- **Predicted label:** `negative`
- **Predicted probability for gold label:** `0.3669`
- **Analysis:** The typo "tought" and the overall vague structure of the sentence make it difficult to categorize. In the absence of clear positive markers, the model defaulted to a negative classification.

## Hugging Face Hub model URL
[https://huggingface.co/Jumana6Mo/m7-app-review-sentiment](https://huggingface.co/Jumana6Mo/m7-app-review-sentiment)