# Database conventions

## Table

### Naming

The name of a table is written in snake case (lowercase with underscore as word separator).

### Column naming

A column name should not repeat the name of the entity. E.g. the column holding the name of an article should be named
`name` and not `article_name`.

A mapping table

## Primary Key

### Naming

The name of the primary key column of a entity is the na

## Mapping table

### Naming

A mapping table connects two entities via a many-to-many relationships. The table name should follow this schema:
`{entity1_name}_to_{entity2_name}`. Example `user_to_category`.

### Primary key

The primary key of a mapping table should be a composite key of the two primary keys of the connected entities. Example:
`user_id + category_id = primary key`.