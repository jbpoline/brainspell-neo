The below API is currently in progress.

We're copying database queries from the original brainspell project to figure out the kinds of information we want to extract.

### Example

```
/api/endpoint

parameters: {this: 'that', foo: 0.0}
returns: {'the-other': 2}
```

(See some_function in some_file.php:LINE_NO)



###Brainspell.php

####Function User-Register //Checks if user is registered otherwise adds inputs to database 

    Global: {$dbname, $connection} // Global variables not passed as inputs
    Parameters: {'username','password','email'} 
    Returns: {"Exists" if credentials in system, otherwise function updates DB} 

(See User_Register in Brainspell.php: 69-99) 


####Function User_login()

    Global: {$dbname, $connection}  
    Parameters: {'username','password','row': mysqli_fetch_array($checklogin)}   
    Returns: {"Yes" if logged in, else "No"}

(See User_login in Brainspell.php: 101-120



####Function home()

    Global: {$dbname, $rootdir, $connection}   
    Parameters: {$result: Count(*) from DB, $count: associative array of $result}   
    Results: {Updates HTML to include articles indexed, username, and login status}  

(See Home() in Brainspell.php: 266-296)


####Function article()

    Global: {$dbname, $rootdir, $connection}   
    Parameters: {$id: 'Article PMID/DOI'}  
    Results: {mysqli_free_result($result): Articles matching PMID/DOI}   

(See Article() in Brainspell.php: 378-461)



####Function article_json_pmid

    Global: {$dbname, $rootdir, $connection}   
    Parameters: {$pmid: Article PMID, $result: Articles matching PMID}  
    Returns: {Articles matching PMID}   

(See article_json_pmid in Brainspell.php: 463-490)


####Function article_json_doi

    Global: {$dbname, $rootdir, $connection}   
    Parameters: {$DOI: Article DOI, $result: Articles matching DOI}  
    Returns: {Articles Matching DOI}  
 
(See article_json_pmid in Brainspell.php: 500-530)


####Function search_lucene
    Global: {$dbname, $connection}   
    Parameters: {$result: articles matching PMID}   

Insert into Database Feature  

    Parameters: {Title, Authors, Abstract, Reference, PMID, DOI, NeuroSynthID, Experiments, Metadata)   
    Result: {Inserts Given article into Database}  

(See search_lucene in Brainspell.php: 536-600)




###Admin_re_index_UserAction_in_Log_table.php
    Global {$dbhost: 'localhost'; $dbname: 'brainspell'; $dbuser: 'root'; $dbpass: '*******'} 
    Parameters: {$result: (select * from db where TYPE = "useraction"), $result2: Artiles matching given PMID}


###Admin_upload_papers.php
    Global {$dbhost: 'localhost'; $dbname: 'brainspell'; $dbuser: 'root'; $dbpass: '*******'} 
    Parameters: {$result: All Articles from database; $record: PMID, DOI, Experiments, and Article Title} 
    Parameters: {$q: Article Title, Experiments, PMID; $result2: Articles pertaining to $q in database} 



    // Sets aspects







