---
layout: post
title:  "Setup Django Environments"
---
In order to actually build projects, we need to make sure our computer is setup. In this project, we'll setup your environment. Made for Mac OS X Mavericks which is the latest, free operating system from Apple.

#### **Step:**
1. Check Python version:

		$ python --version

2. Install PIP (A tool for installing and managing Python packages):

		$ sudo easy_install pip

3. Install Setuptools (Easily download, build, install, upgrade, and uninstall Python packages):

		$ sudo easy_install setuptools

4. Install Virtualenv (A tool to create isolated Python environments):

		$ sudo pip install virtualenv

5. Install MySQL Python:

		$ sudo pip install mysql-python

6. Activate virtual environments:

		$ virtualenv somename
		$ cd somename/
		$ source bin/activate
		(somename)$ pip freeze

7. Install Django in virtual machine:

		(somename)$ pip install django==1.4.5
		(somename)$ pip install django --upgrade
	Tip: replace first line with `#!/usr/bin/env python` in `bin/easy_install`, etc.

8. Deactivate virtual environments:

		(somename)$ deactivate

9. Install Django in your local machine:

		$ sudo easy_install django
		// or
		$ sudo pip install django

10. Start Django project:

		$ django-admin.py startproject my_project
		$ cd my_project/
		$ python manage.py runserver

11. Specify dependencies with Pip:

		$ pip freeze > requirements.txt
		$ pip install -r requirements.txt

#### **Resource:**
* [codingforentrepreneurs.com/projects/get-with-mac/install-django-pip-and-virtualenv/](http://codingforentrepreneurs.com/projects/get-with-mac/install-django-pip-and-virtualenv/)
