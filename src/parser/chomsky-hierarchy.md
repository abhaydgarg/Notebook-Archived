# Chomsky hierarchy

## Formal grammars

> Grammars of computer science.

A formal grammar of this type consists of a finite set of production rules `left-hand side → right-hand side`, where each side consists of a finite sequence of the following symbols:

1. Terminal
2. Non-terminal

## The hierarchy

![The hierarchy](assets/chomsky-hierarchy.png)

> See grammar just means a set of rules which must be complied in order to form valid syntax or sentence. In Chomsky hierarchy, you can think of it as different rules are applied at different level of hierarchy.
>
> Rule => Restrictions. At each level what kind of restrictions and constraints are applied which you cannot break in order to have valid syntax form.
>
> Think of hierarchy in terms of restrictions. How terminal and non-terminal can be arranged and put together at `LHS` and `RHS` to form production rule.

Each inner level has a superset.

### Type-0 grammars

> Unrestricted grammar | recursively enumerable

As a name suggests `Unrestricted grammar`, there are no restrictions in using terminal and non-terminal symbols to form production rule.

#### Restrictions in production rule

No restrictions.

* **LHS** - Can have any number of terminal and non-terminal symbols to form string and it must have atleast one non-terminal symbol.
* **RHS** - Can have any number of terminal and non-terminal symbols to form a string.

### Type-1 grammars

> Context-sensitive grammar | Length increasing | non contracting grammar

left-hand sides and right-hand sides of any production rules are surrounded by a context of terminal and nonterminal symbols.

#### Restrictions in production rule

* **LHS** - Can have any number of terminal and non-terminal symbols to form string and it must have atleast one non-terminal symbol.
* **RHS** - Can have any number of terminal and non-terminal symbols to form a string.
* **LHS ≤ RHS** - Length of LHS should be less than or equal to length of RHS.

```
# Non valid rule - because RHS is empty or zero and LHS has one non terminal symbol. It is contracting which result in LHS > RHS.
S → ε (where ε is the empty string or epsilon)

# Valid rules
A → a
B → Sba
```

### Type-2 grammars

> Context-free grammar
>
> Basis of parsing. Represent language constructs. This is the grammar which is used to write the production rule for programming language.

#### Restrictions in production rule

* **LHS** - Must have one non-terminal and not more than one. Terminal symbol is not allowed.
* **RHS** - Can have any number of terminal and non-terminal symbols to form a string.

```
# Non valid rules
AS → ε // LHS has two non-terminal symbols.
Ab → Sa // LHS has terminal symbol.

# Valid rules
S → ε
A → a
B → Sba
```

### Type-3 grammars

> Regular grammar.
>
> Basis of lexical analysis. Used by tokenizer. Obtain by regular expression.

There are two types:

#### Left linear grammar

##### Restrictions in production rule

* **LHS** - Must have one non-terminal and not more than one. Terminal symbol is not allowed.
* **RHS** - Can have zero or more terminal symbols. If want to use non-terminal then it must not be more than one. only one non-terminal is allowed and it must be the first symbol in the rule.

```
# Non valid rules
A -> aB // B non-terminal symbol is not the first symbol.

# Valid rules
A -> a // Only terminal.
A -> Ba // Only one non-terminal and is a first symbol.
```

#### Right linear grammar

##### Restrictions in production rule

* **LHS** - Must have one non-terminal and not more than one. Terminal symbol is not allowed.
* **RHS** - Can have zero or more terminal symbols. If want to use non-terminal then it must not be more than one. only one non-terminal is allowed and it must be the last symbol in the rule.

```
# Non valid rules
A -> Ba // B non-terminal symbol is not the last symbol.

# Valid rules
A -> a // Only terminal.
A -> aB // Only one non-terminal and is a last symbol.
```
