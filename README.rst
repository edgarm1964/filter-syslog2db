filter-syslog2db
================

Python 3 implementation of a simple UDP syslog server which inserts received 
and filtered, most notably SSH and MAIL messages into a MariaDB_ or MySQL_
database.

About
-----

The Python script will start a syslog server which inserts the received and
filtered messages into a MariaDB_ or MySQL_ database. 

Every time the script is executed it will check if the database and table exist 
and if not create them. Then the script is executed until it will be 
manually closed. Every time a message is recieved on the defined port the 
data will be decoded as UTF-8, inserted in the database and printed out in 
the terminal.

Be aware that the UDP packages are not encypted. It is taken care off avoiding 
SQL injections because the server might be facing the internet.

The script uses the python module socketserver_ and the external module
mysql-connector-python_ . See ``requirements.txt`` for installed packages and the 
used versions. The file is created with ``pip3 freeze > requirements.txt``.

Usage
-----

First install the required mysql-connector-python module in the global Python 3 
environment or in a virtual Python 3 environment. The latter has the advantage that 
the packages are isolated from other projects and also from the system wide 
installed global once. If things get messed up, the virtual environment can 
just be deleted and created from scratch again. For more informations about 
virtual environments in Python 3, see venv1_ and venv2_ .

.. code-block:: console

    pip3 install mysql-connector-python

Then modify the ``PORT``, ``db_name``, ``table_name``, ``db_user``, ``db_password``, 
``db_host`` and ``db_port`` parameters in the ``syslogserver.py`` script and 
execute it.

.. code-block:: console

    python3 syslogserver.py

Or use e.g. tmux_ to execute it in the background.

Systemd service
^^^^^^^^^^^^^^^

An example .service file is also included to show how to run the syslog server
as a systemd service at startup. For more informations, see `systemd.service`_ .
In the example .service file a virtual Python 3 environment is used to execute
the script. Also the script will be automatically restarted if it crashes to
ensure that the syslog server is always running. The local user name and the
path to the virtual Python 3 environment needs to be adjusted before it can be
used.

To activate the systemd service execute the following commands.

.. code-block:: console

    sudo cp syslogserver.service /etc/systemd/system/

    sudo systemctl daemon-reload

    sudo systemctl start syslogserver.service

    sudo systemctl enable syslogserver.service


Credits
-------

pysyslog.py: https://gist.github.com/marcelom/4218010 

py3syslog: https://github.com/choeffer/py3syslog

References
----------

.. target-notes::

.. _MariaDB: https://mariadb.org/
.. _MySQL: https://www.mysql.com/
.. _socketserver: https://docs.python.org/3/library/socketserver.html
.. _mysql-connector-python: https://pypi.org/project/mysql-connector-python/
.. _venv1: https://docs.python.org/3/tutorial/venv.html
.. _venv2: https://docs.python.org/3/library/venv.html
.. _tmux: https://en.wikipedia.org/wiki/Tmux
.. _`systemd.service`: https://www.raspberrypi.org/documentation/linux/usage/systemd.md
