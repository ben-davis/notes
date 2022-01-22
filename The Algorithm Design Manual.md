# Chapter 1 - Introduction to Algorithm Design
Algorithm's are procedures that solves *problems*. It must, in order to be correct, solve all *instances* of that problem. The difference between a general, well-specified problem and instances of that problem are key. Algorithms that only solve a subset of instances, and not the problem more generally, are incorrect.

Three desirable properties of a good algorithm:
1. Correct
2. Efficient
3. Easy to implement

These may not be achievable simultaneously.

An algorithm generally by describes as an input of some description and a desired output based upon that input.

Throughout the book he'll use the traveling salesman problem to describe various lessons on algorithms. In this chapter he ran through some of the naive approaches to solving the problem of how to minimize the total distance traveled when visiting a set of points exactly once. First was the idea randomly choosing a point and then walking the rest via a nearest neighbor search. This is obviously wrong as there's no way to choose the first point other than randomly. Then there was the idea of the closest-pair heuristic that would attempt to build a chain by repeatedly finding the closest pairs of points and then merging them correctly. This also has issues.

Generally the point is:
> There is a fundamental difference between *algorithms*, procedures that always produce a good result, and *heuristics*, which may usually do a good job but provide no guarantee of correctness.

## 1.2 Selecting the right jobs
He then describes the movie scheduling problem: if an actor has n number of possible jobs all of which pay equally but span different intervals, how do they select which jobs to take. The take home lesson is:
> Reasonable looking algorithms can easily be incorrect. Algorithm correctness is a property that must be carefully demonstrated.

## 1.3 Reasoning about Correctness
We need tool that we can use to determine which algorithms are correct. The primary tool is a mathematical proof. A proof has 3 parts:
1. A clear, precise statement of what you're trying to prove
2. A set of assumptions that can be taken as true
3. A chain of reasoning from the axioms to the statement being proved

Proofs are very hard and so aren't in the book. The work has been done by others, and so we learn about the available algorithm and not why they are correct.

### Problems and Properties
A problem has two parts:
1. The set of allowed input instances
2. The required properties of the algorithm's output

Remember: ask the wrong question -> get the wrong answer.

Often there can be a problem specification that allows too broad a class on instances for which no efficient algorithm exists. It's therefore usual and totally okay to restrict the problem specification to the point where such an algorithm does exist.

Defining the output requirements have two traps:
1. Asking an ill-defined question, like "find the best route". Without a definition of "best" this is meaningless.
2. Creating compound goals, where the output requires too many things. Such outputs may be well defined, but are complicated to reason about and solve.

### Expressing an algorithm
We can use human language, pseudocode, or actual code. Which depends on context. But we should always opt of the one that most clearly express the *idea* of the algorithm. If it's not clear enough, perhaps we're using too low-level a language.

### Demonstrating incorrectness
> Searching for counterexamples is the best way to disprove the correctness of a heuristic.

Good counter examples have two important properties:
1. **Verifiability**: A counterexample should calculate what answer the algorithm will give for a given instance and it should display a better answer to prove the algorithm didn't find it.
2. **Simplicity**: Good counterexamples strip everything unnecessary leaving the simplest form of why the algorithm didn;t work.

Techniques for hunting for counterexamples:
1. **Think small**: When algorithms fail, there is often a very simple example on which they fail. Amateurs tend to create big messy instances of how it fails, rather than finding the simplest test case.
2. **Think exhaustively**: For a small number of `n` (the size of the set of instances on which the algorithm is operated), there are usually only a small number of possible instances. Meaning a small number of outputs. One should think exhaustively in these possible outcomes when finding counterexamples.
3. **Hunt of the weakness**: Based on an definition of the problem, you can often use that to find a possible weakness. So if it's "find the biggest", then provide a case where two options are the same size.
4. **Go for a tie**: Provide instances of the same size.
5. **Seek extremes**: It's good to find extreme examples: big and small, left and right, few and many. These test the assumptions of the algorithm.

## 1.4 Induction and Recursion
Failure to find a counter example is not proof of correctness. For that we need a demonstration of correctness and for that mathematical induction is usually the way to go.

Induction is a chain of assumption that logically flow from one to another, starting with a base case. If A, then B, then C.

Two errors in inductive proofs we can look for:
1. **Boundary errors**: Assumptions can be made in an inductive proof that ignore boundary cases.
2. **Extension claims**: Inductive proofs often make a claim at a small scale and the assume that the same assumption holds for any size of `n`. This can often not be true.

