#Querying the API 

###Specify the columns of interest 
In the articles table for example, this data from all columns 
Articles = [Articles.timestamp,Articles.abstract,Articles.authors,Articles.doi,Articles.experiments,Articles.metadata,Articles.neurosynthid,Articles.pmid,Articles.reference,Articles.title,Articles.uniqueid]

###Use the PeeWee API to query (Note additional postgres functionality is also supported)
Ex: search = Articles.select(*x).where(Match(Articles.title,"brain"))
This query yields all columns where a match to brain is found in the article title 

### Execute the search 
search = search.execute() 

### Examine the results 
Search will be set to an iterable <playhouse._speedups._ModelQueryResultWrapper>
Each entry in the iterator is of type <__main__.Articles> where the object attributes
are the columns of the articles table. (E.g. <__main__.Articles>.title will yield one specific article title). 
Note: Though all the Article attributes will exist all columns not selected in the query will yield None if called. 


