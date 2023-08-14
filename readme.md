# LIsplike Object Notation

```lisp
(lion
    description:"LION is inspired by xml but with a S-expression syntax"
    (add-xkcd-reference 927)
    (add-love)
)
```

This repo contains the specification and reference implementations for the LION format.

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

class Object : Value {
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

### Objects
Objects are surrounded by begin and end characters. They are either `( )` `[ ]` or `{ }`. They are considered equal but a begin must be matched by the corresponding end. 

Objects are seperated into 3 section:
1. The name section.
2. The named values (or properties) section.
3. The indexed values (or list) section.

#### The name
This identified the name of the object, and it is a single symbol.

#### The named values
Zero or more `symbol : value`.  For example:
* `name: foo`
* `bar: 42`

In regular S-Expressions theese would be represented as regular objects like:
* `(name foo)`
* `(bar 42)`

but much like attributes and sub-elements are different so are named and indexed values different in LION.

#### The indexed values
Zero or more values.


### Nil/null
Nil or null is represented by either `( )` `[ ]` or `{ }`. It can be consiederd as a empty object without a name symbol.

## Compared to

### XML
XML `<foo bar="baz"/>` vs LION `(foo bar: "baz")` is similar but when adding a element it has to be split into a sub-element, XAML has solved this with custom syntax inside the attribute or sub elements with dot notation to differ it from the rest of the sub elements.

XML
```xml
<foo>
    <foo.bar>
        <baz/>
    </foo.bar>
</foo>
```
vs LION `(foo bar: (baz))`

### JSON
JSON
```json
{
    "type": "foobar",
    "foo": "bar",
    "body": [
        {"type": baz}
    ]
}
```
vs
LION
```lisp
(foobar
    foo: "bar"
    (baz)
)
```

In reality, you probably want to escape the special `type` and `body` in json to not conflict with properties but are ommited here. `$type`, for example, is sometimes used to avoid conflict elsewhere.

   

 
## todo
* Add better description and railroad diagrams to syntax section

