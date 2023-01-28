---
description: Notepad Public
---

# Notepad

*Staging area - Start with H1, later move to `term-notes.md`*

## Android Notes

ADB is utility to interact with android phone. It can install/uninstall apks. change connections etc.
 All commands here, [adb shell](https://adbshell.com/).

Enable Developer Options > USB Debugging

adb must be installed on your mac/pc.

Uninstall blotwares

- `adb devices` see your device
- `adb shell` enter phone shell
- `pm uninstall -k --user 0 com.mipay.wallet.in` to use pm is pkg mgr, and uninstall an app.

References:

- <https://forum.xda-developers.com/t/uninstall-system-apps-without-root-vivo-bloatware.3817230/>
- <https://technastic.com/vivo-bloatware-preinstalled-apps-list/>



## Easy Soft Sys

Color Pallet:

- Blue - #00a1e7, rgb(0,161,231) <https://www.colorhexa.com/00a1e7>
- Grey - #3f3f3f, rgb()
- Orange - #e74600, rgb(231,70,0)

Font: Gill Sans Nova Extra Condensed Bold

## Bookdown

Quick [getting started](http://seankross.com/2016/11/17/How-to-Start-a-Bookdown-Book.html).

Steps:

- `mkdir bookdown`
- `cd bookdown/`
- `git clone https://github.com/seankross/bookdown-start`
- `cd bookdown-start/`
- `r`
- `bookdown::render_book("index.Rmd")`

- all `# heading 1` are chapters.
- Add `Part I` before a chapter to make it part in a book, `# (PART) Data Science {-}`
- `> options(bookdown.render.file_scope = FALSE);` to use parts in diff directories.

To support GitHub flavoured MarkDowm, you need to add the following line to `_output.yml` file:

`md_extensions: +lists_without_preceding_blankline+pipe_tables+raw_html+emoji`

**Working on a book:**

- All mds are in `./data_science` folder.
- All images are in `./images` folder.
- Add new md file to `./_bookdown.yml` file. It also has index order.
- To build and run:
  - `r`
  - `bookdown::render_book("index.Rmd")`
  - new site availabe at `./docs/index.html`
  - `quit()` to exit R shell


References:

- Bookdown [cookbook](https://bookdown.org/yihui/rmarkdown-cookbook/rmarkdown-anatomy.html)
- [Bookdown](https://bookdown.org/yihui/bookdown/html.html)
- Rafalab [dsbook](https://rafalab.github.io/dsbook/introduction-to-productivity-tools.html)
- Rafalab book [source github](https://github.com/rafalab/dsbook/blob/master/_bookdown.yml).
- Bookdown data science notes [book](https://bookdown.org/mpfoley1973/data-sci/)
- Python Visualizations in [bookdown](https://bookdown.org/jamie/python_visualisation/)
- Using [Python Environments](https://bookdown.org/yihui/rmarkdown/language-engines.html)
- Show plotly html js in Rmarkdown [stackoverflow](https://stackoverflow.com/questions/50191208/display-python-plotly-graph-in-rmarkdown-html-document)
- code options [cheat sheet](https://rstudio.com/wp-content/uploads/2015/02/rmarkdown-cheatsheet.pdf)
- publishing on [github](https://bookdown.org/yihui/bookdown/github.html)
- pandoc markdown [formats](https://pandoc.org/MANUAL.html).


# Digital Marketing Notes

Instagram page earning:

- original images
- regular posting

Instagram Bot:

- Scrapper - <https://towardsdatascience.com/increase-your-instagram-followers-with-a-simple-python-bot-fde048dce20d#:~:text=Open%20a%20browser%20and%20login,users%20you%20followed%20using%20the>
- Post - <https://www.youtube.com/watch?v=vnfhv1E1dU4>


# Tableau



## Writeback in Tableau

## Mega String

"( '"
+[CC interaction_ID]
+"', '"
+[CC Status]
+"', '"+[CC Note]+"', '"+USERNAME()+"' )"

## HideInsert

[CC W InsertRun] = 0

## HideReset

[CC W InsertRun] = 4

## IncrementAdd

[CC W Incrementer]+1

## zero

0

## One

1

## Blank

""

## sheet reset

Saved successfully!
Go To Flow View ⮞

## Seet Sumbit

Submit ⮟

## CC Submitted

Writeback proc source

## Actions on form

select - reset - go to - next

select - reset - set - insertrun to 0

select - submit - set - cc mega string

select - submit - set - increment to +1

select - submit - set - insetRun 1

## actions on table sheet

select - table - set - insertRun 1

select - table - set - string blank

select - table - set - id to row selected





# Plotly D3 Vizs

- poltly built on top of D3
- python api uses plotly.js


Plotly Library:

- data - result of go.chartType(x=, y=, others=....)
- layout - title, axis, annotations
  - has param `updatemenus`
  - There are four possible update methods:
    - "restyle": modify data or data attributes
    - "relayout": modify layout attributes
    - "update": modify data and layout attributes
    - "animate": start or pause an animation
- frames: used for animations
  - we can add different frames to a chart
  - this can be used to produce the animations
  - Example can be found [here](https://plotly.com/python/animations/#frames).
- figure - final object combining data and layout.

Dash is putting and linking many plotly charts together.

## Other plotly products

Dash:

- Dash is Python framework for building *analytical web applications*.
- It is built on Flask, Plotly.js and React.js
- Just like flask, we define `app = dash.Dash()` and then at end `app.run_server()`
- We can create complete site with links.
- It has intractable story.

Chart Studio

- is like Tableau web edit and public.
- Can create and host data, charts and dashboards.
- can explore other people's work.
- charts are interactable and linked together.
- can be reverse engineered.
- can host notebooks as well.

ObservableHQ:

- Live, web edit, d3 notebooks.
- markdown and JS blocks
- lots of d3 features. like counts, action buttons etc
- can make dasboard as well.


References:

- [How and why I used Plotly (instead of D3)](https://www.freecodecamp.org/news/how-and-why-i-used-plotly-instead-of-d3-to-visualize-my-lollapalooza-data-d48345e2ca68/)
- [4 interactive Sankey diagrams made in Python](https://medium.com/plotly/4-interactive-sankey-diagram-made-in-python-3057b9ee8616)

# D3

Add D3 library. Then specific module.

- it is collection of module that work together
- data is bounded to the selections, it join-by-index
- By default, the data join happens by index: the first element is bound to the first datum, and so on. Thus, either the enter or exit selection will be empty, or both. If there are more data than elements, the extra data are in the enter selection. And if there are fewer data than elements, the extra elements are in the exit selection.
- selectAll() data() enter() append() - to add elements, SDEA.
<https://observablehq.com/@d3/d3-hierarchy?collection=@d3/d3-hierarchy>



# YouTube Channel Notes

Start creating a web of terms , make understand each thing, chamkao cheezo ko.
makeit understnad to 6yr old guy
start from docs, make reading a habit, start taking notes.
math teacher lessongs, i see, i do, i ...
small age learn, big understand, then decision.

Follow:

- miguel grinberg - <https://twitter.com/miguelgrinberg>
- Claudio Bernasconi - <https://twitter.com/CHBernasconiC>

# Mac OS (Linux) Ubuntu Notes

- `alias vynote="subl ~/path/to/file/notepad.txt"` add to `.bash_profile` to make shortcut
- cheat book - <https://github.com/0nn0/terminal-mac-cheatsheet#english-version>


# Python web app development

# Notes - 20230126

## JWT DB App

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



## Flask

All Flask applications must create an `application instance` (object of class Flask). The web server passes all requests it receives from clients to this object for handling, using a protocol called Web Server Gateway Interface (WSGI).

`app = Flask(__name__)` name is passed to Flask class constructor so it knows the location of app and hence can locate static and template.

Web Browser --request--> web server --> Flask app instance --route--> function to execute

`View Functions` handle application URL.

**Request** from client has lot of information in it, like header, user-agent, data etc. This information is available in `request object` and is made available to a `view-route function` to handle it. This object is not passed as an argument to function, rather it is made available using `contexts`. **Contexts** let certain objects to globally accessible, but are not global variable. They are globally accessible to only one thread. There can be multiple threads serving multiple requests from multiple client.

- Context is simply data that is specific to something. Eg
  - App-context is specific to app, like its mail server, its database location, or other configurations
  - Request-context is specific to request, like its browser, its client, its form data, its headers
- this data is stored in object, in attribute such as `config`
- this data is used by extensions in flask, hence they do not run if context is not available.
- context is automatically made available once app is initialized.
- context can be made explicitly avilable by calling `with app.app_context():`
- when there is request, web server activates a thread that initializes app and this app context is pushed with data that is available globally, similarly request context is alos pushed.

There are two contexts in Flask

- `current_app` variable in Application context, has info of active application.
- `g` variable in Application context, temp access **during** handling of a request. It is reset once request is served. Holds app info hence app context.
- `request`, in request context, obj having client req data.
- `session`, in request context, a dictionary to store values that can be accessed in different requests from same session.

Flask, in backend, makes these availabe to thread before dispaching a request and removes after request is handled. Explicitly, `current_app` can be made availabe by invoking `app.app_context()`

![img](http://speakerdeck.com/patkennedy79/demystifying-flasks-application-and-request-contexts-with-pytest?slide=8)

[ ] How does flask differente requests and clients?

URL-maps can be seen using `app.url_map`

`/static/<filename>` is special route added by Flask to serve static files.

`Requst Object` has methods and attributes having info on method, form, args, cookies, files, remote_addr, get_data().

**Request Hooks** are functions that can execute code before or after each request is processed. They are implemented as decorators  (functions that execute on event). These are the four hooks supported by Flask:

- `before_request` - like authenticate
- `before_first_request` - only before the first request is handled. Eg, to add server initialization tasks.
- `after_request` - after each request, but only if no unhandled exceptions occurred.
- `teardown_request` - after each request, even if unhandled exceptions occurred.

`g` context global storage can be used to share data between hook and view functons.

**Response Object** - Response is returned by view-funciton as a string (usually HTML) along with `status code` but can alos contain headers. So rather than sending comma separated tuple values, flask lets create response onject using `make_response()`.

```python
from flask import make_response

@app.route('/')
def index():
    response = make_response('<h1>Some response with a cookie!</h1>')
    response.set_cookie('message', '51')
return response
```

`return redirect('http://www.example.com')` is a response with URL and status code 302, however Flask lets it do easily using `redirect()` method. Another such is `abort(404)` which is treted as exception.

**Templates** `return render_template('user.html', name=name)` is another return helper function that can make use of template, which is HTML code where dynamic content can be filled on execution.

{{ super() }}, includes code from parent block, uif overriding a block.

`@app.errorhandler` is decorator that lets return a view from template for error responses like 404 and 500.

```python
@app.errorhandler(404)
def page_not_found(e):
    return render_template('404.html'), 404

@app.errorhandler(500)
def internal_server_error(e):
    return render_template('500.html'), 500
```

- `url_for()` utility builds URL for view-function giving route from app-url-map. Eg:
  - `url_for('user', name='john', page=2, version=1)` would return `/user/john?page=2&version=1`, they are good to build dynamic URLs that can be used in templates.
  - `url_for('user', name='john', _external=True)` would return `http://localhost:5000/user/john`.
  - `url_for('static', filename='css/styles.css', _external=True)` would return `http://localhost:5000/static/css/styles.css`.

## Flask Extensions

Most extensions use app context to initialize themselve, eg:

```python
from flask_bootstrap import Bootstrap
app = Flask(__name__)
bootstrap = Bootstrap(app)
```

- **Bootstrap** - provides templates and blocks that can be used in Jinja2 Templates
  - installation `pip install flask-bootstrap`
  - eg, `{% extends "bootstrap/base.html" %}` - base.html does not exist but is available via extension. Others are `navbar`, `content`, `script`

  ```html
  {% block scripts %}
  {{ super() }} <!--includes base scripts too, else overrides-->
  <script type="text/javascript" src="my-script.js"></script>
  {% endblock %}
  ```

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

### Forms

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
  - Includes CSRF, file upload, and reCAPTCHA. You mostly have to use formats of WTForms but write less as few things are done automatically that are realted to Flask patter.
  - Form fields are Class variables with different field type
  - validator functions can help validate, liek `Email()`.
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

### Databases

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
      - `backref` adds a backreference to other models
      - `lazy` does not execute until you add executor.
        - add `lazy='dynamic'` to prevent query execution.
    - `db.create_all()` creates all tables if they dont exist

  - **Create** a new row
    - Create an Object of Class to build a new row. `user_john = User(username='john', role=admin_role)`
    - add - `db.session.add(user_john)`
    - `id` is added to object after `db.session.commit()commit()`
  - **Update** - `db.session.add()` also edits.
  - **Delete** - `db.session.delete(obj_name)`
  - **Read** - `query` object is avilable to all model class. It has filter-options and executors that build a SQL Query statement.
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

### Emails

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

## Blueprint - Large App Structure

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



## Social Blogging App

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

## GIT

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

- `git switch -c <new-branch-name>`

## Python

`Decorators` are a standard feature of the Python language. A com‐
mon use of decorators is to register functions as handler functions
to be invoked when certain events occur.

`Package` is usually a folder with `__init__.py` in it. Other python files are `modules`.

- Public, Private, Protected in python
  - public - every member of class in python is public by defaut, can be accessed outside class using object.
  - protected attribute needs to be prefixed with underscore, `_name`. It can be accessed, just a convension.
  - private members can be `__name` prefixed with double underscore, this makes them non accessible outside "directly". though can be accessed using `_Classname__attrname`
  - Python provides conceptual implementation but not exactly like java or C++. As the underscores tell what is protected and private but does not make them non accessible.
  - To implement a "write-only" attribute use "propertly"
    - getter - use `@property` decorator with function, `def pvt_prop_name(self):` raise err in this func so no one can get this property.
    - setter - use `@pvt_prop_name.setter` with function `def pvt_prop_name(self, value):` to implement setting some values. You can set value to another public property. Thus this makes `pvt_prop_name` as write-only.

  ```python
  class Book():
      scrambled = None # public, can be get or set
      __safe = None    # pvt, can't be directly get or set

      @property
      def secret(self):
          raise AttributeError()
      
      @secret.setter
      def secret(self,value):
          self.scrambled = value[::-1]

      # `secret` is write_only member
  ```


[ ] what is context in python

## Computer Science

A thread is the smallest sequence of instructions that can be man‐
aged independently. It is common for a process to have multiple
active threads, sometimes sharing resources such as memory or file
handles. Multithreaded web servers start a pool of threads and
select a thread from the pool to handle each incoming request.

### OOPS

- Object is a Class and has
  - `attributes` - variables
  - `methods()` - functions
