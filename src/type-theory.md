---
title: Type Theory and Constructive Logic
...

## A Brief Introduction to Type Theory

Type theory was [invented by Bertrand Russell](http://www.jstor.org/stable/2369948) to solve traditional logic's problems with self-referential statements (consider: "this statement is false").
There are a few different kinds of type theory [[1]](https://en.wikipedia.org/wiki/Typed_lambda_calculus) [[2]](https://en.wikipedia.org/wiki/Intuitionistic_type_theory).
Homotopy Type Theory is based on intuitionistic type theory ([developed by Martin-Löf](https://intuitionistic.files.wordpress.com/2010/07/martin-lof-tt.pdf)), so that's what we'll talk about here. 
In combination with the [Curry-Howard correpsondence](https://en.wikipedia.org/wiki/Curry%E2%80%93Howard_correspondence), we can understand types as logical propsitions and members of those types as proofs of those propositions. 
Because this isn't vague enough yet, intuitionistic type theory actually has a few branches. 
The core is Intensional Type Theory (ITT), which contains rules for derivations, but leaves two big unanswered questions: what exactly is a 'type', and how can we tell whether or not two statements are equivalent.

One extension of ITT is [Extensional Type Theory](http://www.cse.chalmers.se/research/group/logic/book/book.pdf) (ETT). 
ETT says that a type is just a set of values that that type can have.
Proving statements equal is done by computing both of them and determining equality between the resulting values.
While this is a straightforward way of defining these issues that is intuitive from a programming perspective, it makes type deduction equivalent to solving the halting problem. 
Computationally, this isn't a problem (plenty of software can't be proven to halt, yet works perfectly fine), but for people wanting provably correct code, this isn't ideal. 

<!--- Are we getting way too far ahead of ourselves here? -->

Enter Homotopy Type Theory! 
In HoTT, types are 'spaces' (not the topological ones, in case you were thinking that). 
Don't worry too much about what that exactly means right now; we'll get an intuition for how they look as we go along. 
HoTT views proofs as paths between two points in this space. 
If two paths in a space have the same endpoints and can be deformed into each other, those paths are 'homotopically equivalent' (also known as 'equivalent up to homotopy' or 'equivalent modulo homotopy'). 
This definition of homotopical equivalence comes (surprisingly!) from homotopy theory. 
The 'type theory' part of HoTT says that if two paths are homotopically equivalent, the corresponding proofs in type theory are equal. 
This is often stated as 'equivalence is equality', which seems redundant, but notice that 'equivalence' and 'equality' have very specific meanings here!

In case you're wondering what one path being deformed into another looks like, here's a gif:
![Continuous Deformation](img/HomotopySmall.gif) <!--- source: https://en.wikipedia.org/wiki/File:HomotopySmall.gif -->

The dots are two points in the space, the dotted lines are two paths, and the solid line shows a continuous deformation from one path to another. 

## Type Theory Syntax

Okay, enough about homotopies and decidability. 
Let's talk about how type theory works a bit. 
The big idea of type theory is to set up a bunch of rules that describe the type of each statement. 
We can use these rules to check that we haven't made any mistakes in a chain of statements. 
(If at this point you're thinking that 'chain of statements' sounds an awful lot like 'proof', good for you.)

Type theory introduces three main concepts: Terms, Types, and Contexts. 
Terms have types; contexts store the information about which term has which type. 

So, what are terms? 
They come in a few forms:

Syntax                          Name
-------                         -----
$\mathsf{t}$                    term
$\mathsf{x}$                    variable
$\lambda\mathsf{x:T.t}$         lambda abstraction
$\mathsf{t\ t}$                 application
$\mathsf{t}\times \mathsf{t}$   product
$\mathsf{fst\ t}$               first projection
$\mathsf{snd\ t}$               second projection
$\mathsf{left\ t}$              sum with left value
$\mathsf{right\ t}$             sum with right value
$\mathsf{case\ left\ of\ t}$    left sum term
$\mathsf{case\ right\ of\ t}$   right sum term
$\mathsf{1}$                    unit

Lambda abstractions are anonymous functions that take one argument.
If you want a function that takes multiple arguments, you have to build it from several lambdas, like $\lambda\mathsf{x.}\lambda\mathsf{y.x y}$, which applies its second argument to its first. 
(This is called [function currying](https://en.wikipedia.org/wiki/Currying).)
A product term is like a struct or class with two members, $\mathsf{fst}$ and $\mathsf{snd}$.
Sum terms are a tagged union, where the value is either $\mathsf{left}$ or $\mathsf{right}$. 
Rust's `Result` type and Haskell's `Either` type are both examples of sum types. 

You may have noticed the $\mathsf{x:T}$ in our lambda abstraction form.
Here, $\mathsf{:}$ is the typing operator and $\mathsf{T}$ is a type. 
We can apply $\mathsf{:}$ to any form to specify its type explicitly. 

Types also have specific forms:

Syntax                                 Name
------                                 -----
$\mathsf{T}$                           type
$\mathsf{1}$                           unit type
$\mathsf{0}$                           void type
$\mathsf{T_1}\rightarrow\mathsf{T_2}$  function type
$\mathsf{T_1}\times\mathsf{T_2}$       product type
$\mathsf{T_1}+\mathsf{T_2}$            sum type

We'll try our hardest to avoid ambiguity between the similar forms that sum and product terms and types take. 
Unit and void types are not commonly seen in practice, but notice that Optional/Nullable things have type $\mathsf{T + 1}$ (either a value of type $\mathsf{T}$ or null/none). 
You may have noticed that void has a type, but no associated term. 
This is intentional: there is no way to make something with void type. 

Finally, let's talk a little bit about contexts. 
First, the forms:

Syntax                 Name
------                 ----
$\Gamma$               context
$\emptyset$            empty context
$\Gamma,\mathsf{x:T}$  variable typing

Contexts are just a way to collect a bunch of types of various variables.
We collect these types together to form a basis for making typing decisions. 
They're mostly there for mathematical rigor, but sometimes their contents will be important to us.
Sometimes we don't need to know anything to make a typing decision, which is what the empty context indicates.

## Typing Rules

Okay, so we've got a bunch of ways to write various terms and types. 
Let's talk about what we can do with these things. 
To do this, we need to introduce a few pieces of notation:

$\Gamma \vdash \mathsf{x:T}$ indicates that under the type assumptions in $\Gamma$, $\mathsf{x}$ has type $\mathsf{T}$.

$\frac{\mathrm{Premises}}{\mathrm{Conclusion}}$ indicates that if we assume $\mathrm{Premises}$, we can conclude $\mathrm{Conclusion}$.
(This is just a nice visual way of writing the process by which we derive the type for various things.)

First off, we have some rules for introducing new terms. 

Syntax                                                                                          Name
------                                                                                          ----
$\bar{\Gamma\vdash \mathsf{x:1}}$                                                               Unit introduction
$\frac{\Gamma,\mathsf{x:T_1}\vdash\mathsf{t:T_2}}{\Gamma\vdash\lambda\mathsf{x:T_1.t:T2}}$      Function introduction

- Basics of type theory: how to express statements in types, etc
- Non-capturing substitution
- Constructive logic
