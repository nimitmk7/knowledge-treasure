Program Synthesizer: Program that takes a specification and outputs a program that satisfies it. 

What specification?
- A logical formula
- Another equivalent program
- Input/Output examples
- Natural Language description

## Synthesis from Logical Specifications 
- When does synthesis make sense? 
	- When it's ==easier to describe what the program should do rather than how it should do it==.
- Unlike natural language generation, in program synthesis ==we can often test if a program satisfies the specification==. 

**First attempt**: Logical formula describing what the program does

### Example: Sorting
-  How would you logically specify a sorting algorithm?
	- First attempt: 
		- Suppose the algorithm takes a list A and outputs a list B.
		- Key property: B should be sorted
			- For all i < length(B), $B[i] <= B[i+1]$
		- ![[Pasted image 20241011100433.png]]
		- So the synthesizer generated code that satisfies the specification we gave, but of course not the program we wanted. **OOPS!**
	- Second attempt:
		- Suppose the algorithm takes a list A and outputs a list B.
		- Key property 1: B should be sorted
			- For all i < length(B), $B[i] <= B[i+1]$
		- Key property #2: B should be a permutation of A
			- length(B) = length(A)
			- For all $B[i]$ there should exist some $A[j]$ such that $A[j]$ = $B[i]$
			- ![[Pasted image 20241011100759.png]]

### Discussion
- Note that the problem is non-trivial: the specification says very little about how!
	- Not just a translation problem
- But logical specifications are ==hard to read==, ==hard to write==, ==hard to check==.

## Synthesis from examples
- Another kind of specification: input/output examples.

### Example: Sorting
- How do you specify sorting a list? 
	- Given $[3, 2, 1, 0]$, should return $[0, 1, 2, 3]$ 
	- Given $[1, 4, 2]$, should return $[1, 2, 4]$
	- Given $[9]$, should return $[9]$
	- Synthesizer output:
		- ![[Pasted image 20241011102119.png]]

### Ambiguity Problem
