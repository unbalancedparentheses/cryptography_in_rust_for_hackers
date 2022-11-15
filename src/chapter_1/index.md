# Theoretical Computer Science

### Computability: An Introduction

Mathematicians have been behind in a machine that performs automatic calculations for a long time. William Schickard built the first record of a machine that performed automated sums in 1623. This machine is a milestone in the process of building a computer machine. Through time, other famous mathematicians made other contributions to obtaining a computer machine: Blaise Pascal, Gottfried Leibniz, Charles Babbage, Ada Lovelace, George Boole, and Herman Hollerith, among others.

Nevertheless, it was not until 1913, when Bertran Russell and Alfred North Whitehead published in Cambridge "*Principia Mathematica*" that pushed mathematicians the path to demonstrate that all the mathematical structures could be determined from a set of fundamental logic axioms. Russel and Whitehead's intent was to provide a Philosophical foundation of mathematics to derive most of the mathematical knowledge based on a set of principles or axioms.

In 1900, David Hilber proposed a set of 23 challenges for mathematicians. Problem 10 in Hilber's list was entitled "*Determination of the solvability of a Diophantine Equation*." It was the first reference to the **Entscheidungsproblem (decision problem)**. The decision problem consists of finding an algorithm that inputs a first-order logic statement and determining if it is universally valid by answering "Yes" or "No" as output. First-order logic is a formal system that allows reason and describes properties of objects, i.e., "*All students are people*" or "*John is a student*", to list but a few. When we talk about "universally valid" we refer to every structure that satisfies the axioms - An axiom is "a statement or principle that is generally accepted to be true." (American Dictionary). In First-order logic exists a theorem called "the completeness theorem" that affirms that a statement is universally valid if and only if it can be deduced from axioms. In other words, the decision problem could be interpreted as requesting an algorithm to determine whether a given statement can be proven from the axioms using rules of logic.

Later in 1931, Godel published his work about formally undecidable propositions, i.e., "*what I am saying is false*." He proved that this kind of proposition could not be true, nor could it be false. In both cases, we obtain logical contradictions (example required?).

Finally, in the 1930s, three different formalizations of computability were formalized:

* Kurt Godel and Jacques Herbrand formally described the **General Recursive Functions**.
* Alonso Chuch created **Lambda Calculus**.
* Alan Turing proposed an abstract machine model, nowadays known as **Turing Machines** or Computing Machine, consisting of a limitless tape divided into small squares and headers which scan them by reading/writing symbols that could realize "computation from some inputs by manipulating symbols on the tape."

Later, Turing, Chuch, and Kleen demonstrated that Turning machines, lambda calculus, and general recursive functions are all "classes of computable functions: a function is lambda computable if and only if is Turing computable, and if and only if is general recursive." 

# Alan Turing
### Decision Problem

Going back to the 10th Hilber's problem, he wants "*a process with a finite number of steps that determines the validity or satisfiability of any given expression*" (Hilber and Ackermann 1928). In other words, he wants a program that checks whether or not an algorithm does what it says to do about a specific problem, just having as output yes or no. It is what the Entscheidungsproblem (decision problem) means.
Hopcroft, Motwani, and Ullman proposed a demonstration of the inexistence of such kind of algorithm. Our approach will be as simple as possible.

We will start accepting the existence of an Algorithm \\( A \\) (hello tester) that takes a program \\( P \\)  and an Input \\( I \\) and can establish, for example, that P prints "Hello", just having as output Yes or No. We say that the problem is decidable if \\( A \\) has as output yes or not. The idea is to demonstrate that \\( A \\) does not exist. 

(poner grafico)  

We will make changes to A to obtain another algorithm \\( A_2 \\), on which we will demonstrate its inexistence. Because A exists, we can perform one change on it. Then, we create another algorithm called \\( A_1 \\), which takes a program P and an Input I, which will print as output yes if P prints "hello" and 
"hello" do not.

(poner grafico)
 
Taking into account that our interest is in programs and not in the ir inputs, we will change \\( A_1 \\) into \\( A_2 \\) by:

*  \\( A_2 \\) only accepts as input P, not P and I.
*  Use \\( A_2 \\)  input of \\( A_2 \\).

(poner grafico)

So, now we use \\( A_2 \\) as input of \\( A_2 \\), en the first output obtained is yes, which implies that \\( A_2 \\), in the box  (grafico)  has established that \\( A_2 \\) is a program that prints "hello" ... but the output then should be yes. So, we have arrived at an absurd, \\( A_2 \\) does not exist, and as a consequence, \\( A_1 \\) can not exist, and finally, A does not exist.

This demonstration consists in reduce one problem into another. 

# Turing Machines

The intent behind decision problem theory is not only to establish this kind of problem but also to provide programmers with a reference or a mechanism for determining what a programmer can or cannot accomplish through programming [hopcroft]. For this reason, it is necessary to have a tool that empowers us to install discussions about untractable and undecidable problems. An intractable problem can be decidable but requires a significant amount of time to be solved. These kinds of problems are hard to handle than undecidable ones.

