# Elixir for a JS Developer

## Table of Contents

- [Basics](#basics)
  - [Code Conventions](#code-conventions)
  - [Variable Declaration](#variable-declaration-or-binding)
  - [Comparison Operators](#comparison-operators)
  - [Imports](#imports)
  - [Functions](#functions)
  - [Method Chaining](#method-chaining)
  - [Destructuring](#destructuring)
- [Control Flow](#control-flow)
  - [Case Statements](#case-statements)
  - [If Else Statements](#if-else-statements)
- [Common List Operations](#common-list-operations)
  - [Map](#map)
  - [Filter](#filter)
  - [Reduce](#reduce)

## Basics

### Code Conventions

- Javascript

```javascript
// camelCasing
const welcomeStatement = 'Hello World';
const welcomeStatement = () => {
  return 'Welcome to Elixir';
};
```

- Elixir

```elixir
# snake_casing
welcome_statement = 'Hello World'
def welcome_statement do
  'Welcome to Elixir'
end
```

### Variable Declaration (or binding)

- Javascript

```javascript
const foo = 1;
let bar = 2;
var baz = 3;

// Export constants for reuse and to avoid duplication
export const FOO = 1;
```

- Elixir

```elixir
foo = 1
# atoms are elixir variables that are used to represent a constant
# whose values value is its own name
# they are often used to express status
:ok
:error

# Declare module attributes for similar purposes as constants export in JS
defmodule MySuperSpecialProject do
  @foo 1
end

# Note that module attributes values are computed at COMPILE time so assigning
# the return value of a function call to a module attribute will remain
# value once compiled
defmodule MySuperSpecialProject do
  @foo DateTime.utc_now()
end
```

"=" is called the match operator. The match operator is similar to "===" in JS.
Unlike JS, all elixir data is immutable and must be assigned
a value at the time the variable is declared.

### Comparison Operators

- Javascript

```javascript
"hi" = "hi" // uncaught SyntaxError: invalid left-hand side in assignment
"hi" == "hi" // true, comparing values
"hi" === "hi" // true, comparing values and types
```

- Elixir

```elixir
"hi" = "hi" # match operator
"hello" = "hi" # ** (MatchError) no match of right hand side value: "hi"
"hi" == "hi" # true, basic comparison
"hello" == "hi" # false
```

### Imports

- Javascript

```javascript
import React, { useState } from 'react'
import React as MyReact from 'react'
```

- Elixir

```elixir
# in elixir, you can use fully qulified name without importing, but it is
# or you can import like this:
# any function in the HelperModule can be called like this without the prefjx:
import MySuperSpecialProject.HelperModule

# or alias HelperModule to avoid typeing the fully qualified name when used
alias MySuperSpecialProject.HelperModule

# or if you only want to mixin certain functions into your module from HelperModule
import MySuperSpecialProject.HelperModule, only: [my_function, 1]
```

### Functions

- Javascript

```javascript
function add(n1, n2) {
  return n1 + n2;
}

const add(n1,n2) => n1 + n2;
```

- Elixir

```elixir
def add(n1, n2) do
  n1 + n2
end

add = fn(n1, n2) -> n1 + n2 end
add &(&1 + &2)

# functions in elixir can be private
defp private_add(n1, n2) do
  n1 + n2
end
```

The last line of the function is the return value.
In elixir, the last line is always the return value.

The ampersand operator is the capture operator.

### Method Chaining

- Javascript

```javascript
const people = [{name: 'Bob', age: 30}, {name: 'Bill', age: 18}];
const filteredAndMapped = people.filter({age} => age > 21).map({name} => name);

// ['Bob']
```

- Elixir

```elixir
people = [%{name: "Bob", age: 30}, %{name: "Bill", age: 18}]
old_neought_names = Enum.filter(people, fn %{age: age} -> age > 21 end)
|> Enum.map(n, fn n -> n.name end)

# ["Bob"]

# The pipe operator (|>) can be applied to more than just arrays:
score = 45
score
|> Kernel./(2)
|> :math.pow(-1)
|> Kernel.*(100)

# equivalent to (((45 / 2) ^ -1) * 100)
```

The pipe operator is very powerful tool in Elixir

### Destructuring

- Javascript

```javascript
const o = { nested: { prop: 'Hi!' } };

const { nested: { prop } = {} } = o;

console.log(prop); // 'Hi!'
```

- Elixir

```elixir
o = %{nested: %{prop: "Hi!"}}

%{nested: %{prop: prop}} = o

IO.inspect prop # "Hi!"
```

Pattern Matching is the Elixir equivalent of destructuring.
In Elixir, it is common to have multiple functions defined
with the same name that match on different properties

- Javascript

```javascript
const list = { user } => {
  if (user.isAdmin) {
    return store.listAll();
  }
  return store.listForUser(user);
};
```

- Elixir

```elixir
# if the user passed in is an admin, this function will be called
defp list(%{is_admin: true}) do
  store.list_all()
end

# regular users will call this function
defp list(user) do
  store.list_for_user(user)
end
```

## Control Flow

### Case Statements

- Javascript

```javascript
const response = getAResponse();
switch (response.status) {
  case 200:
    return 'Sucess';
  case 401:
    return 'Not Allowed';
  default:
    return 'There was a error';
}
```

- Elixir

```elixir
# very common in elixir to use case statements
# to match on the results of a function.
# In this example get_a_response returns a tuple
# where the first element is the status

case get_a_response do
  {:ok, _} -> "Success"
  {:error, :not_allowed} -> "Not Allowed"
  {:error, _} -> "There was an error"
end

cond do
  foo == "foo" -> "Success"
  bar < 1 -> "Not Allowed"
  _ -> "There was an error" # _ is a catch all
end
```

In addition to case statements, elixir also has the cond statement,
which is usefull for checking different conditions and
finding the first one that isn't nil or false, similar to else if statements.

### If Else Statements

- Javascript

```javascript
if (n > 100) {
  return 100;
} else {
  return n;
}

n > 100 ? 100 : n;
```

- Elixir

```elixir
if n > 100 do
  100
else
  n
end

# unless keyworkd is a sort of reverse if
unless n > 100 do
  n
else
  100
end

# ternary isn't really directly supported, but
# this version of an if statement is close
if n > 100, do: 100, else: n
```

With block is also available for if statements but not shown here.

## Common List Operations

### Map

- Javascript

```javascript
const a = [1, 2, 3];
const b = a.map((n) => n * 2);
// [2, 4, 6]
```

- Elixir

```elixir
a = [1, 2, 3]
b = Enum.map(a, fn n -> n * 2 end)
# [2, 4, 6]
```

### Filter

- Javascript

```javascript
const people = [
  { name: 'Bob', age: 30 },
  { name: 'Bill', age: 18 },
];
const oldEnough = people.filter(({ age }) => age > 21);
```

- Elixir

```elixir
people = [%{name: "Bob", age: 30}, %{name: "Bill", age: 18}]
old_enough = Enum.filter(people, fn %{age: age} -> age > 21 end)
```

### Reduce

- Javascript

```javascript
const sum = [1, 2, 3].reduce((acc, n) => acc + n, 0);
// 6
```

- Elixir

```elixir
sum = Enum.reduce([1, 2, 3], 0, fn n, acc -> acc + n end)
# 6
```
