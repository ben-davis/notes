      

**SICP**

  

**Chapter 1.2**

-   Linear recursion requires an expansion of building up a chain of deferred operations and then a reversed contraction where the operations are performed.
	-   The amount of information required to track the process builds up linearly and so is called **linear recursion**.

-   Iteration on requires are the current variables required for the iterative process.
	-   In general an iterative process is one whose state can be summarized by a fixed number of state variables, together with a fixed rule that describes how the state variables should updated as the process movies from state to state and an optional end condition.
	-   If the number of steps required to compute a value N grows with linearly with N and so is called a linear iterative process.

-   Iterative processes only require those three variables. So if we were to stop and resume the process, we only need those variables.
-   But in recursive processes, we require all the information up until that point, managed in the interpreter, for us to know where in the process we are.

-   Procedures and processes are different
	-   Procedures are the definition of what should happen
	-   Process refers to what actually happens when the procedure is invoked

-   Recursive procedures are actually an iterative process as it can be modeled using only three state variables
	-   Functional languages are able to use tail-call recursion, an optimization that flattens the stack frame with each recursive call, meaning a recursive process is a constant one with no linearly expanding memory requirements.
	-   Whereas in non-functional, C for example, recursive processes cannot use tail-call optimization and so grow linearly with the number of recursions.
	-   I believe this because functional languages can make static guarantees about the nature of the recursion such that tail-call optimization can be guaranteed.
	-   But know that tail-call means that another function is called (and presumably returned) as the last line of a function. With Scheme it’s guaranteed it will use tail-call optimization.\
	-   Oh so it can be done in non-functional languages, but it requires a special optimization that requires knowing whether the call stack frame for the caller and the callee will be the same size, otherwise it’ll require resizing it each time its flattened which will be an expensive operation.
	-   In functional languages, iteration is modeled as tail-call recursion and so is very efficient in comparison to non-functional. 

  

**Chapter 1.3**

-   Higher order functions that compose other functions
-   Basic stuff

  

**Chapter 2.1**

-   Data abstraction is similar to procedural abstraction: hiding the implementation details by exposing a basic set of operations on which to operate on the data
	-   Essentially: selectors and constructors
	-   So identifying the types of data and the operations for manipulation is the main jam

-   A lot of lisp works by building up explicitly from some very basic functionality provided by the language
	-   For example, pairs (and the associated car and cdr) can be constructed using nothing by procedures
	-   The basic building block is `cons`, a built-in function that constructs objects in memory from two values or pointers to values

-   Data can be defined by some collection of constructors and selectors along with rules that restrict the behavior of those procedures

  

**Chapter 2.2**

-   Closure property of a procedure means the result of that procedure can then be passed back into the same procedure (this seems unrelated to function closures)
	-   Related to the mathematical concept of set closure: a set of elements is closed under operation if the operation produces an element of that set
	-   It is unrelated to concept of procedures that bind to free variables

-   **Sequences**
-   Using `cons` as a building block, sequences can easily be constructed by nesting cons.
	-   They are functionally equivalent to the builtin `list` function
	-   It must end with the built in `nil` to indicate the end of the list

-   Indexing into a list constructed from pairs is a simple recursive call using `car` and `car`.
	-   As lists end in `nil`, the built int `null?` allows testing for that
	-   Same with a length operation. Just a accumulator with a recursive call

-   The higher order map procedure allows us to easily express a new function of a list without worrying about the low level iteration required

-   **Trees**
	-   Constructing sequences from nested pairs naturally extends to sequences composed of sequences: trees.
		-   It's natural to use recursion to operate on trees - as each level of recursion operates on a deep level of the tree

