<<<<<<< HEAD
# Algorithm Analysis, Spring 2026 (Chapman CPSC-406)

## Topics
The course consists of three (roughly equal) parts on:
Models of computation, algorithm complexity, probabilistic algorithms

## General Information
- Class: CPSC 406 Algorithm Analysis 
- Instructor: [Jonathan Weinberger](https://sites.google.com/view/jonathanweinberger), [jweinberger@chapman.edu](mailto:jweinberger@chapman.edu)
- Lectures: TuTh 1:00 PM -- 2:15 PM  KC 153, 2:30 PM -- 3:45 PM BH 105
- Office Hours: Tu 2:20 PM -- 3:50 Swenson N305, Th 2:20 PM -- 3:50 Swenson N205


## Quick links

* [Syllabus](syllabus-long.md)
* [Report](report.md) and [Feedback](https://hackmd.io/e3Uh-NQ9RJaTmBropvq7kQ?view)
* [Git best practices](git-best-practices.md)
* [Top 12 Git commands every developer must know](https://github.blog/developer-skills/github/top-12-git-commands-every-developer-must-know/)
* [Resources for participation](https://hackmd.io/@jweinberger/H1gkQyZQ1l)

## Resources
* [J. E. Hopcroft, R. Motwani, J. D. Ullman: Introduction to Automata Theory, Languages, and Computation (ITALC)](https://archive.org/details/hopcroft-motwani-ullman-introduction-to-automata-theory-languages-and-computations-3rd-edition/)
* [T. H. Cormen, C. E. Leiserson, R. L. Rivest, C. Stein: Introduction to Algorithms](https://github.com/calvint/AlgorithmsOneProblems/blob/master/Algorithms/Thomas%20H.%20Cormen,%20Charles%20E.%20Leiserson,%20Ronald%20L.%20Rivest,%20Clifford%20Stein%20Introduction%20to%20Algorithms,%20Third%20Edition%20%202009.pdf)
* [N. Deo: Graph Theory with Applications to Engineering and Computer Science (GTAECS)](https://archive.org/details/GraphTheoryWithApplicationsToEngineeringAndComputerScience/)



## Schedule
The outline is as follows (subject to change):

| Week  | Topic                                      |
|-------|--------------------------------------------|
| W1-4    | Part 1: Models of computation |
| W5-9   | Part 2: Algorithm complexity  |
| W10-14    |  Part 3: Probabilistic algorithms                 |


## Week by week

* L1.1: Introduction and puzzle.
* L1.2: Deterministic finite automata (DFAs): [Motivation](https://hackmd.io/dF2SKHuSThWcmtb55siHNA), [definition](https://hackmd.io/B12ryxVpRZWjGOlXU93-Yw)
* L2.1: [DFA processing and design](https://hackmd.io/8bklG4CjRdic3jRSBzWjQQ?view)
* L2.2: [Formal languages](https://hackmd.io/B12ryxVpRZWjGOlXU93-Yw)
* L3.1: [Warmup: Constructions on DFAs](https://hackmd.io/T67z5ztqTkussTfC8g5DNg?view), [operations on automata](https://hackmd.io/08qs4k0hT8-HeojeDRHyqA?view), [Extended transition functions](https://hackmd.io/7PQmGrkfTquROvEdM28FvA?view), [NFAs](https://hackmd.io/dDaFPm6hRUCpxr9OTEVJUw?view)




=======
# Programming Assignment 2 (PA2): A General SAT Solver

This assignment generalizes PA1 from Horn formulas to arbitrary formulas. You will implement a SAT
solver for general formulas in conjunctive normal form (CNF). This paves the way for PA3 in which you will use the SAT solver to implement a Sudoko solver.

Work in groups of 2-4 people. Comment and document your code thoroughly.

## File

Implement your solution in:

```text
sat_solver.py
```

## Background

For a given a boolean formula $\varphi(x_1,\ldots,x_n)$ we call an assignment
$$\mathfrak J \colon \;\; x_1 \mapsto b_1, \;\; x_2 \mapsto b_2, \;\; \ldots \;\;x_n \mapsto b_n$$ 
(with $b_1,\ldots,b_n \in \{0,1\}$) **satisfying** if $\mathfrak J(\varphi) = 1$.

The **SAT problem** asks: is a boolean formula (in CNF) satisfiable? It was the first problem to be shown [NP-complete](https://en.wikipedia.org/wiki/NP-completeness).

One can show that every boolean formula is equivalent to one in *conjunctive normal form (CNF)*, i.e., a conjunction of disjunctions of literals:

$$\varphi \equiv (l_{11} \lor \ldots \lor l_{1k_{1}}) \land  (l_{21} \lor \ldots \lor l_{2k_{2}}) \land \ldots \land (l_{n1} \lor \ldots \lor l_{nk_{n}})$$
A *literal* $l_{ij}$ is either a variable $x$ or the negation $\lnot x$ of a variable $x$. The subformulas $C_{m} = l_{m1} \lor \ldots \lor l_{mk_{m}}$ are called *clauses*. A *unit clause* is a clause $C$ consisting only of one literal (i.e. $C = x$ or $C = \lnot x$).

A single clause can be represented by a set of its literals, hence every CNF (hence every formula) can be represented by a set of sets. E.g., the CNF
$$\varphi(x_1,x_2) = (x_1 \lor \lnot x_2) \land \lnot x_1 $$
can be represented by:
$$\{ \{x_1, \lnot x_2\}, \{\lnot x_1\} \}$$

**Remark:** For a more precise definition of CNFs in terms of context-free grammars see [Wikipedia](https://en.wikipedia.org/wiki/Conjunctive_normal_form). This in particular explains why a binary disjunction like $p \land q$ is itself also a *conjunctive* normal form!

In this
assignment, however, formulas are already given in CNF.

We represent variables by positive integers:

- `1` means variable $x_1$
- `-1` means $\lnot x_1$
- `[1, -2, 3]` means $x_1 \lor \lnot x_2 \lor x_3$
- `[[1, -2], [-1]]` means $(x_1\lor\lnot x_2)\land(\lnot x_1)$

An assignment is a dictionary mapping variable numbers to Boolean values:

```python
{1: True, 2: False}
```

## Required Function

Implement:

```python
def sat_solve(clauses, assignment):
    ...
```

Input:

- `clauses`: a list of clauses, where each clause is a list of integer literals
- `assignment`: a dictionary mapping variables to truth values. In the tests
  below, the tester calls your solver with the empty assignment `{}`.

Output:

- if the formula is satisfiable extending `assignment`, return a satisfying
  assignment dictionary
- otherwise return `None`

## Hints

Unit clauses force choices. For example, `[3]` forces `3: True`, and `[-4]`
forces `4: False`.

A reasonable implementation strategy is the following:

1. Simplify the formula under the current assignment.
2. Apply unit propagation.
3. If all clauses are satisfied, return the assignment.
4. If an empty clause is produced, return `None`.
5. Choose an unassigned variable and recursively try `True` and `False`.

## Examples

Your implementation should produce results equivalent to e.g. the following (the tests only check for satisfiable/unsatisfiable, not for a concrete assignment):

| Input | Output |
| --- | --- |
| `[[1, 2], [-1], [-2], [-1, -2]]` | `None` |
| `[[1, 2], [-1], [2]]` | `{1: False, 2: True}` |
| `[[1], [2], [3], [-4], [-5], [-6]]` | `{1: True, 2: True, 3: True, 4: False, 5: False, 6: False}` |
| `[[1, -2], [-1, 2], [3], [-3, 4], [-4]]` | `None` |

You can run the solver file directly from the shell:

```bash
python3 reference_sat_solver.py "[[1, 2], [-1], [2]]"
```

Your solution may be tested on additional inputs.

## Grading (15 points)

Grading breakdown:

- **9 points**: tests pass
- **2 points**: output contract and CLI usage
- **2 point**: maintaining directory and file structure
- **2 point**: code quality (readable implementation, clear variable naming, no hard-coding)

You are encouraged to add your own test cases. I maintain the right to use additional test cases for grading.

## Run Checks

From the `pa2` directory:

```bash
python3 tests/test_pa2.py --solution sat_solver.py
```
>>>>>>> ea24edb (PA2)
