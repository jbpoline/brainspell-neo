## Organizing Project Development

We collaborated on the project through a Git repository on GitHub. During weekly meetings, we assigned tasks for each developer to complete. We enforced continuous integration tests using Travis CI, ensuring each commit met the minimum criteria for functionality before deploying.

## Architecture Decisions

We first explored the possible options to switch away from PHP and MySQL.

One idea was to switch to a MEAN stack (MongoDB, ExpressJS, AngularJS, and Node). Another was to switch to a Python framework like Flask or Django. A Python framework had the advantages that we could easily incorporate existing machine learning and data analysis libraries, which didn't necessarily have a counterpart in Javascript. Initially we deployed a Flask server to an Amazon Web Services instance of Red Hat Enterprise Linux, to get started on development.

As we further explored our options for finalizing the stack, we realized that we might also need to eventually incorporate WebSockets (e.g., when a user makes a search request that's especially long, or if we want to send bulk amounts of coordinates to a user). With that in mind, we switched from Flask to the Tornado framework. We also moved from AWS to Heroku using Git deployment for cost consideration.

Whichever database solution we chose, we needed it to offer an effective full-text search, since a key functionality of the project is to allow users to search through the annotated literature. One option that we considered was using a search engine library, like Apache Lucene/Solr. We found that Postgres offers [full text search](https://www.postgresql.org/docs/9.5/static/textsearch.html), which we ultimately decided was appropriate for our needs. To migrate the database from MySQL to Postgres, we used Pentaho Kettle.

To modularize the database operations, we used the PeeWee ORM in Python. We separated the JSON API from the user interface, which makes AJAX requests to the API.

## Advantages of Our Solution

These design choices solve the previous implementation limitations described in the Introduction. For one, the new version of Brainspell no longer runs out of memory when making requests with thousands of results (e.g., searching for the word "brain"). Switching away from an Apache web server and leveraging caching has made our new platform more scalable. Because the JSON API is effectively separate from the front end, if a developer wants to access the data from Brainspell, they no longer have to scrape and parse the HTML website; they can make a request directly to our JSON API. Since Python is a more popular, modern language for web development, and one more widely used in the neuroimaging community, we expect that it will be easier to find collaborators to work on the project.

## Implementation of the "collection" concept
Collections of articles enable a group of researcher to create and curate collectively a specific set of articles. Once a set of article has all the meta data necessary for a meta analysis, a key feature is to be able to be able to return to the exact set of articles included in the meta analysis, and their meta data. 
For this purpose, we implemented in brainspell the creation of set of articles ("collection") and the option to save the meta data of a collection as a git repository on github. This allows a collaborator to clone the collection meta data, import it into braispell, and propose some changes to the collection. This should facilitate the creation and curation of collection across collaborators for a meta analysis. 


