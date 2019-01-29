I only added the columns to which I feel plays an important role to make all
of this work.

We create a `users` table that stores the record of both sellers and 
customers. I come up with this decision because I feel that a seller can also
buy a product from different sellers. Thus, acting as both a `seller` and a
`customer`. 

The `products` table simply stores the seller's collection of products. We
add an index to `seller_id` so that we can efficiently query the products that
a seller offers.

## Solution

`Users` table

| Name          | Type           |
| ------------- |:-------------: |
| first_name    | string         |
| last_name     | string         |
| email         | string         |

`Products` table

| Name          | Type           |
| ------------- |:-------------: |
| name          | string         |
| image         | string         |
| description   | text           |
| price         | decimal        |
| seller_id     | integer **(indexed)** |

`Orders` table

| Name          | Type           |
| ------------- |:-------------: |
| ref_no        | string         |
| price         | decimal        |
| product_id    | integer **(indexed)** |
| customer_id   | integer **(indexed)** |
| purchased_at  | datetime **(indexed)** |
| refunded_at   | datetime **(indexed)** |


## Concerns

> How can each purchase increase the seller's balance?

We compute seller's total current balance by getting the total of all the orders
which are not refunded. Every addition of an order record should essentially 
increase the seller's balance.

> How do a seller refund a purchase?

We simply update the order's `refunded_at` column to the current time. This will
mark the order as refunded. I was thinking of using a `status` column but I feel
like recording a timestamp is a far better solution because it lets us query the
refunded orders based on date range.

When a purchase is refunded, it should decrease the seller's balance.

> How do the rolling payouts work?

If we want to roll a payout for a specific time period, we can do that by
getting all non-refunded orders with `purchased_at` only within the given
time period.

