Bytecode is an intermediate representation of code that is compact, platform-independent, and ==designed for execution by a virtual machine or interpreter.== 

It is not directly human-readable like source code, nor is it directly executable by hardware like machine code. Bytecode typically consists of numeric instructions, constants, and references that encode the result of compiling high-level programming languages such as Java or Python. 

In simple words, 

> *“Byte code is an instruction set for a virtual Machine.”*

# **What Byte code Looks Like**

Byte code is commonly represented as a sequence of instructions with opcodes (operation codes) followed by optional operands. 

Each opcode specifies an action, while operands provide additional details required for execution. 

For example, Java bytecode looks like this:

```text
0: iconst_2      // Push the integer constant 2 onto the stack
1: istore_1      // Store the top value from the stack into local variable 1
2: iload_1       // Load the value from local variable 1 onto the stack
3: sipush 1000   // Push the short integer constant 1000 onto the stack
6: if_icmpge 44  // Compare two integers and jump to instruction 44 if greater or equal

```

This format resembles assembly language for the Java Virtual Machine (JVM), where each instruction is represented by a mnemonic and its corresponding operand values.

# Why have Bytecode ?
- Managing complexity when you’re writing an interpreter
# Types of Operations

## CPython 
1. Stack Manipulation: Allow you to manipulate the stack. 
	1. E.g. Load and Store, 
2. Flow Control: Allow you to manipulate the instruction pointer
	1. E.g. Jump
3. Arithmetic: Perform Mathematical Operations
	1. E.g. Add, Shift, Minus, Divide
4. Pythonic: Operations specific to Python interpreter
	1. E.g. Creating a tuple, set, loading a closure

Any program you can express in Python can be expressed in bytecode, but vice versa is not true. 


# References
1. https://youtu.be/0IzXcjHs-P8?si=C5l4nl31YEH9ykZp 
2. 