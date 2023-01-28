---
layout: post
description: Python Conda PIPs Venv RegEx R
date: 2021-05-05
---

# Python

*all learnings here, keep one file for easier updates*

## Python Interpreters

- there may be many python version and interpreter installed on your machine. eg, `/bin/python3` or `~/anaconda/bin/python`
- **identify current** the default, `which python`
- **find all** installed binaries, `locate "bin/python"` - this lists all the binaries including venvs.
- to **set** anaconda python as default python, add following to `~/.bashrc`
  - `export PATH="/home/username/anaconda3/bin:$PATH"`
  - then restart terminal or do `source ~/.bashrc`
  - check with `which python`, should be using anaconda.

## Virtual Environments

- **Why** use virtual environment - It gives different apps isolation and personal environment so that modules don't interfere and it is easy when we have to productionize the app.

- **What** is Python Virtual Environment  
  - Basically a folder having python core binaries and use this new installation to install libraries that will be specific to this installation (or environment).
  - It is an isolated environment
  - When you activate a virtual environment, your PATH variable is changed. The Scripts directory of `venv_app` is put in front of everything else, effectively overriding all the system-wide Python software.

- **Create**
  - `python -m venv venv_app` - creates folder venv_app
  
- **Usage**
  - `source venv_app/bin/activate` - activates this environment
  - `>venv_app\Scripts\activate` in Windows
  - check using `python -V` or `which python`
  - now do `python -m pip install <package-name>` - this installs the package only in this environment.
  - add `#!venv_app/bin/python` on top of main.py file to make it use this python.
  - python or python3 depends on your installation.

- **Deactivate**
  - `deactivate` to deactivate the current env.

- **View & Share**
  - `python -m pip list` to see installed libs.
  - `python3 -m pip freeze` to share libs.
    - `python3 -m pip freeze > requiremeents.txt` to make a file.
    - `python3 -m pip install -r requirements.txt` to install all libs from file

- **Delete**
  - `rm -r venv_app` delete

## Conda Miniconda Anaconda


**Conda** is package and virtual environment manager, like pip, for any language—Python, R, Ruby and more. It is a CLI and can

- install packages like flask, jupyter, pandas etc.
- can manage envs, virtual environment is separate python and its packages. This means each project you work on can have its own set of packages.

**Anaconda** is toolkit for Data Science. Along with conda it includes ds and ml libraries (500Mb) installed.

**Anaconda Navigator** is GUI to use conda.

**Miniconda** includes conda and python but not much libraries.

So we can use conda alone to create a development virtual environment for new data science project or use miniconda.

**Installing Conda** on Linux

- `wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh`
- `bash Miniconda3-latest-Linux-x86_64.sh` this installs python, conda, vevn and others, all in virtual environment.
- After installation, restart terminal, and it will load new environmnet called 'base'. Now python is not the default system python.
  - If you'd prefer that conda's base environment not be activated on startup, set the auto_activate_base parameter to false: `conda config --set auto_activate_base false`
  - more: <https://www.projectdatascience.com/step-by-step-guide-to-setting-up-a-professional-data-science-environment-on-a-linux/>

**Quick Start using Conda** for a new project

```sh
conda deactivate
mkdir prj1
cd prj1
conda create -n prj1env pandas numpy jupyter scikit-learn matplotlib seaborn
conda activate prj1env
touch README.md
code .
```

Run `conda activate prj1env` in code shell.

Undo `conda deactivate && conda remove --name prj1env --all` and remove files if any.

**Conda Basic Commands**

- `conda update conda` updates conda
- `conda install PACKAGENAME` installs pkg to default/active env
- `conda update PACKAGENAME` updated pkg
- `pip install pkg` aslo intalls to active env

**Conda Env Commands**

- `conda create --name py35 python=3.5` created new env called 'py35' and installs py 3.5
- `conda activate py35` activates
- `conda list` lists all packages installed in active env.
- `conda remove --name my-env --all` deletes the env.


## Python Programming

`Decorators` are a standard feature of the Python language. A com‐
mon use of decorators is to register functions as handler functions
to be invoked when certain events occur.

[ ] what is context in python

`Package` is usually a folder with `__init__.py` in it. Other python files are `modules`.

