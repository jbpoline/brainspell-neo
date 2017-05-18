In this section:
1. A detailed description of the repository structure
2. An explanation of important files within the repo
3. API documentation
4. Collection feature description

## Repository Structure

In the root of the repository, there are three folders:
* `archive`: contains scripts that aren't currently in use, but might be of use later, as well as an example of a formatted file that can be passed to our `bulk-add` API endpoint (see below).
* `database_dumps`: contains the entire Brainspell database as a MySQL dump, a PostgreSQL dump, and an XML file (which can be converted to JSON using the file `database_dumps/xmltojson.py`)
* `json_api`: in which we do the vast majority of our development; in this folder is the Tornado web server code, our ORM code, static files, as well as processing scripts to create visualizations and display text. 

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

### Search Brainspell's database

<https://brainspell.herokuapp.com/json/query>

*GET parameters:*
* *q*: a search query, like you would type into Brainspell’s search bar
* *start*: (default: 0) the number of results to offset by. The search endpoint gives ten results at a time. If you want results 11 - 20, then you would pass start = 10
* *req*: (default: t) the type of search. (Options: t = title, author, and abstract; x = experiments; p = PubMed ID; r = reference)

*Response format:*
* *articles*: an array containing one element per article, each consisting of a title, id, and authors
* *start_index*: the offset of the first article; if there are no articles left, then start_index will equal -1.

Sample query: <https://brainspell.herokuapp.com/json/query?q=vision>

### Get the contents of an article

<https://brainspell.herokuapp.com/json/article>

*GET parameters:*
* *pmid*: a PubMed ID for an article, which can be obtained from the /query or /random-query endpoints

*Response format:*
* *pmid*: the PubMed ID for the article
* *id*: the Brainspell ID for the article, for internal use only
* *experiments*: a string consisting of a JSON object containing extracted coordinates from Neurosynth
* *reference*: a citation for the article
* *metadata*: a string consisting of a JSON object containing tags 

Sample query: <https://brainspell.herokuapp.com/json/article?pmid=22521477>

### Add an article to Brainspell's database

<https://brainspell.herokuapp.com/json/add-article>

*GET parameters:*
* *key*: a user's API key, which is provided to them after registration
* *pmid*: the PubMed ID for the article you want to add

*Response format:*
* *success*: a boolean indicator for whether the article was added successfully

Sample query: <https://brainspell.herokuapp.com/json/add-article?key={YOUR_KEY}&pmid=22521477>

### Add several articles at once to Brainspell's database

<https://brainspell.herokuapp.com/json/bulk-add>

*POST parameters:*
* *articlesFile*: a JSON file containing a list of article “dictionaries,” with the format specified on the contribute page of Brainspell

*Response format:*
* *success*: a boolean indicator for whether the articles were added successfully

Sample query: <https://brainspell.herokuapp.com/json/bulk-add>

### Update the authors for an article

<https://brainspell.herokuapp.com/json/set-article-authors>

*GET parameters:*
* *key*: a user's API key, which is provided to them after registration
* *pmid*: the PubMed ID for the article you want to add
* *authors*: a comma-separated string of authors (e.g., "John Doe, John Doe Jr., John Doe Sr.")

*Response format:*
* *success*: a boolean indicator for whether the authors were set successfully

Sample query: <https://brainspell.herokuapp.com/json/set-article-authors?key={YOUR_KEY}&pmid=22521477&authors=A,B,C>


### Add a row to an experiment table

<https://brainspell.herokuapp.com/json/add-row>

*GET parameters:*
* *key*: a user's API key, which is provided to them after registration
* *pmid*: the PubMed ID for the article you want to add
* *experiment*: the zero-based index of the experiment that you want to add a row for (e.g., the first table has an index of 0, the second an index of 1, etc.)
* *coordinates*: a comma-separated string of the x, y, and z coordinate to add (e.g., "3,4,5")

*Response format:*
* *success*: a boolean indicator for whether the row was added successfully

Sample query: <https://brainspell.herokuapp.com/json/add-row?key={YOUR_KEY}&pmid=22521477&experiment=0&coordinates=1,2,3>

### Delete a row from an experiment table

<https://brainspell.herokuapp.com/json/add-row>

*GET parameters:*
* *key*: a user's API key, which is provided to them after registration
* *pmid*: the PubMed ID for the article you want to add
* *experiment*: the zero-based index of the experiment that you want to delete a row from (e.g., the first table has an index of 0, the second an index of 1, etc.)
* *row*: the zero-based row that you want to delete (the first row is `row=0`)

*Response format:*
* *success*: a boolean indicator for whether the row was deleted successfully

Sample query: <https://brainspell.herokuapp.com/json/delete-row?key={YOUR_KEY}&pmid=22521477&experiment=0&row=2>

### Split an experiment table in an article

<https://brainspell.herokuapp.com/json/split-table>

*GET parameters:*
* *key*: a user's API key, which is provided to them after registration
* *pmid*: the PubMed ID for the article you want to add
* *experiment*: the zero-based index of the experiment that you want to split the table for (e.g., the first table has an index of 0, the second an index of 1, etc.)
* *row*: the zero-based row where you want to split the table on; if you specify `row=4`, then rows 0 - 3 will be in the first table, and rows 4 onwards will be in the second

*Response format:*
* *success*: a boolean indicator for whether the table was split successfully

Sample query: <https://brainspell.herokuapp.com/json/split-table?key={YOUR_KEY}&pmid=22521477&experiment=0&row=2>

### Randomly sample five articles

<https://brainspell.herokuapp.com/json/random-query>

*Parameters:* None

*Response format:*
* *articles*: an array containing an element for each article, consisting of the authors, PMID, and title

Sample query: <https://brainspell.herokuapp.com/json/random-query>


## Collection features 



