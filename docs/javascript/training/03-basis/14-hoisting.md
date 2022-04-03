# 14-hoisting

***

## Introduction

At first when a browser receive a javascript file, he read the file content entirely and initialize a stack.

This behavior is due to the fact that Javascript is an interpreted language.

This stack is a reserved memory place to store needed things (var, function, etc.) for running the script.

This process of preparing the stack for running the script is called "Hoisting".

***

## Initialization

### var

Initialized during the hoisting phase to "undefined".

### let and const

They are hoisted and **NOT**, initialized to "undefined".

Initialized during the execution phase.

***
