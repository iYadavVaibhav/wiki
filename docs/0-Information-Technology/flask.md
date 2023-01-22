---
date: 2019-05-03
title: Python Flask
---

# Flask

Flask is a microframework in Python. It is used to create a webapp. It can start a python web server. It can handle HTTP requests. It can also be used to make a webapp API.

## Python Interpretors and Environments

### Why setup a Virtual Environment

Why? - It gives different apps isolation and personal environment so that modules don't interfere and it is easy when we have to productionize the app.

### What is Python Virtual Environment?  

- Basically a folder having python core binaries and use this new installation to install libraries that will be specific to this installation (or environment).
- It is an isolated environment
- When you activate a virtual environment, your PATH variable is changed. The Scripts directory of `venv_app` is put in front of everything else, effectively overriding all the system-wide Python software.

### Set correct Python Interpretor

- there may be many python version and interpreter installed on your machine. eg, `/bin/python3` or `~/anaconda/bin/python`
- identify the default, `which python`
- find all installed binaries, `locate "bin/python"` - this lists all the binaries including venvs.
- to set anaconda python as default python, add following to `~/.bashrc`
  - `export PATH="/home/username/anaconda3/bin:$PATH"`
  - then restart terminal or do `source ~/.bashrc`
  - check with `which python`, should be using anaconda.

### How to use Virtual Environments in Python3?

- **Create**
  - python3 or python depends on your installation.
  - `python3 -m venv venv_app` - creates folder venv_app
  - `source venv_app/bin/activate` - activates this environment, for use
  - check using `python3 -V` or `which python3`
  - now do `python -m pip install <package-name>` - this installs the package only in this environment.
  - add `#!venv_app/bin/python3` on top of main.py file to make it use this python.

- **Deactivate**
  - `deactivate` to deactivate the current env.

- **View & Share**
  - `python -m pip list` to see installed libs.
  - `python3 -m pip freeze` to share libs.
    - `python3 -m pip freeze > requiremeents.txt` to make a file.
    - `python3 -m pip install -r requirements.txt` to install all libs from file

- **Delete**
  - `rm -r venv_app` delete

## Flask Hello World

- Create a python file `main.py`:

  ```python
  #!venv_app/bin/python3
  from flask import Flask
  app = Flask(__name__)

  @app.route('/')
  def index():
      return "Hello from Flask App!"

  if __name__ == '__main__':
      app.run(debug=True)
  ```

- run using `python main.py`

- access at `http://localhost:5000` in browser.

## Run a Flask App - Ways

- Use Python
  - using `path_to_python3 path_to_main.py`

- Make the file executable using
  - `chmod a+x main.py` to make app executable.
  - `./main.py` runs app on localhost in debug mode using `#!venv_app/bin/python3`.

- Using Flask CLI
  - `export FLASK_APP=main.py` will make an variable that tells python which app to run.
  - `flask run` executes the app or if flask is not in path then do `python -m flask run`

## Flask native modules

- `Flask` - is a class of which instance `app` is created using `app = Flask(__name__)`
- `redirect` - takes URL to redirect to.
- `url_for` - takes function name as str and gives its URL.
  - `redirect(url_for("profile"))`
- `session` - can be used to store values, specific to current session, it is server side. Helps to pass values from one function to another.
  - `session["username"] = username`
  - permanent sessions store session data for a timeperiod
- `flash` - lets send extra messages to frontend
  - `flash("The message", "info")` message and level.
  - `get_flashed_messages()` to get messages

### Flask Blueprints

- Blueprint lets us divide app into mini apps. It is a collection of views, templates, static files that can be applied to an application. Blueprints are a great way to organize your application.
- you can have a sub-folder for mini app, having its own static and templtes folder, just app `__init__` to the sub-folder and import it to main app
- In a functional structure, each blueprint is just a collection of views. The templates are all kept together, as are the static files.

```python
from flask import Blueprint

second = Blueprint("second", __name__)

@second.route("/home")
def home():
    return ("from second")
```

- and in `app.py`

```python
from flask import Flask
from second import second

app = Flask(__name__)

app.register_blueprint(second, url_prefix="")

@app.route("/")
def home():
    return "Hi"

if __name__ == "__main__":
    app.run(debug=True)
```

- Link - <http://exploreflask.com/en/latest/blueprints.html>



## Flask Logging

```python
import logging

app = Flask(__name__)
logging.basicConfig(level=logging.DEBUG)

app.logger.info('some log')
```

## Deployment - PythonAnywhere

