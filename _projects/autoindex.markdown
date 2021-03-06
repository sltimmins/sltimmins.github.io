---
layout: page
title: Auto Indexer
permalink: /autoindex/
---

The goal of this program was to convert a file of keywords with page numbers into an indexed list. The index is a text file of the words sorted alphabetically and some words will have sub-entries. The algorithm must not repeat words, except for words that are sub-entries of another word, must ignore the case of the word, and ignore trailing punctuation. An example of a test input file and an output file generated by the algorithm are shown below. I also implemented and used my own vector and doubly linked list classes and used the cstring wrapper class that I had previously implemented in the algorithm.


<table>
    <tr>
        <th>
            Input
        </th>
        <th>
            Output
        </th>
    </tr>
    <tr>
        <td>
            <pre>
<15>
algorithm [binary tree]
analysis heap(memory)
<1>
[binary search tree] analysis
complexity algorithm [2-3 tree]
<5>
[b+ tree](tree) [Binary Tree](tree)
<8>
graph clique Tree
<5>
tree [full binary tree]
[complete binary tree]
<-1>
            </pre>
        </td>
        <td>
            <pre>
[2]
2-3 tree: 1
[A]
algorithm: 1, 15
analysis: 1, 15
[B]
b+ tree: 5
binary search tree: 1
binary tree: 5, 15
[C]
clique: 8
complete binary tree: 5
complexity: 1
[F]
full binary tree: 5
[G]
graph: 8
[H]
heap: 15
[M]
memory: 
    heap: 15
[T]
tree: 5, 8
    b+ tree: 5
    binary tree: 5, 15
            </pre>
        </td>
    </tr>
</table>

In the input file above, the numbers within the angle brackets represent the page numbers and the words following it all belong on that page. Then the individual words are separated by spaces with word phrases surrounded by square brackets. Lastly, if the word or phrase is meant to be a subentry of another word, the parent word will follow that word and be surrounded by parantheses. 

To accomplish this task I created an Entry object with private data members that stored the word itself, the page numbers the word belonged on, and a vector of Entry objects containing that word's subentries if it had any. I then parsed through the input text file adding the index entries to a linked list of entries while checking to see if the entry was already in the list. If the entry was already in the list I would instead add the page number to that specific entry in the list. I then used an overloaded outstream operator to output the entries in the correct format to the output file.

The GitHub repository of the complete algorithm can be found [here][auto-indexer-link].

[auto-indexer-link]: https://github.com/sltimmins/Auto-Indexer
