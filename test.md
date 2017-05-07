# Introducing sql_magic: Jupyter Magic to Write SQL Code for Apache Spark and Relational Databases

Data scientists love Jupyter Notebook, Python, and Pandas. And they also write SQL. I created sql_magic to facilitate writing SQL code from Jupyter Notebook to use with both Apache Spark (or Hive) and relational databases such as PostgreSQL or Pivotal Greenplum. The library supports [SQLAlchemy](https://www.sqlalchemy.org/) connections, [Spark Session and SparkSession](https://docs.databricks.com/spark/latest/gentle-introduction/sparksession.html) objects, and other connections types. The `%%readsql` magic function returns results as a Pandas DataFrame for further analysis and plotting. 

~~~
%%readsql df_result
SELECT {col_names}
FROM {table_name}
WHERE age < 10
~~~

The core components of the sql_magic extension are:

* Execute SQL from a Jupyter cell, returning the result as a Pandas dataframe
* Python variables can be embedded directly inside SQL code using the {string formatting} syntax
* Asynchronous query execution using the `-a` flag
* Browser notifications upon query completion
* SQL syntax highlighting inside a Jupyter cell



## Installation

`pip install sql_magic`

More information can be found in the [GitHub repository](https://github.com/pivotal/sql_magic).

## Usage: Execute SQL on a PostgreSQL Database using SQLAlchemy

Relational databases can be accessed using SQLAlchemy or any library implementing the [Python DB 2.0 Specification](https://www.python.org/dev/peps/pep-0249/) such as the [psycopg2](http://initd.org/psycopg/) Python library.

~~~
# create SQLAlchemy engine for postgres
from sqlalchemy import create_engine
postgres_engine = create_engine('postgresql://{user}:{password}@{host}:5432/{database}'.format(**connect_credentials))
~~~

The sql_magic library is loaded using the `%load_ext` iPython extension syntax and is pointed to the SQLAlchemy engine object as follows: 

~~~
# load and configure extension
%load_ext sql_magic
%config SQLConn.conn_object_name = 'postgres_engine'
~~~

Python variables can be directly referenced in the SQL query using the string formatting syntax as sql_magic executes the code in the Jupyter cell as a string. 

~~~
# variables for use in SQL query
table_name = 'titanic'
cols = ','.join(['age','sex','fare'])
~~~

Finally, SQL code is executed with the %readsql cell magic. A browser notification containing the execution time and result dimensions will automatically appear once the query is finished.

~~~
%%readsql df_result
SELECT {cols}
FROM {table_name}
WHERE age < 10
~~~

*SHOW PLOT HERE

The code can be executed asynchronously using the -a flag.

~~~
%%readsql df_result -a
~~~

Since results are automatically saved as a Pandas dataframe, we can easily visualize our results using the built-in Pandas’ plotting routines:

~~~
df.plot('age', 'fare',kind='scatter')
~~~

*SHOW PLOT HERE

Note: For code that doesn’t return a result such as creating a table, the `%%execsql` magic must be used (relational databases only).

~~~
%%execsql
CREATE TABLE table123
AS
SELECT *
FROM table456 
~~~

## Using sql_magic with Spark or Hive

The syntax for connecting with Spark is the same as above; simply point the connection object to a SparkSession, SQLContext, or HiveContext object:

~~~
# spark 2.0+
%config SQLConn.conn_object_name = 'spark'

# spark 1.6 and before
from pyspark.sql import HiveContext  # or SQLContext
hive_context = HiveContext(sc)
%config SQLConn.conn_object_name = 'hive_context'
~~~

## Configuration

Both browser notifications and displaying results to standard out are enabled by default. Either of these can be temporarily disabled with the `-n` and `-d` flags, respectively. They can also be disabled using the `%config` magic function.

### Flags

Notifications and auto-display can be temporarily disabled with flags:

~~~
positional arguments:
  table_name

optional arguments:
  -h, --help     show this help message and exit
  -n, --notify   Toggle option for notifying query result
  -a, --async    Run query in seperate thread. Please be cautious when
                 assigning result to a variable
  -d, --display  Toggle option for outputing query result
~~~

### Default values

Notifications and auto-display can be disabled by default using `%config`. If this is done for either option, the flags above will temporarily enable these features.

~~~
SQLConn options
-------------
SQLConn.conn_object_name=<Unicode>
    Current: u'conn'
    Object name for accessing computing resource environment
SQLConn.notify_result=<Bool>
    Current: True
    Notify query result to stdout
SQLConn.output_result=<Bool>
    Current: True
    Output query result to stdout
~~~

~~~
%config SQLConn.output_result = False  # disable browser notifications
%config SQLConn.notify_result = False  # disable output to std ou
~~~

That’s it! Give sql_magic a try and let us know what you think. Anybody can contribute improvements or bug fixes on the [GitHub repository](https://github.com/pivotal/sql_magic); just send a pull request.

### Acknowledgements

Thank you to Scott Hajek, Greg Tam, and Srivatsan Ramanujam, along with the rest of the Pivotal Data Science team for their help in developing this library. This library was also inspired from the work of the [ipython-sql](https://github.com/catherinedevlin/ipython-sql) [sparkmagic](https://github.com/jupyter-incubator/sparkmagic) libraries.
