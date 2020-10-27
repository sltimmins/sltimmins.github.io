---
layout: page
title: Flight Planner
permalink: /fltpln/
---

The goal of this program was to find all positive flight paths from one location to another and rank them based on overall cost or time. The program started by sorting a list of direct flights into an adjacency list of flights implented using a doubly linked list that I had previously implemented. I then used iterative backtracking to exhaust all possible flight paths from a specified point A to another specified point B keeping track of the total cost and time, also factoring in the cost and time of any layovers that may occur. To perform the iterative backtracking I created an adaptor stack class that adapted my previously implemented linked list to be used as a stack. The complete GitHub repository for this project can be found [here][flt-pln-link].

<div style="text-align: center"><img src="/images/fltplan.jpg" width="700" height="300" /></div>



[flt-pln-link]: 