# Amazon_Vine_Analysis
Module 16

## Overview of Analysis
The purpose of this analysis is to perform ETL on a dataset of Amazon product reviews and then to determine the bias of Vine Reviews. SellBy, an online retailer, pays a small fee to Amazon to provide products to Amazon Vine members to review. The dataset includes reviews from all consumers; paid (Vine) and unpaid. The dataset chosen for this analysis was specific to musical instruments which can be accessed [here](https://s3.amazonaws.com/amazon-reviews-pds/tsv/amazon_reviews_us_Musical_Instruments_v1_00.tsv.gz). The analysis evaluates if there could be five-star review bias from the Amazon Vine members in the musical instruments dataset.

## Results

In order to analyze the musical instruments Amazon product reviews, the dataset was extracted from an AWS S3 bucket and then transformed using PySpark in a Google Colab Notebook. The `review_id`, `star_rating`, `helpful_votes`, `total_votes`, `vine`, and `verified_purchase` columns were selected from the original dataframe and then filtered to display only the rows with `total_votes` greater than or equal to 20. This subset was further filtered by the rows containing a ratio of `helpful_votes/total_votes` that was greater than or equal to 50%. This filtered out reviews that had less online interaction overall. This dataframe (named `helpful_votes_df`) was then utilized to answer the following questions:

### How many Vine reviews and non-Vine reviews were there?
To determine how many Vine and non-Vine reviews there were in this data subset, two filtered dataframes were made. The following code generated a dataframe (`vine_paid_df`) which filters for the Vine reviews only:

![vine_paid_df](https://user-images.githubusercontent.com/107309793/193490978-e63ac4f8-d4b7-466c-8d03-be9a08fa7b95.png)

The non-Vine reviews dataframe was also created:

![vine_unpaid_df](https://user-images.githubusercontent.com/107309793/193491033-0ec334e5-d367-40e1-94fa-afe103b30834.png)

It was then possible to count the rows of each of these tables and to determine the number of Vine and non-Vine reviews using the following code:

![paid_unpaid_reviews](https://user-images.githubusercontent.com/107309793/193491090-d299af85-cf5f-476b-9867-a8eed2715060.png)

Therefore, there are **60 Vine reviews** and **14,477 non-Vine reviews** in this dataset.

### How many Vine reviews were 5 stars? How many non-Vine reviews were 5 stars?
In order to determine the number of Vine reviews with 5 stars and non-Vine reviews with 5 stars, the same `vine_paid_df` and `vine_unpaid_df` dataframes depicted above were filtered for only the rows when the `star_rating == 5`. These rows were then counted. Here is a screenshot of the code:

![paid_unpaid_fivestar_reviews](https://user-images.githubusercontent.com/107309793/193711753-99dc2811-6dbf-453c-a3e1-36c1c4abf311.png)

Therefore, **34 Vine reviews** and **8,212 non-Vine reviews** were 5 stars.

### What percentage of Vine reviews were 5 stars? What percentage of non-Vine reviews were 5 stars?

In order to answer this question, the total number of five-star reviews overall had to be determined. This was completed by filtering the `helpful_votes_df` using the `star_rating == 5` condition. This resulted in 8,246 five-star reviews as shown below:

![fivestar_reviews](https://user-images.githubusercontent.com/107309793/193725750-3b31c9b8-2cd0-4f6f-9a99-f1ebe8315205.png)

The Vine and non-Vine five-star review percentages could then be calculated by dividing the number of five-star Vine (or non-Vine) reviews over the total number of five-star reviews overall and then multiplying by 100. Here is the result:

![paid_unpaid_fivestar_percents](https://user-images.githubusercontent.com/107309793/193725962-b070999f-02d9-4758-b883-cd8c344adbea.png)

Therefore **0.41%** of the 5 star reviews were from **Vine members** and **99.59%** were from **non-Vine customers**.

## Summary

### Is there any positivity bias for reviews in the Vine program?
Considering the sample analyzed in this dataset as a whole (14,537 musical instrument reviews), only **60** of the reviews were from Amazon Vine members and the other **14,477** reviews were from regular customers. The overall contribution to the overall reviews is not substantial enough to introduce significant bias.

When considering the five-star subset of this sample (8,246 five-star reviews), a similar trend is seen; only **0.41%** of the 5 star reviews were from Vine members while the other **99.59%** were from non-Vine customers. This further supports the evidence that the vast majority of the positive reviews on the musical instruments are coming from the non-Vine customer base, not the Vine members. The lack of five-star reviews coming from Vine-members might actually indicate that these reviewers are rating with far more scrutiny than most customers.

### One Additional Analysis for the Dataset as Further Support
To further support this statement it would be worth incorporating **natural language processing (NLP)** on the `review_headline` and `review_body` columns which appear in the original dataframe. These columns contain the actual content of each review. The text from these reviews could undergo the **NLP pipeline** (raw text, tokenization, stop words filtering, TF-IDF, and machine learning) in order to ascertain which reviews are positive or negative based on a **Naive Bayes model**. Likely the model will show similar information - that the majority of positive reviews are coming from the non-Vine customer base.
