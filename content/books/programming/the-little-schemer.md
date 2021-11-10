---
title: The Little Schemer
tags: [programming, lisp, scheme]
categories: [notes]
lastMod: 2018-05-05
---

**The Little Schemer by Friedman, Felleisen**

## 1. Toys
    
```scheme
; Is 'x' an atom
(define atom?
  (lambda(x)
    (and (not (pair? x)) (not (null? x)))))
```
    

## 2. Do It, Do It Again, and Again, and Again...
    
```scheme
; Is 'l' a list of atoms
(define lat?
  (lambda(l)
    (cond
      ((null? l) #t)
      ((atom? (car l)) (lat? (cdr l)))
      (else #f))))
```
    

```scheme
; Is 'a' a member of 'lat'
(define member?
  (lambda (a lat)
    (cond
      ((null? lat) #f)
      (else (or (eq? (car lat) a)
                (member? a (cdr lat)))))))
```
    
    
***
### The First Commandment
*(preliminary)*   
**Always ask `null?` as the first question in expressing any function.**
   ***
## 3. Cons the Magnificent
    

```scheme
; Remove 'a' from 'lat'
; But not quite right.
(define rember
  (lambda (a lat)
    (cond
      ((null? lat) (quote()))
      (else (cond
              ((eq? (car lat) a) (cdr lat))
              (else (rember a (cdr lat))))))))
```
   
***
### The Second Commandment
*(preliminary)*   
**Use `cons` to build lists.**   
   ***
      
```scheme
; Fixed by consing the first part of the list
(define rember
  (lambda (a lat)
    (cond
      ((null? lat) (quote()))
      (else (cond
              ((eq? (car lat) a) (cdr lat))
              (else (cons (car lat)
                          (rember a
                                  (cdr lat)))))))))

```
    
```scheme
; Simplified.
(define rember
  (lambda (a lat)
    (cond
      ((null? lat) (quote()))
      ((eq? (car lat) a) (cdr lat))
      (else (cons (car lat)
                (rember a (cdr lat)))))))
```
   
   
   ***
### The Third Commandment
*(preliminary)*   
**When building a list, describe the first typical element, and then `cons` it onto the natural recursion.**
***
    
```scheme
; Take all the firsts elements of all the list-elements of 'l'
(define firsts
  (lambda (l)
    (cond
      ((null? l) (quote()))
      (else (cons (car (car l))
              (firsts (cdr l)))))))
```
   
```scheme
; Insert 'new' to the right of 'old' in list of atoms 'lat'
(define insertR
  (lambda (new old lat)
    (cond
      ((null? lat) (quote()))
      ((eq? (car lat) old)
       (cons old (cons new (cdr lat))))
      (else (cons (car lat)
                  (insertR new old
                          (cdr lat)))))))
```
   

```scheme
; Insert 'new' to the left of 'old' in list of atoms 'lat'
(define insertL
  (lambda (new old lat)
    (cond
      ((null? lat) (quote()))
      ((eq? (car lat) old)
       (cons new lat))
      (else (cons (car lat)
                  (insertR new old
                          (cdr lat)))))))
```

```scheme
; Substitute the first occurence of 'new' with 'old' in list of atoms 'lat'
(define subst
  (lambda (new old lat)
    (cond
      ((null? lat) (quote()))
      ((eq? (car lat) old)
       (cons new (cdr lat)))
      (else (cons (car lat)
                  (insertR new old
                          (cdr lat)))))))
```

```scheme
; Remove all occurences of 'a' from 'lat'
(define multirember
  (lambda (a lat)
    (cond
      ((null? lat) (quote()))
      ((eq? (car lat) a) (multirember a (cdr lat)))
      (else (cons (car lat)
                  (multirember a
                          (cdr lat)))))))
```
***
### The Fourth Commandment
*(preliminary)*   
**Always change at least one argument while recurring. It must be changed to be closer to termination. The changing argument must be tested in the termination condition: when using `cdr`, test termination with `null?`.**   
   ***

```scheme
; Substitute 'new' with 'old' in list of atoms 'lat'
(define multisubst
  (lambda (new old lat)
    (cond
      ((null? lat) (quote()))
      ((eq? (car lat) old)
       (cons new (multisubst new old (cdr lat))))
      (else (cons (car lat)
                  (multisubst new old
                          (cdr lat)))))))
```

## Numbers Games

```scheme
(define add1
  (lambda (n)
    (+ n 1)))

(define sub1
  (lambda (n)
    (- n 1)))
```

