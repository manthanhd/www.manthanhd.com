---
id: 52
title: 'Best way to query database in Visual C#'
date: 2012-01-20T15:44:00+00:00
author: Manthan Dave
layout: post
guid: http://www.manthanhd.com/2012/01/20/best-way-to-query-database-in-visual-c/
permalink: /2012/01/20/best-way-to-query-database-in-visual-c/
blogger_blog:
  - codeninjutsu.blogspot.com
blogger_author:
  - ManthanHD
blogger_permalink:
  - /2012/01/best-way-to-query-database-in-visual-c.html
blogger_internal:
  - /feeds/2026358850785924011/posts/default/2138101711177939382
categories:
  - Findings
tags:
  - .net
  - c
  - database
---
Best way to query database in C#
Well, this is actually not an official way but my personal choice. In this instance, I will use an access database as an example.
While querying the database is a long process in C#, it becomes simplified once you use object orientation. First of all, here's the conventional way of querying the database:
<pre class="lang:c#">string connectionString = "...";
OleDbConnection dbConnection = new OleDbConnection(connectionString);
dbConnection.Open();
string queryString = "SELECT * FROM Customers";
OleDbCommand dbCommand = new OleDbCommand(queryString,dbConnection);
//since this is select query...
OleDbDataReader dr = dbCommand.ExecuteReader();
//and then you do the reading process...</pre>
Now, if you have a lot of queries to run, then this would be really painful process to do again and again. Hence, it is better to put the process in a method like this:
<pre class="lang:c#">public OleDbDataReader runSelectQuery(string queryString){
string connectionString = "...";
OleDbConnection dbConnection = new OleDbConnection(connectionString);
dbConnection.Open();
//using the query string...
OleDbCommand dbCommand = new OleDbCommand(queryString,dbConnection);
//since this is select query...
OleDbDataReader dr = dbCommand.ExecuteReader();
//and then you do the reading process...
return dr;
}</pre>
The method takes in the select query string and returns the data reader which can then be used to extract the data. To make this more efficient, make a class and put the above method as static. Also, make the connectionString a global variable so that it if you need to change it, then you only have to change it in one place. In the end, the code would look like this:
<pre class="lang:c#">public class QueryClass{

private static string connectionString = "...";

public static OleDbDataReader runSelectQuery(string queryString){
OleDbConnection dbConnection = new OleDbConnection(connectionString);
dbConnection.Open();
//using the query string...
OleDbCommand dbCommand = new OleDbCommand(queryString,dbConnection);
//since this is select query...
OleDbDataReader dr = dbCommand.ExecuteReader();
return dr;
}
}</pre>
Later, to access the method, you'll just have to do:

<code>OleDbDataReader reader = QueryClass.runSelectQuery("SELECT * FROM Customers");
</code>
Comment below if you have any queries regarding the code.