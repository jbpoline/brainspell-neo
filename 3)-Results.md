In this section:
1. A detailed description of the repository structure
2. A summary of important files within the repo
3. API documentation

## Repository Structure

In the root of the repository, there are three folders:
* `archive`: contains scripts that aren't currently in use, but might be of use later
* `database_dumps`: contains the entire Brainspell database as a MySQL dump, a PostgreSQL dump, and an XML file (which can be converted to JSON using the file `database_dumps/xmltojson.py`)
* `json_api`: in which we do the vast majority of our development; in this folder is the Tornado web server code, our ORM code, static files, etc. (nearly all of the Brainspell project)

Within the `json_api` folder:
* `brainspell.py`: runs the Tornado main event loop and contains all of the RequestHandlers (routes). Our naming convention is to use `____EndpointHandler` for handlers related to the JSON API, and `____Handler` for web interface handlers. We map JSON endpoints to URLs matching the pattern `/json/*`.
* `helper_functions.py`: as the name suggests, contains helper functions. Some of these are for adding articles (e.g., fetching relevant data from PubMed), some are for the `bulk-add` endpoint (e.g., cleaning JSON data), etc.
* `models.py`: for our ORM, PeeWee, which lets us treat our database like a Python object.
* `test_tornado.py`: our suite of continuous integration tests, which are executed by Travis CI before Heroku deployment.
* the `static` folder: the contents of which are fairly self-explanatory (HTML, CSS, images, fonts, and Javascript)

## Explanation of Important Files

### brainspell.py

At the bottom of the `brainspell.py` file is the function where we start the Tornado main event loop (within the block `if __name__ == "__main__":`). We call the `make_app` function.

In the `make_app` function, we return a Tornado web application object, mapping the appropriate routes to each of our handlers (which are subclasses of Tornado's `tornado.web.RequestHandler`). Note the function and route naming conventions. As stated earlier, our naming convention is to use `____EndpointHandler` for handlers related to the JSON API, and `____Handler` for web interface handlers. We map JSON endpoints to URLs matching the pattern `/json/*`.

Throughout the rest of the file, each function has a short comment above it, specifying the purpose of the function.

### models.py

An explanation of the `config` dict:
* database: specifies the Heroku Postgres database
* user: database username 
* password: database password
* host: database host site 
* port: database port number 
* sslmode: (required to instantiate Postgres database with ORM) 

The classes at the beginning of the file (`Articles`, `Concepts`, etc.) correspond to tables within our Postgres database. These are PeeWee "models," which map the columns of each table to a Python object. They implement the `signals.Model` class.

The rest of `models.py` contains all functions that require database operations. This includes querying for articles, adding new users, etc.

## API Documentation