---
date: 2019-05-03
---

# Flask

Flask is a microframework in Python. It is used to create a webapp. It can start a python web server. It can handle HTTP requests. It can also be used to make a webapp API.

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

- python
  - `python main.py`

- executable
  - `chmod a+x main.py` to make app executable.
  - `./main.py` runs app.

- Flask CLI
  - `flask --app main.py run`
    or
  - `export FLASK_APP=main.py` will make an variable that tells python which app to run.
  - `flask run` executes the app or if flask is not in path then do `python -m flask run`



## Flask native modules

_flask basics, request-response handling, contexts_

- **Application Instance**
  - `Flask()` - is a class.
  - All Flask applications must create an **application instance** (object of class `Flask`). The web server passes all requests it receives from clients to this object for handling, using a protocol called Web Server Gateway Interface (WSGI).
  - `app = Flask(__name__)` name is passed to `Flask` class constructor so it knows the location of app and hence can locate static and template.

- **Requests**
  - Request from client has lot of information in it, like header, user-agent, data etc. This information is available in `request object` and is made available to a `view-route function` to handle it. This object is not passed as an argument to function, rather it is made available using `contexts`.
  - `request` Object has methods and attributes having info on method, form, args, cookies, files, remote_addr, get_data().

- **Contexts**
  - It let certain objects to be globally accessible, but are not global variable. They are globally accessible to only one thread. There can be multiple threads serving multiple requests from multiple client.
  - Context is simply data that is specific to something. Eg
    - **App-context** is specific to app, like its mail server, its database location, or other configurations
    - **Request-context** is specific to request, like its browser, its client, its form data, its headers
  - this data is stored in object, in attribute such as `config`
  - this data is used by extensions in flask, hence they do not run if context is not available.
  - context is automatically made available once app is initialized.
  - context can be made explicitly available by calling `with app.app_context():`
  
- **Request Handling**
  - when there is request, web server activates a thread that initializes app and this app context is pushed with data that is available globally, similarly request context is also pushed.

    ```mermaid
    graph LR;
    Web_Browser --> request --> web_server --> Flask_app_instance --> route --> function_to_execute
    ```

- **Flask variables for Request Handling**
  - `current_app` variable in Application context, has info of active application.
  - `g` variable in Application context, temp access **during** handling of a request. It is reset once request is served. Holds app info hence app context.
  - `request`, in request context, obj having client req data.
  - `session`, in request context, a dictionary to store values that can be accessed in different requests from same session.
  - Flask, in backend, makes these available to thread before dispatching a request and removes after request is handled. Explicitly, `current_app` can be made available by invoking `app.app_context()`
  - [ ] How does flask differentiate requests and clients?

- **Request Hooks**
  - They are functions that can execute code before or after each request is processed. They are implemented as decorators  (functions that execute on event). These are the four hooks supported by Flask:
  - `before_request` - like authenticate
  - `before_first_request` - only before the first request is handled. Eg, to add server initialization tasks.
  - `after_request` - after each request, but only if no unhandled exceptions occurred.
  - `teardown_request` - after each request, even if unhandled exceptions occurred.
  - `g` context global storage can be used to share data between hook and view functions.

- **Routes or View Functions**
  - They handle application URLs.
  - URL-maps can be seen using `app.url_map`
  - redirect to url
    - `redirect` - takes URL to redirect to.
    - `redirect(url_for("profile"))`
    - `url_for()` utility builds URL for view-function giving route from app-url-map. takes function name as str and gives its URL. Eg:
      - `url_for('user', name='john', page=2, version=1)` would return `/user/john?page=2&version=1`, they are good to build dynamic URLs that can be used in templates.
      - `url_for('user', name='john', _external=True)` would return `http://localhost:5000/user/john`.
      - `url_for('static', filename='css/styles.css', _external=True)` would return `http://localhost:5000/static/css/styles.css`.
      - `/static/<filename>` is special route added by Flask to serve static files.

