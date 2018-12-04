

## Artificial Intelligence Final Review

#### 1. Agents

#### 2. Logical Agents : Propositional Logic

- What is Entailment
  - Entailment by truth table : statement a and b, if in all var assignments where a is true,  b is always true, then a entails b.
- Entailment by inference : 
  - TODO : Know the basic formulas so you can use that search method.
    - notice the three dash equal and its transformation
  - Know what is Logical Equivalence
  - then by orive tgat KB ^ not a is unsatisfiable
  - Resolution: a rule of inference defined for conjunctive Normal Form 
  - Q : some strategies
    - Unit resolution ......
  - Rule-Based Reasoning : Horn Clauses
    - what does the reversed 'T' symbol mean
    - Forward Chaining
    - Backward Chaining
  - 

#### 3. Informed and uninformed search

- Informed search
- uninformed search
  - BFS : FIFO queue
  - DFS : 

#### 4. Game Search : Adversarial Search

- Minimax Search
- alpha-beta pruning
  - how to understand alpha and beta value when doing searching?
  - only way is to look at the tutorial on course website

#### 5. Constraint Satisfaction Problems

- binary CSP  : each constraint  relates at most two variables
- Q : can we have a not binary CSP?
- Q :what is LP method in Continuous CSP
- Q: can a higher order function be decomposed to binary constraints?
  - yres we can reduce a higher order to binary constraints
- represemt a higher order CSP using a graph
- give examples of real-world CSP problems. List 3
- What is Naive Search Formulation and how to carry that?
  - why this is naive? IT searches something beyond actual need. We can figure this out by indicating how many possible children can a node at depth l have (n - l) d where d is the domain size of each variable
- Why we can use Backtraking
  - Each step assign to a fixed variable since the order of assignment doesn't matter at all. Search faail when there is no legal assignments to a variable
- Strategies to improve backtracking search
  - Choose the variable with the fewest legal values
  - Choose the variable which have the most connections with unassigned vars when 1 doesn't work
  - After deciding which variable to assign, we have to decide which value to assign. We choose the one that rules out the fewest values in the remaining variables.
  - Q : which one should we choose at last, we should the one kills least or the one left least.
- Forward Checking
  -  keep track of remaining legal values for  unassigned variables, terminate when **any** variable has no legal values
  - Q :  I think here should be one not any.
  - This is called Constraint Propagation
- Arc Consistency 
  - AC is one of the Constraint Propagation. Every time we do a forward checking, we will check if the current graph is arch consistent. If any var lose a value, all its neighbor needs to be checked if they can be used
  - Must act exactly as the algorithm behaves
- Other Issues
  - Tree structure
  - Nearly Tree Structure
  - Iterative

#### 6  First Order Logic 

- Components of FOL
  - Constants 
  - Predicate Symbols
  - Functions
  - Connectives, Equality, Quantifiers
- Assertions
  - expressed by formulas
- Term 
  - A logical expression that refers to an object
- Atomic Sentences
  - All the above  are not important, I see, all non-sense
- Gloabal Quantifier : using => rather than ^ in most cases
- Existential Quantifier : using ^ rather than => in most cases
- Know how to transform a formula to another formula
  - Exist x Any y is not equal to Any y Exist x
  - Know the Quantifier Duality : neg form
  - Equality : three dash means two formula equal and has same truth table, two dashes means refer to the same object
- TODO : do some exercise using FOL to express some sentence
- FOL  Knowledge Build base
  - Q : How to do the query given the database

#### 7. FOL Inference

- Propositionalisation
  - Replace all Any to (and) and Exist to (or`)
  - then ground the kb and query and run inference for propositional logic
- Lifted Inference : (Unification)
  - making 2 formulas the same by finding appropriate substitution for variables
  - Unify with a ground  means substitute var with grounds
  - Its like a>b and b>c so a >c
- Need to know how to unification : Look at the example
- Q ： what is the use of unification?
- TODO : Using Resolution
- Inference in FOL
  - convert KB ^ not a to CNF
  - then apply resolution to this

####  8. Planning

#### 9. Probability and  Bayesian Networks

##### Probability

- Elements
  - random variable
  - atom : assignment of value to variable
  - sentence : boolean combinations of atoms
  - possible world : a comolete assignment of values to every variable
  - event or proposition : a set of possible worlds
  - atomic event : a single possible world
  - Q : is there  a mistake in slide 12 ? 

- conditional probability
  - basic formula
  - chain rule
- joint probablity
- Prior Probability
- inference by enumeration
  - what is hidden variable
  - normalization
- independence
- conditional independence
- Baye's Rule
  - Q : got a typo in this slide
  - what is the naive Baye's Rule
  - TODO ： derive the conditonal independence formula in wikipedia (equal form)

##### Bayesian Network

- Calculate how many rows in total for a Bayesian Network
- Calculate the global semantics
- Calculate the local semantics
- Inference Tasks
- inference by enumeration
- inference using irrelevant variables
- Temporal Probability Models: Markov Chain

#### 10. Decision Tree learning

#### 11. Neural Network

#### 13. HW4

#### 14. Problem Prediction

1. Bayesian Networks Probability Calculation
2. Decision Tree Learning Calculation
3. Markov Chain Calculation
4. Minimax Value and alpha-beta pruning
5. Constraint Satisfaction Problem
6. Arc Consistency Algorithm
7. Back Propagation and perceptron calculation





