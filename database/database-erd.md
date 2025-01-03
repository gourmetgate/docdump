# Gourmetgate Entity Relationship Diagram

```mermaid
erDiagram
    CATEGORY ||--o{ ARTICLE: ""
    CATEGORY {
        varchar(32) category_id
        varchar(80) name
    }
    ARTICLE ||--|{ LINE-ITEM: contains
    ARTICLE {
        varchar(32) article_id
        varchar(80) name
        varchar(32) category_id
        varchar(32) var_id
        numeric price
        numeric cost
        boolean available
    }
    LINE-ITEM {
        string productCode
        int quantity
        float pricePerUnit
    }
```