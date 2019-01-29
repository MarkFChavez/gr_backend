## Backend

### Explanation
I only added the columns to which I feel plays an important role to make all
of this work.

We create a `users` table that stores the record of both sellers and 
customers. I come up with this decision because I feel that a seller can also
buy a product from a different seller. Thus, acting as both a `seller` and a
`customer`. 

The `products` table simply stores the seller's collection of products. We
add an index to `seller_id` so that we can efficiently query the products that
a seller offers.

> Every purchase should increase the seller's balance.

We query all the orders that has null `purchased_at` values.

> The seller can at any time refund a purchase, which should decrease his/her
balance.

We simply update the order's `refunded_at` column to the current time. This will
mark the order as refunded. I was thinking of using a `status` column but I feel
like recording a timestamp is a far better solution because it lets us query the
refunded orders based on date range.

> How about the rolling payouts?

Rolling payouts should be handled by querying the data based from the
`purchased_at` value.

## Data Design

User
====
first_name string
last_name  string
email      string

Product
=======
name        string
image       string
description text
price       decimal
seller_id   integer (indexed)

Order
=====
ref_no       string
price        decimal
product_id   integer (indexed)
customer_id  integer (indexed)
purchased_at datetime (indexed)
refunded_at  datetime (indexed)
