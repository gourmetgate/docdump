# Database model

This document gives an overview over the data structure of the application and describes the purpose of the entities.

## Entities

### Article

A purchasable article. Each article has to belong to a category and must have a vat percentage assigned.

| Property name | Type    | Description                                          |
|---------------|---------|------------------------------------------------------|
| article_id    | UUID    | Unique identifier of the article.                    |
| name          | String  | Name of the article.                                 |
| category_id   | UUID    | ID of the category, the article belongs to.          |
| price         | String  | Selling price of the article in CHF inclusive vat.   |
| vat_id        | UUID    | ID of the vat percentage.                            |
| cost          | String  | Purchase cost of the article (optional).             |
| available     | boolean | Indicates, if the article is available for purchase. |

### Category

A category of articles to enable logical grouping.

| Property name | Type   | Description                        |
|---------------|--------|------------------------------------|
| category_id   | UUID   | Unique identifier of the category. |
| name          | String | Name of the category.              |

### Order

A order of a customer.

| Property name | Type     | Description                                                    |
|---------------|----------|----------------------------------------------------------------|
| order_id      | UUID     | Unique identifier of the order.                                |
| number        | Long     | Human readable unique order number, auto-increments.           |
| evt_create    | Datetime | Datetime when the order is created                             |
| table_id      | UUID     | Reference to the table, the customer sits on. (Soft reference) |
| user_id       | UUID     | Reference to the user, who created the order.                  |
| tip           | String   | Amount of tip in CHF for this order (Optional).                |

### Order item

An article, added to an order.

| Property name | Type   | Description                                         |
|---------------|--------|-----------------------------------------------------|
| order_item_id | UUID   | Unique identifier of the order item.                |
| order_id      | UUID   | Reference to the order the item belongs to.         |
| article_ID    | UUID   | Reference to the original article. (Soft reference) |
| name          | String | Name of the order item (same as article name).      |
| price         | String | Selling price of the order item inclusive vat.      |
| vat           | String | Amount of vat in CHF.                               |
| cost          | String | Purchase cost of order item. (optional)             |
| comment       | String | User comment on this article item. (optional)       |

### Order item option

A chosen variant option of an order item. E.g. "Ketchup" for the order item "Fries".

| Property name        | Type   | Description                                                                            |
|----------------------|--------|----------------------------------------------------------------------------------------|
| order_item_option_id | UUID   | Unique identifier of the order item option.                                            |
| order_item_id        | UUID   | Reference to the order item the option belongs to.                                     |
| variant_option_id    | UUID   | Reference to the original variant option. (Soft reference)                             |
| name                 | String | Name of the option. E.g. "Ketchup".                                                    |
| additional_price     | String | Additional price of the option. This is not included in the price of the order option. |

### Payment

Payment attempts, does not have to be successful. The amount is the final amount on the bill. This includes the summed
price of all order items, summed with the additional price of the order options and combined with the tip.

| Property name | Type     | Description                                                              |
|---------------|----------|--------------------------------------------------------------------------|
| payment_id    | UUID     | Unique identifier of the payment attempt.                                |
| external_id   | String   | Unique identifier of the external payment provider.                      |
| order_id      | UUID     | Reference to the order the payment belongs to.                           |
| amount        | String   | Total amount on the bill. Sum of order positions, order options and tip. |
| state         | String   | State of the payment (enum). Possible states: `PAYED`, `FAILED`, `OPEN`. |
| evt_pay       | Datetime | Date and time when the payment was paid (optional).                      |

### Printer

A printer which prints orders onto a label or paper to be prepared in the kitchen.

| Property name     | Type    | Description                                                                                                           |
|-------------------|---------|-----------------------------------------------------------------------------------------------------------------------|
| printer_id        | UUID    | Unique identifier of the printer.                                                                                     |
| name              | String  | Printer name.                                                                                                         |
| ip_address        | String  | IP âddress of the printer in the LAN of the Printplate application.                                                   |
| active            | boolean | Indicates if the printer is active and orders can be printed on.                                                      |
| group_order_items | boolean | When true all order items are printed on the same label. When false, a separate label for each order item is printed. |

### Print job

Print jobs that have to be printed by Printplate.

| Property name | Type   | Description                                                                                                                |
|---------------|--------|----------------------------------------------------------------------------------------------------------------------------|
| print_job_id  | UUID   | Unique identifier of the print job.                                                                                        |
| printer_id    | UUID   | Reference to the printer the job is printed on.                                                                            |
| order_id      | UUID   | Reference to the order.                                                                                                    |
| order_item_id | UUID   | References the printed order item, when `group_order_item` is disabled and just s single item is printed. Otherwise `null` |
| content       | blob   | Content to be printed.                                                                                                     |
| state         | String | State of the print job (enum). Possible states: `OPEN`, `PRINTED`.                                                         |

### Printer to category

Mapping table to specify, which printer is can print order items of corresponding category

- printer_id
- category_id

### Printer to user

Mapping table to specify, which user can print order on printer

- printer_id
- user_id

### Settings

- setting_id
- user_id (null for application wide setting)
- setting_data (JSON)

### Room

A room where tables are located.

- room_id
- name

### Table

A table where the customer sits on and orders articles to.

- table_id
- room_id
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

### Variant to article

Mapping table to assign a one or multiple variants to an article.

- variant_id
- article_id

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
- pay_advance

### User to category

Indicates, which categories can be sold by a specific user.

- user_id
- category_id