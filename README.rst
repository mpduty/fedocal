fedocal
=======

:Author: Pierre-Yves Chibon <pingou@pingoured.fr>


fedocal is a web based calendar application.


Get this project:
-----------------
Source:  https://github.com/pypingou/fedocal



Dependencies:
-------------
.. _python: http://www.python.org
.. _Flask: http://flask.pocoo.org/
.. _python-flask: http://flask.pocoo.org/
.. _python-flask-wtf: http://packages.python.org/Flask-WTF/
.. _python-wtforms: http://wtforms.simplecodes.com/docs/1.0.1/
.. _SQLAlchemy: http://www.sqlalchemy.org/
.. _python-sqlalchemy: http://www.sqlalchemy.org/
.. _python-vobject: http://vobject.skyhouseconsulting.com/
.. _iCal: http://en.wikipedia.org/wiki/ICalendar
.. _python-kitchen: http://packages.python.org/kitchen/

This project is a `Flask`_ application. The calendars and meetings are
stored into a relational database using `SQLAlchemy`_ as Object Relational
Mapper (ORM). fedocal provides an `iCal`_ feed for each calendar and relies
on `python-vobject`_ for this.


The dependency list is therefore:

- `python`_ (2.5 minimum)
- `python-flask`_
- `python-flask-wtf`_
- `python-wtforms`_
- `python-sqlalchemy`_
- `python-vobject`_
- `python-kitchen`_


Deploying this project:
-----------------------

.. _Flask deployment documentation: http://flask.pocoo.org/docs/deploying/

Instruction to deploy this application is available on the
`Flask deployment documentation`_ page.

 **My approach.**

Below is the approach I took to deploy the instance on a local (test) machine.

Retrieve
the sources::

 cd /srv/
 git clone <repo>
 cd fedocal

Copy the
configuration file::

 cp fedocal.cfg.sample fedocal.cfg

Adjust the configuration file (secret key, database URL, admin group...)


Then configure apache::

 sudo vim /etc/httd/conf.d/wsgi.conf

and put in this file::

 WSGIScriptAlias /fedocal /var/www/wsgi/fedocal.wsgi
 <Directory /var/www/wsgi/>
    Order deny,allow
    Allow from all
 </Directory>

Then create the file /var/www/wsgi/fedocal.wsgi with::

 import sys
 sys.path.insert(0, '/mnt/fedocal/')
 
 import fedocal
 application = fedocal.APP


Then restart apache and you should be able to access the website on
http://localhost/fedocal


Testing:
--------

This project contains unit-tests allowing you to check if your server
has all the dependencies correctly set.

To run them::

 ./run_tests.sh


License:
--------

This project is licensed GPLv3+.