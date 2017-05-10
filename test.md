# Introducing sql_magic: Jupyter Magic to Write SQL Code for Apache Spark and Relational Databases
<p align="center">
  <img src="img.JPG" width="60%" align="middle">
</p>

# this is the new title

Data scientists love Jupyter Notebook, Python, and Pandas. And they also write SQL. I created sql_magic to facilitate writing SQL code from Jupyter Notebook to use with both Apache Spark (or Hive) and relational databases such as PostgreSQL, MySQL, Pivotal Greenplum and HDB, and others. The library supports [SQLAlchemy](https://www.sqlalchemy.org/) connections, [SparkSession and SQLContext](https://docs.databricks.com/spark/latest/gentle-introduction/sparksession.html) objects, and other connections types. The `%%readsql` magic function returns results as a Pandas DataFrame for further analysis and plotting. 

~~~
%%readsql df_result
SELECT {col_names}
FROM {table_name}
WHERE age < 10
~~~

The sql_magic library expands upon current libraries such as [ipython-sql](https://github.com/catherinedevlin/ipython-sql) and [sparkmagic](https://github.com/jupyter-incubator/sparkmagic) with the following features: 

* Support for both Apache Spark and relational databases
* Asynchronous execution (useful for long queries)
* SQL syntax highlighting
* Browser notifications for query completion
* Results directly returned as Pandas dataframes 

For more information check out the [GitHub repository](https://github.com/pivotal/sql_magic).
