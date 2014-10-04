---
layout: post
category : java
tagline: ""
tags : [java, ARM]
---
{% include JB/setup %}

###Automatic resource management since Java 7
In old times(Before java 7) when you're dealing with resource cleanup, there would be a bunch of boiler-plate code. Consider this nasty bit of JDBC code:
    
    Connection connection = null;
	PreparedStatement statement = null;
	ResultSet resultSet = null;
	try
	{
		connection = dataSource.getConnection();
		statement = connection.prepareStatement(...);
		// set up statement
		resultSet = statement.executeQuery();
		// do something with result set
	}
	catch(SQLException e)
	{
		// do something with exception
	}
	finally
	{
		if(resultSet != null) {
		try {
			resultSet.close();
		} catch(SQLException ignore) { }
		}
		if(statement != null) {
			try {
			statement.close();
			} catch(SQLException ignore) { }
		}
		if(connection != null && !connection.isClosed()) {
		try {
			connection.close();
		} catch(SQLException ignore) { }
		}
	}
    
The boiler-plate code sure is annoying and meaningless in most circumstances. So Java 7 solves this problem with new try-with-resources feature:
   
    try(Connection connection = dataSource.getConnection();
		PreparedStatement statement = connection.prepareStatement(...))
	{
		// set up statement
		try(ResultSet resultSet = statement.executeQuery())
		{
		// do something with result set
	}
	}
	catch(SQLException e)
	{
		// do something with exception
	}

And also the multi-catch feature:

	try
	{
		// do something
	}
	catch(Exception1 | Exception2 e)
	{
		// handle these exceptions the same way
	}

One caveat to keep in mind is that you can't multi-catch two or more exceptions such that one inherits from another.
    
####Under the hood
If you open the source code of `java.io.BufferedReader`, you will see it implements the `java.lang.AutoCloseable` interface. This interface have only one method: `void close() throws Exception;` and can be implemented on any resource that must be closed when it is no longer needed. The close method is call by JVM immediately after finishing the try block.

If you want to use this feature in your custom resources, just implement the AutoCloseable interface.


###A shortcut for generic type instantiation since Java 7
Short story: now you can declare generic types as: Map<String, Map<String, Map<Integer, List<MyBean>>>> map = new HashMap<>();

Actually it's an old feature of [Guava](https://code.google.com/p/guava-libraries/), great library by the way.

<br/><br/>
Reference:

<<Professional Java for Web Applications>>
