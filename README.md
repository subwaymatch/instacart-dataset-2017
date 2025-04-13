This is an archive of the dataset titled "The Instacart Online Grocery Shopping Dataset 2017".

The dataset is provided as-is for non-commercial use, and is subject to our [Terms and Conditions](https://gist.github.com/jeremystan/582eba13d6ee27ed465c43dc78934700).

If you make use of this dataset, please use the following citation as requested by Instacart:

_"The Instacart Online Grocery Shopping Dataset 2017", Accessed from https://www.instacart.com/datasets/grocery-shopping-2017 on \<date\>_


## Data Dictionary

`orders` (3.4m rows, 206k users):
* `order_id`: order identifier
* `user_id`: customer identifier
* `eval_set`: which evaluation set this order belongs in (see `SET` described below)
* `order_number`: the order sequence number for this user (1 = first, n = nth)
* `order_dow`: the day of the week the order was placed on
* `order_hour_of_day`: the hour of the day the order was placed on
* `days_since_prior`: days since the last order, capped at 30 (with NAs for `order_number` = 1)

`products` (50k rows):
* `product_id`: product identifier
* `product_name`: name of the product
* `aisle_id`: foreign key
* `department_id`: foreign key

`aisles` (134 rows):
* `aisle_id`: aisle identifier
* `aisle`: the name of the aisle

`deptartments` (21 rows):
* `department_id`: department identifier
* `department`: the name of the department

`order_products__SET` (30m+ rows):
* `order_id`: foreign key
* `product_id`: foreign key
* `add_to_cart_order`: order in which each product was added to cart
* `reordered`: 1 if this product has been ordered by this user in the past, 0 otherwise

where `SET` is one of the four following evaluation sets (`eval_set` in `orders`):
* `"prior"`: orders prior to that users most recent order (~3.2m orders)
* `"train"`: training data supplied to participants (~131k orders)
* `"test"`: test data reserved for machine learning competitions (~75k orders



## [3 Million Instacart Orders, Open Sourced](https://tech.instacart.com/3-million-instacart-orders-open-sourced-d40d29ead6f2)

* This is a copy of the original blog post published at the time of the dataset's release.

By [Jeremy Stanley](https://medium.com/@jeremy.stanley?source=post_page---byline--d40d29ead6f2---------------------------------------)
Published on May 3, 2017

Curious about the food Americans eat? Look no further.

Instacart is excited to announce our first public dataset release, “The Instacart Online Grocery Shopping Dataset 2017”. This anonymized dataset contains a sample of over 3 million grocery orders from more than 200,000 Instacart users.

For each user, we provide between 4 and 100 of their orders, with the sequence of products purchased in each order. We also provide the week and hour of day the order was placed, and a relative measure of time between orders.

In this post, we’ll briefly introduce the dataset and why it is important to Instacart and provide a link to download the data.

Then, we’ll finish by highlighting a few interesting patterns we’ve found in the data. Can you guess what product is most likely to be ordered late at night?

### The applications

We hope the machine learning community will use this data to test models for predicting products that a user will buy again, try for the first time or add to cart next during a session.

Instacart currently uses XGBoost, word2vec and Annoy in production on similar data to sort items for users to “buy again”:

![image](https://github.com/user-attachments/assets/1f4a043b-5054-407d-857c-5de35675c618)

and to recommend items for users while they shop:

![image](https://github.com/user-attachments/assets/d6dad4e8-2ff6-4c92-8555-f6b68eb2277e)

This data, and the algorithms trained upon it, are enabling Instacart to revolutionize how consumers discover and purchase groceries.

For example, the first two orders for user_id 1 are:

![image](https://github.com/user-attachments/assets/7035af2b-bee0-4fb4-96ff-5c6b24d61946)

If you make use of this dataset, please use the following citation:

_"The Instacart Online Grocery Shopping Dataset 2017", Accessed from https://www.instacart.com/datasets/grocery-shopping-2017 on \<date\>_

If you have questions about this dataset, you can reach out to us directly at [open.data@instacart.com](mailto:open.data@instacart.com).

### A word about privacy

We have taken great care to protect the privacy of our users and retail partners and to ensure that the data is entirely anonymous:

The only information provided about users is their sequence of orders and the products in those orders
All of the IDs in the dataset are entirely randomized, and cannot be linked back to any other ID
Only products that are bought by multiple people at multiple retailers are included, and no retailer ID is provided
Note that this dataset includes orders from many different retailers and is a heavily biased subset of Instacart’s production data, and so is not a representative sample of our products, users or their purchasing behavior.

### Some interesting findings

There are many interesting patterns to be found in this dataset.

For example, this plot shows that the most common aisles are more likely to be reordered from.

![image](https://github.com/user-attachments/assets/03314341-e190-43ad-b9c5-6622ffdfc084)

Fruits are reordered more frequently than vegetables — perhaps because vegetables are more intermittently purchased for recipes. Staples like soups and baking ingredients are least likely to be reordered — perhaps because they are less frequently needed.

We can also see the time of day that users purchase specific products.

![image](https://github.com/user-attachments/assets/341b2541-9c2a-4486-bf4a-454a1a718883)

Healthier snacks and staples tend to be purchased earlier in the day, whereas ice cream (especially Half Baked and The Tonight Dough) are far more popular when customers are ordering in the evening.

In fact, of the top 25 latest ordered products, the first 24 are ice cream! The last one, of course, is a frozen pizza.

