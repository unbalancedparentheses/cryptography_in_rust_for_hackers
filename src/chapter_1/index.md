# Theoretical Computer Science

### Computability: An Introduction

Mathematicians have been behind in a machine that performs automatic calculations for a long time. William Schickard built the first record of a machine that performed automated sums in 1623. This machine is a milestone in the process of building a computer machine. Through time, other famous mathematicians made other contributions to obtaining a computer machine: Blaise Pascal, Gottfried Leibniz, Charles Babbage, Ada Lovelace, George Boole, and Herman Hollerith, among others.

Nevertheless, it was not until 1913, when Bertran Russell and Alfred North Whitehead published in Cambridge "Principia Mathematica" that pushed mathematicians the path to demonstrate that all the mathematical structures could be determined from a set of fundamental logic axioms. Russel and Whitehead's intent was to provide a Philosophical foundation of mathematics to derive most of the mathematical knowledge based on a set of principles or axioms.

In 1900, David Hilber proposed a set of 23 challenges for mathematicians. Problem 10 in Hilber's list was entitled "Determination of the solvability of a Diophantine Equation." It was the first reference to the Entscheidungsproblem (decision problem). The decision problem consists of finding an algorithm that inputs a first-order logic statement and determining if it is universally valid by answering "yes" or "no" as output. First-order logic is a formal system that allows reason and describes properties of objects, i.e., "All students are people" or "John is a student", to list but a few. When we talk about "universally valid" we refer to every structure that satisfies the axioms - An axiom is "a statement or principle that is generally accepted to be true    ." American Dictionary. In First-order logic exists a theorem called "the completeness theorem" that affirms that a statement is universally valid if and only if it can be deduced from axioms. In other words, the decision problem could be interpreted as requesting an algorithm to determine whether a given statement can be proven from the axioms using rules of logic.

Later in 1931, Godel published his work about formally undecidable propositions, i.e., "what I am saying is false." He proved that this kind of proposition could not be true, nor could it be false. In both cases, we obtain logical contradictions (example required?).

Finally, in the 1930s, three different formalizations of computability were formalized:

* Kurt Godel and Jacques Herbrand formally described the General Recursive Functions.
* Alonso Chuch created lambda Calculus.
* Alan Turing proposed an abstract machine model, nowadays known as Turing Machines or Computing Machine, consisting of a limitless tape divided into small squares and headers which scan them by reading/writing symbols that could realize "computation from some inputs by manipulating symbols on the tape."

Later, Turing, Chuch, and Kleen demonstrated that Turning machines, lambda calculus, and general recursive functions are all "classes of computable functions: a function is lambda computable if and only if is Turing computable, and if and only if is general recursive." 

### Alan Turing
#### Decision Problem
Going back to the 10th Hilber's problem, he wants "a process with a finite number of steps that determines the validity or satisfiability of any given expression" (Hilber and Ackermann 1928). In other words, he wants a program that checks whether or not an algorithm does what it says to do about a specific problem, just having as output yes or no. It is what the Entscheidungsproblem (decision problem) means.
Hopcroft, Motwani, and Ullman proposed a demonstration of the inexistence of such kind of algorithm. Our approach will be as simple as possible.

We will start accepting the existence of an Algorithm A (hello tester) that takes a program P and an Input I and can establish, for example, that P prints "Hello", just having as output Yes or no. We say that the problem is decidable if  A has as output yes or not. The idea is to demonstrate that A does not exist. (poner grafico)  
We will make changes to A to obtain another algorithm A2, on which we will demonstrate its inexistence. Because A exists, we can perform one change on it. Then, we create another algorithm called A1, which takes a program P and an Input I, which will print as output yes if P prints "hello" and "hello" do not. 
Taking into account that our interest is in programs and not in the ir inputs, we will change A1 into A2 by:
A2 only accepts as input P, not P and I.
Use A2  input of A2.
So, now we use A2 as input of A2, en the first output obtained is yes, which implies that A2, in the box  (grafico)  has established that A2 is a program that prints "hello" ... but the output then should be yes. So, we have arrived at an absurd, A2 does not exist, and as a consequence, A1 can not exist, and finally, A does not exist.

This demonstration consists in reduce one problem into another. 

#### Turing Machines

#### Halting Problem
  

### Alonso Church: Lambda Calculus
 
 
### Von Neuman: Universal Stored-programmed computing machine
 
 
### Algorithm Complexity
#### Asymptotic Analysis 
#### Big-O Notation
 
### Intractable Problems
#### P Problems 
#### NP Complete Problems
 
### References
 
- Copeland, B. Jack, ed. "The essential turing". Clarendon Press, 2004.

- Petzold, Charles."The annotated Turing: a guided tour through Alan Turing's historic paper on computability and the Turing machine". Wiley Publishing, 2008.

-Gödel, Kurt. "Über formal unentscheidbare Sätze der Principia Mathematica und verwandter Systeme I." Monatshefte für mathematik und physik 38.1 (    1931): 173-198.

- Turing, Alan Mathison. "On computable numbers, with an application to the Entscheidungsproblem. A correction." Proceedings of the London Mathematical Society 2.1 (1938): 544-546.

- Alonzo, Church. "An unsolvable problem of elementary number theory". American journal of mathematics, vol. 58 (1936), pp. 345–363.
