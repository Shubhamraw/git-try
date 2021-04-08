# git-try
# Learn how to Deploy Django App to Heroku using Whitenoise

In ubuntu 20.04 git is pre- installed. For other linux version: you can installed by "sudo apt-get install git".
Check the git version with "git --version"

## SQLite to Postgres conversion For production

For production, sqlite database isnt a good choice, so one have to convert it in to postresql database. For this:
1. install postgresql by "sudo apt-get install postgresql postgresql-contrib"
2. ls /etc/postgresql/12/main/
3. service postgresql to check working and the command line associated with it.
4. service postgresql status/start/stop
5. sudo su postgres then type psql to open a postgres terminal then type "\l to list all databases \du to check roles(user)
6. try a query such as: ALTER USER postgres WITH PASSWORD 'password'; create user by "CREATE xyz WITH PASSWORD 'xyz'.
7. "\conninfo" to check for connected database
8. CREATE DATABASE mydb; \l to check. GRANT ALL PRIVILEGES ON DATABASE mydb TO postgres;. \l to check. 
9. CREATE ROLE abcd WITH SUPERUSER CREATEDB CREATEROLE LOGIN ENCRYPTED PASSWORD 'xyz';. \du to check
10. ALTER DATABASE mydb OWNER TO userabcd;. \l to check
11. finally we got a blank database "mydb"
12. Now it's time to dump adn load data
13. python manage.py dumpdata > datadump.json
14. then change the database settings.py as per your database
15. python manage.py makemigrations & python manage.py migrate
16. python manage.py loaddata datadump.json # to load data to our postgres database.



## Usage

* Make a copy of your project or use a seperate git branch.

* Make sure your virtual environment is activated.

* Add your dependencies to requirements.txt by typing in the terminal,
```shell
pip freeze > requirements.txt
```

* Add this in settings.py
```python
STATIC_ROOT = os.path.join(BASE_DIR, 'static')
```

* [Make a Heroku account](https://signup.heroku.com/)

* Download Heroku CLI by "sudo snap install --classic heroku"

* [Configure Django Heroku](https://devcenter.heroku.com/articles/django-app-configuration)
* Hope you got Procfile, requirements.txt, and runtime.txt(python version) in manage.py location

* In your terminal, type in
 ```shell
git init
git add .
git commit -m "initial_commit"

heroku login
heroku create app_name

This is for database link
heroku addons:create heroku-postgresql:hobby-dev
heroku config -s | grep DATABASE_URL #this is to check for heroku db name
PGUSER=your_role_name PGPASSWORD=your_role_password heroku pg:push postgres://localhost/mydb(my local database name)  postgresql-heroku_database_name

git push heroku master
heroku open

heroku run python manage.py migrate
```
** PS: if Heroku isn't recognized as a command, please close your terminal and editor and then re-open it.

* DEBUG = False in settings.py

* ALLOWED_HOSTS = ['your_app_name.herokuapp.com', 'localhost', '127.0.0.1'] in settings.py

* If you make edits, then just type in the terminal,
```shell
git add .
git commit -m "changes setting"
git push heroku master
```

