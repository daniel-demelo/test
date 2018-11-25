.. _virtualenvironments-ref:

Pipenv & Virtual Environments
=============================

.. image:: /_static/photos/35294660055_42c02b2316_k_d.jpg

This tutorial walks you through installing and using Python packages.

It will show you how to install and use the necessary tools and make strong
recommendations on best practices. Keep in mind that Python is used for a great
many different purposes, and precisely how you want to manage your dependencies
may change based on how you decide to publish your software. The guidance
presented here is most directly applicable to the development and deployment of
network services (including web applications), but is also very well suited to
managing development and testing environments for any kind of project.

.. Note:: This guide is written for Python 3, however, these instructions
    should work fine on Python 2.7—if you are still using it, for some reason.


Make sure you've got Python & pip
---------------------------------

Before you go any further, make sure you have Python and that it's available
from your command line. You can check this by simply running:

.. code-block:: bash

    $ python --version

You should get some output like ``3.6.2``. If you do not have Python, please
install the latest 3.x version from `python.org`_ or refer to the
`Installing Python`_ section of this guide.

.. Note:: If you're newcomer and you get an error like this:

    .. code-block:: python

        >>> python
        Traceback (most recent call last):
          File "<stdin>", line 1, in <module>
        NameError: name 'python' is not defined

    It's because this command is intended to be run in a *shell* (also called
    a *terminal* or *console*). See the Python for Beginners
    `getting started tutorial`_ for an introduction to using your operating
    system's shell and interacting with Python.

Additionally, you'll need to make sure you have `pip`_ available. You can
check this by running:

.. code-block:: bash

    $ pip --version

If you installed Python from source, with an installer from `python.org`_, or
via `Homebrew`_ you should already have pip. If you're on Linux and installed
using your OS package manager, you may have to `install pip <https://pip.pypa.io/en/stable/installing/>`_ separately.

.. _getting started tutorial: https://opentechschool.github.io/python-beginners/en/getting_started.html#what-is-python-exactly
.. _python.org: https://python.org
.. _pip: https://pypi.org/project/pip/
.. _Homebrew: https://brew.sh
.. _Installing Python: https://docs.python-guide.org/starting/installation/


Installing Pipenv
-----------------

`Pipenv`_ is a dependency manager for Python projects. If you're familiar
with Node.js' `npm`_ or Ruby's `bundler`_, it is similar in spirit to those
tools. While `pip`_ can install Python packages, Pipenv is recommended as
it's a higher-level tool that simplifies dependency management for common use
cases.

Use ``pip`` to install Pipenv:

.. code-block:: console

    $ pip install --user pipenv


.. Note:: This does a `user installation`_ to prevent breaking any system-wide
    packages. If ``pipenv`` isn't available in your shell after installation,
    you'll need to add the `user base`_'s binary directory to your ``PATH``.

    On Linux and macOS you can find the user base binary directory by running
    ``python -m site --user-base`` and adding ``bin`` to the end. For example,
    this will typically print ``~/.local`` (with ``~`` expanded to the
    absolute path to your home directory) so you'll need to add
    ``~/.local/bin`` to your ``PATH``. You can set your ``PATH`` permanently by
    `modifying ~/.profile`_.

    On Windows you can find the user base binary directory by running
    ``py -m site --user-site`` and replacing ``site-packages`` with
    ``Scripts``. For example, this could return
    ``C:\Users\Username\AppData\Roaming\Python36\site-packages`` so you would
    need to set your ``PATH`` to include
    ``C:\Users\Username\AppData\Roaming\Python36\Scripts``. You can set your
    user ``PATH`` permanently in the `Control Panel`_. You may need to log
    out for the ``PATH`` changes to take effect.

.. _Pipenv: https://docs.pipenv.org/
.. _npm: https://www.npmjs.com/
.. _bundler: http://bundler.io/
.. _user base: https://docs.python.org/3/library/site.html#site.USER_BASE
.. _user installation: https://pip.pypa.io/en/stable/user_guide/#user-installs
.. _modifying ~/.profile: https://stackoverflow.com/a/14638025
.. _Control Panel: https://msdn.microsoft.com/en-us/library/windows/desktop/bb776899(v=vs.85).aspx

virtualenv
----------

`virtualenv <http://pypi.python.org/pypi/virtualenv>`_ is a tool to create
isolated Python environments. virtualenv creates a folder which contains all the
necessary executables to use the packages that a Python project would need.

It can be used standalone, in place of Pipenv.

Install virtualenv via pip:

.. code-block:: console

  $ pip install virtualenv

Test your installation

.. code-block:: console

   $ virtualenv --version

Basic Usage
~~~~~~~~~~~

1. Create a virtual environment for a project:

.. code-block:: console

   $ cd my_project_folder
   $ virtualenv my_project

``virtualenv my_project`` will create a folder in the current directory which will
contain the Python executable files, and a copy of the ``pip`` library which you
can use to install other packages. The name of the virtual environment (in this
case, it was ``my_project``) can be anything; omitting the name will place the files
in the current directory instead.

This creates a copy of Python in whichever directory you ran the command in,
placing it in a folder named :file:`my_project`.

