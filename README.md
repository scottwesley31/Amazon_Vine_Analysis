# Amazon_Vine_Analysis
Module 16

## Overview of Analysis
The purpose of this analysis is to perform ETL on a dataset of Amazon product reviews and then to determine the bias of Vine Reviews. SellBy, an online retailer, pays a small fee to Amazon to provide products to Amazon Vine members to review. The dataset includes reviews from all consumers; paid (Vine) and unpaid. The dataset chosen for this analysis was specific to musical instruments which can be accessed [here](https://s3.amazonaws.com/amazon-reviews-pds/tsv/amazon_reviews_us_Musical_Instruments_v1_00.tsv.gz). The analysis evaluates if there could be five-star review bias from the Amazon Vine members in the musical instruments dataset.

## Results

In order to analyze the musical instruments Amazon product reviews, the dataset was extracted from an AWS S3 bucket and then transformed using PySpark in a Google Colab Notebook. The `review_id`, `star_rating`, `helpful_votes`, `total_votes`, `vine`, and `verified_purchase` columns were selected from the original dataframe and then filtered to display only the rows with `total_votes` greater than or equal to 20. This subset was further filtered by the rows containing a ratio of `helpful_votes/total_votes` that was greater than or equal to 50%. This filtered out review that had less online interaction overall. This dataframe (named `helpful_votes_df`) was then utilized to answer the following questions:

### How many Vine reviews and non-Vine reviews were there?
To determine how many Vine and non-Vine reviews there were in this data subset, two filtered dataframes were made. The following code generated a dataframe (`vine_paid_df`) which filters for the Vine reviews only:

![vine_paid_df](https://user-images.githubusercontent.com/107309793/193490978-e63ac4f8-d4b7-466c-8d03-be9a08fa7b95.png)

The non-Vine reviews dataframe was also created:

![vine_unpaid_df](https://user-images.githubusercontent.com/107309793/193491033-0ec334e5-d367-40e1-94fa-afe103b30834.png)

It was then possible to count the rows of each of these tables and to determine the number of Vine and non-Vine reviews using the following code:

![paid_unpaid_reviews](https://user-images.githubusercontent.com/107309793/193491090-d299af85-cf5f-476b-9867-a8eed2715060.png)

Therefore, there are 60 Vine reviews and 14,477 non-Vine reviews in this dataset.

### How many Vine reviews were 5 stars? How many non-Vine reviews were 5 stars?
In order to determine the number of Vine reviews with 5 stars and non-Vine reviews with 5 stars, the same `vine_paid_df` and `vine_unpaid_df` dataframes depicted above were filtered for only the rows when the `star_rating == 5`. These rows were then counted. Here is a screenshot of the code:

![paid_unpaid_fivestar_reviews](https://user-images.githubusercontent.com/107309793/193711753-99dc2811-6dbf-453c-a3e1-36c1c4abf311.png)

Therefore, 34 Vine reviews and 8,212 non-Vine reviews were 5 stars.

### What percentage of Vine reviews were 5 stars? What percentage of non-Vine reviews were 5 stars?

## Summary
