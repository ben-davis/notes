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


# Chapter 2 - Algorithm Analysis
In order to understand the effectiveness of an algorithm, we need a way to compare it with others without actually implementing it. There are 2 important tools for this:

1. The RAM model of computation
2. The asymptotic analysis of computational complexity

Before we get started, remember that Big Oh is just a description of the asymptotic growth behaviour of a function as the size of its inputs grow. That wasn't always in my head as I read this, but always remember that's what we're doing is key.

## 2.1 The RAM model of Computation
Machine-independent algorithm design depends on a hypothetical machine called the *Random Access Machine*:

1. Each *simple* operation (+, \*, -, =, if, call) takes exactly one time step
2. Loops and subroutines are *not* considered simple operations. Instead they are a composition of many single-step ops.
3. Each memory access takes exactly one time step. We also have as much memory as we need. The RAM model doesn't differentiate between cache or disk access.


**Essentially: under RAM, we measure run time by counting the steps an algorithm takes.**

Although the assumptions made above are demonstratably not how a machine operates in reality, the simplified model proves to be a very good measure of real world performance. Think of it like the flat earth model. We know it's not reality, but in our daily lives, building houses, putting up shelves, etc, we use this model as a reliable measure of reality. The RAM model is the same.

### Best, Worst, and Average Complexity
To use the RAM model to understand the real-world performance of an algorithm, we need to use it to determine performance of an algorithm over *all possible instances*. Take sorting. With a sort algorithm, the possible instances include every arrangement of `n` keys, for all possible values of `n`. Each of these (a value of `n` and every arrangement of `n`) can be plotted on a graph:

```
                        │
            │
            │
            │                                                    .. x  Worst
            │                                                 ..    x
            │                                              .        x
            │                                            .          x
            │                                         .
            │                                     . x
            │                                  .                    x
Number of   │                               .                       x
  Steps     │                             .         x               x
            │                           .           x               x
            │                       .x
            │                    .                                  x
            │                 .            .     ..  .   .     .  ...  Average
            │             .     .    .                              x
            │          .   . .                      x               x
            │        x . ..   . ..                                  x
            │                        x .            x               x
            │                             .
            │                                .  .                   x
            │                                    .  x  .            x
            │                                              .    .   x
            │                                                       .  Best
            │
            └────────────────────────────────────────────────────────────

                     1               2              3               4

                                    Problem Size
```
Along the X axis is the size of the problem, with Y the number of steps, and the points the performance of a given arrangement of `n`.

There are 3 interesting plots over these points:

1. *Worst-case complexity*: the maximum steps taken in any instance of size `n`.
2. *Best-case complexity*: the minimum steps taken in any instance of size `n`.
3. *Average-case complexity* (or *expected time*): the average steps taken over all instances of size `n`.

Worst-case generally provdes to be the most useful. Best-case is often unlikely and so is useless to think about. Average case can often be difficult to determine. What does average even mean? Worst-case is often easy to calculate.

That said, average-case is useful with respect to *randomized algorithms* which use random numbers to make decisions in an algorithm.

## 2.2 Big Oh Notation
The best, worst, and average complexities are hard to work with in practice as their functions tend to:

1. *Have too many bumps*: There are often irregularities with the performance of algorithms based on some particular value of `n` (binary search is typically faster when `n = 2^k - 1` because the array partitions work out nicely). Capturing the irregularities exactly would serve no real purpose when talking about the generalized performance of an algorithm.
2. *Require too much detail to specify precisely*: We would essentially need to implement the entire algorithm to fully understand all the steps an algorithm could possibly take. Doing so would be exact, but offers little benefit over saying "the time grows quadratically with *n*".

Big Oh notation allows us to discuss the upper and lower bounds of an algorith while ignoring its impactless specifics.

**Note**: Big Oh ignores constants. So `f(n) = n == g(n) = 2n`. Constants tell us nothing aboout the performance of an algorithm as it scales. The constant might occur due to a difference in implementation (Java vs C) of the same algorithm.

Here are the formal definitions of Big Oh are:

