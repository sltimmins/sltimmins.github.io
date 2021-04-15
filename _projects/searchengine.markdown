---
layout: page
title: COVID-19 Research Database Boolean Query Engine
permalink: /searchengine/
---

The goal of this team-based project was to design and implement a boolean query search engine capable of parsing, storing, and searching a database of COVID-19 research papers. In order to complete this task my teammate and I created a hashmap class used to store the index of authors and an AVL tree in order to create an inverse file index of the words that made up the papers. 

The papers were stored in 12,000 json files with each paper having it's own file. We utilized the rapidJSON library to parse the documents and then sorted the authors, removed stop words, stemmed all remaining words, and sorted them accordingly. Our algorithm's speed was ranked in the top 25% of the course's teams across all sections in our semester. 

Next, we created a query engine that was able to return all papers that met the requirements of the boolean query input. Users were allowed to search papers using keywords such as AND, OR, NOT, and AUTHOR and the query processor would return the pertinent information of the top 15 papers that met the requirements given. The top 15 papers were determined using term frequency - inverse document frequency. The user could then select one of the produced papers and be presented with the first 300 words of the paper. This was all controlled through a terminal based user interface designed and implemented by my teammember and myself. 

The UI also presented the ability for the user to save the information parsed directly from the files to a persistence .json file that could be opened and reparsed faster than parsing the entire database again. Finally, the UI also presented the user with several other options to access pertinent information about the database as a whole such as total number of papers in the database, average number of words per paper, the total number of words in the database excluding stop words, total number of unique authors, and the 50 most frequent words in the database.

<div style="text-align: center"><img src="/images/search-engine-ui.jpg"  /></div>


The complete GitHub repository for this project can be found [here][search-engine-link].

[search-engine-link]: https://github.com/sltimmins/search-engine