---
title: "Schedule"
layout: splash
---

_I will strive to get slides uploaded before lecture_

_This schedule is tentative, based on previous offerings. We may change topics. We will discuss any changes throughout class and we will keep this schedule up to date_

_Unless explicitly mentioned, Readings will refer to Engineering a Compiler (EAC), see the references [page](https://siero.github.io/CSE110A-W26/references.html)_


### Module 1: Lexing

| Date             | Topic                       | Slides                                            |   Readings     |
|------------------|-----------------------------|---------------------------------------------------|----------------|
| Tue, 1/6         | Administrative (v5) 	     | [slides](./PDFS/C110-01A-Admin-v5.pdf)            | [Overview page](https://siero.github.io/CSE110A-W26/overview.html) |
| 				   | Introduction to Compilers 	 | [slides](./PDFS/C110-01B-Intro2Compilers1.pdf)    | |
| 				   | Journey into a Compiler  	 | [slides](./PDFS/C110-02A-Journey-Into-A-Compiler.pdf) | |
| Thu, 1/8         | Intro to Lexical Analysis   | [slides](./PDFS/C110-01C-Intro2Compilers2.pdf)    | EAC Chapter 1 |
| 				   | Lexer: Naive (version 3)    | [slides](./PDFS/C110-03A-lexers-naive-v3.pdf)     | |
|------------------|-----------------------------|---------------------------------------------------| |
| Tue, 1/13        | Lexer: Regular Expressions] | [slides](./PDFS/C110-04A-lexers-re-v2.pdf)        | [docs on REs](https://docs.python.org/3/howto/regex.html) |
| 				   | Lexer: EM-SOS-NG            | [slides](./PDFS/C110-05A-lexers-EM-SOS-NG.pdf)    | 
| Thu, 1/15        | Lexer: EM-SOS-NG Review     | [slides](./PDFS/C110-05B-REVIEW-RE-Naive-EM-SOS-NG.pdf) | |
|------------------|-----------------------------|---------------------------------------------------|----------------
| Tue, 1/20        | Lexer Actions               | [slides](./PDFS/C110-06A-lexer-actions-v3.pdf)	 | EAC Chap. 3.2, 3.3 (first half) |
| 				   | Quiz2 Warnings & Errors Review   | [slides](QUIZZES/Quiz-02-Warnings-Errors-Equiv.pdf) 	| | 
| 				   | Quiz3 Intro to Lexers Review     | [slides](QUIZZES/Quiz-03-Intro-Lexers.pdf) 				| | 
| 				   | Quiz3 Regular Expr Review        | [slides](QUIZZES/Quiz-04-REs.pdf) 						| | 
|-------------|-------------------|--------------------------------------------|

### Module 2: Parsing  

| Date        | Topic    | Slides |   Readings                                 | 
|-------------|-------------------|--------------------------------------------|  
| Thu, 1/22   | CFG,BNF,Derivations & Parse Trees | [slides](PDFS/C110-07A-CFG-Ambig-rev3.pdf) | EAC Chapt. 3.2, 3.3 (first half) |
| 			  | Precedence and Associativity (ver2) | [slides](PDFS/C110-08-PREC-ASSOC.pdf)        | |
|-------------|-------------------|--------------------------------------------|  
| Tue, 1/27   | Precedence and Associativity (ver2) | [slides](PDFS/C110-08-PREC-ASSOC.pdf)      ||
| Thu, 1/29   | TOP DOWN PARSING, LL(1), Recursive Descent | [slides](PDFS/C110-09-TOP-DOWN-PARSING.pdf) | [PLY Documentation](https://www.dabeaz.com/ply/ply.html) | |
|-------------|-------------------|--------------------------------------------|  
| Tue, 2/03   | TOP DOWN PARSING, LL(1), Recursive Descent (ver2) | [slides](PDFS/C110-09-TOP-DOWN-PARSING.pdf) | [left-indirect-recursion pdf](PDFS/LEFT-RECURSE-ELIM/left-rec-elim.pdf) | | 
|             | WARSHALL'S ALGORITHM | [slides](PDFS/C110-09A-WARSHALLS-ALGORITHM.pdf)   |  |
|             |                      | [Quiz-06 review](PDFS/Quiz-06-CFG-AMBIG-PREC.pdf) |  |
| Thu, 2/05   |  SCOPE               | [slides](PDFS/C110-10-SCOPE.pdf)                  |  |
|             | Bottom-Up Parsing    | [slides](PDFS/C110-11A-BOTTOM-UP-PARSING.pdf)     |  |
|             |                      | [Quiz-07 review](PDFS/Quiz-07-AMB-TOP-DOWN-REC-DESC.pdf) | |
|-------------|----------------------|---------------------------------------------------|--|  

### Module 3: Intermediate representations  

| Date        | Topic             | Slides |   Readings                        | 
|-------------|-------------------|--------------------------------------------|  
| Tue, 2/10   | MIDTERM REVIEW    | [slides](PDFS/C110A-MIDTERM-REVIEW.pdf) | | 
|             | Quiz AMB/TOP-DOWN/REC-DESC | [Quiz-07](PDFS/Quiz-07-AMB-TOP-DOWN-REC-DESC.pdf)   | |
| Thu, 2/12   | MIDTERM           | | | 
|-------------|-------------------|--------------------------------------------|  
| Tue, 2/17   | PostOrder Traversal Example | [slides](PDFS/C110-POSTORDER-TRAVERSAL-EXAMPLE-rev2.pdf) | | 
| 			  | IR & Abstract Syntax Trees (ASTs) | [slides](PDFS/C110-M3-01-AST.pdf)| EAC Chapter 5.1 |
| 			  | Review of Floating-Point | [slides](PDFS/C110-M3-01A-Floating-Point.pdf) | |
| Thu, 2/19   | Review Quiz 08 Symbols Tables |  |  | 
|             | AST and Type Systems | [slides](PDFS/C110-M3-02-AST-POT-TYPES.pdf) |  | 
|-------------|-------------------|--------------------------------------------|  
| Tue, 2/24   | AST,Type,Inference,Functions | [slides](PDFS/C110-M3-03-TYPES-FUNCTIONS.pdf)|   | 
|             | IR and 3-address code (rev2) | [slides](PDFS/C110-M3-04-FUNCTIONS-IR-SCOPE.pdf)| EAC Chapter 5.3 |
|-------------|-------------------|--------------------------------------------|  |
| Thu, 2/26   | IR and 3-address code (rev2) | [slides](PDFS/C110-M3-04-FUNCTIONS-IR-SCOPE.pdf)| |
|-------------|-------------------|--------------------------------------------|  

### Module 4: Optimization and Other Topics  

| Date        | Topic              | Slides                                          | Readings        |
|-------------|--------------------|-------------------------------------------------|                 |
| Tue, 3/03   | Quiz09-TYPE-IR-GEN | [slides](PDFS/Quiz-09-TYPE-AND-IR-GEN.pdf)      |                 |
|             | Optimizations      | [slides](PDFS/C110-M4-01A-OPTIMIZATIONS.pdf)    | EAC Chapter 8.1 |
|             | Basic Blocks, LVN  | [slides](PDFS/C110-M4-01B-BASIC-BLOCKS-LVN.pdf) |                 | 
| Thu, 3/05   | <TBD>              | <TBD>                                           |                 | 
|-------------|--------------------|-------------------------------------------------|-----------------|
| Tue, 3/10   | <TBD>              | <TBD>                                           | <TBD>           | 
| Thu, 3/12   | <TBD>              | <TBD>                                           | <TBD>           | 
|-------------|--------------------|-------------------------------------------------|-----------------| 

## Final  