- WSGI configuration
  - On your Web App Configuration page, open "WSGI configuration file", and ensure you add your project folder to code below.

  ```python
  import sys

  # add your project directory to the sys.path
  project_home = u'/home/username/mysite'
  if project_home not in sys.path:
      sys.path = [project_home] + sys.path

  # import flask app but need to call it "application" for WSGI to work
  from flask_app import app as application  # noqa
  ```

## Access "Virtual Machine localhost flask app" on a host machine

- Suppose on an Ubuntu VM a flask app is running on localhost and you want to access it from you host machine that is Mac.

- Run flask app with `app.run(host='0.0.0.0', debug=True)`
- This tells your operating system to listen on all public IPs.
- then access `192.168.10.33:5000` from host machine.

Now that our app is running we can add a database to this app. We will use FlaskSQLAlchemy package for this. Or Pandas.


## Flask App for Machine Learning - Pandas

You can use Flask it with Pandas, Matplot and other ML libraries to make it easily usable for end users.

- Import all your libs in flask app that you have used in Jupyter NB.
- Add code and functions to read data and perform tasks.
- Flask routes are executed for each request, so keep data reads outsite to read them once.
- `return render_template( 'search.html', data=df_result.to_html(classes='table table-striped table-hover')` - to_html makes html table that can be passed to html page.
- `{{ data|safe }}` - safe makes it as markup and browser renders it.

Reference:

- <https://sarahleejane.github.io/learning/python/2015/08/09/simple-tables-in-webapps-using-flask-and-pandas-with-python.html>



## Add DataBase to Flask app using Flask-SQLAlchemy

It is an extension of `SQLAlchemy` which is ORM for major databases.

- It is an designe for Flask that adds support for SQLAlchemy to your application.
- You can define table as a class, called model, with member variables as column names.
- Create an object to make new instance.
- Example

```python
class User(db.Model):
    __tablename__ = 'users'
    id = db.Column(db.Integer, primary_key = True)
    username = db.Column(db.String(50), unique=True)
    admin = db.Column(db.Boolean)
    created_on = db.Column(db.DateTime(), default=datetime.now)
    updated_at = db.Column(db.DateTime, default=datetime.utcnow, onupdate=datetime.now)


db.session.add(obj)     # insert new record to database
db.session.delete(obj)  # delete a record from database
db.session.commit()     # updates modifications in object if any

# where clause
user = User.query.filter_by(username=data['username']).first() # or .all()

# select * from users
users = User.query.all()
```

### Creating Tables

Once you have created a db model in flask app, you can create db and tables using follwing steps:

- `python`
- `from main import db` main is filename of flask app
- `db.create_all()` creates all tables from model class

Now you can check SQL for tables created. You can do:

- `sqlite3 filename.db`
- `.tables`

Caution:

- Provide full path to db file, so that apache can locate it.

### Migrations using Alembic and Flask-Migrate

Alembic is a database migration tool for SQLAlchemy. It generates Py script to keep the database schema update with the models defined. It help upgrade and roll back the schemas.

- We use library `Flask-Migrate` library that helps use alembic for flask apps using cli
  - `flask db migrate -m 'relations_added'` - creates migration script to update the tables. -m helps add caption to file.
  - `flask db upgrade` can be used to run the Py file generated and this will updated the database schema.

Reference:

- <https://www.digitalocean.com/community/tutorials/how-to-make-a-web-application-using-flask-in-python-3>
- SQLite explorer - <https://sqlitebrowser.org/>




## REST API and RESTful Web Services - The Basics Guide

What is REST?

- Client and Server are separate
- **Stateless:** No information from a request is stored on client to be used in other requests, eg, no session can be started, if authentication is required, username and password need to be sent with every request.

What is RESTful API:

- There is a resource, eg, tasks
- It has endpoints for CRUD operations
- HTTP methods, GET PUT POST DELETE, are used for these operations.
- Data is provided with these requests in no particular format but usually as:
  - JSON blob in request body, or
  - Query String arguments as portion of URL.


| HTTP Method | URI | Action |
| ---|---|--- |
| GET | <http://[hostname]/todo/api/v1.0/tasks> | Retrieve list of tasks |
| GET | <http://[hostname]/todo/api/v1.0/tasks/[task_id]> | Retrieve a task |
| POST |  <http://[hostname]/todo/api/v1.0/tasks> | Create a new task |
| PUT | <http://[hostname]/todo/api/v1.0/tasks/[task_id]> | Update an existing task |
| DELETE |  <http://[hostname]/todo/api/v1.0/tasks/[task_id]> | Delete a task |

Data of a task can be, JSON blob, as:

