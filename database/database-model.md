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

The user can configure, which category of articles will be printed on which printer. E.g. drinks will be printed at the
bar and food will be printed in the kitchen. This table represents the n:n relation between these two entities.

| Property name | Type | Description                                         |
|---------------|------|-----------------------------------------------------|
| printer_id    | UUID | Reference to the printer. Part of the primary key.  |
| category_id   | UUID | Reference to the category. Part of the primary key. |

### Printer to user

Depending on the user who places the order, a different printer should print the order. E.g. when a coke is ordered at
bar, the order should come out of a different printer than when the coke is ordered in the restaurant hall. This table
represents the n:n relation between these two entities.

| Property name | Type | Description                                        |
|---------------|------|----------------------------------------------------|
| printer_id    | UUID | Reference to the printer. Part of the primary key. |
| user_id       | UUID | Reference to the user. Part of the primary key.    |

### Settings

Json based storage table for settings. Both, application-wide and user specific settings are stored in this table.

| Property name | Type | Description                                                              |
|---------------|------|--------------------------------------------------------------------------|
| setting_id    | UUID | Unique identifier of the setting.                                        |
| user_id       | UUID | Reference to the user. Null when an application wide setting (optional). |
| data          | JSON | Setting serialized as JSON.                                              |

### Room

A physical room in a building where customers are served.

| Property name | Type   | Description                    |
|---------------|--------|--------------------------------|
| room_id       | UUID   | Unique identifier of the room. |
| name          | String | Name of the room               |

### Table

A table (not a database table) where the customer sits on and orders articles to.

// TODO rename entity -> conflicting keyword in SQL

| Property name | Type   | Description                     |
|---------------|--------|---------------------------------|
| table_id      | UUID   | Unique identifier of the table. |
| room_id       | UUID   | Room the table stands in.       |
| name          | String | Name or number of the table.    |

### Variant

A variant is a modifier for an article. E.g. the article "HotDog" has the variant "Sauce". A variant can be added to
multiple articles.

| Property name | Type    | Description                                                |
|---------------|---------|------------------------------------------------------------|
| variant_id    | UUID    | Unique identifier of the variant.                          |
| name          | String  | Name of the variant.                                       |
| single_option | Boolean | Indicates if only a single option of this variant allowed. |

### Variant option

A concrete option of a variant. E.g. "Ketchup" for the variant "Sauce".

| Property name     | Type   | Description                              |
|-------------------|--------|------------------------------------------|
| variant_option_id | UUID   | Unique identifier of the variant option. |
| variant_id        | UUID   | The variant the option belongs to.       |
| name              | String | The name of the variant option.          |
| additional_price  | String | The price this option costs.             |

### Variant to article

Mapping table to assign a one or multiple variants to an article.

| Property name | Type | Description                                        |
|---------------|------|----------------------------------------------------|
| variant_id    | UUID | Reference to the variant. Part of the primary key. |
| article_id    | UUID | Reference to the article. Part of the primary key. |

### Vat

Possible vat percentage for

| Property name | Type   | Description                                                 |
|---------------|--------|-------------------------------------------------------------|
| vat_id        | UUID   | Unique identifier of the vat.                               |
| percentage    | String | Decimal number representing the vat percentage.             |
| description   | String | Description of the vat percentage (E.g. "Base percentage"). |

### User

User of the software.

| Property name   | Type    | Description                                             |
|-----------------|---------|---------------------------------------------------------|
| user_id         | UUID    | Unique identifier of the user.                          |
| username        | String  | Unique name of the user.                                |
| password_sha256 | String  | SHA-256 has representation of the password.             |
| salt            | String  | Password salt.                                          |
| blocked         | Boolean | Indicates if the user is blocked.                       |
| anonymous       | Boolean | Indicates if the user can login without authentication. |
| admin           | Boolean | Indicates if the user has access to the admin view.     |

### User to category

Not every user can sell every article category. E.g. the Barkeeper shouldn't be able to sell food. This table represents
the n:n relation between a user and a category.

| Property name | Type | Description                                         |
|---------------|------|-----------------------------------------------------|
| user_id       | UUID | Reference to the user. Part of the primary key.     |
| category_id   | UUID | Reference to the category. Part of the primary key. |