```scheme
;Addition using primitives
(define o+
  (lambda (n m)
    (cond
      ((zero? m) n)
      (else (add1 (o+ n (sub1 m)))))))

;Subtraction using primitives
(define o-
  (lambda (n m)
    (cond
      ((zero? m) n)
      (else (sub1 (o- n (sub1 m)))))))
```
***
### The First Commandment
*(first revision)*   
**When recurring on a list of atoms, `lat`, ask two questions about it: (`null? lat`) and `else`.**   
**When recurring on a number, `n`, ask two questions about it: (`zero? n`) and `else`.**   
***
   
```scheme
; Add numbers in a tuple
(define addtup
  (lambda (tup)
    (cond
      ((null? tup) 0)
      (else (+ (car tup) (addtup (cdr tup)))))))
```
***
### The Fourth Commandment
*(first revision)*   
**Always change at least one argument while recurring. It must be changed to be closer to termination.** 
**The changing argument must be tested in the termination condition: when using `cdr`, test termination with `null?` and when using `sub1`, test termination with `zero?`.**
***
```scheme
; Multiplication using primitives
(define o*
  (lambda (n m)
    (cond
      ((zero? m) 0)
      (else (o+ n (o* n (sub1 m)))))))
```
***
### The Fifth Commandment
**When building a value with `o+`, always use `0` for the value of the terminating line, for adding `0` does not change the value of an addition.**
**When building a value with `o*`, always use `1` for the value of the terminating line, for multiplying by `1` does not change the value of a multiplication.**
**When building a value with `cons`, always consider `()` for the value of the terminating line.**
***
```scheme
; Addition of two tuples
(define tup+
  (lambda (tup1 tup2)
    (cond
      ((null? tup1) tup2)
      ((null? tup2) tup1)
      (else 
        (cons (o+ (car tup1) (car tup2)) 
          (tup+ (cdr tup1) (cdr tup2)))))))
```
   
```scheme
; Greater than
(define >
  (lambda (n m)
    (cond
      ((zero? n) #f)
      ((zero? m) #t)
      (else (> (sub1 n) (sub1 m))))))
```
   

```scheme
; Is Equal to (using > and <)
(define =
  (lambda (n m)
    (cond
      ((> n m) #f)
      ((< n m) #f)
      (else #t))))
```

```scheme
; exponential
(define expt
  (lambda (n m)
    (cond
      ((zero? m) 1)
      (else (* n (sub1 m))))))
```

```scheme
; Division
(define quotient
  (lambda (n m)
    (cond
      ((< n m) 0)
      (else (add1 (quotient (- n m) m))))))
```

```scheme
; Length
(define length
  (lambda (lat)
    (cond
      ((null? lat) 0)
      (else (add1 (length (cdr lat)))))))
```

```scheme
; Equal number or atom
(define eqan
  (lambda (a1 a2)
    (cond
      ((and (number? a1) (number? a2)) 
        (= a1 a2))
      ((or (number? a1) (number? a2)) #f)  
      (else (eq? a1 a2)))))
```

## *Oh My Gawd*: It's Full of Stars

```scheme
; remove member recursively
(define rember*
  (lambda (a l)
    (cond
      ((null? l) (quote()))
      ((atom? (car l))
        (cond 
          ((eq? (car l) a)
            (rember* a (cd l)))
          (else (cons (car l)
                  (rember* a (cdr l))))))
      (else (cons (rember* a (car l)) 
              (rember* a (cdr l)))))))
```


```scheme
; Insert atom `new` to the right of `old` recursively
(define insertR*
  (lambda (new old l)
    (cond
      ((null? l) (quote()))
      ((atom? (car l))
        (cond 
          ((eq? (car l) old)
            (cons old
              (cons new 
                (insertR* new old (cdr l)))))
          (else (cons (car l)
                  (insertR* new old (cdr l))))))
      (else (cons (insertR* new old (car l)) 
              (insertR* new old (cdr l)))))))
```

***
### The First Commandment
*(final version)*   
**When recurring on a list of atoms, `lat`, ask two questions about it: `(null? lat)` and `else`.**   
**When recurring on a number, `n`, ask two questions about it: `(zero? n)` and `else`.**   
**When recurring on a list of S-expressions, `l`, ask three questions about it: `(null? l)`, `(atom? (car l))`and `else`.**   

***
### The Fourth Commandment
*(final version)*   
**Always change at least one argument while recurring.**   
**When recurring on a list of atoms, `lat`, use `(cdr lat)`.**   
**When recurring on a number, `n`, use `(sub1 n)`.**   
**And when recurring on a list of S-expressions, `l`, use `(car l)` and `(cdr l)` if neither `(null? l)` or `(atom? l)` are true.**    
   
