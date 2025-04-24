# Use cases

This document lists all use cases which Gourmetgate needs to fullfill. Each use case is justified by a user story.

## Actors

- Consumer: The one who wants to order
- Servant: The one who satisfies the needs of the customer
- Administrator: The one who manages the offered articles 

## Ordering use cases

### Cart

- Scan QR code to initiate order
- Have a overview about the articles
- See the articles grouped by category
- Add articles to cart
- Remove articles from cart
- Choose between article variants
- Place order with cart content
- Get confirmation after placing order

### Payment

- Pay order (twint, card)
- Add tip to price
- See confirmation when payment succeeds
- See error message when payment fails
- Receive receipt per mail

## Servant use cases

### Taking orders

- Select table the customer sits on
- Split the bill
- Send amount to card terminal

### Printing

- Print order on corresponding printer
- Print ordered articles separate on corresponding printer

## Backoffice use cases

### Assortment

- Configure categories (name)
- Configure articles (name, category, vat, price, cost, available)
- Configure variants (name, variant option, additional price)

### User

- Create users with password
- Change password of user
- Block a user
- Promote/demote a user with admin permission (admin = access to backoffice)
- Flag/deflag user as anonymus (anonymus = no login required)
- Configure available printers per user
- Enable/disable paying before ordering

### Economics

- Configure vat percentages
- See selling statistics

### Printers

- Add label printer by IP Address
- Add a name for the printer
- Choose categories printed on this printer
- See which user can print on this printer
- Delete printer

### Tables

- Add/remove dining tabels 
- Name a table
- See order history for specific table