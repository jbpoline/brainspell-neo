While neuroimaging studies are generating a large number of articles, tools to integrate information across studies are still sparse. The most well known databases for voxel based coordinates are the brainmap (Fox et al.) and the neurosynth databases. Brainmap is a very carefully curated database that also comes with an ontology, while neurosynth (Yarkoni et al) is based on an automatic extraction of xyz coordinates in article text. The careful curation by the brainmap team and the close development of the project are limitations. On the other hand, neurosynth may lack curation. Brainspell (Toro et al., ) was developed to answer the curation need of the Neurosynth database. 
This purpose of the Brainspell project is to 
1. facilitate the human-curation of neuroimaging literature and 
2. provide tools for researchers to better perform meta-analyses.

## Why Brainspell?
While a centralized curation of the literature may provide a more homogeneous and better quality curation, it becomes impracticable as the number of studies becomes very large. An estimate of the number of articles containing the word "fMRI" in the title or in the abstract in 2016 is ???. Brainspell is a platform for the *crowd-sourced* curation and annotation of neuroimaging literature. Individual papers reproducibility is hard to assess and therefore the community rely more and more on meta analyses to extract across papers the stable results. Articles are difficult to automatically parse for the information needed to perfrom meta-analyses and this extraction is therefore generally manual and performed by a small group of researchers. Brainspell seeks to combine machine-parsed image data with users' annotations to curate a repository of neuroimaging literature and facilitate meta-analyses.  


## What were the problems that we wanted to solve?
Brainspell's original PHP design had problems with scalability, modularization, and efficiency. Queries with over a few thousand results caused the server to run out of memory, and the structure of the code didn't allow developers to access Brainspell's database without scraping and parsing the HTML website. Furthermore, finding PHP developers who were willing to collaborate on the project was difficult. .

## What changes did we make?
To solve for the issues listed above and enable Brainspell as a collaborative tools for curation of meta analysis, we 1) swich to a more modern platform and 2) introduced the concept of "collection" of article in Brainspell. 
We rebuilt the PHP platform using the Tornado framework in Python. We also switched the database from MySQL to PostgreSQL, designed a JSON API, and built a user interface on top of the API.
