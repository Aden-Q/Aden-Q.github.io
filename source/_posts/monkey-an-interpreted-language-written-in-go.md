---
title: 'Monkey: An Interpreted Language Written in Go'
tags:
  - Go
categories:
  - Programming Language
  - Compiler
date: 2024-05-15 16:00:00
---


Monkey is an interpreted/scripting language written in Go, strictly following the design outlined in the book **Writing An Interpreter In Go** by Thorsten Ball. The architecture is extended with additional functionalities. The code is hosted on my [GitHub repo](https://github.com/Aden-Q/monkey/tree/main), and the project is still actively under development.

---

**Content**
- [Introduction](#introduction)
- [Semicolons](#semicolons)
- [Variables](#variables)
- [Data types](#data-types)
  - [Primitive data types](#primitive-data-types)
    - [Integer](#integer)
    - [String](#string)
    - [Boolean](#boolean)
  - [Collections](#collections)
    - [Arrays](#arrays)
    - [Hashes](#hashes)
- [Control structures](#control-structures)
  - [If](#if)
- [Functions](#functions)
- [More examples](#more-examples)
  - [Map-reduce pattern](#map-reduce-pattern)
- [References](#references)

## Introduction

Monkey, as an interpreted language, comprises six major components:

+ Token set
+ Lexer
+ Pratt Parser
+ Abstract syntax tree (AST)
+ Object system
+ Evaluator

The primary method to interact with and explore Monkey is through the Read-Evaluate-Print-Loop (REPL) running in a shell:

```bash
➜  ~ monkey
            __,__
   .--.  .-"     "-.  .--.
  / .. \/  .-. .-.  \/ .. \
 | |  '|  /   Y   \  |'  | |
 | \   \  \ 0 | 0 /  /   / |
  \ '- ,\.-"""""""-./, -' /
   ''-' /_   ^ ^   _\ '-''
       |  \._   _./  |
       \   \ '~' /   /
        '._ '-=-' _.'
           '-----'
Hello xxx! This is the Monkey programming language!
>>> print("Hello, world!");
Hello, world!
```

## Semicolons

By convention, statements in Monkey are terminated by semicolons. Only statements can be evaluated by the REPL:

```bash
>>> 1+1;  
2
>>> 1+1
parser errors:
        unexpected token type
```

## Variables

Variables in Monkey consist of alphabet letters and underscores. They are case-sensitive and dynamically typed, meaning they can be bound to any type of data.

As an example:

```bash
>>> let x = 1;
>>> x;
1
>>> let x = "Hello, world!";
>>> x;
Hello, world!
```

## Data types

### Primitive data types

#### Integer

Integers in Monkey are signed and represented with 8 bytes. Unlike Python, Monkey can experience overflow issues with integers. Arithmetic operations `+, -, *, /` are allowed on integers, and parentheses can be used to change the precedence of operations.

As an example:

```bash
>>> 1 + 2 * 3 - 6 / 2;
4
>>> (1 + 2) * 3 - 6 / 2;
6
```

#### String

Strings in Monkey are immutable data type that allow:

Retrieving the length of a string (`len` is a built-in function that can retrieve the length attribute of a string/array/hash):

```bash
>>> let s = "Hello, world!";
>>> len(s);
13
```

Concatenation of multiple strings:

```bash
>>> s + s;
Hello, world!Hello, world!
>>> s + s + s;
Hello, world!Hello, world!Hello, world!
```

#### Boolean

In Monkey, there are only two boolean literals: `true` and `false`.

### Collections

#### Arrays

An array in Monkey is ordered list of items, which can be of different types. We use square brackets `[]` to index into an array.

As an example:

```bash
>>> let arr = [1, true, "hello", "world"];
>>> arr[0];
1
>>> arr[1];
true
>>> arr[2];
hello
>>> arr[3];
world
```

#### Hashes

A hash, also known as a map or dictionary in other programming languages, maps keys to values. In Monkey, only hashable data types can be used as keys in a map. Integer, boolean, and string are all hashable in the context of Monkey. Similarly, we use square brackets `[]` to index into a hash.

As an example:

```bash
>>> let hash = {"name": "alice", "age": 20};
>>> hash["name"];
alice
>>> hash["age"];
20
```

## Control structures

### If

The `if` statement is treated as an expression in Monkey, which means it can be assigned to an identifier like this:

```bash
>>> let x = if (true) { return true; }; 
>>> x;
true
```

Currently there are two ways to play with `if`:

+ A single `if` branch without `else`
+ A single `if` branch with a single `else` branch

As an example:

```bash
>>> if (2 > 1) { print("2 is greater than 1"); };
2 is greater than 1
>>> if (1 > 2) { print("1 is greater than 2"); } else { print("1 is smaller than 2"); };
1 is smaller than 2
```

## Functions

In Monkey, functions are first-class citizens, which means they can be treated just like any other expressions. We use the keyword `fn` to create a function.

As an example:

```bash
>>> let add = fn(a, b) { return a + b; };
>>> add(1, 2);
3
```

Anonymous functions are also callable:

```bash
>>> fn(a, b) { return a + b; } (1, 2);
3
```

## More examples

### Map-reduce pattern

## References

+ *Writing An Interpreter In Go* by Thorsten Ball
+ *Top Down Operator Precedence* by Vaughan Pratt
+ *The Structure and Interpretation of Computer Programs (SICP)* by Harold Abelson
