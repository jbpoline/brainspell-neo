# models.py

###Configuration 
* database: specifies the heroku PostGres database added to Path 
* user: database username 
* password: database password
* host: database host site 
* port: database port number 
* sslmode: (required to instantiate PostGres database with ORM) 

### Class Articles (specifies columns of Brainspell Articles table) 
* timestamp 
* abstract
* doi (digital object identifier)
* experiments (contains x,y,z coordinates for each table) 
* metadata (A set of extracted tags for each article) 
* neurosynthid (identifiers for neurosynth.org) 
* pmid (NCBI identifier) 
* reference 
* title 
* uniqueid (a sequentially assigned Article identifier) 

### Function article_search
* query: a user inputted string 
* start: an offset for the database 
* return: ModelQueryResultWrapper --> an iterator over Article objects
**Notes: **
* Matches against title, authors, and abstract columns 
* Search holds a formed query before it is executed 

### Function random_search
* Randomly selects 5 articles for display on main page 
* Selection done using random order_by tag

### Function get_article
* query: an article PMID 
* return: ModelQueryResultWrapper containing relevant articles

### Function insert_user 
* user: entered username 
* pw: user entered password 
* email: user entered emailaddress
* return: inserts user into Users table 
 
 









