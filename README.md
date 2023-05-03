Download Link: https://assignmentchef.com/product/solved-cs161-homework-4-satisfiability-sat-problem
<br>
In this assignment you will write a LISP program to solve the <em>satisfiability </em>(SAT) problem. In particular, given a propositional sentence ∆ in <em>conjunctive normal form </em>(CNF), you will decide whether ∆ is satisfiable. A propositional sentence ∆ is in CNF if and only if it is a conjunction of <em>clauses</em>, where a clause is a disjunction of literals (a literal is a variable or its negation). For instances, the following sentence is a CNF with three clauses, which is defined over binary variables X, Y, and Z.

<h3>∆ = (<em>X </em>∨¬<em>Y </em>∨ <em>Z</em>) ∧¬<em>X </em>∧ (¬<em>Y </em>∨¬<em>Z</em>)</h3>

A CNF is <em>satisfiable </em>if there exists a complete variable assignment that satisfies each clause of the CNF (otherwise, it is unsatisfiable). In this case, the corresponding variable assignment is called a model of the CNF. For example, CNF ∆ is satisfiable as the complete variable assignment {<em>X </em>= <em>False</em>, <em>Y </em>= <em>False</em>, <em>Z </em>= <em>True</em>} is a model of CNF ∆ (note that there is another model of ∆).

Indeed, one can easily formulate a SAT problem as a constraint satisfaction problem (CSP). Basically, variables of the CNF will correspond to the variables of the CSP, each having a domain with two values (i.e., True and False), and each clause of the CNF will represent a constraint of the CSP.Then a solution to the CSP will correspond to a model of the CNF, and vice versa. In this assignment, your task is to treat the SAT as a CSP and solve it using backtracking search while detecting states that violate constraints. You are encouraged to use some of the techniques discussed in class for improving the performance of backtracking search, including variable ordering and forward checking in particular. To do that, you will represent a CNF in LISP as follows:

<ul>

 <li>A variable is an integer indexing from 1 to <em>n</em>, where <em>n </em>is the number of variables of the CNF. So, a positive literal can be represented by a positive integer. Respectively, a negative literal can be represented by a negative integer. (e.g., Positive literal of variable 2 is 2, and the negative literal of variable 2 is -2).</li>

 <li>A clause is a list of integers. For example, the list(1 -2 3) represents the clause (1∨¬2∨3). NOte that a unit clause is also represented asa list, e.g., the list (−2) represents the unit clause -2.</li>

 <li>A CNF is a list of lists. For example, the list ((1 -2 3) (-1) (-2 -3)) represents CNF ∆ above, where variables <em>X</em>, <em>Y </em>and <em>Z </em>are indexed by 1, 2, and 3, respectively.</li>

</ul>

Given this representation, your top-level function must have the following signature:

<em>(defun sat? (n delta)…)</em>

where <em>n </em>is an integer and delta is a CNF defined over <em>n </em>variables. The function <em>sat<sub>f</sub>un </em>returns a list of n integers, representing a model of delta, if delta is satisfiable, otherwise it returns <em>NIL</em>, e.g.,

<em>sat? 3 ’((1 -2 3) (-1) (-2 -3))) returns (-1 -2 3) sat? 1 ’((1) (-1))) returns NIL</em>

When a CNF has more than one model, sat? can return any one of the models, where the order of literals in the model can be arbitrary.

2        Reading CNF files

A common and easy way to represent CNFs is through DIMACS file format (see below for details). To help you test your program with bigger CNFs, we provide you with LISP code that can parse CNF files in DIMACS format (see the file parse cnf.lsp). In particular, given such a CNF file as input, the function parse-cnf will return a list of two elements: the number of variables of the CNF and its LISP representation, respectively. It should then be trivial to call the sat? function. We also provide you with some CNF files in DIMACS format, where the CNFs become harder as the number of variables increases (see the folder cnfs/ coming with the assignment).

<strong>DIMACS format: </strong>Consider the following CNF which is over binary variables 1, 2, and 3:

(1 ∨¬2 ∨ 3) ∧−1 ∧ (¬2 ∨¬3) ∧ 3

This CNF can be represented using DIMACS format as follows

<ol>

 <li>c this is a comment line</li>

 <li>p cnf 3 4</li>

 <li>1 -2 3 0</li>

 <li>-1 0</li>

 <li>-2 -3 0</li>

 <li>3 0</li>

</ol>

In general, a CNF file may start with a number of comment lines, where each such line must begin with lowercase c. Next, we must have what is known as the ”problem line”, which begins with lowercase p, followed by cnf followed by the number of variables <em>n</em>, followed by the number of clauses <em>m</em>. This followed by clause lines. A clause line is defined by listing clause literals one by one, where a negative literal is preceded by a – sign. The end of a clause is defined by 0. Note that variables are indexed from 1 to <em>n</em>. There can also be comments in between clause lines.

5       Honor Code

Remember that you <strong>cannot </strong>use any outside references for this or any assignment. However, you are allowed (and encourage) to experiment with actual SAT solver (which can be found online). Obtaining test problems and testing SAT solver from the Internet are acceptable. It is <strong>not acceptable </strong>to copy solution for any function you have to write. In general, any idea that is not originally yours must be attributed to the appropriate sources. If you have any questions, please contact the TA.