# Data model

This document gives an overview over the data structure of the application and describes the purpose of the entities.

## Overview

Lorem ipsum

## Entities

### Article

This entity represents an purchasable article. An article has to be assigned to a category.

| Property name | Type      | Description                                                               |
|---------------|-----------|---------------------------------------------------------------------------|
| article_id    | UUID      | Unique identifyer of the article.                                         |
| name          | String    | Name of the article.                                                      |
| category_id   | UUID      | ID of the category, the article belongs to.                               |
| price         | String    | Selling price of the article in CHF.                                      |
| vat_id        | UUID      | ID of the vat.                                                            |
| cost          | String    | Purchase price of the article (optional).                                 |
| options       | Options[] | Options of this article. See article options for more details (optional). |