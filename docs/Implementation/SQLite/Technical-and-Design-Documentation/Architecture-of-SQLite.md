# [Architecture of SQLite](https://www.sqlite.org/arch.html)



## Introduction

A nearby diagram shows the main components of SQLite and how they interoperate. The text below explains the roles of the various components.

![](./arch2.gif)

## Overview

SQLite works by compiling SQL text into [bytecode](https://www.sqlite.org/opcode.html), then running that bytecode using a virtual machine.

| interface                                                    | function                                                     |                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ---------------------------- |
| [sqlite3_prepare_v2()](https://www.sqlite.org/c3ref/prepare.html) and related interfaces | compiler for converting SQL text into bytecode               | 对应的是上图中的sql compiler |
| [sqlite3_stmt](https://www.sqlite.org/c3ref/stmt.html) object | container for a single bytecode program using to implement a single SQL statement |                              |
| [sqlite3_step()](https://www.sqlite.org/c3ref/step.html) interface | passes a bytecode program into the virtual machine, and runs the program until it either completes, or forms a row of result to be returned, or hits a fatal error, or is [interrupted](https://www.sqlite.org/c3ref/interrupt.html) |                              |



## Interface



## Tokenizer

When a string containing SQL statements is to be evaluated it is first sent to the tokenizer. The tokenizer breaks the SQL text into tokens and hands those tokens one by one to the parser. The tokenizer is hand-coded in the file `tokenize.c`.

Note that in this design, the tokenizer calls the parser. People who are familiar with [YACC](https://en.wikipedia.org/wiki/Yacc) and [BISON](https://en.wikipedia.org/wiki/GNU_Bison) may be accustomed to doing things the other way around — having the parser call the tokenizer. Having the tokenizer call the parser is better, though, because it can be made threadsafe and it runs faster.

> NOTE: 上面这段话的意思是，sqlite的tokenizer是自己实现的，并没有使用[Lex (software)](https://en.wikipedia.org/wiki/Lex_(software))



## Parser

The parser assigns meaning to tokens based on their context. The parser for SQLite is generated using the [Lemon parser generator](https://www.sqlite.org/lemon.html). 



## Code Generator

> NOTE: 这个步骤是最最复杂的，它需要考虑database的底层实现，比如table是如何实现的；然后转换为对应的code

After the parser assembles tokens into a **parse tree**, the **code generator** runs to analyze the parser tree and generate [bytecode](https://www.sqlite.org/opcode.html) that performs the work of the SQL statement. 



## Bytecode Engine

The [bytecode](https://www.sqlite.org/opcode.html) program created by the code generator is run by a virtual machine.

SQLite implements SQL functions using callbacks to C-language routines. Even the built-in SQL functions are implemented this way.



## B-Tree

> NOTE: 关于B-Tree，参见工程discrete。

An SQLite database is maintained on disk using a B-tree implementation found in the [btree.c](https://sqlite.org/src/file/src/btree.c) source file. 



## OS Interface

In order to provide portability between across operating systems, SQLite uses abstract object called the [VFS](https://www.sqlite.org/vfs.html).

> NOTE: VFS是virtual file system，显然它是一种abstraction



## Utilities