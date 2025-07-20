---
title: "C++ GUIDELINES"
layout: default
---

# C++ Guidelines

These are the guidelines to use when working on a C++ project, both for general C++ development and embedded systems development.

## General C++ Guidelines

### Core Guidelines

1) Use the ISO 2020 C++ Standard

2) Prefer `constexpr`{:.cpp}, `enum`{:.cpp}, and `inline`{:.cpp} to `#define`{:.cpp}

3) Use `const`{:.cpp} to indicate intent (that a variable is immutable)

4) Use `const`{:.cpp} whenever possible

5) Use spaces instead of tab characters

6) Use `nullptr`{:.cpp} for the null pointer constant

7) Do not assign the address of a variable to a pointer of greater 
lifetime

8) Do not make assumptions about the internal representation of an object

9) Use symbolic names instead of literal values

10) Use parantheses to indicate the intent of an expression

11) Do not use bitwise operators with signed variables

12) Always initialize an object

13) Always explicitly define the behavior for the default constructor, destructor, and copy/assignment operators

14) Prefer smart pointers to C-style pointers

15) Use `U`{:.cpp} suffix for unsigned integral literals

16) Use `f`{:.cpp} suffix for float literals

17) Ensure all paths of a condition can be executed (`if`{:.cpp}, `else if`{:.cpp}, `else`{:.cpp}, `switch`{:.cpp})

18) Avoid the use of global variables

### Idioms

1) Use RAII for resource management

2) Use PIMPL for components where implementation details need to support multiple platforms

3) Use the publish-subscribe model for sharing data between tasks/threads

### Style Guide

1) Prepend `m_`{:.cpp} to class and struct member variables

2) Use Doxygen style comments

3) Use the Allman bracketing style

4) Use `{}`{:.cpp} for initialization

5) Always enclose the body of a condition in `{}`{:.cpp}

6) Use camel case for functions and types or objects defined at file/global scope

7) Use snake case for local and member variables

8) Capitalize the first letter of each word in a type or class name

9) Capitalize all words after the first word in function names (including member functions)

10) Prepend `s_`{:.cpp} to static variables

11) Prepend `g`{:.cpp} to global variables

12) Append `_ptr`{:.cpp} to pointers

13) Append `_t`{:.cpp} to typenames

14) Use `#define`{:.cpp} file guards in header files

## Embedded C++

### Core Guidelines

1) Minimize the use of string objects
    * Both C-type strings and `std::string`{:.cpp}
    
    * Prefer compile-time hash values for strings whenver possible

2) Use PIMPL for HAL design

3) Use `std::atomic`{:.cpp} for variables changed by ISRs and threads

4) Use `volatile`{:.cpp} for memory mapped I/O

5) Use an RTOS for projects with high complexity, otherwise prefer simple design patterns
