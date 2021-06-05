# ArkScript macros - DRAFT

*feel free to provide examples if you think something can be unclear*

## Definitions

_macro_: a fragment of code which has been given a name. Whenever the name is used, it is replaced by the contents of the macro. They are two kinds of macros: objects-like macros and functions-like macros.

_conditional macro_: a condition executed at compile time, possibily generating code

_object-like macro_: ressemble data object when used

_function-like macro_: ressemble function call when used

_preprocessor_: system which handles the macro application process

## Precisions

*remove if empty, not necessary in the draft*

## Elimination of macros

Macros should be applied in the order they appear after parsing ArkScript code.

Every macro, used or not, should be eliminated from the source code before sending the code to the optimizer.

The preprocessor should run right after the parsing and before the optimizer.

A programmer should be able to remove a macro definition using `undef`, without it yielding an error if the specified macro name can not be found in the nearest scope.

## Builtin macros

### Creating symbols

One should be able to create new AST nodes, thus new symbol names. This implies the need for `symcat`, taking a symbol and a number or string, creating a new symbol (should eliminate `.` in numbers and `"` from strings, as well as `\\`).

### Getting informations on nodes

We need a macro `argcount` to retrieve the number of arguments of a given function, which can be used later to craft proxy-functions through macros for said function.

## Syntax

### Conditional macro

```
!{if condition then else}
```

The `condition` must be compile-time evaluated because the `if` macro must be eliminated from the AST before compiling it. The `then` block is mandatory while the `else` one isn't. We can add other macros definitions in the `then` and/or the `else` part.

### Object-like macro

```
!{name body}
```

`name` will be replaced by `body` when used in ArkScript code.

### Function-like macro

```
!{name (args) body}
```

`(name args...)` should be replaced by `body` when used in ArkScript code. The number of arguments sent to the macro should be checked, and if it's wrong, it should be reported and should abort the compilation process.

### Variadic arguments

We introduce the notation `...name` to tell the preprocessor that `name` will be a list of all the other arguments given to a macro. Using the triple-dot notation before an argument name terminates the argument list.

Example:

```clojure
# valid
!{foo (...name) body}

# valid
!{foo (a b c ...name) body}

# invalid, ...name isn't the last element
!{foo (a b ...name c) body}
```

The list of argument created by `...name` will result in a real ArkScript list at runtime, and will be manipulated as one, thus notations like
* `(len name)`
* `(@ name 1)`
* ...

are valid.

## Macro definition rules

1. macros can only be defined in any block
1. following the previous rule, macros can be nested (a macro defined inside a macro)
1. macros can be chained:
```clojure
# first, foo is applied, and the macro bar is modified
!{foo 1}
# then, bar is applied
!{bar (egg) (+ foo egg)}

(let a (bar 1))
```
1. macros can be recursives (call themselves, but you should take care yourself of the recursion limit)

# Authors

(add your name here)

* [Alexandre Plateau](https://github.com/SuperFola)
