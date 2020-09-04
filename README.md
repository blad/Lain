Lain
---------------

Lain is a lisp dialect used within [Ronin, an experimental graphics 
terminal][ronin]. This guide explains the Lain language. Lain does not have a 
standard library by default so there are many basic operations that can not be 
done without defining some basic operations as part of the run time 
environment. However, this also gives flexibility to any program that uses Lain 
to include standard library functions that are fit for their task.

The primary characteristic of lisp, and this Lain, is that it's syntax is based 
on s-expressions. S-Expressions are characterized by their heavy use of 
parenthesis, and represent data or code. An s-expression can be an atom, or 
another s-expression. This gives us a program where logic is typically deeply 
paretherized.

Examples of atoms, are values or names that can not be broken down further. For 
example: `10`, `name`, and `"Hello World"` are considered atoms. Atoms can then 
be constructed into lists by parentherizing multiple atoms together, such as 
`(1 2 3 4)` and `("a" "b" "c")`, which are lists of values. However within 
Lain, lists can also represent function calls, where the first item in the list 
is the function name followed by all the function's arguments.

Lain gives us the following features as part of the core language:

- Binding values to names
- Defining named functions
- Defining lambda functions (anonymous functions)
- Binding multiple values to names using `let`
- Conditional expressions via `if`
- Defining lists
- Defining objects/dictionaries and accessing values via a key
- Primatives include: strings, numbers, booleans, and symbols.

## About Lists

Lists are core to Lain, in most instances anything wrapped in parenthesis will 
be treated as a list. The two exceptions are when a list's first item is a 
function name or a keyword.

When a function name is encountered, the function is evaluated with the 
remaining list items as the function's arguments.

When a keyword is encountered, the remaining list items are treated accoding to 
the keyword's pre-defined behaviour. In some instances subsequent list items 
are expected in a specific order, or evaluated by rules defined for the 
keyword. Below those behaviours are defined for: `def`, `defn`, `λ`, `let`, 
`if` and property accessors.

## Binding Values to Names (Variables)

Variables are names that map to a value.

The value of a variable can be a primitive, list, or result of a function.

```
;; A primitive value:
(def game-name "pong")
(def ball-speed 100)
(def friction 0.1)
(def game-over false)

;; A list:
(def levels (1 2 3))

;; Result of a function call:
(def player-position (position 0 0))
(def difficulty (double 1))
```

## Creating Named Functions


```
; Return the first argument
(defn identity (value) value)

; Wrap the first argument in a list
(defn short-list (value) (value))

; Wrap two arguments in a list
(defn pair (a b) (a b))

; Get the first element of the list
(defn first (xs) (:0 xs))

; Get the second element of the list
(defn second (xs) (:1 xs))
```


## Lambda Functions

Lambda functions are function that do not have a name...this is useful when a 
function expects a function as parameter, but we'd prefer to define the 
function in line.

```
; Identity lambda function
(λ (value) value)

; Wrap the first argument in a list
(λ (value) (value))

; Wrap two arguments in a list
(λ (a b) (a b))
```

Lambda functions can also be used in conjunction with variable definitions to 
give them a name.

While this is less common, there are times when it may be convenient to do this.

```
(def identity (λ (value) value))
```

## Binding Values Using `let`

`let` is a keyword that allows us to bind multiple values to names as a single 
expression. This is most useful inside of function bodies when there is need to 
calculate intermediate values.

The syntax has 3 parts, the `let` keyword, followed by a list of key-value 
pairs for the bindings, and lastly the return atom or s-expression.

`(let {list of pairs} {expression value})`

An example of a standalone `let` is:

```
; Bind values to x and y, use them to calculate total
; return total.
(let 
  (
    (x 1) 
    (y 2) 
    (total (add x y)))
  total)
```

## Conditional Expressions Using `if`

`if` is another keyword that allows us to conditionally evaluate one of two 
s-expressions given a condition is met.

The syntax has 4 parts. The `if` keyword, an expression evaluating to a true or 
false, an expression to be evaluated when true, and an expression to be 
evaluated when false.

`(if {condition} {true expression} {optional false expression})`

An example of a standalone `if` is:
```
(if 
  (is-less-than-100 value)
  (capitalize "yes!")
  (capitalize "no!"))
```

[ronin]: https://github.com/hundredrabbits/Ronin "Ronin: An Experimental 
Graphics Terminal"