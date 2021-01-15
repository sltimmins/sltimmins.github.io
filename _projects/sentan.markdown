---
layout: page
title: Sentiment Analysis
permalink: /sentan/
---

The goal of this program was to analyze the sentiment of movie reviews. To accomplish this goal I used a file of 50,000 different movie reviews that had already been classified as having a positive or negative sentiment. I then utilized 40,000 of these reviews as a training set for the algorithm and the remaining reviews as test cases to quantify the accuracy of my algorithm. Furthermore, I implemented my own cstring wrapper class that was used throughout the program to store the reviews and individual words. 

To analyze these reviews I used word frequency analysis by first parsing the first 40,000 reviews into their individual words and assigning a "sentiment value," everytime a word appeared in a positive review I would increment its sentiment value by one, everytime it appeared in a negative review I would decrement it's value by one. I then removed all words with a sentiment value greater than a certain number and below a certain number. This allowed me some degree of tuning my algorithm to be more accurate. Then, in order to classify the 10,000 reviews being used to test my algorithm, I parsed them into their individual words as well, I then iterated through each review adding up the sentiment values of each word that appeared. If the resulting number was positive the review was classified as a positive, if it was negative the review was classified as negative. This method resulted in a calculated accuracy of 76.33%.

The GitHub repository for this project can be found [here][sent-an-link].

[sent-an-link]: https://github.com/sltimmins/Sentiment-Analysis