You can also use the Python interpreter of your choice (like
``python2.7``).

.. code-block:: console

   $ virtualenv -p /usr/bin/python2.7 my_project

or change the interpreter globally with an env variable in ``~/.bashrc``:

.. code-block:: console

   $ export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python2.7

2. To begin using the virtual environment, it needs to be activated:

.. code-block:: console

   $ source my_project/bin/activate

The name of the current virtual environment will now appear on the left of
the prompt (e.g. ``(my_project)Your-Computer:your_project UserName$)`` to let you know
that it's active. From now on, any package that you install using pip will be
placed in the ``my_project`` folder, isolated from the global Python installation.

Install packages as usual, for example:

.. code-block:: console

    $ pip install requests

3. If you are done working in the virtual environment for the moment, you can
   deactivate it:

.. code-block:: console

   $ deactivate

This puts you back to the system's default Python interpreter with all its
installed libraries.

To delete a virtual environment, just delete its folder. (In this case,
it would be ``rm -rf my_project``.)

After a while, though, you might end up with a lot of virtual environments
littered across your system, and its possible you'll forget their names or
where they were placed.

Other Notes
~~~~~~~~~~~

Running ``virtualenv`` with the option ``--no-site-packages`` will not
include the packages that are installed globally. This can be useful
for keeping the package list clean in case it needs to be accessed later.
[This is the default behavior for ``virtualenv`` 1.7 and later.]

In order to keep your environment consistent, it's a good idea to "freeze"
the current state of the environment packages. To do this, run

.. code-block:: console

    $ pip freeze > requirements.txt

This will create a :file:`requirements.txt` file, which contains a simple
list of all the packages in the current environment, and their respective
versions. You can see the list of installed packages without the requirements
format using "pip list". Later it will be easier for a different developer
(or you, if you need to re-create the environment) to install the same packages
using the same versions:

.. code-block:: console

    $ pip install -r requirements.txt

This can help ensure consistency across installations, across deployments,
and across developers.

Lastly, remember to exclude the virtual environment folder from source
control by adding it to the ignore list.

.. _virtualenvwrapper-ref:

virtualenvwrapper
-----------------

`virtualenvwrapper <https://virtualenvwrapper.readthedocs.io/en/latest/index.html>`_
provides a set of commands which makes working with virtual environments much
more pleasant. It also places all your virtual environments in one place.

To install (make sure **virtualenv** is already installed):

.. code-block:: console

  $ pip install virtualenvwrapper
  $ export WORKON_HOME=~/Envs
  $ source /usr/local/bin/virtualenvwrapper.sh

(`Full virtualenvwrapper install instructions <https://virtualenvwrapper.readthedocs.io/en/latest/install.html>`_.)

For Windows, you can use the `virtualenvwrapper-win <https://github.com/davidmarble/virtualenvwrapper-win/>`_.

To install (make sure **virtualenv** is already installed):

.. code-block:: console

  $ pip install virtualenvwrapper-win

In Windows, the default path for WORKON_HOME is %USERPROFILE%\Envs

Basic Usage
~~~~~~~~~~~

1. Create a virtual environment:

.. code-block:: console

   $ mkvirtualenv my_project

This creates the :file:`my_project` folder inside :file:`~/Envs`.

2. Work on a virtual environment:

.. code-block:: console

   $ workon my_project

Alternatively, you can make a project, which creates the virtual environment,
and also a project directory inside ``$PROJECT_HOME``, which is ``cd`` -ed into
when you ``workon myproject``.

.. code-block:: console

   $ mkproject myproject

**virtualenvwrapper** provides tab-completion on environment names. It really
helps when you have a lot of environments and have trouble remembering their
names.

``workon`` also deactivates whatever environment you are currently in, so you
can quickly switch between environments.

3. Deactivating is still the same:

.. code-block:: console

   $ deactivate

4. To delete:

.. code-block:: console

   $ rmvirtualenv venv

Other useful commands
~~~~~~~~~~~~~~~~~~~~~

``lsvirtualenv``
  List all of the environments.

``cdvirtualenv``
  Navigate into the directory of the currently activated virtual environment,
  so you can browse its :file:`site-packages`, for example.

``cdsitepackages``
  Like the above, but directly into :file:`site-packages` directory.

``lssitepackages``
  Shows contents of :file:`site-packages` directory.

`Full list of virtualenvwrapper commands <https://virtualenvwrapper.readthedocs.io/en/latest/command_ref.html>`_.

virtualenv-burrito
------------------

With `virtualenv-burrito <https://github.com/brainsik/virtualenv-burrito>`_, you
can have a working virtualenv + virtualenvwrapper environment in a single command.

autoenv
-------
When you ``cd`` into a directory containing a :file:`.env`, `autoenv <https://github.com/kennethreitz/autoenv>`_
automagically activates the environment.

Install it on Mac OS X using ``brew``:

.. code-block:: console

   $ brew install autoenv

And on Linux:

.. code-block:: console

   $ git clone git://github.com/kennethreitz/autoenv.git ~/.autoenv
   $ echo 'source ~/.autoenv/activate.sh' >> ~/.bashrc