- Public, Private, Protected in python
  - public - every member of class in python is public by defaut, can be accessed outside class using object.
  - protected attribute needs to be prefixed with underscore, `_name`. It can be accessed, just a convension.
  - private members can be `__name` prefixed with double underscore, this makes them non accessible outside "directly". though can be accessed using `_Classname__attrname`
  - Python provides conceptual implementation but not exactly like java or C++. As the underscores tell what is protected and private but does not make them non accessible.
  - To implement a "write-only" attribute use "property"
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




## Files in python

`f  = open(filename, mode)` Where the following mode is supported:

- r: open an existing file for a read operation.
- w: open an existing file for a write operation. If the file already contains some data then it will be overridden but if the file is not present then it creates the file as well.
- a:  open an existing file for append operation. It won’t override existing data. creates if not exists.
- r+:  To read and write data into the file. The previous data in the file will be overridden.
- w+: To write and read data. It will override existing data.
- a+: To append and read data from the file. It won’t override existing data.

```python
## read
file_handle = open('notes/abc.txt', 'r')
content = file_handle.read()
file_handle.close()

## write
file = open('note.txt','a')
file.write("quick brown")
file.write("munde, not fox")
file.close()
```

- non recursive replace`[os.rename(f, f.replace('_', '-')) for f in os.listdir('.') if not f.startswith('.')]`

```python title="Replace '_' with '-' in filenames recursively"
directory = '.'
for subdir, dirs, files in os.walk(directory):
  for filename in files: # !! use dirs instead of files to rename dirs first
    new_filename = filename.replace('_','-')
    subdirectory_path = os.path.relpath(subdir, directory) #get the path to your subdirectory
    file_path = os.path.join(subdirectory_path, filename) #get the path to your file
    new_file_path = os.path.join(subdirectory_path, new_filename) #create the new name
    # os.rename(file_path, new_file_path) #rename your file
    print(file_path, new_file_path) #rename your file
```



## Logging in Python

```python

#----------set up logging configuration
log_time=time.strftime('%Y%m%d_%H%M', localtime())
logfilepath=_enviroment_variable.log_path
log_filename=str(logfilepath)+str(log_time)+'_app_'+str(parent_process_id)+'_'+str(process_id)+'.log'
logging.basicConfig(level=logging.DEBUG,
format='%(asctime)s - %(levelname)s - %(message)s',
datefmt='%Y/%m/%d %H:%M:%S %p',
filename=log_filename,
filemode='w')

logging.info("New Working directory is: " + str(os.getcwd()))

## Other Way
import logging
logger = logging.getLogger(__name__)
logger.info('df.shape post cleanup: ' + str(result.shape))


## ---------------------------------------------------------------------

## Other PDF pipeline
import logging
from datetime import datetime
my_logger = logging.getLogger(cg.sql_query_prefix + " Log")
my_logger.setLevel(logging.INFO)
my_logger_filehandler = logging.FileHandler(cg.data_path + 'logs/' + cg.sql_query_prefix + "_" + datetime.today().strftime('%d-%m-%Y') +  '.log')
my_logger_filehandler_format = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
my_logger_filehandler.setFormatter(my_logger_filehandler_format)
my_logger.addHandler(my_logger_filehandler)
my_logger.info('New Run of Prospect Intelligence')

## ---------------------------------------------------------------------

import logging
logger = logging.getLogger()
logger.setLevel(logging.DEBUG)
logging.debug("test")

## ---------------------------------------------------------------------

import logging
logger = logging.getLogger()
fhandler = logging.FileHandler(filename='mylog.log', mode='a')
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
fhandler.setFormatter(formatter)
logger.addHandler(fhandler)
logger.setLevel(logging.DEBUG)

logging.error('hello!')
logging.debug('This is a debug message')
logging.info('this is an info message')
logging.warning('tbllalfhldfhd, warning.')

## 2015-01-28 09:49:25,026 - root - ERROR - hello!
## 2015-01-28 09:49:25,028 - root - DEBUG - This is a debug message
## 2015-01-28 09:49:25,029 - root - INFO - this is an info message
## 2015-01-28 09:49:25,032 - root - WARNING - tbllalfhldfhd, warning.
```

Links

- [ ] <https://realpython.com/python-logging/>


## Datetime and Time in Python

