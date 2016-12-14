# accessors

[![Build Status](https://travis-ci.org/funkwerk/accessors.svg?branch=master)](https://travis-ci.org/funkwerk/accessors)
[![License](https://img.shields.io/badge/license-BSL_1.0-blue.svg)](https://raw.githubusercontent.com/funkwerk/accessors/master/LICENSE)

`accessors` module allows to generate getters and setters automatically.

## Usage

```d
import accessors;

class WithAccessors
{
    @Read @Write
    private int num_;

    mixin(GenerateFieldAccessors);
}
```

`@Read` and `@Write` generate the following two methods:

```d
public final inout(int) num() inout
{
    return this.num_;
}

public final void num(int num)
{
    this.num_ = num;
}
```

The accessors names are derived from the appropriate member variables by
removing an underscore from the beginning or the end of the variable name.

### Available user defined attributes

* `@Read`
* `@RefRead`
* `@ConstRead`
* `@Write`

As you can see there are multiple attributes that can be used to generate
getters and only one for the setters. The getters differ by the return
type. `@RefRead` returns a `ref` to the member variable, `@ConstRead`
returns a `const` value.

### Accessor visibility

Visibility of the generated accessors is by default `public`, but it can be
changed. In order to achieve this, you have to pass `public`, `protected`,
`private` or `package` as argument to the attribute:

```d
import accessors;

class WithAccessors
{
    @Read("public") @Write("protected")
    private int num_;

    mixin(GenerateFieldAccessors);
}
```

## Example

```d
import accessors;
import std.stdio;

class Person
{
    @Read @Write
    private uint age_;

    @ConstRead
    private string name_;

    this(in string name, in uint age = 0)
    {
        this.name_ = name;
        this.age_ = age;
    }

    mixin(GenerateFieldAccessors);
}

void main()
{
    auto person = new Person("Saul Kripke");

    person.age = 57;

    writeln(person.name, ": ", person.age);
}
```

## Bugs

If you experience compile-time problems, open an
[issue](https://github.com/funkwerk/accessors/issues) with the
information about the type of the member variable fails.