- **Response Object**
  - Response is returned by view-function as a string (usually HTML) along with **status code** but can also contain **headers**. So rather than sending comma separated tuple values, flask lets create response object using `make_response()`.

    ```python
    from flask import make_response

    @app.route('/')
    def index():
        response = make_response('<h1>Some response with a cookie!</h1>')
        response.set_cookie('message', '51')
    return response
    ```

  - `return redirect('http://www.example.com')` is a response with URL and status code 302, however Flask lets it do easily using `redirect()` method. Another such is `abort(404)` which is treated as exception.


  - **session** - can be used to store values, specific to current session, it is server side. Helps to pass values from one function to another.
    - `session["username"] = username`
    - permanent sessions store session data for a time period

  - **flash** - lets send extra messages to frontend
    - `flash("The message", "info")` message and level.
    - `get_flashed_messages()` to get messages
    - it lets record a message at the end of a request and access it next request and only next request.

## Templates in Flask

Templates can be used to build responses.

- **render_template()**
  - it makes use of template
  - `return render_template('user.html', name=name)`

- **Jinja Templates**
  - Templates are HTML file having additional Python like code in **placeholders**.
  - Placeholders can have **variables** and **expressions**.
  - They get replaced with value when template is rendered by **JinJa2**, the template engine.
  - This lets build dynamic content on execution.
  - It lets **inherit**, **extend** and **import** templates.
  - More documentation on [template design](https://jinja.palletsprojects.com/en/latest/templates/) and [tips and tricks](https://jinja.palletsprojects.com/en/latest/tricks/)
  - Example template is below.

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <title>My Webpage</title>
    </head>
    <body>
        <ul id="navigation">
        {% for item in navigation %}
            <li><a href="{{ item.href }}">{{ item.caption }}</a></li>
        {% endfor %}
        </ul>

        {% if kenny.sick %}
          Kenny is sick.
        {% elif kenny.dead %}
          You killed Kenny!  You bastard!!!
        {% else %}
          Kenny looks okay --- so far
        {% endif %}

        <h1>My Webpage</h1>
        {{ a_variable }}

        {# a comment #}
    </body>
    </html>
    ```

  - **Filters**
    - pass value over pipe to filter functions like upper, lower, title, trim, safe.
    - eg, `Hello, {{ name|capitalize }}`
    - Full list of filters [here](https://jinja.palletsprojects.com/en/latest/templates/#list-of-builtin-filters)
  
  - **Delimiters**
    - {% ... %} for Statements
    - {{ ... }} for Expressions to print
    - {# ... #} for Comments, not included in the template output.

  - **Assignments**
    - lets set a value to var and use it for logic building. We have to use namespace.

      ```html
      {% set ns = namespace(index=0) %}
      {% for nav_item in nav %} 
        {% if ns.index !=0 %}
          --- some stuff ---
        {% endif %}
        {% set ns.index = ns.index + 1 %}
      {% endfor %}
      ```

  - {{ super() }}, includes code from parent block, if overriding a block.


- **Error Handlers**
  - `@app.errorhandler` is decorator that lets return a view from template for error responses like 404 and 500.

    ```python
    @app.errorhandler(404)
    def page_not_found(e):
        return render_template('404.html'), 404

    @app.errorhandler(500)
    def internal_server_error(e):
        return render_template('500.html'), 500
    ```


## Flask Extensions

Most extensions use app context to initialize themselves, eg:

```python
from flask_bootstrap import Bootstrap
app = Flask(__name__)
bootstrap = Bootstrap(app)
```

### Bootstrap in Flask

- **Bootstrap** - provides templates and blocks that can be used in Jinja2 Templates
  - installation `pip install flask-bootstrap`
  - eg, `{% extends "bootstrap/base.html" %}` - base.html does not exist but is available via extension. Others are `navbar`, `content`, `script`

  ```html
  {% block scripts %}
  {{ super() }} <!--includes base scripts too, else overrides-->
  <script type="text/javascript" src="my-script.js"></script>
  {% endblock %}
  ```

### Moment JS in Flask

- **Moment.js Flask-Moment** - Localization of Dates and Times
  - server should send UTC, client should present in local time and formatted to region using JavaScript.
  - `Moment.js` is perfect for this and is available as flask extension. It can be used in Jinja2 template.
  - `pip install flask-moment`
  - include the script, `jQuery` is already attached as part of bootstrap

    ```html
    {% block scripts %}
      {{ super() }}
      {{ moment.include_moment() }}
    {% endblock %}
    ```

    ```python
    from flask_moment import Moment

    moment = Moment(app)
    
    return moment(datetime.utcnow()).format('LLL') # local time
    return moment(datetime.utcnow()).fromNow(refresh=True) # a few seconds ago..
    ```

## Forms in Flask

- **WTForms** - Object Oriented Form building, rendering and validations.
  - supports forms validation, CSRF protection, internationalization (I18N), rendering form and more for any Python framework, its generic. [WTForms](http://wtforms.simplecodes.com/).
  - Cons - It lets you build form in python and help validate it. It adds a extra learning curve than using HTML for the same. It same as ORM that you define things as class variables.
  - You have to build a template but can use `Form.field`
  - Pros - It lets you extend forms.
    - lets you show error easily without using `flash`.
  - Building
  
    ```python
    from wtforms import Form, BooleanField, StringField, validators

    class RegistrationForm(Form):
      username     = StringField('Username', [validators.Length(min=4, max=25)])
      email        = StringField('Email Address', [validators.Length(min=6, max=35)])
      accept_rules = BooleanField('I accept the site rules', [validators.InputRequired()])
    ```

- **Flask-WTF** integration of Flask and WTForms
  - Includes CSRF, file upload, and reCAPTCHA. You mostly have to use formats of WTForms but write less as few things are done automatically that are realated to Flask patter.
  - Form fields are Class variables with different field type
  - validator functions can help validate, like `Email()`.
  - Link to [Flask-WTF](http://pythonhosted.org/Flask-WTF/)
  - Building

    ```python
    from flask import Flask, render_template, session, redirect, url_for

    @app.route('/', methods=['GET', 'POST'])
    def index():
      form = NameForm() # defined as OOP model
      if form.validate_on_submit(): # cheks POST and validates
        session['name'] = form.name.data
        return redirect(url_for('index')) # POST -> back to this function as GET
      return render_template('index.html', form=form, name=session.get('name')) # When GET
    ```

  - Rendering

    ```html
    <form method="POST">
    {{ form.hidden_tag() }}
    {{ form.name.label }} {{ form.name() }}
    {{ form.submit() }}
    </form>
    ```

    - You can use bootstrap and flask-wtf combine to avoid writing template code and just use

      ```html
      {% import "bootstrap/wtf.html" as wtf %}
      {{ wtf.quick_form(form) }}
      ```

## Databases in Flask

Python has packages for most database engines like MySQL, Postgres, SQLite, MongoDb etc. If not, you can use ORM that lets you use Python objects to do SQL operations, SQLAlchemy or MongoEngine are such packages.

- [Flask-SQLAlchemy](http://pythonhosted.org/Flask-SQLAlchemy/) is wrapper on [SQLAlchemy](http://www.sqlalchemy.org/). You have to use SQLAlchemy pattern but it helps by making things tied to Flask way like session of SQLAlchemy is tied to web-request of flask.

  - installation `pip install flask-sqlalchemy`
  - Initiation

    ```python
    from flask_sqlalchemy import SQLAlchemy
    app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///' + os.path.join(basedir, 'data.sqlite')
    app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
    db = SQLAlchemy(app) # Object for all ops
    ```

  - **Model** - It is a Class which represents application entities, like, User, Task, Author, Book etc.
    - `__tablename__ = 'users'`
    - It has attributes that represent column name. `name = db.Column(db.String(64), unique=True)`
    - **Relationships** To create ER in OOPs way `users = db.relationship('User', backref='role')`
      - `backref` adds a back-reference to other models
      - `lazy` does not execute until you add executor.
        - add `lazy='dynamic'` to prevent query execution.
    - `db.create_all()` creates all tables if they don't exist

  - **Create** a new row
    - Create an Object of Class to build a new row. `user_john = User(username='john', role=admin_role)`
    - add - `db.session.add(user_john)`
    - `id` is added to object after `db.session.commit()commit()`
  - **Update** - `db.session.add()` also edits.
  - **Delete** - `db.session.delete(obj_name)`
  - **Read** - `query` object is available to all model class. It has filter-options and executors that build a SQL Query statement.
    - Filter-Options - `filter()`, `filter_by()`, `limit()`, `offset()`, `order_by()`, `group_by()`
    - Executors - `all()`, `first()`, `first_or_404()`, `get()`, `get_or_404()`, `count()`, `paginate()`
    - Examples
      - `User.query.all()` reads all records
      - `User.query.filter_by(role=user_role).all()`
    - `str(User.query.filter_by(role=user_role))` returns SQL query

  - **RAW SQL** - give your SQL statements
    - `db.session.execute(SQL)` returns cursor
    - `db.session.execute(SQL).all()` - returns List result set

  - **Shell Operations** - CRUD from Flask Shell
    - `flask --app hello.py shell` start shell with *app_context*, python shell will not have that.
    - `db.cratte_all()` creates SQLite file.

- **Migrations** DB changes version controlled
  - When DB is handled using ORM, all changes to DB is done via ORM. If you have to add a column it is added by ORM so it will delete the table and create new but to prevent data loss in table it will create a migration script to create and populate again.
  - `Flask-Migrate` is wrapper on `Alembic` a SQLAlchemy migration framework.
  - `pip install flask-migrate`
  - `from flask_migrate import Migrate`
  - `migrate = Migrate(app, db)`
  - `flask --app hello.py db init` migration directory, script is generated
  - `upgrade()` has data changes to be done in this migration
  - `downgrade()` rolls back to previous state
  - Steps to make a migration
    - Make changes to model classes
    - `flask --app hello.py db migrate -m "initial migration"` to generate script
    - review for accurate changes. add to source control
    - `flask--app hello.py db upgrade` to do migration in database

More on DB


It is an extension of `SQLAlchemy` which is ORM for major databases.

- It is an design for Flask that adds support for SQLAlchemy to your application.
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

Once you have created a db model in flask app, you can create db and tables using following steps:

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


## Emails in Flask

Emails can be sent using `smtplib` package from Python standard library. Email is sent by connecting to SMTP Server which takes request to send email to recipient. Localhost on port 25 is local server that can send email. External SMTP server like `mail.googlemail.com` on `587` port can be used to send emails through Google Gmail account.

- **Flask-Mail** is a extension that wraps `smtplib`
  - Installation `pip install flask-mail`
  - import `from flask_mail import Mail, Message`
  - instantiate and initialize `mail = Mail(app)`
  - build obj `msg_obj = Message('sub','sender','to')`
    - add body and html to obj, may use template for it `msg.body = render_template(template + '.txt', **kwargs)`
  - mail.send(msg_obj)
  - Sending Asynchronout Email
    - Message() object can be build in mail python file but Mail() object, which sends the email using msg_obj, should run in separate thread to avoid lags.
    - use python Thread() class from threading package to make new thread that runs the send_async_email(app,msg) functions. this functions has
      - app object of FLask() for context
      - msg object of Message() for content
      - uses mail object of Mail() to send.
    - `from threading import Thread`
    - The function which build Message(), add line
      - `thr = Thread(target=send_async_email, args=[app, msg])` - build thread obj
      - `thr.start()` execute thread separately
      - `return thr` [ ] why this is added

## Blueprint - Large App Structure in Flask

- **Application Factory** is way of initializing app
  - to serve a request, when single file app in invoked, app gets initialized with configs to serve the request. You do not have flexibility to make changes to config dynamically
  - app initialization can be delayed (or controlled) by making a function to do it, called `factory function`. This can be explicitly controlled.

- **Single py file** overall flow
  - import flask modules and extentions
  - instantiate flask app `app = Flask(__name__)`
  - configure app with all configs, eg, `app.config['MAIL_PORT'] = 587`
  - initialize extentions, eg, `mail = Mail(app)`. Not all extensions are initialized, eg, FlaskForm
  - DB ORM Classes
  - Email functions - may use templates
  - Form Classes
  - error handlers functions - may use templates
  - routes, they may use use
    - above extensions, eg - checks Form, sends email, writes to db, or returns an error
    - native - session, flash, g
    - templates.

- **templates** and **static** Resources flow
  - base template is HTML, it has blocks. Block-content can be replaced or appended
  - base-template is used to build different pages which put dynamic content in blocks.
  - static files can be used from static folder.
  - example flow
    - `base.html` has blocks, title, nav, page_content
    - index or profile have `{% extends "base.html" %}`, it tells Jinja to use base.
    - block-content can be replaced or appended using `{{ super() }}`
    - files from `static` folder using `{{ url_for('static', 'favicon.ico') }}`
    - external packages can be imported as py_var to build content as py_var and use in content. eg - wtf tempate can be imported from bootstrap to build content from form_object using `{{ wtf.quick_form(form) }}`.

- restructuring - multiple files basic structure
  
  ```python
  |-app_name      # 0 top level dir - any name
    |-app/          # 2 package having flask application
      |-templates/
      |-static/
      |-main/         # 5 BP sub pkg
        |-__init__.py   # 5.1 pkg const defines BP
        |-errors.py     # 5.2 err handlers
        |-forms.py      # 5.3 form classes
        |-views.py      # 5.4 routes functions
      |-__init__.py   # 2.1 app pkg constructor, factory
      |-models.py     # 2.2 db models
      |-email.py      # 2.3 email 
    |-config.py     # 3 configuration variables as OOPs
    |-flasky.py     # 4 factory is invoked
  ```

- 3 - `config.py` config as OOPs
  - the config variables like secret-key and mail-server, are now attributes of `Config` class.
  - `Config` class has `@staticmethod` as `init_app(app)` which can be used to initialize app and more.
  - This Config base class has common vars but can be extended to build different environment classes like dev, test, prod. that can have env specific vars like dev db-location.
  - add a dictionary `conf_env` to pick the correct env class.
- 2 - `app/` App Package
  - dir having code, template and static files.
  - 2.1 - `app/__init__.py` App Pkg Constructor
    - this is where we build the `factory function` to initalize app explicitly and controlled.
    - import Flask modules (only Flask)
    - import Flask-Extensions (only those that need app init)
    - instantiate extensions without `app`
    - factory function `def create_app(conf_env):` function to have
      - arg `conf_env` is dictionary key name (str) to pick required Env_Config_Class from `config.py` so that we have correct config vars.
      - instantiate app
      - add configs from object `app.config.from_object()`
      - add configs to extensions using `ext_obj.init_app(app)`
      - return app
    - while this makes config available in controlled way, however, it missing `@app.routes()` and other decorators associated to `@app` like error handles. This is handled using `Blueprint`.
    - import BP file and register it with app using `register_blueprint()` method.

      ```python
      from flask import Flask
      from flask_bootstrap import Bootstrap
      from flask_sqlalchemy import SQLAlchemy
      from config import config

      bootstrap = Bootstrap()
      db = SQLAlchemy()

      def create_app(config_name):
          app = Flask(__name__)
          app.config.from_object(config[config_name])
          config[config_name].init_app(app)

          bootstrap.init_app(app)
          db.init_app(app)

          # Routes or blueprints
          from .main import main as main_blueprint
          app.register_blueprint(main_blueprint)

          return app
      ```

- 5 Blueprint - sub pkg
  - Blueprint is like app having routes but in dormant state until registered with an application which gives it a context.
  - Blueprint can be a single file, or structured as a sub-package having multiple modules and the package constructor creates blueprint.
  - 5.1 `app/main/__init__.py` main bp creation
    - Blueprint is native flask module
    - create object of `Blueprint()` class and pass it a name and location.
    - import associated modules

      ```python
      from flask import Blueprint
      main = Blueprint('main', __name__)

      from . import views, errors 
      # last line to avoid circular dependency
      ```

  - 5.4 `app/main/views.py` view routes
    - route function name now has namespace with BP name as prefix, so `url_for('main.index')` should be used so that 'index' of any other BP is not picked.
  - 5.2 `app/main/errors.py` error handlers  
    - they respond to only BP route error, for app wide use `app_errorhandler` decorator instead of `errorhandler`.
  - 5.3 `app/main/forms.py` has form objects.

- 4 `flasky.py` module where app instance is deined
  - `create_app()` function is called.

More on Blueprints

- Blueprint lets us divide app into mini apps. It is a collection of views, templates, static files that can be applied to an application. Blueprints are a great way to organize your application.
- you can have a sub-folder for mini app, having its own static and templates folder, just app `__init__` to the sub-folder and import it to main app
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



## Flask Testing

- Testing can be done using flask shell and executing functions `flask --app flasky.py shell`
- What you test in shell should be automated by making test cases.
- Unit Tests - test small units
  - use py native `import unittest`
  - in `tests/test_basics.py`
    - import modules you need for test, `create_app`, `db`
    - import modules you need to test, `User`, `current_app`
    - define class `class BasicsTestCase(unittest.TestCase):`
      - build functions
        - `setUp()` runs before each test, builds env for testing
        - `tearDown()` runs after each test, removes things from env
        - `test_somecase()` these functions run as test.
          - `assertTrue` Ok if True
          - `assertFalse` Ok if False
          - `with self.assertRaises(AttributeError):` statement that raise error.
  - tests can be written in separate py files (modules) and the folder `tests` can have `__init__.py` as blank to make it a pkg
  - in `flasky.py` you can add code to run tests automatically by adding a cli command.
  - do `flask --app flasky.py test` to run all test cases

  ```python
  @app.cli.command()
  def test():
    """Run the unit tests.""" # help msg on cli
    import unittest
    tests = unittest.TestLoader().discover('tests')
    unittest.TextTestRunner(verbosity=2).run(tests)
  ```


## Logging in Flask

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

## Access localhost flask app on Network

- Suppose on an Ubuntu VM a flask app is running on localhost and you want to access it from you host machine that is Mac.

- Run flask app with `app.run(host='0.0.0.0', debug=True)`
- This tells your operating system to listen on all public IPs.
- then access `192.168.10.33:5000` from host machine.

Now that our app is running we can add a database to this app. We will use FlaskSQLAlchemy package for this. Or Pandas.


## Machine Learning Pandas App in Flask

You can use Flask it with Pandas, Matplot and other ML libraries to make it easily usable for end users.

- Import all your libs in flask app that you have used in Jupyter NB.
- Add code and functions to read data and perform tasks.
- Flask routes are executed for each request, so keep data reads outside to read them once.
- `return render_template( 'search.html', data=df_result.to_html(classes='table table-striped table-hover')` - to_html makes html table that can be passed to html page.
- `{{ data|safe }}` - safe makes it as markup and browser renders it.

Reference:

- <https://sarahleejane.github.io/learning/python/2015/08/09/simple-tables-in-webapps-using-flask-and-pandas-with-python.html>


## RestAPI in Flask

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
- Note, JSON object is defined in python as dict, `jonify` converts and send as JSON Object.

## Serving over HTTPS

- generate certificate key using `openssl req -x509 -newkey rsa:4096 -nodes -out cert.pem -keyout key.pem -days 365`
- allow insecure connection to localhost in chrome, paste `chrome://flags/#allow-insecure-localhost`
- <https://blog.miguelgrinberg.com/post/running-your-flask-application-over-https>

## JWT Authentication in Flask

- JWT Authentication
  - `jwt` python library is used to make a `token` that can be send in every request instead of sending username and password.
  - Token is encoded string which has a valid time and it expires after that time.
  - `ExpiredSignatureError` is raised if you `decode` and expired token string.
  - [ ] how to add remember me.

- how to register?
  - comment `token_authentication` for `create_user`
  - `curl -i -X POST -H "Content-Type: application/json" -d '{"username":"admin","password":"admin"}' http://127.0.0.1:5000/admin`

- CURL Requests
  - Send Username and password to get token
    - `curl -u username:password -i -X GET http://127.0.0.1:5000/login` returns token and duration
  - Send token in header to access protected resources
    - `curl -H "x-access-token: token" -i -X GET http://127.0.0.1:5000/users`
    - `curl -H "x-access-token: token" -i -X GET http://127.0.0.1:5000/users/9d8c738b-3a39-482d-8a17-0c1b755f9a23`
    - `curl -H "x-access-token: token" -i -X GET http://127.0.0.1:5000/api/v1.0/tasks`
    - `curl -H "x-access-token: token" -i -X GET http://127.0.0.1:5000/api/v1.0/tasks/19`
  - [ ] will this be more secure and beneficial?
    - `curl -u username_or_token:password_or_unused -i -X GET http://127.0.0.1:5000/users`

## App - RESTful API in Flask

to be added

## App - Social Blogging App in Flask

- User Authentication
  - Password hashing
    - extensions - `from werkzeug.security import generate_password_hash, check_password_hash` this is tried and tested lib that is safe to use.
    - model - implement `password` as write-only property of `User` class to set `password_hash`
  - Blueprint - structure it as sub-module `auth` Blueprint. It has view having login-route
  - Flask-login is ext having functions and decorators that make authentication easy.
    - model - few required class members can either be declared in `User` class or can be importe from `UserMixin` class of Flask-login.
    - initialize and instantiate extension with required conf

      ```python
      from flask_login import LoginManager
      
      login_manager = LoginManager()
      login_manager.login_view = 'auth.login'
      
      def create_app(config_name):
        # ...
        login_manager.init_app(app)
        # ...
      ```

    - model implement user_loader in `User` class

      ```python
      from . import login_manager

      @login_manager.user_loader
      def load_user(user_id):
        return User.query.get(int(user_id))
      ```

    - `login_required` decorator lets protect route.
    - Flask-Login’s `login_user()` logs user in once verified. It setts user session.
    - `logout_user()` logs user out.
  - Register User
    - build a form class in new `auth/forms.py`, add unique email and username validator using `validate_` function
    - build a template that uses form `templates/auth/register.html`
    - build a register route in `auth/views.py`
      - get - render form
      - post - validate and add user to db
  - account confirmations
    - use expiry token to validate email url.
    - model - add token generation and validation function.
    - view - send email on registration
    - view - `@auth.route('/confirm/<token>')`
- Roles and Permissions
  - database implementation
    - add `role` table
    - add `permission` column to role table as integer
      - multiple permission can be binary numbers, 1,2,4,8,16
      - add them and subtract them to get unique number as total permission of user. 2+4=6
      - do bit wise and operation to match permission. 6&2=2, 6&4=4
  - add decorator function to make it easy to protect route to access only if permission is checked.
- User Profiles

## App - Mega Tutorial by MG

_This is understanding of a tutorial by Miguel Grinberg, we are learning to create a micro-blogging site using flask and other dependencies._

- request
  - `from flask import Flask, render_template, request, url_for, flash, redirect` libraries
    - render_template - to show html page.
    - request - to get get/post data
    - url_for - to get path for resource
    - flash - to store messages to session var, handy for form error messages.
      - add session info, after `app = Flask(__name__)` add `app.secret_key = 'super secret key'`
  - `before_request` is a flask native function that gets executed before any request is made. It can be used to update last_seen timestamp.

- Flask Debugging
  - `export FLASK_DEBUG=0` makes the environment PROD else it is DEV or TEST.
  - `export FLASK_APP=microblog.py` will make an variable that tells python which app to run.
  - `flask run` executes the app or if flask is not in path then do `> python -m flask run`

- Flask DB Migrate commands
  - We can use to create table and manage changes

  - `flask db init` creates migration folder
  - `flask db migrate` creates scripts for table creation
  - `flask db upgrade` executes scripts to make table changes

### Start a python email server

`(venv) $ python -m smtpd -n -c DebuggingServer localhost:8025` this command starts emulated email server.

Some variables that we might need to export:

```bash
export MAIL_SERVER=smtp.googlemail.com
export MAIL_PORT=587
export MAIL_USE_TLS=1
export MAIL_USERNAME=email_id@domain.com
export MAIL_PASSWORD="password"
```

### Making new forms in app

We need to define following:

- **(M)** Form Class - in module `forms.py` make class with fields and validate functions.
- **(V)** HTML template - in `templates` folder, rendered from route with form passed as data. Display form elements, errors and hidden_tag.
- **(C)** Route - in module `routes.py`, define new route, import form class and use. On submit, create object of model class to save/query data.
- Create a link to access new route.

- Saving form data and tables
  - **ORM** can be created in `modules.py`. We can define classes for our tables. Classes also define relationships. Have functions to query data.

  - use them in routes to save and query data.
  - uses db service to handle database operations.

### Routes.py

- imports forms classes from `forms.py` for:
    1. getting form data via POST
    2. passing form to html template for making form components and displaying errors

- import orm model classes from `models.py` for:
    1. making object of DB Table model
    2. submit and commit new data
    3. query model class to fetch data

### app \_\_init\_\_.py

This defines app folder as a python package.

- It is where we link all modules (.py files in folder app) together as a Flask app.
- We import routes, models and error modules here.

**Services:** Here we have various services defined. We use them across app like db, login, mail.

### Blueprints

We can separate out application logical modules in application. Like we can create separate package for auth, error handling etc.

`from flask import Blueprint` is what we need to use.

To register a blueprint, the `register_blueprint()` method of the Flask application instance is used. When a blueprint is registered, any view functions, templates, static files, error handlers, etc. are connected to the application.

In each Blueprint package we make \_\_initII.py that has name information and imports routes. Routes further imports and links model and controller.

### Open doubts

- UserMixin?
- Why we pass Classes as param to Class?

### Making modules

- to get url_for use `module.route`. For eg: main.index.
- But to render template use only `index.html` without module name.

### Elastic Search

- You can install elastic search by `brew install elasticsearch` on mac.
- Access `http://localhost:9200` to view service JSON output.
- Also, install in python `pip install elasticsearch`
- To have launched start elasticsearch now and restart at login: `brew services start elasticsearch`
- Or, if you don't want/need a background service you can just run: `elasticsearch`

Todo: Search not working, fix it.


## Links

- [ ] - Flaskr - Blogging Template - <https://flask.palletsprojects.com/en/0.12.x/tutorial/introduction/>

- Generate SQLAlchemy class model from database table - `sqlacodegen mssql+pyodbc://GBRDSM050000617\GMI_SQLVIRT_TEST/TC09_Dev_Db/historic_responses?driver=SQL+Server --outfile db.py`