- **Conventional Interfaces**
	-   Working with sequences and trees reveals a higher order abstract method in dealing with these data structures.
	-   That of enumerate, filter, map, reduce
		-   This is the essence of signal-processing
	-   Often procedures dealing with sequences could be structured in a way similar to signal-processing, but instead mingle each of the distinct operations into one big step. Refactoring into these discrete steps allows for more conceptual clarity.
	-   In programming, the "signal" of signal processing is actually a "list". Each stage in the process receives and returns a list. The exception of course is the enumerator, which produces the list, and the accumulator which reduces the list to a single output.
	-   So the only step that changes between dealing with a flat list and a tree is the enumeration step (with the tree flattening the leaves). This is clearly why Python's `enumerate` is named as such: it takes the complex dict data type and produces a list. 
	-   **Flat map**
		-   It's so common to deal with nested structures that needs be appended into a flat list, that a lot of languages provide 	a `flatMap` procedure that maps and accumulates in one step.
-   **Stratified Design** 
	-   The notion that a complex system should be structured as a sequence of levels tat described using a sequence of languages. Each level is constructed bu combining parts that are regarded as primitive at that level, and the parts constructed at each level are used as primitives at the next level. The language used at each level of a stratified design has primitives, means of combination, and means of abstraction appropriate at that level of detail.
		-   Computer engineering is stratified:
			-   Resistors and transistors -> and-gates / or-gates -> digital circuits -> processors, bus structures, memory structures -> computers -> distributed systems.

  

**Chapter 2.3**

-   A clever way to use the quote functionality of Lisp to support algebraic expressions. This is generally the idea of working with arbitrary symbols as data.
-   **Sets**
	-   There a quite a few ways we could build a set construct. All it has to do is satisfy the requirements of a set (union, intersection, uniqueness, etc).
	-   We could use a regular list, but that requires inefficient scanning to ensure things like uniqueness
	-   We could use an ordered set, reducing the complexity of guaranteeing uniqueness. It goes from O(n) to on average, O(n/2) (which is still O(n) I think).
		-   Intersection gives us more efficiencies as we can make a lot of assumptions about the rest of the set based on what's at the beginning
		-   Bear in mind this all requires that the items in a set are orderable. Presumably in Python that's why a set's items must be hashable.
	-   The better approach is to use a binary tree.
		-   Each node represents a single element and to the left, a smaller element, and to the right, a larger element. Either one can be empty.
		-   This gives us obvious efficiencies for testing if an element is in the set:
			-   We collapse the search space by half at each level (assuming the tree is "balanced")
			-   As the search space decreases by a fixed value with each step (0.5), the complexity is O(log n).
		-   Trees can be built using nested lists, with each item a list of 3 elements, the node, the left subtree and the right subtree.
		-   Adding to the set has the same complexity as searching for the existence of an element
		-   We must continually balance the tree to ensure efficiencies.
		-   Anything can be stored in a binary tree as long as each item is hashable by a provided hash function that generates a unique key.
-   **Huffman Trees**
	-   A method of variable-length data encoding that uses a binary tree.
	-   Variable-length data encoding improves storage efficiency by using a smaller encoding for elements that occur more frequently in the data to be encoded.
	-   Each leaf node contains a symbol to be encoded
	-   Each non-leaf node contains a set of all the symbols in all the leaves below the node along with a weighting for the combined frequency of those symbols.
	-   To encode something, we move down the tree, adding 0 if we go left, 1 if we go right.
	-   To decode we use the bit sequence to direct us down the tree towards a symbol.
	-   Generally with variable length encodings, we need a way to determine if we've reach the end of a bit sequence. Huffman trees to do this by making it clear when we've reach a leaf of the tree (because the set will have a single item in it).
		-   Prefix codes ensure that any given complete encoding is not the prefix of another encoding. That allows us to always know if we're at the end of a sequence. Presumably though the encoding methods needs a way to indicate when we've reach the end of a complete encoding. It's just that once that's happened you know for sure it's not complete and not the incomplete prefix of another encoding.
		-   Yeah prefix-free is what you want because it means it doesn't require redundant information. Huffman encoding happens to produce those based on how the encoding algorithm works.
	-   Huffman provided an algorithm to generate a tree that produces encodings with the fewest amount of bits. Essentially you just build the tree by starting with the least frequent symbols, merging weights at each node, until you reach the root.  The weights are used at each step to determine which nodes should be merged next. This produces a tree where the least frequent symbols are furthest from the root.
	
	