- `datetime` and `time` are separate modules.
- `datetime` has most methods to work with date and time, some sub modules are
  - `datetime.datetime` has both date and time handling methods, mostly used, `from datetime import datetime as dt`
    - `dt.now()` => datetime.datetime(2022, 10, 20, 8, 14, 44, 277307)
  - `datetime.date` date specific methods
  - `datetime.time` time specific methods
- `time` module has `sleep()` function to pause program execution

```python
import time

time.time() # timestamp
time.sleep(2) # sleeps for two seconds
```

```python
from datetime import datetime as dt
start = dt.now() # datetime.datetime(2022, 10, 20, 8, 14, 44, 277307)
dt.now().ctime() # Thu Oct 20 08:16:51 2022
end = dt.now()
end - start # datetime.timedelta(seconds=11, microseconds=129035)
delta = (end - start).seconds # 11
```

## Testing in Python

Manual testing is done to check if all functionalities are performed as expected.

Now if you change the code you have to check all the functionalities again. To avoid this, you can do Automated Testing.

There is `test step` and `test assertion`. Eg, step is that button will turn on light, to see that the light is on is assertion.

An integration test checks that components in your application operate with each other. Eg, switch turns on light.

A unit test checks a small component in your application. Eg, switch, battery, wire, bulb etc.

You can check a output against a `known output`

`assert` keywork is used to check a logical statement. In case of False, error is raised.

```py
def test_sum():
    assert sum([1, 2, 3]) == 6, "Should be 6"
```

`AssertionError` is raised in case of failure, else nothing.

`unittest` is a test runner, based on JUnit from java. It requires that:

- You put your tests into classes as methods
- :TODO <https://realpython.com/python-testing/>


## Snippets Python

Taking input - `msg = str(input("Message? : "))`


## Web Scraping - Selenium

Install web driver

- visit `https://chromedriver.chromium.org/downloads` and download version same as your browser version.
- unzip and move `chromedriver` to `/usr/local/bin/chromedriver`

More - <https://realpython.com/modern-web-automation-with-python-and-selenium/>



## Data Science Setup

Python installed in Ubuntu or Mac should not be used. Instead create a Virtual Environment for it.

**Virtual Environments** can be created using conda or venv module. Each virtual environment has its own Python binary and can have its own independent set of installed Python packages in its site directories. More [here](https://docs.python.org/3/library/venv.html).

**Quick Start on Linux** more [here](https://cloud.google.com/python/docs/setup).

```sh
sudo apt update
sudo apt install python3 python3-dev python3-venv

sudo apt-get install wget
wget https://bootstrap.pypa.io/get-pip.py
sudo python3 get-pip.py

pip --version

cd your-project
python3 -m venv env

source env/bin/activate
```

This will create a dir `env` and will have its own python, python3, pip and pip3. Now you can install any packages and this will not interfere with system.

**Install Jupyter** in the venv. Now that we have an environment (base) you can use it, or create a new. Then

- `pip install jupyter`
- `which jupyter` shows `/home/vaibhav/code/miniconda3/bin/jupyter` it does not effect the system python.
- it is pkg, same as flask
- `jupyter notebook` runs a server to server jupyter notebooks at <http://localhost:8888/tree>


## Animation and Modelling in Python

VPython GlowScript

- can be used to create objects and animate them
- VPython makes it unusually easy to write programs that generate navigable real-time 3D animations.
- <https://www.glowscript.org/docs/VPythonDocs/videos.html>


Manim

- can animate equations and plots
- <https://github.com/3b1b/manim>
- <https://www.youtube.com/watch?v=ENMyFGmq5OA>

Sage

- Allows equation animations and plotting
- <https://www.sagemath.org/download-mac.html>

Povray

- The Persistence of Vision Raytracer is a high-quality, Free Software tool for creating stunning three-dimensional graphics. The source code is available for those wanting to do their own ports.
- <http://www.povray.org/>

ImageMagick

- Create, edit, compose, or convert digital images.
- It can resize, flip, mirror, rotate, distort, shear and transform images, adjust image colors, apply various special effects, or draw text, lines, polygons, ellipses and Bézier curves.

EdX

- <https://learning.edx.org/course/course-v1:CornellX+ENGR2000X+1T2017/home>



## Other iYV Python Notes

- [Python Coding Kaggle](https://www.kaggle.com/iyadavvaibhav/python-notes)
- [Pandas Kaggle](https://www.kaggle.com/iyadavvaibhav/pandas-notes)
- [Flask](flask.md) - back end web framework micro
