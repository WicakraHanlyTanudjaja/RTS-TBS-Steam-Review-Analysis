# Topic-Based Sentiment Analysis of Steam Strategy Game Reviews

## 1. Project Overview

This repository contains the research implementation of a Topic-Based Sentiment Analysis framework for Steam strategy game reviews. The study analyzes player reviews from two strategy game subgenres, Real-Time Strategy (RTS) and Turn-Based Strategy (TBS), to identify recurring discussion topics and examine sentiment patterns associated with each topic.

The framework combines topic modeling and sentiment classification to transform large collections of unstructured player reviews into structured information about player experiences.

The research investigates two main components:

1. Topic modeling: comparing Latent Dirichlet Allocation (LDA) and BERTopic to identify meaningful discussion topics in Steam reviews.
2. Sentiment analysis: comparing VADER and Support Vector Machine (SVM) to evaluate sentiment classification performance and examine sentiment distributions across the identified topics.

RTS and TBS reviews are processed separately to preserve differences in their gameplay contexts and discussion patterns.

## 2. Research Objectives

The objectives of this research are:

- To compare LDA and BERTopic for discovering recurring topics in RTS and TBS reviews.
- To evaluate topic quality using coherence, topic diversity, outlier rate, and qualitative interpretability.
- To compare VADER and SVM for sentiment classification.
- To analyze positive and negative sentiment patterns within each identified topic.
- To compare discussion patterns and sentiment distributions between RTS and TBS games.
- To provide interpretable summaries of player feedback that support further research in game review analytics.

## 3. Dataset

### 3.1 Raw Dataset

The original dataset used in this research is:

`Steam_Game_Reviews_20260507_192259`

This dataset contains Steam game review records used as the starting point for data selection and preprocessing.

The relevant fields include review text, game identifiers, game names, recommendation labels, and supporting metadata. The `voted_up` field provides the recommendation label used as weak supervision for sentiment classification.

The exact collection procedure, review date coverage, and original dataset source should be documented alongside the dataset metadata to support reproducibility.

### 3.2 Dataset Separation

After data selection and preprocessing, the reviews are separated into two subgenres.

| Dataset | Total Reviews | Unique Games |
|---|---:|---:|
| Real-Time Strategy (RTS) | 33,306 | 796 |
| Turn-Based Strategy (TBS) | 63,897 | 1,055 |
| Total | 97,203 | 1,851 |

The two subgenres are analyzed independently rather than combined into a single topic model. This approach allows each subgenre to develop its own topic structure and sentiment profile.

## 4. Research Workflow

The overall workflow consists of the following stages:

1. Load the raw Steam review dataset.
2. Select relevant strategy game reviews and preprocess the text.
3. Separate the dataset into RTS and TBS subsets.
4. Evaluate LDA and BERTopic using candidate topic configurations.
5. Select the final BERTopic configurations based on quantitative metrics and qualitative interpretation.
6. Apply outlier reduction to improve topic coverage.
7. Generate separate topic-assigned review datasets for RTS and TBS.
8. Perform sentiment classification using VADER and SVM.
9. Evaluate classification performance and compare sentiment distributions across topics.
10. Interpret the resulting topic-based sentiment patterns.

## 5. Topic Modeling

### 5.1 LDA Baseline

Latent Dirichlet Allocation (LDA) is used as a traditional probabilistic topic modeling baseline. LDA represents documents as mixtures of topics, with each topic represented by a distribution over words.

Several candidate topic counts are evaluated to examine the relationship between topic quality and granularity.

### 5.2 BERTopic

BERTopic is used as an embedding-based topic modeling approach. It identifies groups of semantically related reviews and represents each topic using characteristic terms.

The selected configurations are:

| Subgenre | Number of Topics | Coherence | Topic Diversity | Initial Outlier Rate |
|---|---:|---:|---:|---:|
| RTS | 15 | 0.55 | 0.89 | 60.07% |
| TBS | 10 | 0.68 | 0.92 | 61.78% |

Although configurations with five topics achieved higher coherence and diversity scores in earlier experiments, they grouped several distinct discussion themes into broader categories. The final configurations prioritize topic granularity and interpretability for subsequent topic-based sentiment analysis.

The reported outlier rates represent the initial clustering results before the subsequent outlier-reduction procedure. Final outlier counts should be reported separately when the corresponding outputs are available.

### 5.3 Topic Interpretation

Topics are interpreted using their characteristic keywords and representative reviews. Descriptive labels are assigned to help readers understand the dominant discussion themes.

These labels are interpretive summaries of topic content. They do not establish that every review assigned to a topic discusses only that theme.

## 6. Topic-Assigned Review Datasets

After BERTopic modeling and outlier processing, the reviews are stored in separate datasets for subsequent sentiment analysis.

### RTS Dataset

`rts_steam_reviews_with_optimal_bertopics_20260628_134731`

This file contains the RTS reviews and their associated topic assignments generated by the selected BERTopic configuration.

### TBS Dataset

`tbs_steam_reviews_with_optimal_bertopics_20260628_134735`

This file contains the TBS reviews and their associated topic assignments generated by the selected BERTopic configuration.

These files serve as inputs for the separate RTS and TBS sentiment analysis stages.

The exact output columns, final topic assignment counts, and post-reassignment outlier counts should be verified against the corresponding files.

