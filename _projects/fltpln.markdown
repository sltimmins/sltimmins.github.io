---
layout: page
title: Flight Planner
permalink: /fltpln/
---

The goal of this program was to find all positive flight paths from one location to another and rank them based on overall cost or time. The program started by sorting a list of direct flights into an adjacency list of flights implented using a doubly linked list that I had previously implemented. I then used recursive backtracking to exhaust all possible flight paths from a specified point A to another specified point B keeping track of the total cost and time, also factoring in the cost and time of any layovers that may occur. To perform the recursive backtracking I created an adaptor stack class that adapted my previously implemented linked list to be used as a stack. 

<div style="text-align: center"><img src="/images/fltplan.jpg" width="700" height="300" /></div>

The program will take two input files similar to the ones shown below and find the three best possible flight paths based on either time or cost. The first input file is a text file of the possible direct flights, the file begins with the number of direct flights, then for each flight it gives the origin, destination, cost, time, and airline in that order. The second input file is a list of the requested flight plans and the requested method beginning with the number of plans requested followed with the origin, destination, and the requested sort method with "T" representing a sort by time and "C" representing a sort by cost. It will then provide the three best options based on the sorting parameter and present them in a sorted order including both total time and total cost.

<table>
    <tr>
        <th>
            Direct Flight Data
        </th>
        <th>
            Requested Flight Plans
        </th>
    </tr>
    <tr>
        <td>
            <pre>
5
Dallas|Austin|98|47|Spirit
Dallas|Austin|98|59|American
Austin|Houston|95|39|United
Dallas|Houston|101|51|Spirit
Austin|Chicago|144|192|American
            </pre>
        </td>
        <td>
            <pre>
2
Dallas|Houston|T
Chicago|Dallas|C
            </pre>
        </td>
    </tr>
</table>
<table>
    <tr>
        <th>
            Output File
        </th>
    </tr>
    <tr>
        <td>
            <pre>
Flight 1: Dallas, Houston (Time)
Dallas -> Houston (Spirit). Time: 51 Cost: $101.00
Dallas -> Austin (Spirit) -> Houston (United). Time: 129 Cost: $212.00
Dallas -> Austin (American) -> Houston (United). Time: 141 Cost: $212.00

Flight 2: Chicago, Dallas (Cost)
Chicago -> Austin (American) -> Dallas (Spirit). Time: 282 Cost: $261.00
Chicago -> Austin (American) -> Dallas (American). Time: 294 Cost: $261.00
Chicago -> Austin (American) -> Houston (United) -> Dallas (Spirit). Time: 368 Cost: $378.00
            </pre>
        </td>
    </tr>
</table>

The complete GitHub repository for this project can be found [here][flt-pln-link].

[flt-pln-link]: https://github.com/sltimmins/Flight-Planner
