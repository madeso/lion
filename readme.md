# LIsplike Object Notation

```lisp
(lion
    description="Lion is inspired by xml but with a S-expression syntax and types inspired(aka copied) by TOML"
    (with-xkcd-reference 927)
    (made-with-love true)
)
```

This repo contains the specification and reference implementations for the lion format.

![xkcd comic about standards, please see explainxkcd link below](xkcd.png)

https://xkcd.com/927/

https://www.explainxkcd.com/wiki/index.php/927:_Standards

## Simple DOM representation

A DOM could be represented with the following c#

```csharp
interface Value {}

class Nil : Value {}
class String : Value {}
class Symbol : Value {}
class Bool : Value {}
class Int : Value {}
class Float : Value {}

class Node : Value {
    string Name;
    Dictionary<string, Value> NamedValues;
    List<Value> IndexValues;
}
```

## Syntax

* LION is case-sensitive.
* A LION file must be a valid UTF-8 encoded Unicode document.
* Whitespace means tab (0x09), space (0x20), LF (0x0A), CRLF (0x0D 0x0A) or comma (0x2C).

### Symbol
Symbols are c++ like identifiers that also can start and include operators. Valid symbols are `fred`, `the_dogs_are_good`, `cats-are-awesome` and `+`.

### String
Strings are `"double quoted"`.

### Bool
Bools are either `true` or `false`.


### Integer
### Floats

### Nodes
Nodes are surrounded by begin and end characters. They are either `( )` `[ ]` or `{ }`. They are considered equal but a begin must be matched by the corresponding end. 

Nodes are seperated into 3 section:
1. The name section.
2. The named values (or properties) section.
3. The indexed values (or list) section.

#### The name
This identified the anem of the node, and it is a single symbol.

#### The named values
Zero or more `symbol = value`.

#### The indexed values
Zero or more values.


### Nil/null
Nil or null is represented by either `( )` `[ ]` or `{ }`. It can be consiederd as a empty node without a name symbol.


## todo
* Add better description and railroad diagrams to syntax section
* Add samples comparing with xml and json.