**Chapter 2.4**
- With a properly abstracted data model, representation becomes an implementation detail, entire separate from use.
- This can be thought of as the "principle of least commitment" - the choice of concrete data representation is left to the last possible moment.
- It can be taken further by "tagging" the data with its type and then building constructors and selectors that act appropriately based on type.
- The stripping of a type by a generic procedure and passing it untyped down to a method that deals with its concrete representation can be an important organizational strategy.
- This approach of checking and type and passing it to a procedure is called "dispatch on type".
	- An issue with this is that each generic function needs to know about each type and explicitly handle it - creating extra work any time a new type is added.
	- A method to further modularize is called "data-directed programming".
	- This essentially involves creating a table that contains the dispatch information. Generic methods use this to determine where to dispatch, but don't require specific knowledge of those types.
	- Adding a new type requires only modifying this dispatch table.

**Chapter 2.5**
- Types can be deeply nested, so that each level its passed down to, the outer type is stripped.
- Cross-type operations (adding ordinals to complex) can be handled either:
	- By explicitly defining operations for each permutation
	- By taking advantage of latent similarities in the type system that allows one type to easily be coerced into another.
- This can be taken further to consider the type system a hierarchy of types. So rather than requiring type relations as pairs, we can consider it a tree. This allows operations to work on distinct types that aren't coercable with each other, but can both be coerced to a common parent type.
	- This allows generic methods to test for the existence of a given operation for a given type and if it doesn't exist, coerce to the next type up in the tower (the name apparently for the child->parent->parent relationships) until it is defined. 
	- Although it's often not the case to have such neatly defined type relationships, with many types having many possible parents, and many parents have many possible children. This then requires that the generic methods have some complex logic for determine how and what to coerce to.


**Chapter 3.1**
- A properly designed system enables us to add new features or debug an old one by working only on a localized part of the system.
- Objects
	- We can view the world as populated by independent objects, each of which has a state that changes over time. An object "has state" if its behaviour is influenced by its history.
	- We characterize an objects state by one more state variables.
	- A system composed of objects rarely has truly independent objects. Usually objects with interconnected state dependencies are organized into sub systems.
	- A language needs to have an `assignment` operator in order to retain the name, but change its state, a requirement of an object who's state changes with time.
- Message-passing involves dispatchng on a method name to procedure that handles the message
- Functional programming
	- Programming without the use of any assignments
- Using assignment fundamentally changes the method of evaluation
	- The simplistic substitution model can be used in functional environments because all the procedures are essentially mathematical functions. So variables are synonymous with the values they represent - they are just names for values
	-  But with assignment that's no longer true. They are pointers to values in memory that can change.
-  With functional programming, two calls of the same procedure with the same inputs yields equal results. With assignment that is no longer a guarantee of the language.
	-  A language that supports "equals can be subsituted for equals" in an expression without changing the value of the expression is said to be `referentially transparent`. Referential transparency is violated by assignment. This makes it difficult to know when complexity can be reduced via substitution.
	-  The idea of "sameness" also gets more complex with assignment. "sameness" requires some prior knowledge of what "same" actually means. Is it a certain collection of properties remain equal? It's position in memory? The size of the entire object?
-  Without a change, we can regard a data object as precisely the totality of its pieces. A rational number for example is just numerator + denominator. But in the presence of change, we give an object an identity that is separate from it's pieces/data.
	-  A bank account is still the same bank account even if its balance changes.
	-  Two accounts aren't the same if they have equal balances.
	-  The issue stems from not the language, but our notion of the bank account as an "object" - distinct from its parts.
-  Programming that makes extensive use of assignment is called `imperative programming`.
-  Assignment introduces many complexities
	-  Order of operations/assignment becomes critically important as it forces us to ensure that the value of the variable is correct at the time its called.
	-  This issue simply doesn't exist in functional.
	-  Concurrent processes makes this even more complex.