```py
{
  'id': 1,
  'title': 'Title of a to do task',
  'description': 'Description of to do task', 
  'done': False
}
```

- This API can be consumed by client side app which can be single page HTML.
- Note, JSON obect is defined in python as dict, `jonify` converts and send as JSON Object.

## Serving over HTTPS

- generate certificate key using `openssl req -x509 -newkey rsa:4096 -nodes -out cert.pem -keyout key.pem -days 365`
- allow insecure connection to localhost in chrome, paste `chrome://flags/#allow-insecure-localhost`
- <https://blog.miguelgrinberg.com/post/running-your-flask-application-over-https>





----------------------------------------------------------

*Refinement Needed*

# Flask Mega Tutorial Notes

This is understanding of a tutorial by Miguel Grinberg, we are learning to create a micro-blogging site using flask and other dependencies.

- `from flask import Flask, render_template, request, url_for, flash, redirect` libraries
  - render_template - to show html page.
  - request - to get get/post data
  - url_for - to get path for resource
  - flash - to store messages to session var, handy for form error messages.
    - add session info, after `app = Flask(__name__)` add `app.secret_key = 'super secret key'`


- `before_request` is a flask native function that gets executed before any request is made. It can be used to update last_seen timestamp.

## Flask Debugging

- `export FLASK_DEBUG=0` makes the environment PROD else it is DEV or TEST.
- `export FLASK_APP=microblog.py` will make an variable that tells python which app to run.
- `flask run` executes the app or if flask is not in path then do `> python -m flask run`

## Flask DB Migrate commands

We can use to create table and manage changes

- `flask db init` creates migration folder
- `flask db migrate` creates scripts for table creation
- `flask db upgrade` executes scripts to make table changes

## Start a python email server

`(venv) $ python -m smtpd -n -c DebuggingServer localhost:8025` this command starts emulated email server.

Some variables that we might need to export:

```bash
export MAIL_SERVER=smtp.googlemail.com
export MAIL_PORT=587
export MAIL_USE_TLS=1
export MAIL_USERNAME=email_id@domain.com
export MAIL_PASSWORD="password"
```

## Making new forms in app

We need to define following:

- **(M)** Form Class - in module `forms.py` make class with fields and validate functions.
- **(V)** HTML template - in `templates` folder, rendered from route with form passed as data. Display form elements, errors and hidden_tag.
- **(C)** Route - in module `routes.py`, define new route, import form class and use. On submit, create object of model class to save/query data.
- Create a link to access new route.

## Saving form data and tables

- **ORM** can be created in `modules.py`. We can define classes for our tables. Classes also define relationships. Have functions to query data.

- use them in routes to save and query data.
- uses db service to handle database operations.

## Routes.py

- imports forms classes from `forms.py` for:
    1. getting form data via POST
    2. passing form to html template for making form components and displaying errors

- import orm model classes from `models.py` for:
    1. making object of DB Table model
    2. submit and commit new data
    3. query model class to fetch data

## app \_\_init\_\_.py

This defines app folder as a python package.

- It is where we link all modules (.py files in folder app) together as a Flask app.
- We import routes, models and error modules here.

**Services:** Here we have various services defined. We use them across app like db, login, mail.

## Blueprints

We can separate out application logical modules in application. Like we can create separate package for auth, error handling etc.

`from flask import Blueprint` is what we need to use.

To register a blueprint, the `register_blueprint()` method of the Flask application instance is used. When a blueprint is registered, any view functions, templates, static files, error handlers, etc. are connected to the application.

In each Blueprint package we make \_\_initII.py that has name information and imports routes. Routes further imports and links model and controller.

## Open doubts

- UserMixin?
- Why we pass Classes as param to Class?

## Making modules

- to get url_for use `module.route`. For eg: main.index.
- But to render template use only `index.html` without module name.

## Elastic Search

- You can install elastic search by `brew install elasticsearch` on mac.
- Access `http://localhost:9200` to view service JSON output.
- Also, install in python `pip install elasticsearch`
- To have launched start elasticsearch now and restart at login: `brew services start elasticsearch`
- Or, if you don't want/need a background service you can just run: `elasticsearch`

Todo: Search not working, fix it.

## Links

- [ ] - Flaskr - Blogging Template - <https://flask.palletsprojects.com/en/0.12.x/tutorial/introduction/>

- Generate SQLAlchemy class model from database table - `sqlacodegen mssql+pyodbc://GBRDSM050000617\GMI_SQLVIRT_TEST/TC09_Dev_Db/historic_responses?driver=SQL+Server --outfile db.py`
