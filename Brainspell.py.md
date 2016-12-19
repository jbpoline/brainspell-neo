# brainspell.py

### MainHandler 
* Yields loading page 

### LoginHandler 
#### Get 
* Renders login page
 
#### Post 
* Queries database for corresponding user 

### RegisterHandler 
#### Get 
* Renders register page
 
#### Post 
* Adds user to database 

### Search Handler 
* Renders main search page 

### Article Handler 
* Redirects to main page of query is not passed 

### Search Endpoint Handler 
* yields top 10 scoring articles on main search page
* Articles appended with id, title, and authors 

### Random Endpoint Handler 
* Appends 5 random articles to Main loading page 

### ArticleEndPointHandler 
* Yields Json data of individual article data 