**Chapter 3.2**
- Assignment forces us to change our model of execution. It requires a storage area for changeable variables called `environments`.
	- Composed of `frames`.
	- Each frame is a possible empty table of `bindings`, with associate variable names and their corresponding values.
	- A single frame may contain at most on binding of any variable.
	- Each frame has  a pointer to its `enclosing environment`, unless the frame is `global`.
	- The value of a variable with respect to an environment is the value given by the binding of the variable in the first frame in the environment that contains a binding for that variable. If no frame in the sequence specifies a binding for the variable, then the variable is send to be `unbound` in the environment.
	- Environments are what give meaning to programmatic expressions. Without an environment that defines `1` and `+`, `(+ 1 1)` doesn't mean anything. Therefore the root frame of an environment must contain bindings for those built-ins.
- Defining a procedure creates a new binding in first frame of the current environment.
	- The procedure itself is a pair of the procedure code itself (created via a lambda in lisp) and a pointer to its environment.
- Calling a procedure involves creating a new environment that begins with a frame in which the variables are created that binds each formal parameter of the procedure to the arguments provided.
	- The enclosing environment is the one pointed to by the frame in which the procedure was defined in.
- Assignment finds the binding of the variable in the current environment frame and changes its binding to indicate the new value.
	- It finds the first frame in the environment that contains a binding for that variable and modifies that frame.
- If a lambda is constructed and returned by a procedure, then its enclosing environment will the one that's constructed when calling the initial procedure. 
	- So if a you have a factory function that returns a lambda, when that lambda is called, a new frame will be created that binds the parameters to the args given.
	- That frame's enclosing environment will point to the one created by the factory function, which contains bindings for the parameters of the factory function.
	- ![[Screen Shot 2021-11-05 at 12.43.32 PM.png]]
	- Therefore the initial call to the factory function contains the frame that is the "place" that stores the bound state of the lambda function returned.
	- Each time the lambda is called, a new frame is created that finds its args. But it always points to the same enclosing environment constructed when the factory function was called.


**Chapter 3.3**
- In addition to constructors and selectors we now need to consider mutators.
- `mutable data objects` are objects with mutators
- Queues
	- A sequence in which objects are inserted at the rear and deleted from the front
	- Can be constructed using mutable pointers that point to the front and end of the queue, with each pointing to next in the chain,
- Note from Ben
	- Generally this book is making it clear that a complex system can be built by understanding its primitive elements (whether those are objects, relationships between objects, etc), then building up into new layers of abstraction. 
	- For example you can build a digital circuit simulator by defining wires, gates, and a system of signal propagation. Then you build half-adders on top, then full-adders, etc.
	- You could also build an arithmetic system by creating a series of constraint primitives that establish the relationship between a collection of symbols and an operator. These constraints would then build upon each other to model a constraint network. Then connectors hold values that are then connected a terminal of these constraints. The values could either be defined by a constant, provided by a user, or calculated via its connection to constraints and their further connectors.
		- Through the connectors and constraints, new values for connectors propagate through the network, eventually possibly leading to the resolution of various terminals of a connector.


**Chapter 3.4 - Concurrency**
- The root of complexities in concurrency stems from mutable state shared among processes.
- One method of controlling access to shared procedures is to create a serialized set of them whereby any invocation of a procedure in the set prohibits the invocation of any other procedure in the set.
- Serializers can be created via a mutex, a simple object that can either be acquired or released. When acquired no other objects can acquire it until relreased.
	- Crucially the acquire method of a mutex must be atomic - meaning that if that method finds the mutex to be free/false, then it must be set to true before any other process can test its value again.
	- The method to achieve atomicity is dependent on the system that's running the current processes.
- If two protected (meaning that require a lock on a shared mutex) processes start that both depend on each other being free at some point during the running of each function (a1 wants to call a2, but a2 wants to call a1), then we hit `deadlock`.
- The issue with serialized procedures is that there isn't always a single source of shared truth. Due to pipelining and cached memory, it can't be relied on.
- The basic phenomenon here is that synchronizing different processes, establishing shared state, or imposing an order on events requires communication among the processes. In essence, any notion of time in concurrency control must be intimately tied to communication.

**Chapter 3.5 - Streams**
- Streams is alternative way to model state.
- I think we think of how time is modeled in math: x(t). The function x doesn't consider time, but it can be evolved over time by an external process. Time is an input to the function.
- A stream is simple a sequence.
	- It can be a simple list but that doesn't reveal the true power of stream processing
	- An alternative is `delayed evaluation` which enabled us to represent very large or even infinite sequences as streams.
