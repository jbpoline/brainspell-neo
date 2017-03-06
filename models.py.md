# models.py

###Configuration 
* database: specifies the Heroku Postgres database
* user: database username 
* password: database password
* host: database host site 
* port: database port number 
* sslmode: (required to instantiate Postgres database with ORM) 

### Class Articles
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

The rest of the file contains all functions that require database operations. This includes querying for articles, adding new users, etc.