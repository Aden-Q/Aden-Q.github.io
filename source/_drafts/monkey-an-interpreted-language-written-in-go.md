---
title: 'Monkey: An Interpreted Language Written in Go'
tags:
  - Go
date: 2024-05-15 16:00:00
categories:
  - Programming Language
  - Compiler
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
- [Functions](#functions)
- [More examples](#more-examples)
  - [Map-reduce pattern](#map-reduce-pattern)

## Introduction

Monkey, as an interpreted language, comprises six major components:

+ Token set
+ Lexer
+ Pratt Parser
+ Abstract syntax tree (AST)
+ Object system
+ Evaluator

The primary method to interact with and explore Monkey is through a Read-Evaluate-Print-Loop (REPL) running in a shell:

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
>>> 
```

## Semicolons

By convention, statements in Monkey are terminated by semicolons. Only statements can be evaluated by REPL:

```bash
>>> 1+1;  
2
>>> 1+1
parser errors:
        unexpected token type
```

## Variables

Variables in Monkey consists of alphabet letters and underscore, and are case-sensitive. They are also dynamically typed, which means they can be bind to any type of data.

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

Intergers in Monkey have range of int64. Unlike Python, there will be an overflow problem in Monkey. Operators on integers include:

+ +
+ -
+ *
+ /

Monkey also respect the precedednce of operators. So we can do some arithmetic calculation like this:

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

Technically there are only two boolean literals in Monkey: true and false.

### Collections

#### Arrays

#### Hashes


## Control structures

## Functions

## More examples

### Map-reduce pattern
