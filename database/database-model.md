# Database model

This document gives an overview over the data structure of the application and describes the purpose of the entities.

## Overview

Lorem ipsum

## Entities

### Article

A purchasable article. Each article has to belong to a category and must have a vat percentage assigned.

| Property name | Type    | Description                                          |
|---------------|---------|------------------------------------------------------|
| article_id    | UUID    | Unique identifier of the article.                    |
| name          | String  | Name of the article.                                 |
| category_id   | UUID    | ID of the category, the article belongs to.          |
| price         | String  | Selling price of the article in CHF.                 |
| vat_id        | UUID    | ID of the vat.                                       |
| cost          | String  | Purchase cost of the article (optional).             |
| available     | boolean | Indicates, if the article is available for purchase. |

### Category

A category of articles to enable logical grouping.

- category_id
- name

### Order

A order of articles.

- order_id
- number
- evt_create
- table_id (soft reference)

### Order item

An article, added to an order.

- order_item_id
- order_id
- article_id (soft reference)
- name
- price
- vat
- comment

### Order item option

A selected variant option of an order item

- order_item_option_id
- order_item_id
- variant_option_id (soft reference)
- name
- additional_price

### Payment

Payment attempts, does not have to be successful. The amount is the final amount on the bill. This includes the summed
price of all order items, combined with the tip.

- payment_id
- external_reference
- order_id
- amount
- tip
- state
- evt_pay

### Printer

- printer_id
- name
- ip_address
- active
- print_items_separate

### Print job

- print_job_id
- printer_id
- order_id
- order_item_id (null when print_items_separate is false)
- content

### Printer to category

Mapping table to specify, which printer is can print order items of corresponding category

- printer_category_id
- printer_id
- category_id

### Printer to user

Mapping table to specify, which user can print order on printer

- printer_user_id
- printer_id
- user_id

### Settings

- setting_id
- user_id (null for application wide setting)
- setting_data (JSON)

### Table

- table_id
- name

### Variant

A variant can be added to an article to extend its property. A sauce could be a variant.

- variant_id
- name

### Variant option

A concrete option of a variant. E.g. Ketchup for the variant "sauce".

- variant_option_id
- variant_id
- name
- additional_price

### Article to variant

Mapping table to assign a one or multiple variants to an article.

- article_id
- variant_id

### Vat

Vat percentage.

- vat_id
- percentage

### User

User of the software.

- user_id
- username
- password_sha256
- salt
- blocked
- anonymous
- admin

### User to category

Indicates, which categories can be sold by a specific user.

- user_id
- category_id