- Stream processing allows us to model systems that have state without assignment or mutable data.
- It also allows us to use the common stream processing procedures like map, filter, reduce without incurring the overhead of having to evaluate each step in its entirety.
	- Essentially delayed evaluation is Python generators	
- Streams and list are the same data abstraction, but they differ in their implementation and their efficiencies.
- Streams work through a construct called a `delay <exp>`.  It returns not the expression result, but a `delayed object` - a promise to evaluate `<exp>` at some later date.
	- A companion procedure is `force` which actually performs the evaluation.
- So to construct a stream (remember a list is a list of pairs: the value and a pointer to the next list that contains the rest), we only create the first `car` and the `cdr` is a promise to evaluate it.
- In general we can think of delayed evaluation as "demand-driven" programming whereby each stage in the stream process is activated only enough to satisfy the next stage.
	- We've decoupled the actual order of events in the computation from the apparent structure of our procedures. We write procedures as if the stream existed all at once when in reality it is computed on demand.
- `delay` is super simple: `(delay <exp>) = (lambda () <exp>)`.
- `force` is just `(define (force delayed-object) (delayed-object))`
	- We can extend this to memoize the result.

- **Infinite Streams**
	- We can easily define infinite streams by creating a function that recursively passes itself to a stream generator.
	- These can be treated like other streams, as long as there's some termination condition.
	
