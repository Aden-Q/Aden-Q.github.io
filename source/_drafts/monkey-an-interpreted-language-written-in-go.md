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
- [Language Specification](#language-specification)

## Introduction

Monkey, as an interpreted language, consists of six major components:

+ Token set
+ Lexer
+ Pratt Parser
+ Abstract syntax tree (AST)
+ Object system
+ Evaluator

The main way to run and play with Monkey is through Read-Evaluate-Print-Loop (REPL) running in a shell:

```bash

```

## Identifiers

Identifiers/Variables in Monkey consists of alphabet letters and underscore, and are case-sensitive. They are also dynamically typed, which means you can bind any type of data to a single variable.

As an example:

```bash
>>> 
```

## Data types

### Primitive data types

#### Integer

#### String

#### Boolean

### Collections

#### Arrays

#### Hashes


## Control structures

## Functions

