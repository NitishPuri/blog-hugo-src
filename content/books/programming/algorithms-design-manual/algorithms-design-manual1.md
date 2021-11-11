---
title: The Algorithms Design Manual
tags: [algorithms, programming, book, notes]
mathjax: true
lastMod: 2017-10-01
---

### How to design algorithms?

#### Using 'The right stuff'.,..

1. Do I really understand the problem?
    * What exactly does the input consist of?
    * What exactly are the desired results or outputs?
    * Can I construct an input example small enough to solve by hand? What happens when I try 
    to solve it?
    * How important is it to my application that i always find the optimal answer? Can I 
    settle for something close to the optimal answer? 
    * How large is a typical instance of my problem? Will I be working on 10 items? 1,000 
    items? 1,000,000 items?
    * How important is speed in my application? Must the problem be solved within one second?
    One minute? One hour? One day?
    * How much time and effort can I invest in implementation? Will I be limited to simple 
    algorithms that can be coded up in a day, or do I have the freedom to experiment with a 
    couple of approaches and see which is best?
    * Am I trying to solve a numerical problem? A graph algorithm problem? A geometric problem?
    A string problem? A set problem? Which formulation seems easiest?
2. Can I find a simple algorithm or heuristic for my problem?
    * Will brute force solve my problem *correctly* by searching through all subsets or 
    arrangements and picking the best one?
        * If so, why am I sure this algorithm always gives the correct answer?
        * How do I measure the quality of a solution once I construct it?
        * Does this simple, slow solution run in polynomial or exponential time? Is my problem 
        small enough that this brute-force solution will suffice?
        * Am I certain that my problem is sufficiently well defined to actually *have* a 
        correct solution?
    * Can I solve my problem by repeatedly trying some simple rule,, like picking the biggest 
    item first? the smallest item first? A random item first?
        * If so, on what types of inputs does this heuristic work well? Do these correspond to 
        the data that might arise in my application?
        * On what types of inputs does this heuristic work badly? If no such examples can be 
        found, can I show that it always works well?
        * How fast does my heuristic come up with an answer? Does it have a simple 
        implementation?
3. Is my problem in the catalog of algorithmic problems in the back of *this* book? 
    * What is known about the problem? Is there an implementation available that I can use?
    * Did I look into the right place for my problem? Did I browse through all pictures? Did I 
    look in the index under all possible keywords?
    * Are there relevant resources available on the World Wide Web? Did I do a Google web and 
    Google Scholar search? Did I go to the page associated with *this* book?
4. Are there special cases of the problem that I know how to solve?
    * Can I solve the problem efficiently if I ignore some of the input parameters?
    * Does the problem become easier to solve when I set some of the input parameters to trivial 
    values, such as 0 or 1?
    * Can I simplify the problem to the point where I *can* solve it efficiently?
    * Why can't this special case algorithm be generalized to a wider class of inputs?
    * Is my problem a special case of a more general problem in the catalog?
5. Which of the standard algorithm design paradigms are most relevant to my problem?
    * Is there a set of items that can be sorted by size or some key? Does this sorted order 
    make it easier to find the answer?
    * Is there a way to split the problem in two smaller problems, perhaps by doing a binary 
    search? How about partitioning the elements into big and small, or left and right? Does this 
    suggest a divide-a-conquer algorithm?
    * Do the input objects or desired solution have a natural left-to-right order, such as 
    characters in a string, elements of a permutation, or leaves of a tree? Can I use dynamic 
    programming to exploit this order?
    * Are there certain operations being done repeatedly, such as searching, or finding the 
    largest/smallest element? Can I use a data structure to speed up these queries? What about a 
    dictionary/hash table or a heap/priority queue?
    * Can I use random sampling to select which object to pick next? What about constructing 
    many random configurations and picking the best one? Can I use some kind of directed 
    randomness like simulated annealing to zoom in on the best solution?
    * Can I formulate my problem as a linear program? How about an integer program?
    * Does my problem seem something like satisfiability, the travelling salesman problem, or 
    some other NP-complete problem? Might the problem be NP-complete problem? Might the problem 
    be NP-complete and thus not have an efficient algorithm? Is it in the problem list in the 
    back of *Garey and Johnson*?
6. Am I sill stumped?
    * Am I willing to spend money to hire an expert to tell me what to do? 
    * Why don't I go back to the beginning and work through these questions again? Did any of my 
    answers change during my latest trip through the list? `Backtrack!!`

### A Catalog of Algorithmic Problems 

#### [Data Structures]({{< ref "algorithms-design-manual2">}})
#### [Numerical Problems]({{< ref "algorithms-design-manual3">}})
#### [Combinatorial Problems]({{< ref "algorithms-design-manual4">}})
#### [Graph Problems : Polynomial-Time]({{< ref "algorithms-design-manual5">}})
#### [Graph Problems : Hard Problems]({{< ref "algorithms-design-manual6">}})
#### [Computational Geometry]({{< ref "algorithms-design-manual7">}})
#### [Set and String Problems]({{< ref "algorithms-design-manual8">}})




