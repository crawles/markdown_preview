<p align="center">
  <img src="img.jpg" width="60%" align="middle">
</p>

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

---

Links:
* [GitHub repository](https://github.com/pivotal/sql_magic)
* [Jupyter Notebook](http://jupyter.org/)
* [Pandas](http://pandas.pydata.org/)
* [SQLAlchemy](https://www.sqlalchemy.org/)
* [Pivotal Greenplum](https://pivotal.io/pivotal-greenplum)
* [Pivotal HDB](https://pivotal.io/pivotal-hdb)
* [Apache Spark](http://spark.apache.org/)
* [ipython-sql](https://github.com/catherinedevlin/ipython-sql)
