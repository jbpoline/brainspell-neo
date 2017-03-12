This purpose of this project is to create a platform to 1) facilitate the human-curation of neuroimaging literature and 2) provide tools for researchers to better perform meta-analyses.

In this section:

**1. Why Brainspell? (Why does the platform exist?)**
Brainspell is the first web-based initiative to allow for the manual, crowd-sourced curation and annotation of neuroimaging literature. In recent years, the amount of neuroimaging publishings has increased substantially. These papers often have issues of poor reproducibility and difficulties in being properly parsed and formatted for any kind of substantial meta-analysis.  Brainspell seeks to combine extensive machine parsed imaging data with user-submissions to create a complete set of well-annotated neuroimaging literature.  


**2. What were the problems that we wanted to solve? (architecture; difficulty with getting PHP developers)**
Brainspell's original PHP design had substantial errors in scalability, modularization, and efficiency. Queries with large return values had the potential to create memory overflow errors, and the structure of the code prevented an independent json api separate from the html webpage. Furthermore, acquiring PHP developers who were willing to collaboratively construct new features for the page was difficult, and thus a more modern approach to web application development was required. 

3. A summary of the choices made.