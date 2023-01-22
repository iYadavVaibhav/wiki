# SQL Operations in Python

## Database Connections 101

- Connection should **not** be left open, it should be open right after write and disposed once written. It can be open and closed multiple times but never left open
- Connection object should be completely **separate** from business logic.
- `with` performs the cleanup activity automatically. "Basically, if you have an object that you want to make sure it is cleaned once you are done with it or some kind of errors occur, you can define it as a **context manager** and `with` statement will call its `__enter__()` and `__exit__()` methods on entry to and exit from the with block."

### PyODBC

Works with most databased but not well with MSSQL+Pandas

```python

import pyodbc
conn_str = f"driver=SQL Server;server=<name>database=<name>;Trusted_Connection=yes"
connection = pyodbc.connect(conn_str)
cursor = connection.cursor()
sql = "select top 10 * from [db].[schema].[table]"
cursor.execute(sql)
res = cursor.fetchall()    # list of rows    
connection.close()

## Teradata
conn_str = "DRIVER={DRIVERNAME};DBCNAME={hostname};;UID={uid};PWD={pwd}"
connection = pyodbc.connect(conn_str)
fx_df = pd.read_sql(query, connection)

```

### SQLAlchemy Connection

Works as connection engine as well as ORM

  ```python
  import sqlalchemy
  conn_str = "mssql+pyodbc://server_name\schema_name/database_name?driver=SQL+Server"
  engine = sqlalchemy.create_engine(conn_str, echo=False)
  connection = engine.connect()
  sql = "select top 10 * from [db].[schema].[table]"
  cursor = connection.execute(sql)
  res = cursor.fetchall()    # list of rows 
  connection.close()
  ```

### Flask_sqlalchemy

Flask wrapper for sqlalchemy

  ```python
  from flask_sqlalchemy import SQLAlchemy

  conn_str = "mssql+pyodbc://server_name\schema_name/database_name?driver=SQL+Server"
  app.config['SQLALCHEMY_DATABASE_URI'] = conn_str
  db = SQLAlchemy(app)
  db.session.execute(sql).all() # list of rows

  ```

### Pandas

- needs a connector to database like sqlalchemy or pyodbc
- `df.to_sql('table_name', con=engine)` - sqlalchemy


### SQLite

```python

import sqlite3
conn = sqlite3.connect('database_name')
c = conn.cursor()
c.execute(query)
rows = c.fetchall()
conn.commit() # for non read tasks
conn.close()

```

### mysql-connector-python

```python
import mysql.connector
conn = mysql.connector.connect(host=host_name,user=user_name,passwd=user_password)
cursor = conn.cursor()
cursor.execute(query)
conn.commit() # for non read tasks
```

## SQLAlchemy ORM

When you’re working in an **object-oriented** language like Python, it’s often useful to think in terms of objects. It’s possible to map the results returned by SQL queries to objects, but doing so works against the grain of how the database works. Sticking with the scalar results provided by SQL works against the grain of how Python developers work. This problem is known as **object-relational impedance mismatch**.

The ORM provided by SQLAlchemy sits between the SQLite database and your Python program and transforms the data flow between the database engine and Python objects. SQLAlchemy allows you to think in terms of objects and still retain the powerful features of a database engine.

- It is ORM for Python, has two parts
  - CORE - can be used to manage SQL from python,
  - ORM - can be used in Object oriented way to access SQL from python.
- FlaskSQL_Alchemy is extenstion to SQLAlchemy.
- **ORMs** allow applications to manage a database using high-level entities such as classes, objects and methods instead of tables and SQL. The job of the ORM is to translate the high-level operations into database commands.
- It is an ORM not for one, but for many relational databases. SQLAlchemy supports a long list of database engines, including the popular MySQL, PostgreSQL and SQLite.
- The ORM translates Python classes to tables for relational databases and automatically converts Pythonic SQLAlchemy Expression Language to SQL statements

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

Steps:

- create the `author_publisher` **association table model**
- define the `Author` **class model** to the `author` database table
- This uses SQLAlchemy ORM features, including `Table`, `ForeignKey`, `relationship`, and `backref`.

<https://realpython.com/python-sqlite-sqlalchemy/#working-with-sqlalchemy-and-python-objects>

```python

## MS SQL
conn_str = "mssql+pyodbc:///?odbc_connect="+urllib.parse.quote('driver={%s};server=%s;database=%s;Trusted_Connection=yes')
## Postgres
conn_str = "postgresql://user:pass@server:port/database"

engine = sqlalchemy.create_engine(conn_str)

conn = engine.connect()
conn.execute(f'INSERT INTO ..')

with engine.connect() as conn:
  conn.execute("UPDATE emp set flag=1")

df.to_sql('table_name', con=engine, schema='dbo', if_exists='append', index=False)

```