- **Implicit Streams**
	- Previous streams were created by creating "generator" procedures that computed the stream elements one by one.
	- We instead can just define the stream upfront as a stream with a value as its first value (car) and itself as the second. If the second element is generated via a procedure call, then it can be used to create things like the infinite stream of integers:
		- `“(define ones (cons-stream 1 ones))”
		- `“(define integers (cons-stream 1 (add-streams ones integers)))”`
		- It works because at any point enough of the stream has been generated so that we can feed it back into the definition of itself to create the next value.


- **More on streams**
	- We can take advantage of their nature to solve infinite precision / approximate problems like the square root of x or generating values of pi.
	- Expressing these approximate problems as streams allows us to take advantages of ways to accelerate the approximation without changing the initial generator. For example there's a way to approximate the sqrt faster invented by Euler if certain conditions of the series are true.


- Streams can used to directly model signal processing by taking the signals with respect to time as a time series of signals produced by a stream.

- **Streams and Delayed Evaluation**
	- The intepreters ability to deal with infinite streams that defined in terms of itself is due the `delay` function. Without it, it would need to fully evaluate the rest of the stream before advancing, which would be an infinite process.
	- So this is essentially streams with loops (very Gather Flow with loops).
	- The hidden aspect of the `delay` method is useful for lots of stream operations, but sometimes an argument to the delay method may itself need to be delayed until it's generated in a next step. So by lazy loading the attribute we can define it in another step. 
		- Note: not 100% sure I get this. But generally I think it's just this: if an argument is defined by another variable that's defined by the argument, then lazy loading needs to be used on the variable.


- **Normal-order evaluation**
	- One issue with the delayed approach is they often (like above) need to leak their abstraction. So that means that generally the entire app needs to be aware of normal procedures and delayed procedures. I suppose exactly like async/await, where async functions always need to be awaited anywhere they're used.

- **Modularity of functional programs**
	- Assignment allows us to encapsulate state - hiding the implementation details. This comes at the cost of having to deal with mutation.
	- Functional programs can provide equivalent modularity without the use of assignment. This is because instead of storing state we can model the change of state as a time series. So with a random number generator that requires its previous input to generate the next value, with imperative we had to store that value as a mutable variable and keep updating it and passing it to the rand-update proc.
		- With stream processing we just define the stream as a stream of successive calls to that rand-update method with a recursive definition of itself:
			- `“(define random-numbers
					  (cons-stream random-init	
	    	              (stream-map rand-update random-numbers)))”`
						  
						  
- **A functional programming view of time**
	- Temporal behaviour with assignment is modeled by mutable computational objects.
	- We model the changing quantity using a stream that represents the time history of successive states.
		- We represent time explicitly using streams so that we decouple time in our simulated world from the sequence of events that take place during evaluation.
	- So a bank account is a good example:
		- On the imperative side, we model an account as a collection of procedures and state whereby a procedure emulates the "user transaction" and mutates local state.
		- On the functional side, we see the user's transactions as a stream of transaction requests passed through a stateless functional procedure. The end balance is entirely a function of that stream of requests.
			- From a user's POV they're the same, as the temporal aspect that we expect from reality is imposed on the functional system through the user's temporal existence. So although a stateless system that actually has state seems paradoxical, its resolved by the fact that an external temporal process (the user) applies the time dimension to the system.
	- Although concurrency is far easier to deal with in the functional system, it does still creep in. If two users have a stream of interactions that represent the changing state, the question of how to merge those streams is still important and difficult to manage. But IMO at least it's contained to a single area of the system, namely the I/O.
	- But the object system is far more intuitive as it matches with our normal understanding of reality.
		- In reality the concurrency issues with computational objects are resolved through the cause and effect logic imposed by the limit of lights speed.
	- Both system have tradeoffs. It seems one that unifies them doesn't exist (the book claims this but it's old - is there a system?)

**Chapter 4 - Metalinguistic Abstraction**
- `evaluator (or interpreter)` determines the meaning of an expression in a programming language. Its a procedure that, when applied to an expression of the language, performs the actions required to evaluate that expression.
- We can regard almost any program as the evaluator of some language. A language defined by the constructs we have created to model our computation.
- All that is required are: primitives, means of combination, and means of abstraction.
- Lisp is particularly good at implementing evaluators of other languages due to its support for symbolic expressions.

- **Chapter 4.1 - The metacircular evaluator**
	- An evaluator that's written in the language that its evaluated in is said to be metacircular
	- The essence of evaluation:
		- To evaluate a combination (a compound expression), evaluate the subexpressions and then apply the value of the operator subexpression to the values of the operand subexpression.
		- To apply a compouned procedure to a set of arguments, evaluate the body of the procedure in a new environment. To construct this environment, extend the environment part of the procedure object by a frame in which the formal parameters of the procedure are bound to the arguments to which the procedure is applied.
	- The basic cycle is: evaluate expressions in an environment by reducing them to procedures to be applied to arguments specified by that environment, which in turn are reduced to new expressions to be evaluated in new environments, and so on, until we get down to symbols, whose values are looked up in the environment, and to primitive procedures, which are applied directly.
		- `eval` and `apply` embody this two-part cycle.
	- `eval`
		- Takes as arguments an expression and an environment
		- Essentially a switch statement (or data dispatch) on the syntactic type of the expression
		- Each expression type is tested for abstractly, not based on the underlying representation. Each has a predicate that tests for it and an abstract means for selecting its parts.
		- The abstract syntax makes it easy to see how we can change the syntax of the language by using the same evaluator, but with a different collection of syntax procedures.
	- `apply`
		- Takes two arguments, a procedure and a list of arguments.
            - Directly calls primitive procedures using the primitive.
			- Handles calling into procedures and `eval`ing them.
			- Constructs new environments for each invocation of a procedure.
				- Extends the base env
	- Some expressions (can't be bothered to write them all out)
		- `if`
			- Handled by `eval` the if and switching based on the result
	- Expressions that are syntactic transformations are called dervied expressions.
	- It doesn't really matter how we chose to represent things like functions. Could easily just be a list (chain of pairs) of the procedure name, the args and the body. As long as we have it well-defined, it's all good.
	- A nice way to think about evaluator: data as programs
		- If we think of a program a description of a machine, then an evaluator is a description of a machine that takes as input another description of a machine.
		- This makes it a universal machine. It mimics are other machines when they are described as lisp programs (but could be anything).
		- Imagine the same for an analog circuit. A signal would need to be the description of another circuit. Crazy complex. Whereas a program evaluator is actually quite simple.
	- From the POV of a programmer, expressions are abstract constructs of the language. The the evaluator of the language, the expressions are simply a, for example, list of symbols that is to be manipulated according to some well-defined set of rules.
	- Often procedures or variables can refer to each other in their definitions. So we need a way out of circular references.
		- One way is to scan the body of a procedure and "hoist" the variable definitions to the top of the procedure, with their implementation defined later.
	- We can increase performance of `eval` by splitting the syntactic analysis and the execution. This allows to to memoize the syntax and only execute with a changing environment. 


- **Chapter 4.2 - Lazy evaluation**
	- Normal order vs applicative order
		- Applicative: all arguments to a procedure are evaluated when the procedure is applied.
		- Normal: Delays evaluation of a procedure arguments until the actual argument values are needed.
		- This is what allows languages like Python to do `a = b or c` without evaluating `c`.
			- Actually I discovered that the boolean "short circuiting" is a form of non-strict evaluation but I don't believe its an example of normal order evaluation.
			- Normal/applicative applies strictly to when arguments to a procedure are evaluated.
		- In normal order, we say that procedure is non-strict in its arguments.
			- To do this, `thunks` are what delayed arguments are transformed into.
			- The processing of evaluating thunks is called `forcing`.


- **Chapter 4.3 - Non-deterministic computing**
	- Skipping this as it's interesting, but pretty obscure.

- - **Chapter 4.4 - Logic programming**
	- Most expression-oriented languages rely on the fact that the expression used to define a value of a function is also used as a means of computing that value. This gives rise to an input-output style of expressions.
	- But you can instead build language constructs that describes relations rather than expressions, resulting in a system without input/output or a defined order of operations and instead only relations between objects that can be evaluated in some way.
		- Programs written in terms of relationships can provide definite answers to "what is" style questions without the programmer having to specify their answers.
		- Where procedural languages have `procedures`, logic languages have `rules`. The "how to" part of a logic language is provided by the language in the form of "queries".
		- Based on relatively few rules, the evaluator could allow us to ask many different queries. 
		- Because of this databases make a good case for logic programming.
		- SQL is not a logic language, but rather a query language. Really a domain-specific language.
		- But the way you build queries and combine them is similar in spirit to SQL.
	- Implementation
		- Requires a pattern matcher to determine whether some datum fits a pattern.
		- Unification
			- A generalization of a pattern matcher where the datum and the pattern may contain variables.
			- In moves through the variables to determine whether the pattern should match by binding frames that match (I'm guessing frames means objects that satisfy the current expanded query). This is the complex part of a logic language. It must build up deduction strategies that allow it to determine when a rule should apply. 
	- Not strictly mathematical logic as logic programming allows the programmer to determine the order of evaluation of the goal specified by their query. Mathematical logic contains no such thing.
		- It's essentially a procedurally interpretable subset of mathematical logic.
	- It's very easy to express rules & queries that result in infinite deductions.


**Chapter 5 - Computing with register machines**
- The underlying control structures of a language like lisp are executed in terms of a step-by-step process that sequentially executes instructions that manipulate the contents of a fixed set of storage elements called `registers`.
	- A typical register-machine instruction applies a primitive operation to the contents of some registers and assigns the results to another register.
- A register machine has:
	- data paths (registers and operations)
	- controllers (sequences opereations)
- Controllers can be thought of as a marble rolling through the data paths, following paths indicated by conditions and setting register values when it arrives at a operation that indicates it should do so.
	- There are lots details about the operations and primitives that are offered by the machine language beyond conditions and setting/retrieving registers. Thinks like `goto` to jump to a new instruction, `continue` that allows a value to be stored in the controller sequence that allows execution to continue at a designated entry point - this allows for subroutines.
	- We then need a stack to allows multiple of these entry point registers to be created to support recursion, along with operations to place and receive register values in the stack.
		- This allows a single data path to be reused to compute recursive controller processes.
- Assemblers take the controller expressions (with its labels and other human-friendly constructs) and converts it to statements directly readable by the machine.
	- The begins by separating labels from the instructions.
	- It builds a list of instructions to perform and a table with pointers from labels to instructions.
	- Then it augments the instruction list by replacing the instructions with execution procedures (that correspond to the direct machine instructions).
	- Basically we're just doing the same thing as the basic evaluator: taking data is a code, expressions as primitives, combination, and abstraction, and executing it at a lower level.

- **Chapter 5.3 - Storage Allocation and Garbage Collection**
	- In order to execute the execution procedures generated by the assembler, we need to provide the means for memory storage.
	- Garbage collection allows us to provide a language with the illusion of infinite memory by continually freeing memory no longer in use.
	- Generally we store data in memory locations with addresses and manipulate them in registers.
	- The data structure `vector` can be used to model computer memory, specifically list-structured memory. It's a sequential list that supports constant lookup.
	- **GC**
		- Basic method: stop-and-copy
		- Divide memory into working and free memory.
		- Implemented using a root pointer that points to beginning of all accessible memory.
			- a free pointer that points to the beginning of free memory. It's incremented as we allocate. GC is run when we free pointer is at the end of accessible memory.

- **Chapter 5.4- Explicit-control evaluator**
	- Complex control constructs could be implemented in assembly using the basic register instructions described above. But to make assembly languages more terse, we can introduce small abstractions that allow for things like pre-defined register labels for the results of expressions, registers for evaluating combinators, etc.
		- Essentially provided lower-level constructs for the higher level `eval`. So how to eval an expression using primitive branching and register operators.
	- An evaluator that can execute a procedure without requiring increasing storage as the procedure continues to call itself is called a tail-recursive evaluator.
		- Basically intermediate results can be thrown away as it doesn't need to walk back up the tree to continue executing the rest of the procedure. So the expressions at the end of a procedure is the final state of the procedure so it can be bound as arguments in the next frame with the previous being thrown away.
	- Each lower level of abstraction requires understanding and resolving issues not apparent at higher levels of abstraction. For example the saving of intermediate expression required in evaluating an `if`statement.

- **Chapter 5.5 - Compliation**
	- In order for our evaluator machine to run anywhere, to be universal, it requires a register machine that itself is universal., with a collection of registers and operations that constitute an efficient and convenient universal set of data paths.
	- Programs written in machine language are sequences of instructions that use the machine's data paths.
	- Two commons strategies for bridging the gap between higher-level languages and register-machine languages:
		- Interpretation. An interpreter is written in the native language of the machine and configures the machine to execute programs written in a language that may differ from the native machine.
			- It traverses the source program as a data structures, and as it does so simulates the intended behaviour of the source program by calling appropriate primitive subroutines of the source language (implemented in native language)
			- Basically the interpreted languages provides a bunch of native primitives that the interpreter then invokes when processing the source program.
		- Compliation
			- Translates a source program into an equivalent program (called the object program) written in the machine's native language.
		- So an interpreter executes a program invoking machine code language primitives as it executes it, whereas a compiler translates source into machine language, which is then executed directly by the machine.
		- A compiler works very similarly to an interpreter, but instead of executing register instructions that would evaluate an expression, we instead accumulate the machine instructions into a sequence that becomes the object code.
		- Compiled code is not only faster due to not needing the analyze expressions constantly (it just has the instructions for the operator and the operands), but also in being able to intelligently reduce unnecessary stack operations by knowing when a given register value is actually needed.
			- We can also skip the search to figure out which frame will contain a given variable and instead access it directly.
	- Compiling an expression requires three arguments
		1. The expression
		2. A target: which register to put the value of the expression
		3. A linkage descriptor: describes how the code resulting from the compliation of the expression should processed when it has finished its execution.
			1. Continue to the next instruction in sequence
			2. Return from the procedure being compiled
			3. Jump to a named entry point
	- Based on the linkage descriptor, we provide the appropriate machine code.

	- **Lexical addressing**
		- Replacing complex variable lookup with a specific frame number and displacement number that tells the machine precisely where the variables value can be found.
		- The compiler can do this by maintaining a compile-time environment which keeps track of variables and their positions in which frames. There will be no values as they are produced as the program runs, but their locations are known.


- **Generally, an interpreter raises the machine to the level of the user program, a compiler lowers the user program to the level of the machine language**