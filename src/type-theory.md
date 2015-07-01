---
title: Type Theory and Constructive Logic
...

## A Brief Introduction to Type Theory

Type theory was [invented by Bertrand Russell](http://www.jstor.org/stable/2369948) to solve traditional logic's problems with self-referential statements (consider: "this statement is false").
There are a few different kinds of type theory [[1]](https://en.wikipedia.org/wiki/Typed_lambda_calculus) [[2]](https://en.wikipedia.org/wiki/Intuitionistic_type_theory), but we'll be focusing on intuitionistic type theory ([developed by Martin-Löf](https://intuitionistic.files.wordpress.com/2010/07/martin-lof-tt.pdf)). 
Homotopy Type Theory is based on intuitionistic type theory, so that's what we'll talk about here. 
In combination with the [Curry-Howard correpsondence](https://en.wikipedia.org/wiki/Curry%E2%80%93Howard_correspondence), we can understand types as logical propsitions and members of those types as proofs of those propositions. 
Because this isn't vague enough yet, intuitionistic type theory actually has a few branches. 
The core is Intensional Type Theory (ITT), which contains rules for derivations, but leaves two big unanswered questions: what exactly is a 'type', and how can we tell whether or not two statements are equivalent.

One extension of ITT is [Extensional Type Theory](http://www.cse.chalmers.se/research/group/logic/book/book.pdf) (ETT). 
ETT says that a type is just a set of values that that type can have.
Proving statements equal is done by computing both of them and determining equality between the resulting values.
While this is a straightforward way of defining these issues that is intuitive from a programming perspective, it makes type deduction equivalent to solving the halting problem. 
Computationally, this isn't a problem (plenty of software can't be proven to halt, yet works perfectly fine), but for people wanting provably correct code, this isn't ideal. 

Enter Homotopy Type Theory! 
In HoTT, types are 'spaces' (not the topological ones, in case you were thinking that). 
Don't worry too much about what that exactly means; we'll get an intuition for how they look as we go along. 
HoTT views proofs as paths between two points in this space. 
If two paths in a space have the same endpoints and can be deformed into each other, those paths are 'homotopically equivalent' (also known as 'equivalent up to homotopy' or 'equivalent modulo homotopy'). 
This definition of homotopical equivalence comes (surprisingly!) from homotopy theory. 
The 'type theory' part of HoTT says that if two paths are homotopically equivalent, the corresponding proofs in type theory are equal. 
This is often stated as 'equivalence is equality', which seems redundant, but notice that 'equivalence' and 'equality' have very specific meanings here!

In case you're wondering what one path being deformed into another looks like, here's a gif:
![Continuous Deformation](img/HomotopySmall.gif) <!--- source: https://en.wikipedia.org/wiki/File:HomotopySmall.gif -->

The dots are two points in the space, the dotted lines are two paths, and the solid line shows a continuous deformation from one path to another. 

- Basics of type theory: how to express statements in types, etc
- Non-capturing substitution
- Constructive logic
- What is a 'type'? (unspecified/set/???)