Induction and recursion are both similar due to them both having general and boundary conditions. Induction has general claims and boundaries to which those claims are true. Recursion has a general function that has a boundary condition that stops infinite recursion. Therefore often recursive algorithms can be proved by induction.

## 1.5 Modeling the Problem
Modeling is the art of formulating your application in terms of precisely defined, well-understood problems. Algorithms in the literature are often designed to work on rigorously defined abstract structures, not specific real-world objects. Therefore in order to effectively use the many many algorithms that exist, we need to be able to model our application and the problems it solves in terms of these fundamental structures.

Remember though that not all problems always fit tidily within a well-defined abstract structure. Sometimes modeling in different ways is possible.

 > Modeling your application in terms of well-defined structures and algorithms is the most important single step towards a solution.

### Combinatorial objects
1. **Permutations**: arrangements, or orderings, of items. So {1,3,2,4} and {4,2,3,1} are distinct permutations of the same set.
	1. Likely the object in question when the problem seeks an "arrangement", "tour", "ordering", "sequence"
2. **Subset**: selections from a set of items. E.g. {1,3,4} and {2} are two distinct subsets of the first four integers. Order does not matter.
	1. Likely the object in question when the problem seeks an "cluster", "collection", "committee", "group", "packaging", "selection"
3. **Trees**: hierarchical relationships between items.
	1. Likely the object in question when the problem seeks an "hierarchy", "dominance relationship", "ancestor/descendant relationship", "taxonomy"
4. **Graphs**: relationships between arbitrary pairs of objects.
	1. Likely the object in question when the problem seeks an "network", "circuit", "web", "relationship"
5. **Points**: locations in geometric space.
	1. Likely the object in question when the problem seeks an "sites", "positions", "data records", "locations".
6. **Polygons**: regions in geometric space.
	1. Likely the object in question when the problem seeks an "shapes", "regions", "configurations", "boundaries".
7. **Strings**: sequences of characters, or patterns.
	1. Likely the object in question when the problem seeks an "text", "characters", "patterns", "labels".

### Recursive objects
**Thinking recursively is learning to look for bug things that are made from smaller things of *exactly the same type as the big thing*.**

Recursive structures occur everywhere in the algorithmic world. Each of the structures above have a way to describe in terms of recursion:
1. **Permutations**: Delete the first element of a permutation of `n` and you get a permutation of `n-1`.
2. **Subset**: Every subset of `{1, ..., n}` contains a subset of `{1, ..., n - 1}`.
3. **Trees**: Delete the root of a tree and you get a collection of smaller trees. Delete a leaf and you get a slightly smaller tree.
4. **Graphs**: Delete any vertex from a graph and you get a smaller graph. Divide vertices into groups of left and right and you get another collection of smaller graphs (and broken edges).
5. **Points**: Take a cloud of points and draw a line through them -> two groups of points.
6. **Polygons**: Inserting any internal chord between two non-adjacent vertices of a simple polygon cuts it into two smaller polygons.
7. **Strings**: Delete a character from a string -> still a string.

Recursive descriptions require both decomposition rules and basis cases. Basic cases = the specification of the smallest and simplest objects where the decomposition stops.

## 1.6 Proof by Contradiction
Basics of a contradiction argument:
1. Assume the hypothesis (the statement you want to prove) is false
2. Develop some logical consequences of this assumption
3. Show that one consequence is demonstrably false, therefore showing the assumption is incorrect hypothesis true.

Take Euclid's proof that there are infinite primes:
1. Let's take the negation and assume that there are a fixed number of primes `m`
2. If we take the product `N` of all of the primes in `m`, then it has the property that `N` is divisible by all primes within `m`.
3. But if we consider the integer `N + 1`. 1 effectively becomes a remainder of a division by any of the prime factors of `N`.
4. Therefore N is prime.
5. Therefore the assumption is wrong and the hypothesis true.

(I don't understand how this is true. Surely it only proves that N+1 is indivisible by any factor of N. But there are other integers between those factors, right?)

**NOTE**: 1.7 and 1.8 are the war stories that I'm not always making notes on.

## 1.9 Estimation
When you don't know the answer, estimate (principled guessing). E,g, the running time of a program.

Estimation is usually either principled calculation (function of quantities you already know or could easily find) or analogies (based on past experience). 

E.g. pennies in a jar. Using the prior calculation of how many pennies in a roll of pennies and how tall those rolls are, we can estimate how many rolls make up the volume.

A best practice in estimating is to use multiple methods to come up with an estimate to see if they are about equal.
