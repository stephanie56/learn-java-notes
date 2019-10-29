# Chapter 3 Working with Data

Book: Spring in Action

## Reading and writing data with JDBC

- When it comes to working with relational data, the most common choices are JDBC and JPA.
- JDBC support is rooted in the JdbcTemplate class, which helps developer to perform SQL operations against a relational database without all the boilerplate typically required when working with JDBC.
- An example of performing a simple query in Java without JdbcTemplate
- Create a connection, statement and resultSet -> connect to database -> add a SQL query -> update SQL query -> execute SQL query -> if resultSet exists, build the result object -> catch SQL exception -> clean up connection, statement and resultSet
- The problem with this approach:

1. Difficult to maintain code: have a hard time spotting the query
2. Any number of things could go wrong when creating the connection, the statement, the resultSet, or when performing the query, this requires that you catch a SQLException, which may not be helpful in figuring out what went wrong.
3. SQLException is a checked exception, which requires handling in a catch block. But the most common problems, such as failure to create a connection to the database or a mistyped query, can’t be addressed in a catch block and are likely to be regrown for handling upstream.

```
@Override
Public Ingredient findOne(String id) {
// declare connection, SQL queries and result set
Connection connection = null;
PreparedStatement statement = null;
ResultSet resultSet = null;

Try {
Connection = dataSource.getConnection(); // Connect to the database
Statement = connection.prepareStatement(“select id, name, type from Ingredient”) // Add SQL query
Statement.setString(1, id); // set SQL query id to parameter id
resultSet = statement.executeQuery(); // perform SQL operation, get results
Ingredient ingredient = null;
If (resultSet.next()) {
Ingredient = new Ingredient(
	resultSet.getString(“id”),
	resultSet.getString(“name”),
	Ingredient.Type.valueOf(resultSet.getString(“type”));
)
return ingredient;
} catch (SQLException e) {}
} finally {
// Clean up and close resultSet, statement and connection
If (resultSet != null) {
 Try { resultSet.close();} catch(SQLException e) {}
}
If (statement !=null) {
try{statement.close();} catch(SQLException e) {}
}
If (connection !=null) {
try{connection.close()}catch(SQLException e){}
}
Return null;
}
}
```

## Using JdbcTemplate

- There aren’t any statements or connections being created
- There isn’t any clean up of those objects after the method is finished
- There isn’t any handling of exceptions that can’t properly be handled in a catch block
- What’s left is code that’s focused on performing a query
- Mapping the results to an Ingredient object

## Defining JDBC Repositories

- The repository is annotated with @Repository
- Inject the JdbcTemplate via the @Autowired annotated construction: the constructor assigns JdbcTemplate to an instance variable that will be used in other methods to query and insert into the database

```
@Repository
Public class JdbcIngredientRepository implements IngredientRepository {
  Private JdbcTemplate jdbc;

  @Autowired
  Public JdbcIngredientRepository(JdbcTemplate jdbc) {this.jdbc = jdbc;}

  @Overried
  public Ingredient findOne(String id) {
    return jdbc.queryForObject(
    “Select id, name, type from Ingredient where id=?”,
    This::mapRowToIngredient, id);
  }

  private Ingredient mapRowToIngredient(ResultSet rs, int rowNum) throws SQLException {
    return new Ingredient {
      Rs.getString(“id”),
      Rs.getString(“name”),
      Ingredient.Type.valueOf(rs.getString(“type”));
    }
  }
}

```

## Update and Save to Database

- JdbcTemplate has an update() method to writes or updates data in the database
- The update() method only requires a String containing the SQL to perform as well as values to assign to any query parameters.

```
@Override
Public Ingredient save(Ingredient ingredient) {
  Jdbc.update(
    “Insert into Ingredient (id, name, type) values (?,?,?)”,
    Ingredient.getId(),
    Ingredient.getName(),
    Ingredient.getType().toString()
  );
  Return ingredient;
}
```
