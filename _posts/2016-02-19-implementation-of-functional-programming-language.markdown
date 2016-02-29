---
layout: post
title:  "The implementation of functional programming language"
date:   2016-02-19 Fri 17:41:05
categories: programming
---

Ref:

<ol type="a">
    <li> Jones, The implementation of functional programming language </li>
</ol>

<section>
<h1>  </h1>
<div>
<ol>
    <li> \(\lambda\) expression
        <pre style="font-size: 100%; width: 70%">
&lt;exp> :: = &lt;constant>
      |  = &lt;variable>
      |  = &lt;exp> &lt;exp>
      |  = lambda &lt;variable>.&lt;exp> </pre>
     </li>
     <li> Free and bound variables: Free = External, Bound = Local </li>
     <li> CONS, HEAD, TAIL
        <pre style="font-size: 100%; width: 70%">
CONS = (lambda a . lambda b . lambda f . f a b)
HEAD = (lambda c . c (lambda a . lambda b . a))
TAIL = (lambda c . c (lambda a . lambda b . b)) </pre>
    How to understand the three definitions?
        <pre style="font-size: 100%; width: 70%">
CONS x y = lambda f . f x y
CONS (CONS x y) z = lambda f . f (CONS x y) z = lambda f . f (lambda f . f x y) z </pre>
     We may think of (lambda f . f ) as kind of a bracket, which encloses x and y inside.
     Then what about HEAD?
     Note that (CONS x y) is a function that requires a function as input, and HEAD requires a function as its input, and this function itself must take a function as input.
        <pre style="font-size: 100%; width: 70%">
(lambda a . lambda b . a) x y = (lambda b x) y = x </pre>
     So the key is the inside part of HEAD.
     </li>
     <li> Beta reduction, beta conversion </li>
     <li> Alpha conversion: change name of variable </li>
     <li> Eta conversion: equivalence of lambda abstractions </li>
     <li> Proving interconvertibility: if two expressions can be converted into the same expression, then the two are equivalent. </li>
     <li> E[M/x] = (lambda x . E) M </li>
        <pre style="font-size: 100%; width: 70%">
(E F)[M/x] = (lambda x . (E F)) M = E[M/x] F[M/x]
(lambda x . E)[M/x] = (lambda x (lambda x . E)) M = (lambda x . E)
(lambda y . E)[M/x] = (lambda x (lambda y . E)) M = lambda y . E if x does not appear free in E
                                                  = lambda y . E[M/x] if y does not appear free in M
                                                  = lambda z . (E[z/y])[M/x] otherwise </pre>
<li> Reduction order: no redexes = normal form
    <ul>
        <li> Not every expression has a normal form.
        <pre style="font-size: 100%; width: 70%">
(lambda x x x) (lambda x x x) = ((lambda x x x) (lambda x x x)) </pre>
which is equal to itself.</li>
    </ul>
    <ul>
        <li> Dependence on reduction path
        <pre style="font-size: 100%; width: 70%">
(lambda x 3) (D D) = 3, or infinite loop?
</pre>
    </li>
    </ul>
</li>
<li> Uniqueness of order reduction
    <ul>
    <li> Church-Rosser theorem I
        <br>
        If \(E_1\leftrightarrow E_2\), then there exists an expression \(E\), such that
        \[ E_1 \rightarrow E\ \text{and}\ E_2 \rightarrow E.\]
        <br> Welch 1975; Rosser 1982
    </li>
    <li>
    Hence all reduction sequences which terminate will reach the same result.
    </li>
    <li> Normal order reduction: the leftmost outermost redex should be reduced at first. <br>
         Intuition: function should be evaluated ealier than its parameters; from outer to inner.
    </li>
    <li> Church-Rosser theorem II
    <br>
        If \(E_1\leftrightarrow E_2\), and \(E_2\) is in normal form, then there exists a normal order reduction sequence from \(E_1\) to \(E_2\).
    </li>
    <li>
    Optimal reduction orders: normal order does not guarantee to be the fastest; for some cases (tree reduction) it is provably least favorable; for graph reduction is is almost optimal.
    </li>
    </ul>
</li>
<li>
Recursive functions
<ul>
    <li>
    FAC = (lambda n . IF (= n 0) 1 (* n (FAC (- n 1)))), but this is not lambda abstraction.
    </li>
    <li>
    FAC = H FAC, where H = (lambda fac . (lambda n . (... fac ...))).
    FAC is a fixed point of H.
    </li>
    <li> Fixed point combinator Y: Y H = H (Y H) </li>
    <li> Y can be defined as a lambda abstraction: <br>
        Y = (lambda h . (lambda x . h (x x)) (lambda x . h (x x))) <br>
        Y H = (lambda x . H (x x)) (lambda x . H (x x)) = H ((lambda x . H (x x)) (lambda x . H (x x))) = H Y H <br>
        (lambda x . h (x x)) does not have a finite type.
    </li>
    <li> Uniqueness of the Y operation? Check domain theory. </li>
</ul>
</li>
</ol>
</div>

</section>
