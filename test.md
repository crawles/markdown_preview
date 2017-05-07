# Introducing sql_magic: Jupyter Magic For Working With Spark and Databases

Data scientists love Jupyter Notebook, Python, and Pandas. And they also write SQL. Thus I created this extension to facilitate writing SQL code from Jupyter Notebook with both Spark (or Hive) and relational databases. The %%readsql magic function returns results as a Pandas DataFrame further analysis and plotting. 

~~~
%%readsql df_result
SELECT {col_names}
FROM {table_name}
WHERE age < 10
~~~

The core components of the sql_magic extension are:

Execute SQL from a Jupyter cell, returning the result as a Pandas dataframe
Python variables can be embedded directly inside SQL code using the {string formatting} syntax
Asynchronous query execution using the `-a` flag
Browser notifications upon query completion
SQL syntax highlighting inside a Jupyter cell



## Installation

`pip install sql_magic`

More information can be found in the [GitHub repository](https://github.com/pivotal/sql_magic).

## Example using a SQLAlchemy Connection Engine

Relational databases can be accessed using SQLAlchemy or any library implementing the (Python DB 2.0 Specification)[https://www.python.org/dev/peps/pep-0249/] the such as the (psycopg2)[http://initd.org/psycopg/].

#create SQLAlchemy engine for postgres
from sqlalchemy import create_engine
postgres_engine = create_engine('postgresql://{user}:{password}@{host}:5432/{database}'.format(**connect_credentials))

The sql_magic library is loaded using the %load_ext iPython extension syntax. The connection object SQLConn.conn_object_name is configured using %config. 

#load and configure extension
%load_ext sql_magic
%config SQLConn.conn_object_name = 'postgres_engine'

Python variables can be directly referenced in the SQL query using the string formatting syntax as sql_magic executes the code in the Jupyter cell as a string. 

#variables for use in SQL query
table_name = 'titanic'
cols = ','.join(['age','sex','fare'])

Finally, SQL code is executed with the %readsql cell magic. A browser notification will automatically appear once the query is finished.

%%readsql df_result
SELECT {cols}
FROM {table_name}
WHERE age < 10

The code can be executed asynchronously using the -a flag.

%%readsql df_result -a
SELECT {cols}
FROM {table_name}
WHERE age < 10

Since results are automatically saved as a pandas dataframe, we can easily visualize our results using the built-in Pandas’ plotting routines:

%matplotlib inline
df.plot('age', 'fare',kind='scatter')

## sql_magic with Spark and/or Hive

The syntax for connecting with Spark is the same as above with the difference being a Spark connection object must be provided for config.

# spark 2.0+
%config SQLConn.conn_object_name = 'spark'

# spark 1.6 and before
from pyspark.sql import HiveContext  # or sqlContext
hive_context = HiveContext(sc)
%config SQLConn.conn_object_name = 'hive_context'

## Configuration

Both browser notifications and displaying results to standard out are enabled by default. Either of these can be temporarily disabled with the -n and -d flags, respectively. They can also be disabled using the %config magic function.

### Flags

Notifications and auto-display can be temporarily disabled with flags:
<pre>
positional arguments:
  table_name

optional arguments:
  -h, --help     show this help message and exit
  -n, --notify   Toggle option for notifying query result
  -a, --async    Run query in seperate thread. Please be cautious when
                 assigning result to a variable
  -d, --display  Toggle option for outputing query result
</pre>

### Default values

#alerts and display are automatically enabled
%config SQLConn

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

%config SQLConn.output_result = False
%config SQLConn.notify_result = False

That’s it! Give sql_magic a try and let us know what you think. Anybody can contribute improvements or bug fixes on the [GitHub repository](https://github.com/pivotal/sql_magic); just send a pull request.

### Acknowledgements

Thank you to Scott Hajek, Greg Tam, and Srivatsan Ramanujam, along with the rest of the Pivotal Data Science team for their help in developing this library.