1. `f(n) = O(g(n))` means `c · g(n)` is an *upper bound* on `f(n)`. Therefore, some constant `c` exists such that `f(n) <= c · g(n)` for every `n` large enough (meaning `n >= n₀` where `n₀` is some constant).
2. `f(n) = Ω(g(n))` means `c · g(n)` is a *lower bound* on `f(n)`. Therefore, some constant `c` exists such that `f(n) >= c · g(n)` for all `n >= n₀`.
3. `f(n) = Θ(g(n))` means `c₁ · g(n)` is an upper bound on `f(n)` and `c₂ · g(n)` is a lower bound on `f(n)`, for all `n >= n₀`, for all `n >= n₀`. Therefore, some constant `c` exists such that `f(n) <= c₁ · g(n)` and `f(n) >= c₂ · g(n)`, for all `n >= n₀`. This means that `g(n)` is a tight bound on `f(n)`. Essentially a marge of the previous two using two constants.

These all hold for a constant `n₀` such that `n >= n₀`. This reflects that there's always a lower bound on values of `n` that aren't particularly interesting and so we don't both modelling their complexity. E.g. evaluating a sorting algorithm for `n=4` isn't all that interesting, `n=10000` is far more interesting.

Generally when choosing constants we're looking for constants that prove the inequalities in an obvious way.


### Examples

Let's start with `O(g(n))`. Let's assume our function is `3n² - 100n + 6`. Let's figure out its `O(g(n))`. We're looking for a `g(n)` so that `f(n) <= c · g(n)`.

Let's start with `c=3`: is `3n² - 100n + 6` less than `3 · g(n)`? If `g(n)` is `n²`, then yes, because `3n² >= f(n) = 3n² - 100n + 6`. **So:** `f(n) = O(n²)`.

Let's see if it's `O(n³)`: starting with `c=1`, is `n³ >= 3n² - 100n + 6`? Yes, but only when `n > 3`. So, `f(n) = O(n³)` when `n₀ > 3`

Is it `O(n)`? Let's put `c=100` again: is `100n > 3n² - 100n + 6`? Well, yes, but only for values of `n <= (c + 100)/3` (derived from the `f(n)`). So even for a high constant, it only holds for small values of `n`. We're not intersted in that, so `f(n) != O(n)`.

There are examples in the book for `Ω(n)` and `Θ(n)`, but they follow the same logic as above. It's jarring to read `n² = O(n³)`. It's easier to read "=" as "one of the functions of". So `n²` is one of the functions of `n³`. To me it's also easy to read it as `n³` is an upper bound of `n²`.


One more: is `2ⁿ⁺¹ = O(2ⁿ)`? Well it is if there is a constant such that `2ⁿ⁺¹ <= c ·2ⁿ`. If `c=2` then, `2ⁿ⁺¹ = 2· 2ⁿ`, so clearly `2 · 2ⁿ ≤ c · 2ⁿ` for any `c >= 2`.


## 2.3 Growth Rates and Dominance Relations
It might seem crazy to ignore constants as it means `f(n) = 0.001n²` is identical to `f(n) = 1000n²`. How can that be? Growth rates baby. For most values of `n`, the constants become irrelevant, resulting in effectively the same runtime. 

Take a look at these runtimes to understand why:

![](./images/common-function-runtimes.png)

From this we can see:

1. All algorithms take roughly the same time for `n=10`
2. Any algorithm with `n!` running time becomes useless for `n >= 20`
3. Algorithms whose running time is `2ⁿ` are useless for `n > 40`
4. Quadratic-time algorithms (`n²`) are usable up to `n = 10,000`, but deteriorate for larger inputs. `n > 1,000,000` is hopeless.
5. Linear-time and `nlogn` remain practical on inputs of one billion items.
6. Any `O(logn)` algorithm hardly sweats for any imaginable value of `n`.


Bottom line: even ignoring constant values, we get an excellent idea of performance for any given size of n.

### Function classes
The values of `g(n)` create distinct sets of classes we can use to describe most algorithms. Just a note, we say that a faster growing function *dominates* the slower growing one. So `n³` dominates `n²`. Sometimes written `n³ >> n²`.

The classes are:

