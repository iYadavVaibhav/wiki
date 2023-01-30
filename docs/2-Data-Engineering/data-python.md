# SQL Operations in Python

## Database Connections 101

- Connection should **not** be left open, it should be open right after write and disposed once written. It can be open and closed multiple times but never left open
- Connection object should be completely **separate** from business logic.
- `with` performs the cleanup activity automatically. "Basically, if you have an object that you want to make sure it is cleaned once you are done with it or some kind of errors occur, you can define it as a **context manager** and `with` statement will call its `__enter__()` and `__exit__()` methods on entry to and exit from the with block."

### PyODBC

Works with most databases but not well with MSSQL+Pandas

```python

import pyodbc

# MS SQL Server
connection_url = f"driver=SQL Server;server=<name>database=<name>;Trusted_Connection=yes"

## Teradata
connection_url = "DRIVER={DRIVERNAME};DBCNAME={hostname};;UID={uid};PWD={pwd}"

connection = pyodbc.connect(connection_url)
cursor = connection.cursor()
sql = "select top 10 * from [db].[schema].[table]"
cursor.execute(sql)
res = cursor.fetchall()    # list of rows    
connection.close()

fx_df = pd.read_sql(query, connection)

```

### SQLAlchemy Connection

Works as connection engine as well as ORM

  ```python

  import sqlalchemy

  connection_url = "mssql+pyodbc://server_name\schema_name/database_name?driver=SQL+Server"
  
  ## MS SQL
  connection_url = "mssql+pyodbc:///?odbc_connect="+urllib.parse.quote('driver={%s};server=%s;database=%s;Trusted_Connection=yes')
  
  ## Postgres
  connection_url = "postgresql://user:pass@server:port/database"

  engine = sqlalchemy.create_engine(connection_url, echo=False)
  connection = engine.connect()
  sql = "select top 10 * from [db].[schema].[table]"
  cursor = connection.execute(sql)
  res = cursor.fetchall()    # list of rows 
  connection.close()

  # OR -- 

  with engine.connect() as connection:
    connection.execute("UPDATE emp set flag=1")

  df.to_sql('table_name', con=engine, schema='dbo', if_exists='append', index=False)
  ```

### Flask_sqlalchemy

Flask wrapper for sqlalchemy

  ```python
  from flask_sqlalchemy import SQLAlchemy

  connection_url = "mssql+pyodbc://server_name\schema_name/database_name?driver=SQL+Server"

  app.config['SQLALCHEMY_DATABASE_URI'] = connection_url
  
  db = SQLAlchemy(app)
  db.session.execute(sql).all() # list of rows

  ```

### Flask SQLite SQLAlchemy

```python
import os, sqlite3

basedir = os.path.abspath(os.path.dirname(__file__))
connection_url = 'sqlite:///' + os.path.join(basedir, 'data.sqlite')

# then same as SQLAlchemy
```

- Execution - from flask shell do `db.create_all()` - creates table with schema.

### Pandas

- needs a connector to database like sqlalchemy or pyodbc
- `df.to_sql('table_name', con=engine)` - sqlalchemy


### SQLite

```python

import sqlite3
connection = sqlite3.connect('database_name')
cursor = connection.cursor()
cursor.execute(query)
rows = cursor.fetchall()
connection.commit() # for non read tasks
connection.close()

```

### mysql-connector-python

```python
import mysql.connector
connection = mysql.connector.connect(host=host_name,user=user_name,passwd=user_password)
cursor = connection.cursor()
cursor.execute(query)
connection.commit() # for non read tasks
```

## SQLAlchemy ORM

- **Why?** - When you’re working in an **object-oriented** language like Python, it’s often useful to think in terms of objects. It’s possible to map the results returned by SQL queries to objects, but doing so works against the grain of how the database works. Sticking with the scalar results provided by SQL works against the grain of how Python developers work. This problem is known as **object-relational impedance mismatch**.

- **What?**
  - The ORM provided by SQLAlchemy sits between the database and Python program and transforms the data flow between the database engine and Python objects. SQLAlchemy allows you to think in terms of objects and still retain the powerful features of a database engine.
  - It is ORM for Python, has two parts
    - CORE - can be used to manage SQL from python,
    - ORM - can be used in Object oriented way to access SQL from python.

- **ORMs** allow applications to manage a database using high-level entities such as classes, objects and methods instead of tables and SQL. The job of the ORM is to translate the high-level operations into database commands.
  - It is an ORM not for one, but for many relational databases. SQLAlchemy supports a long list of database engines, including the popular MySQL, PostgreSQL and SQLite.
  - The ORM translates Python classes to tables for relational databases and automatically converts Pythonic SQLAlchemy Expression Language to SQL statements

- **Mappings** - There are two types of mapping
  - Declarative - new - more like oops
  - Imperative - old - less like oops

```python
from sqlalchemy import Column, Integer, String, ForeignKey, Table
from sqlalchemy.orm import relationship, backref
from sqlalchemy.ext.declarative import declarative_base

Base = declarative_base()

## Instance of Table Class, creates many to many association
author_publisher = Table(
  "author_publisher",
  Base.metadata,
  Column("author_id", Integer, ForeignKey("author.author_id")),
  Column("publisher_id", Integer, ForeignKey("publisher.publisher_id")),
)

## Inherits Base class
class Author(Base):
  __tablename__ = "author"
  author_id = Column(Integer, primary_key=True)
  first_name = Column(String)
  last_name = Column(String)
  books = relationship("Book", backref=backref("author"))
  publishers = relationship(
    "Publisher", secondary=author_publisher, back_populates="authors"
  )
```

- **Steps**
  - create the **association table model**, `author_publisher`
  - define the **class model**, `Author` for `author` database table
  - This uses SQLAlchemy ORM features, including `Table`, `ForeignKey`, `relationship`, and `backref`.

- **Links**
  - <https://realpython.com/python-sqlite-sqlalchemy/#working-with-sqlalchemy-and-python-objects>
  - Flask SQLAlchemy in Flask Notes