**It must be changed to be closer to termination.**
**The changing argument must be tested in the termination condition:**   
   
 **when using `cdr`, test termination with `null?` and**   
 **when using `sub1`, test termination with `zero?`.**

 ***

```scheme
; Occurence
(define occur*
  (lambda (a l)
    (cond
      ((null? l) 0)
      ((atom? (car l))
        (cond 
          ((eq? (car l) a)
            (add1 (occur* a (cdr l))))
          (else (occur* a (cdr l)))))
      (else (+ (occur* a (car l)) 
              (occur* a (cdr l)))))))
```


```scheme
; Is a member?
(define occur*
  (lambda (a l)
    (cond
      ((null? l) #f)
      ((atom? (car l))
        (or 
          (eq? (car l) a)
          (member* a (cdr l))))
      (else (or (member* a (car l)) 
              (member* a (cdr l)))))))
```

```scheme
; Are the two lists equal?
(define eqlist*
  (lambda (l1 l2)
    (cond
      ((and (null? l1) (null? l2)) #t)
      ((or (null? l1) (null? l2)) #f)
      ((and (atom? (car l1)) 
          (atom? (car l2)))
          (and (eqan? (car l1) (car l2)) 
            (eqan? (cdr l1) (cdr l2))))
      ((or (atom? (car l1)) 
        (atom? (car l2))) #f)
      (else (and (eqlist* (car l1) (car l2)) 
              (eqlist* (cdr l1) (cdr l2)))))))
```

```scheme
; Are the S-expressions equal?
(define equal*
  (lambda (s1 s2)
    (cond
      ((and (atom? ls1) (atom? s2))
        (eqan? s1 s2))
      ((or (atom? s1) (atom? s2)) #f)
      (else (eqlist? s1 s2)))))
```

```scheme
; using equal?
(define eqlist*
  (lambda (l1 l2)
    (cond
      ((and (null? l1) (null? l2)) #t)
      ((or (null? l1) (null? l2)) #f)
      (else (and (eqlist* (car l1) (car l2)) 
              (eqlist* (cdr l1) (cdr l2)))))))
```

***
### The Sixth Commandment
**Simplify only after the function is correct.**
***


```scheme
; Is the input an arithmetic expression?
(define numbered?
  (lambda (aexp)
    (cond
      ((atom? aexp) (number? aexp))
      (else 
        (and (numbered? (car aexp)) 
          (numbered? (car (cdr (cdr aexp)))))))))
```

```scheme
; Value of the given expression
(define value
  (lambda (nexp)
    (cond
      ((atom? nexp) ...)
      ((eq? (car (cdr nexp)) (quote +)) 
        ...)
      ((eq? (car (cdr nexp)) (quote x)) 
        ...)
      (else 
        ...))))
```

***
### The Seventh Commandment
**Recur on the *subparts* that are of the same nature:**   
* **On the sublists of a list.**
* **On the subexpressions of an arithematic expression.** 
***

```scheme
; Value of the given expression
(define value
  (lambda (nexp)
    (cond
      ((atom? nexp) nexp)
      ((eq? (car (cdr nexp)) (quote +)) 
        (+ (value (car nexp))
          (value (car (cdr (cdr nexp))))))
      ((eq? (car (cdr nexp)) (quote x)) 
        (x (value (car nexp))
          (value (car (cdr (cdr nexp))))))
      (else 
        (** (value (car nexp))
          (value (car (cdr (cdr nexp)))))))))
```

```scheme
; Some Abstraction
(define 1st-sub-exp
  (lambda (aexp) 
    (car (cdr aexp))))
(define 2nd-sub-exp
  (lambda (aexp) 
    (car (cdr (cdr aexp)))))
(define operator
  (lambda (aexp) 
    (car aexp)))

(define value
  (lambda (nexp)
    (cond
      ((atom? nexp) nexp)
      ((eq? (operator nexp) (quote +)) 
        (+ (1st-sub-exp nexp)
          (2nd-sub-exp nexp)))
      ((eq? (operator nexp) (quote x)) 
        (x (1st-sub-exp nexp)
          (2nd-sub-exp nexp)))
      (else 
        (** (value (1st-sub-exp nexp))
          (value (2nd-sub-exp nexp)))))))
```

***
### The Eighth Commandment
**Use help functions to abstract from representations.**   
***

***
### The Tenth Commandment
**Build functions to collect more than one value at a time.**   
***





