# Database conventions

## Table

### Naming

The name of a table is written in snake case (lowercase with underscore as word separator).

### Column naming

A column name should not repeat the name of the entity. E.g. the column holding the name of an article should be named
`name` and not `article_name`. The only exception is the primary key column.

## Primary key columns

### Naming

The name of the primary key column of an entity consists of the entity name and `_id` suffix. Example `article_id`.

When referencing an ID of another entity, the same column name can be used.

### Data type

The ID (primary key) of an entity is represented by an UUID v4. The database type is a `varchar(32)`.

## Varchar columns

### Names

A column holding a name (e.g. of an entity) should hold a max amount of 80 characters. The database type is therefore
`varchar(80)`.

### Multiline text

A multi line text field mustn't have more than 500 characters. The database type for this one is `varchar(500)`.

## Date column

### Naming

The name of a column holding a date is prefixed with `evt_`, short for event. The name of the date should be a verb in
the infinitive form. The verb describes the event which happened at the specific date.

Example `evt_pay`, `evt_create`.

### Data type

When ever somehow sensible, the `datetime` datatype is used. Only in cases, where explicitly only a date without time is
needed, the `date` datatype is used.

Examples:

- `evt_create` -> `datetime` (creation date of an entity)
- `evt_birth` -> `date` (birthday of a person)

## Mapping table

### Naming

A mapping table connects two entities with a many-to-many relationship. The table name should follow this schema:
`{entity1_name}_to_{entity2_name}`. Example `user_to_category`.

The `to` does not often appear in a normal column name and helps to recognise a mapping table just by its name.

### Primary key

The primary key of a mapping table should be a composite key of the two primary keys of the connected entities. Example:
`user_id + category_id = primary key`.