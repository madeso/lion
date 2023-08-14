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
Nil or null is represented by either `( )`, `[ ]`, `{ }` or `< >`. It can be consiederd as a empty object without a name symbol.

## Compared to

### XML
LION `(foo bar: "baz")` vs XML `<foo bar="baz"/>` are similar, but differences and the short LION syntax is more visible when we want to set a element/object instead of a string for `bar`.
With LION this is easy: `(foo bar: (baz))` but XML can't.

XAML has solved this with custom syntax inside the attribute or sub elements with dot notation to differ it from the rest of the sub elements. Like the following (reduced) from [msdn](https://learn.microsoft.com/en-us/dotnet/api/system.windows.data.binding.source?view=windowsdesktop-7.0) where we set the text property (dot syntax) to a source binding that comes from a resource(custom format).
```xaml
<TextBox>
    <TextBox.Text>
        <Binding Source="{StaticResource myDataSource}"/>
    </TextBox.Text>
</TextBox>
```

So our xml example would be:
XML
```xml
<foo>
    <foo.bar>
        <baz/>
    </foo.bar>
</foo>
```

For completion, the textbox xaml would be written in LION like (using begin/end that match xml):
```
<TextBox
    Text: <Binding Source: {StaticResource myDataSource}>
>
```

### JSON
The following LION object:
```lisp
(foobar
    foo: "bar"
    (baz)
)
```

Is represented in xml as the following:
```xml
<foobar
    foo="bar">
    <baz />
</foobar>
```

A bit wordy but not by alot. JSON on the other is written as:
```json
{
    "$type": "foobar",
    "foo": "bar",
    "$body": [
        {"$type": "baz"}
    ]
}
```

Since all objects are anonymous, we need to specify a type. Just like we do in LION we follow the name with the named properties and then the indexed properties. Json can't handle indexes inside a object so we need to place those in  a body object. Since our named values might collide with type and body we must make it unique and prefix it with a `$` is [done elsewhere](https://www.newtonsoft.com/json/help/html/serializetypenamehandling.htm) and we just assume there are less users naming their properties `$type` than `type`.
Also take note that in both LION and xml both values are indented, but in json, the of the 3 rows that are indented one step 1/3 entries are actual values, and at 2 indentations we have the rest of the values.


 
## todo
* Add better description and railroad diagrams to syntax section

