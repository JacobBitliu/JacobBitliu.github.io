---
layout:     post
title:      An easy JDBC turtorial
category: blog
description: An easy JDBC turtorial
---
#  JDBC

JDBC is a java API used to connect to database , like mysql ,oracle ,etc. This manual 's point lies in how to use jdbc to analyse data .

##Connect

The first step to analyse data with database is connect to the database . In JDBC ,connect to mysql for example, we use the next code to connect to database .

```
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

Connection conn = null;
...
try {
    conn =
       DriverManager.getConnection("jdbc:mysql://localhost/test?" +
                                   "user=minty&password=greatsqldb");

    // Do something with the Connection

   ...
} catch (SQLException ex) {
    // handle any errors
    System.out.println("SQLException: " + ex.getMessage());
    System.out.println("SQLState: " + ex.getSQLState());
    System.out.println("VendorError: " + ex.getErrorCode());
}

```
This code is from connector-j doc , the standard method to connect to mysql database.

The doc has a sentence to show how importent the connect step.

>Once a Connection is established, it can be used to create Statement and PreparedStatement objects, as well as retrieve metadata about the database. This is explained in the following sections.

## Exce the sql and get the result

Exce the sql and get the result is our next step . Different sql sentence have to use different method JDBC provide .

For example , if you just want to find the data you want , you can do just like next.

```
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;
import java.sql.ResultSet;

// assume that conn is an already created JDBC connection (see previous examples)

Statement stmt = null;
ResultSet rs = null;

try {
    stmt = conn.createStatement();
    rs = stmt.executeQuery("SELECT foo FROM bar");

    // or alternatively, if you don't know ahead of time that
    // the query will be a SELECT...

    if (stmt.execute("SELECT foo FROM bar")) {
        rs = stmt.getResultSet();
    }

    // Now do something with the ResultSet ....
}
catch (SQLException ex){
    // handle any errors
    System.out.println("SQLException: " + ex.getMessage());
    System.out.println("SQLState: " + ex.getSQLState());
    System.out.println("VendorError: " + ex.getErrorCode());
}
finally {
    // it is a good idea to release
    // resources in a finally{} block
    // in reverse-order of their creation
    // if they are no-longer needed

    if (rs != null) {
        try {
            rs.close();
        } catch (SQLException sqlEx) { } // ignore

        rs = null;
    }

    if (stmt != null) {
        try {
            stmt.close();
        } catch (SQLException sqlEx) { } // ignore

        stmt = null;
    }
}

```

But if you want to update the data in the database , you can't use the executeQuery() method , you should use execute() . Just obvious observe next code for my test.

```
import java.sql.*;


public class Main {

    public static void main(String[] args) {


        try {
            Class.forName("com.mysql.jdbc.Driver");

            Connection connection = DriverManager.getConnection("jdbc:mysql://localhost/graduationTest","root","root");

            Statement sta = connection.createStatement();

            String sql = "create table mytable (name char(20) not null)";

            boolean res = sta.execute(sql);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        }


        System.out.println("Hello World!");

    }
}
```

## Close the connect

There is nothing to say , but it didn't mean this part is not importent , on the contrary , id very importent .



