# Database software module (gourmetgate.db)

## Schema

The schema of the database is defined in the java code. This ensures, that the schema is only defined at one place and
is consistent over all systems. Through hibernate, the schema, defined in the code is written to the database.

Execute `[database] migrate` to initialize or migrate the database schema on your physical database.

### Schema definition

The entire schema is defined in the `app.gourmetgate.gourmetgate.db.schema` package. Each class in this package
represents a table in the physical database. Each entity must fulfill these criteria:

- The suffix of the class name shall be `Entity`, e.g. `ExampleEntity`.
- Implement `ISchemaEntity`: Marker interface to allow the jandex class inventory to collect all schema classes.
- Annotate class with `@Entity` to mark it as hibernate entity.
- Annotate class with `@Table(name = "example")`to set the physical table name.
- Create a primary key member variable of type `UUID` and annotate it with `@Id`.

> **_Note:_**  Consider checking
> the [database conventions](https://github.com/gourmetgate/docdump/blob/main/database/database-conventions.md) when
> modifying the database schema.

#### Code example

```java

@Entity
@Table(name = "example")
public class ExampleEntity implements ISchemaEntity {

    @Id
    private UUID exampleId;

    @NotNull
    private String name;

    private String description;

    // getter and setter
}
```

## Example data

An empty database is not really useful in the development process. Therefore, example data can be generated, if needed.

Execute `[database] reset` to reset the entire database and fill it with example data.

### Create example data

The example data is defined in the `app.gourmetgate.gourmetgate.db.data` package. For each entity, a `DataProvider`
class exists, that extends the `AbstractInitialDataProvider` which generates the example data.

The implementation of `getInitialData()` returns a list of entities filled with data.

### Code example

````java
public class ExampleDataProvider extends AbstractInitialDataProvider<ExampleEntity> {

    public static final UUID EXAMPLE_ID = UUID.randomUUID();

    @Override
    public List<ExampleEntity> getInitialData() {
        ExampleEntity entity = new ExampleEntity();
        entity.setExampleId(EXAMPLE_ID);
        entity.setName("This name is required");
        entity.setDescription("This is an optional description.");
        return List.of(entity);
    }
}
````

## jOOQ Code

The persistence layer uses jOOQ to write database queries. It provides an code generator, that generates java code based
on the database schema to be able to write type-save queries.

Execute `[database] generate jooq code` to create jOOQ code based on the current database schema.  