## 7. Sentiment Analysis

### 7.1 VADER

VADER is used as a lexicon-based sentiment classification baseline. It estimates sentiment polarity using a predefined sentiment lexicon and rule-based scoring.

The resulting sentiment labels are compared with the reference recommendation labels under the evaluation procedure used in the experiments.

### 7.2 Support Vector Machine

Support Vector Machine (SVM) is implemented using Term Frequency-Inverse Document Frequency (TF-IDF) text features.

The Steam `voted_up` field is used as a weak supervision label:

- `True`: the review recommends the game.
- `False`: the review does not recommend the game.

SVM learns to predict these recommendation labels from textual features. Therefore, evaluation against `voted_up` measures recommendation-label prediction performance and should not automatically be interpreted as independently validated textual sentiment accuracy.

### 7.3 Evaluation Metrics

The sentiment classification experiments use the following metrics:

- Accuracy
- Precision
- Recall
- F1-score
- Macro F1-score

McNemar's test is also used to compare paired classifier predictions. The test statistics, discordant prediction counts, and p-values should be verified before final publication.

## 8. Sentiment Classification Results

The current reported overall results are summarized below.

| Subgenre | Model | Accuracy | Macro F1 |
|---|---|---:|---:|
| RTS | VADER | 78.36% | 67.25% |
| RTS | SVM | 91.21% | 88.35% |
| TBS | VADER | 80.59% | 67.66% |
| TBS | SVM | 91.29% | 87.09% |

SVM achieves higher reported accuracy and Macro F1 than VADER in both subgenres under the existing evaluation setup.

These results should be interpreted in the context of the evaluation labels, class distribution, and train-test protocol. Further evaluation using independently annotated textual sentiment labels would help establish how accurately the models identify sentiment expressed in review text.

## 9. Topic-Based Sentiment Analysis

The final analysis associates sentiment predictions with BERTopic-assigned review groups.

For each topic, the analysis examines:

- Number of assigned reviews.
- Positive and negative sentiment distributions.
- Representative review examples.
- Differences in discussion patterns between RTS and TBS.

For example, the current RTS results show that General Strategy contains 14,161 reviews, with 90.55% classified as positive. Multiplayer contains 3,279 reviews, with 57.12% classified as positive, while Technical Performance contains 1,449 reviews, with 58.59% classified as positive.

For TBS, Turn-Based Tactics contains 41,041 reviews, with 85.89% classified as positive. Card and Deckbuilding contains 19,503 reviews, with 76.18% classified as positive.

These percentages describe the sentiment distribution of reviews assigned to each topic. Because a review may discuss multiple gameplay aspects while receiving only one topic assignment, the results should be interpreted as topic-conditioned review sentiment rather than independently validated sentiment toward every aspect mentioned in a review.

## 10. Interpretation and Practical Relevance

The framework provides a structured way to explore player feedback beyond aggregate recommendation statistics.

The topic distributions reveal which themes dominate discussion within each strategy subgenre. Sentiment distributions then provide an additional perspective on how reviews assigned to those topics relate to positive or negative recommendations.

The results help identify topics that warrant further investigation, particularly when a topic combines a large review volume with a comparatively low positive sentiment proportion.

However, sentiment percentages alone do not establish causal relationships between game features and player satisfaction. Representative reviews and independent validation are necessary for stronger conclusions about specific gameplay elements.

## 11. Repository Reproducibility

For reproducibility, the repository should document:

- Dataset source and collection dates.
- Review selection and preprocessing procedures.
- Game classification criteria for RTS and TBS.
- Topic modeling parameters and selected configurations.
- BERTopic outlier-reduction method and settings.
- Topic labels and representative review selection.
- SVM feature extraction and training parameters.
- Train-test split strategy and random seed.
- Evaluation metrics and statistical test settings.
- Software dependencies and execution instructions.

Large datasets should be distributed only when licensing, privacy, and source-platform requirements permit. Otherwise, provide the dataset acquisition procedure and the scripts required to reproduce the analysis.

## 12. Research Scope and Limitations

This research focuses on English-language Steam reviews from RTS and TBS games. Its results depend on the selected games, available reviews, preprocessing decisions, topic assignments, and sentiment classification procedures.

Steam recommendation labels are used as weak supervision rather than as independent ground-truth sentiment annotations. Topic labels are also assigned through interpretation of keywords and representative reviews.

Future work should validate topic assignments and sentiment labels using human annotations, investigate reviews that contain multiple topics or mixed sentiment, and assess model generalization across unseen games.

## 13. Conclusion

This repository documents a topic-based sentiment analysis framework that integrates topic modeling and sentiment classification for Steam strategy game reviews. By analyzing RTS and TBS separately, the framework produces subgenre-specific topic structures and sentiment summaries.

The experiments compare LDA with BERTopic and VADER with SVM, providing a basis for evaluating alternative methods for organizing and interpreting large-scale player feedback.

The resulting topic-based sentiment profiles support further investigation of player review patterns and provide a foundation for future research in game review analytics.

## Citation

**Paper title:** Topic-Based Sentiment Analysis of Player Reviews in Strategy Games Using BERTopic and Support Vector Machine

**Authors:** [Wicakra Hanly Tanudjaja]

**Publication:** [-]