Turing proposed an abstract machine model. This machine is not only another thing but a finite automaton with a limitless tape divided into cells called squares. A scanner or head can read or write data on each square because the tape goes under the scanner, which performs these two operations. Additionally, a unit control keeps the machine's state and decides the next operation. Turing describes his model in the article published in 1936, entitled "*On computable numbers, with an application on the Entscheindungsproblems*."

(Turing machine image required) 

This machine works as follows the initial machine's Input is a string of symbols with a finite length. This string is placed on the tape. The rest of the tape's squares are initialized with a special symbol called "blank ."The scanner's position is on the leftmost square on the input string. When it is running, the machine performs one step at a time. This step consists in:

1. Changhe machine's state. 
2. Write a new symbol in the square under the scanner; it always has to replace whatever symbol is on the square. 
3. move the scanner to the left or the right.


Once we have an informal notion about what a turin machine is. Let us formalize a definition. There are many books where a good definition can be found; we will use Hopcroft-Ullman's book.

A Turing Machine is a 7-tuple  \\( M=(Q, \Sigma , \Gamma, \delta ,  q_0 , B,  q_a \\)) where:

* \\( Q \\) is a set of finite states or m-configuration of the machine's Unit Control.
i
* \\( \Sigma \\)  is a set of finite input symbols called an alphabet.  

* \\( \Gamma \\)  The complete set of tape's symbols, where \\( b \in \Gamma \\) and \\( \Sigma  \subseteq   \Gamma \\) .

* \\( \delta \\) is the transition function. It is defined as \\(  Q  \times \Gamma  \rightarrow   Q  \times \Gamma \times \{ L, R \} \\). A transition can be seen as \\( \delta (q_0,s) \rightarrow (q_1,y,d) \\) where:
    * \\( q_1 \\) is the next state.
	* \\( y \\) is a symbol \\( \in \Gamma \\) which is written below the machine's head.
	* \\( d \\) is a movement direction, Left or Right, that can perform the machine's head.

* \\( q_0 \\) is the initial state.
* \\( B \\) is the symbol that represents the blank. 
* \\(q_a\\)  is the set of acceptance states. 


#### Universal Turing Machine

We have seen, so far, that a Turing Machine is an abstract machine model consisting of a tape divided into cells. Each cell initially contains a blank symbol. Before it starts to run, the tape must be loaded with a specific input. Finally, the machine starts running and can achieve three final states:

* accept the Input
* reject the Input 
* continue running on a loop.

A Universal Turing Machine U is a T.M.  U  that accepts as Input a tuple:
<M, I>, where M is a Turing machine description, and I is its Input. In other words, a Turing machine U reads the tape, interprets all the states of M, and prints M's output should print. 

In his article, Turing describes the Universal Machine as follows:
      
"It is possible to invent a single machine which can be used to compute any computable sequence. If this machine M is  supplied with a tape on the beginning of which is written the S.D of some computing machine . At, then It will compute the same sequence as it. " 

S. D. stands for Standard Description.


In other words, a universal Turing machine is an interpreter, a general-purpose computer!


#### Halting Problem
The Halting Problem

Due to the existence of a Universal Turing Machine, we can propose HALT=<M, x> where M is a T. M., and M halt with the input x, UTM input. The halting problem is based on whether a U.T.M can determine if M will halt having x as input in a finite number of steps. 
The proof of HALT can be achieved by assuming that HALT is computable. In that case, exist a UTM U defined as:

\\[
\begin{equation}
 	   U_i(M_i,x)=
 	   \begin{cases}
  	       halt & \text{if $M_i$ halts on $x$ } \\\\ \\\\  
  	       loop & \text{if $M_i$ loops on $x$}
 	   \end{cases}
\end{equation}
\\]
As we did in the Entscheidungsproblem, we will make some changes on \\( U_i \\) to obtain \\( U_d \\), it will receive as input \\( M_i, D.N. of M_i \\), where D.N. of \\( M_i \\) stands for Description Number. This change makes that  \\( U_i \\) input is a T.M. and its description:

\\[
    \begin{equation}
 	U_d(M_i,D.N of M_i)=
 	 \begin{cases}
      halt & \text{if $M_i$ does not halts on D.N of M_i } \\\\ \\\\
  	  loop & \text{if $M_i$ alts on D.N of $M_i$ $x$}
\end{cases}
	\end{equation}
\\]

Note:  each Turing machine has a unique Description Number.

Here is where the contradiction arises if \\( M_i \\) is \\( U_d \\):
 if \\( U_d \\) does not halt it means that \\( U_d \\)  halts.
if \\( U_d \\) halts it means that \\( U_d \\) does no halts.
There not exists a Turing Machin capable of determining whether or not a Turing machine will halt on an Input x.

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

- Gödel, Kurt. "Über formal unentscheidbare Sätze der Principia Mathematica und verwandter Systeme I." Monatshefte für mathematik und physik 38.1 (1931): 173-198.

- Turing, Alan Mathison. "On computable numbers, with an application to the Entscheidungsproblem. A correction." Proceedings of the London Mathematical Society 2.1 (1938): 544-546.

- Alonzo, Church. "An unsolvable problem of elementary number theory". American journal of mathematics, vol. 58 (1936), pp. 345–363.