1. *Constant functions*, `f(n) = 1`. No dependence on `n`.
2. *Logarithmic functions*, `f(n) = log n`. Shows up in binary search. Grows faster than constant, but still fairly slowly.
3. *Linear functions*, `f(n) = n`. Such functions measure the cost of looking at each item once (or twice, or ten times, remember constants aren't relevant) in an n-element array.
4. *Superlinear functions*, `f(n) = n log n`. Arise in algorithms like quicksort and mergesort.
5. *Quadractic functions*, `f(n) = n²`. Measure the cost of looking at most or all *pairs* of items in an n-element universe. Arise in insertion sort and selection sort.
6. *Cubic functions*, `f(n) = cⁿ` for any given constant `c > 1`. Functions like `2ⁿ` arise when enumerating all subsets of n items.
7. *Factorial functions*, `f(n) = n!`. Occur when generating all permutations or orderings of n items.

Using the dominance notation: `n! ≫ 2n ≫ n3 ≫ n2 ≫ nlogn≫ n≫ logn ≫ 1`.

## 2.4 Working with Big Oh
These various operations are common in simplifying functions to determine their complexity.

### Adding Functions
The sum of two functions is governed by the dominant one:

```
f (n) + g(n) → Θ(max(f (n), g(n)))
```

This is very useful in simplifying expressions. E.g. `n³ + n² + n + 1 = Θ(n3).` The intutition for this is that at least half of the bulk must come from the larger value. The dominant function will provide the larger value as `n → ∞`. Thus dropping the smaller functions has at most a factor of 1/2 reduction, which is just a multiplicative constant. Example, if `f(n) = O(n2)` and `g(n) = O(n2)`, then `f(n) + g(n) = O(n2)`.

### Multiplying Functions
Multiplication is like repeated addition. When multiplying by any constant, we know that the constant cannot affect its asymptotic behaviour, so `O(c · f(n)) → O(f(n))`.

But multipling a function with another function is different, as the two functions are increasing with `n`. So in general:

```
O(f (n)) · O(g(n)) → O(f (n) · g(n))
Ω(f (n)) · Ω(g(n)) → Ω(f (n) · g(n))
Θ(f (n)) · Θ(g(n)) → Θ(f (n) · g(n))
```

Also the Big Oh relationships are transitive, so if `f(n) = O(g(n)) and g(n) = O(h(n)), then f(n) = O(h(n))`.

## 2.5 Reasoning about Efficiency
Let's run through some examples of how to use Big Oh reasoning.

### Selection Sort
A selection sort in C:

```c

void selection_sort(item_type s[], int n) {
    int i, j; /* counters */
    int min; /* index of minimum */

    for (i = 0; i < n; i++) {
        min = i;
        for (j = i + 1; j < n; j++) {
            if (s[j] < s[min]) {
                min = j; }
        }
        swap(&s[i], &s[min]);
    }
}
```
It's a nested loop. The outer loop goes around `n` times, and the inner goes around `n - (i + 1)` times, where `i` is the index of the outer loop. So the number of steps is given by the series `T (n) = (n − 1) + (n − 2) + (n − 3) + ... + 2 + 1`. Each term in the series is the number of times for the inner loop, repeated `n` times.

One way to think of it is we're adding up `n - 1` terms, whose average value is `n/2`. So this gives: `T (n) ≈ (n − 1)n/2`. This becomes `(n² - n) / 2` which due to dominate addition, is `n² / 2`. As we ignore constants, it's therefore simply `O(n²)`.


### Insertion Sort
A basic rule of thumb is the worst-case running time follows from multiplying the largest number of times each nested loop can iterate. Take the insertion sort:

```c
for (i = 1; i < n; i++) {
    j = i;
    while ((j > 0) && (s[j] < s[j - 1])) {
        swap(&s[j], &s[j - 1]);
        j = j-1;
    }
}
```

So the question is how often does the inner loop go round? Well it has two termination conditions, one is to prevent out of bounds (`j > 0`) and the other is when it finds an element in its proper order. As worst-case seeks an upper bound, we ignore, and assume the inner loop always goes around `i` times. Since `i < n`, let's assume `n = i`. So therefore the inner loop goes `n` times. The outer loop also goes `n` times. Therefore it's worst-case is `O(n²)`.

### String Pattern Matching
Here's a basic pattern matching algorithm:

```c
int findmatch(char *p, char *t) {
  int i, j;       /* counters */
  int plen, tlen; /* string lengths */

  plen = strlen(p);
  tlen = strlen(t);

  for (i = 0; i <= (tlen - plen); i = i + 1) {
    j = 0;
    while ((j < plen) && (t[i + j] == p[j])) {
      j = j + 1;
    }
    if (j == plen) {
      return (i); /* location of the first match */
    }
  }

  return (-1); /* there is no match */
}
```

Again let's figure out the worst-case of the nested loops. The inner *while* goes around at most `m` times (where `m` is the number of characters in the substring). The outer loop goes around `n - m` times (as it makes no sense to go beyond `n - m` as there needs to be at least `m` characters to get a match). So the worst-case is: `O((n − m)(m + 2))`.

But we need to account of the time it takes to get the length of each string. As it has to count each character in the string it will be linear in the length of the string, so the worst-case is now `O(n + m + (n − m)(m + 2))`.

So now the job is to simplify using the rules of addition and multplication and when noticing when the terms satisfy the inequalities of the base function definitions.

So since `m + 2 = Θ(m)`, we get `O(n + m + (n − m)m)`. Multiplying this out yields `O(n + m + nm − m²)`.

In any interesting problem (this idea of only considering interesting values of `n` comes up a lot), `n >= m`, since the substring needs to be less than the text itself. This means that `n + m <= 2n`, which because of the Big Oh rules, means `n + m <= 2n = Θ(n)`. So replacing `n + m`: `O(n + nm − m²)`.

Next, we now that `n <= nm` as `m >= 1` for any intresting search pattern. Because in addition terms dominate, `n + nm` => `O(nm)`. We now have `O(nm - m²)`

Lastly, `-m²` is negative and therefore only lowers the value. Because we now `n > m` (pattern is less than the whole text), it implies `mn > m²`. It means then that `-m²` is not big enough to cancel out the `nm`. So finally, we have `O(nm)`.

### Matrix multiplication
Matrix multiplication involves 3 nested loops (for two-dimensional matrices). Because of that we should immediately think `O(n³)`. It can easily be derived by understanding that the number of steps in the multiplication (two matrices, one `X x Y`, the other `X x Z`) is calculated by knowing that the inner-most loop runs `z` times, the middle loop `y` times, and the outer `x` times. So `O(xyz)` The common case is where the dimensions of `x`, `y`, and `z` are all the same. So it can be written as `O(n³)`.

## 2.6 Summations
Summations often show up in algorithmic analysis due to the nature of computation (loops and such). So understanding how summations relate to complexity is key.

Summation formulae descrive the addition of an arbitrarily large set of numbers. The general formula:

![](./images/summation-formula.png)

For many simple summations, we can simplify. For example the sum of `n` ones, is just `n`, so we'd write that.

When `n` is even, the sum of the first `n = 2k` integers can be seen by pairing the `ith` and `(n - i + 1)th` integers. This ends up simplifying to `n(n+ 1)/2` (the same we saw in the selection sort), which we know simplifies to `O(n²)`.

Generally there are two base classes of summation formulae:

1. *Sum of a power of integers*: We know that the summation of the pairs evaluated in a selection sort is `n(n + 1)/2` = `O(n²)`. In general, for a sum of integers where the exponent is `p`, the complexity is `Θ(ⁿ⁺¹)`. Therefore the sume of sqaures is cubic, the sum of cubes is quartic.
2. *Sum of geometric progression*: In geomtric progressions, the index of the loop affects the exponent, that is the summation is `(aⁿ⁺¹ - 1)/(a - 1)`. How we interpret the sum depends on the base `a`. So if `a < 1`, then the complexity converges to a constant as `n -> infinity`. This is the "free lunch" of algorithmic analysis. If `a > 1` though, the sum rapidly grows with each new term -> `Θ(aⁿ⁺¹)` for `a > 1`.

## 2.7 Logarithms and Their Applications
A logarithm is simply an inverse exponential function. Saying `bٰˣ = y` is the same as saying `x = log_b(y)`. (Note: \_b means subscribt b but I can't easily write it).

Exponentials grow at a distressingly fast rate. Logarithms therefore grow refreshingly slow.

**Logarithms arise in any process where things are repeatedly halved**.

### Logarithms and Binary Search
Binary search is a good example of an `O(log n)` algorithm. You build the tree, and then on each iteration of the loop, you have the search space. Halving == logarithm. The number of steps is equal to the number of times we can halve `n` until we have only 1 left. By definition, this is exactly `log₂n`.

One of the most powerful ideas in algorithm design.

### Logarithms and Trees
If we consider a general tree, where each level of the tree multiplies the number of nodes by `d`. When `d=2`, we have a binary tree, so `n = 2ʰ`, where `h` is the height of the tree. So the height (and therefore the number of iterations until an algorithm exhausts the data structure) is `h = log₂n`.

But when `d` is larger it means that each level has more leaves. So each iteration has to consider more leaves, but discards more with each iteration. I think that's better but the book isn't clear.

**I believe that  log_3 is better than log_2, but I'm not sure. I should rewrite this when I've finished the book and am reviewing these notes.**

### Logarithms and Bits
To represent `n` different possibilities with bits, we need `n = 2ʷ`. So therefore `w` bits is needed: `w = log₂n` bits.

### Logarithms and Multiplication
Maybe feel this in when reviewing notes. By brain is too tired to remember multiplication rules for logs.

### Fast Exponentiation
The worst algorithm to compute `aⁿ` would just do `a x a x a ...` `n-1` times. But we can do better. We know that `n` is equal to itself times two. So we can use that to know that if `n` is even, then `aⁿ = (a^(n/2))²`. If it's odd, `aⁿ = a(a^(n/2)²)`. In either case, we've halved the size of the exponent (and therefore the amount of work) at the cost of two multiplications. So `O(lgn)` is enough to compute the value.

This generally leads us to *divide and conquer*. It always pays to divide a job as evenly as possible.

### Logarithms and Summations
Harmonic numbers are a special case of a sum of a power of integers, where the terms being summed are reciprocals of `i`. So `1 + 1 + 1/2 + 1/3 + 1/4` etc. The complexity is `θ(logn)`.

They are important as they arise in many algorithms are usually explain "where the log comes from" when one magically pops out of algebriac manipulation.

For example, analyzing quicksort requires the summation of `n * (sum of harmonic numbers up to n)`. Because the harmonic sequence is just `θ(logn)`, then the complexity of quicksort is `θ(nlogn)`.

### Properties of Logarithms
As we have seen `bˣ = y` is equivalent to `x = log_b(y)`. The `b` is the base of the logarithm. Three bases are important:

1. `Base b = 2`. The *binary logarithm*, usually denoted `lg x`. Arises when halving things. Most algorithmic applications imply binary logarithms.
2. `Base b = e`. The *natural logarithm*, using denoted `ln x`. Base is `2.71828...`. NOTE: He doesn't explain and I don't know where the number comes from.
3. `Base b = 10`. The *common logarithm*. Less common nowadays. Used before pocket calculators.

Important properties:
1. Log of the product is the sum of the logs of each: `logₐ(xy) = logₐ(x) + logₐ(y)`.
2. Easy to convert between bases: `logₐb = log_c(b) / logₐ(y)`. So converting from base a to c involves multiply by `log_c(a)`.

Two implications of these properties:
1. *The base of the logarithm has no real impact on the growth rate*: A big change in the base has little effect on the value. Because converting between bases involving multiplying, we can simply ignore bases in algorithmic analysis.
2. *Logarithms cut any function down to size*: The growth rate of the logarithm of any polynomial function is `O(lg n)`. This follow because:

```
logₐnᵇ = b · logₐn
```

which simplifies to `lgn` because we ignore the base `a` and the constant `b`. The effectiveness of binary search on a wide range of problems is a consequence of this observation. Note that a binary search on `n²` items is only twice as slow as `n` items.

**HEY**: this is important. The term binary search implies that its performance comes from the fact that each iteration splits the search space in half. So what if we were to split it not in half, but 1/3 and 2/3? So for a binaru tree of 1,000,000 items, the iterations is `log₂(1,000,000) = 20`. Taking 2/3 (worst case): `log_\[2/3\](1,000,000) = 35`. So not a lot.

**The main lesson:** changing the base of the log does not affect the asymptotic complexity. The effectiveness of binary search comes from its logarithmic running time, not the base of the log.


## 2.10 Advanced Analysis
These techniques are not used in the main text of the book and are considered optional. But to really understand some of the algorithms in the hitchikers guide part of the book, they will be useful.

**NOTE:** Come back to these and makes notes when I've finished the book.

## Chapter Notes
Little Oh notation: `f(n) = o(g(n))` if `g(n)` dominates `f(n)`. So asking for `o(n²)` means an algorithm that is better than quadratic in the worst case.
