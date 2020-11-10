---
layout: page
title: Simple C Compiler w/ Flex and Bison
description: An intermediate code generator for a subset of C language by Back-Patching
img: /assets/img/simple_compiler.png
importance: 2
github: https://github.com/mushfek/compiler-lab-assignments/tree/main/Exp-06
---

#### Objectives
The front end of a compiler translates a source program into an independent intermediate code, then the back end of the compiler uses this intermediate code to generate the target code (which can be understood by the machine) which facilitates a lot of optimizations.
For this reason intermediate code generation is a very important concept for compiler designing. This project aims at developing an intermediate code generator for the following constructs:
1. Arithmetic expressions
2. Nested if-else statements
3. Nested while loop
4. Nested for loop
5. Nested do-while loop
6. Nested compound statements
7. Arrays

Full source code is available in [Github](https://github.com/mushfek/compiler-lab-assignments/tree/main/Exp-06) and a detailed report can be found [here](https://github.com/mushfek/compiler-lab-assignments/blob/main/Exp-06/Exp-06.docx).

#### Steps to run
1. lex s_parser.l
2. yacc s_parser.y
3. gcc y.tab.c -ll -ly -w
4. ./a.out test1.c

#### Future Enhancements
1. Procedures
2. Recursive Procedures
3. Arrays
4. Structures
5. Pointers