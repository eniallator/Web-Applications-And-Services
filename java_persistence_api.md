# Java Persistence API (JPA)

- Moving data from the managed bean to an EJB
- For interacting with a database
- Persistence refers to information which exists even after the thing that created it doesn't exist
- JPA works with relational databses
  - In JPA you don't need to write SQL code
  - In JPA you don't need to write JDBC code

## Object Relational Mapping (ORM)

- In a relational database, data is organised using one or more tables
- E.g in a comment class, there is the following attributes

| Attribute | DataType |
| --------- | -------- |
| id        | integer  |
| name      | string   |
| comment   | string   |
| visitDate | date     |

## Java Database Connectivity (JDBC)

- JDBC is a java API that provides a mechanism for java programs to connect to databases
- JPA writes SQL and uses JDBC to read/write to database tables (all behind the scenes - less work for us)
- JPA is the standard specification for ORM in Java EE

## JPA Has Two Main Concepts

- Entity class
  - Requirements of an entity class:
    - Annotated with the `@Entity` annotation
    - A default, no-argument constructor
    - Persistent instance variables must be declared private
    - Can be access directly by getters
  - Need to add annotations to use JPA (there are docs)
    - `@Entity` for the class
    - `@Id` signifies the primary key attribute in the class
    - `@GeneratedValue(strategy = GenerationType.AUTO)` auto-increment for IDs
    - `@Temporal` used to specify the temporal type of the attribute (e.g java.util.Date or java.util.Calendar)
    - `@NotNull`
    - `@Column(name="my_name")` specifies the column in the db table
    - `@Lob` attribute is a large object type (e.g images)
    - `@Transient` for attributes which aren't committed to the db
    - Foreign keys
      - `@Embeddable` - class
      - `@Embedded` - attribute
    - `@ElementCollection`
    - ...
- Entity Manager
  - `createNamedQuery` - how to execute a JPA query
  - A persistence unit defines a set of all entity classes that are managed by `EntityManager` instances in an application
  - `EntityManagerFactory` is a factory capable to create on-demand `EntityManager` instances
  - Each `EntityManager` instance is associated with a persistence context
  - A set of managed entity instances that exist in a particular data store
  - A persistence context defines the scope under which particular entity instances are created, persisted, and removed
