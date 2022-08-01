## Amazon Vine Analysis for Videos

## Overview
An ETL process is performed on an Amazon Reviews Dataset for videos reviewed under the Vine Program. This dataset is stored in an AWS S3 bucket. We process the data into 4 dataframes that we upload into our Postgres database. 

The backend of our database is connected to the AWS Postgres RDS. We create the following tables on our database:

customers_table - lists all unique customer ids and the number of reviews they made
![customers_table](https://github.com/namin1993/Amazon_Vine_Analysis/blob/6545ff93e015a27aef0842495e4a5d9e75248313/Resources/Screenshots/customers_table.png)
![customers_table_size_size](https://github.com/namin1993/Amazon_Vine_Analysis/blob/6545ff93e015a27aef0842495e4a5d9e75248313/Resources/Screenshots/Size_of_customers_table.png)

products_table - lists all video titles and their corresponding product id
![products_table](https://github.com/namin1993/Amazon_Vine_Analysis/blob/6545ff93e015a27aef0842495e4a5d9e75248313/Resources/Screenshots/products_table.png)
![products_table_size](https://github.com/namin1993/Amazon_Vine_Analysis/blob/6545ff93e015a27aef0842495e4a5d9e75248313/Resources/Screenshots/Size_of_products_table.png)

review_id_table - lists all review ids and their corresponding customer and product
![review_id_table](https://github.com/namin1993/Amazon_Vine_Analysis/blob/6545ff93e015a27aef0842495e4a5d9e75248313/Resources/Screenshots/review_id_table.png)
![review_id_table_size](https://github.com/namin1993/Amazon_Vine_Analysis/blob/6545ff93e015a27aef0842495e4a5d9e75248313/Resources/Screenshots/Size_of_review_id_table.png)

vine_table - lists all vine reviews
![vine_table](https://github.com/namin1993/Amazon_Vine_Analysis/blob/6545ff93e015a27aef0842495e4a5d9e75248313/Resources/Screenshots/vine_table.png)
![vine_table_size](https://github.com/namin1993/Amazon_Vine_Analysis/blob/6545ff93e015a27aef0842495e4a5d9e75248313/Resources/Screenshots/Size_of_vine_table.png)

## Results
The total number of reviews from the Amazon .tsv dataset is 5,069,140.
However, for the review analysis, I first filtered out any product reviews where the total votes were greater than 20. From that first filtered dataframe, I then filtered out any product reviews where the helpful_votes / total_votes >= 0.5. This was done to only work with the most relevant product reviews which had enough votes by customers and had a greater proportion of helpful votes to total votes.

From this filtered dataset, I can produce an analysis of paid and unpaid reviews in order to determine any bias of a paid review and the amount of 5 star reviews a product receives.

- Total number of reviews from dataset: 5,069,140
- Total number of 5-star reviews: 
- Percentage of 5-star reviews: 

Paid Reviews Analysis:
Total Number of Paid Reviews: 1075849
Total number of 5-Star Reviews: 721246
Percentage of 5-Star Reviews: 34.33%

![paid_reviews_summary](https://github.com/namin1993/Amazon_Vine_Analysis/blob/6545ff93e015a27aef0842495e4a5d9e75248313/Resources/Screenshots/Paid-Reviews_Analysis.png)

Unpaid Reviews Analysis:
Total Number of Unpaid Reviews: 1025083
Total number of 5-Star Reviews: 557876
Percentage of 5-Star Reviews: 26.55%

![unpaid_reviews_summary](https://github.com/namin1993/Amazon_Vine_Analysis/blob/6545ff93e015a27aef0842495e4a5d9e75248313/Resources/Screenshots/Unpaid-Reviews_Analysis.png)

## Summary
The summary states whether or not there is bias, and the results support this statement

From the Amazon Vine Reviews analysis on videos, we see that there is not a significant difference between the percentage of 5-star reviews assigned to movies when the review is paid or unpaid. There is only an 8% difference, indicating that there is only a slightly higher positive correlation in being paid to give a review.

If we wanted to further analyze whether there was bias in being paid to give video reviews, we might want look into each customer id's proportion of 5-star reviews.

From performing an inner join between the customers table, review_id table, and vine table by customer_id and review_id respectively, we would then export a new csv dataset and perform an analysis to determine the proportion of 5-star reviews given by each customer_id to the total number of votes given by each customer.

We would need to group the dataset by customer_ids and retrieve the aggregate number of 5-star votes given by that customer.

From there, we would create a new dataframe that contained customer_id, number of 5-star votes given by that customer, customer_count, and verified purchase.

We would seperate this dataframe by paid vs unpaid purchasers, find the percentage of 5-star reviews given by each customer_id to the total number of votes given by each customer, and then determine the overall average percentage from each dataframe. By comparing the value of the average percentage of 5-star reviews given by paid and non-paid customers, we can determine if the difference in the average is significant enough to claim that there is a bias. 
