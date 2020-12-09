## Consider Type Abbreviations to shorten signatures

[Type Abbreviations](../language-reference/type-abbreviations.md) are a convenient way to assign a label to another type, such as a function signature or a more complex type. For example, the following alias assigns a label to what's needed to define a computation with [CNTK](/cognitive-toolkit/), a deep learning library:

```fsharp
open CNTK

// DeviceDescriptor, Variable, and Function all come from CNTK
type Computation = DeviceDescriptor -> Variable -> Function
```

The `Computation` name is a convenient way to denote any function that matches the signature it is aliasing. Using Type Abbreviations like this is convenient and allows for more succinct code.

### Avoid using Type Abbreviations to represent your domain

Although Type Abbreviations are convenient for giving a name to function signatures, they can be confusing when abbreviating other types. Consider this abbreviation:

```fsharp
// Does not actually abstract integers.
type BufferSize = int
```

This can be confusing in multiple ways:

* `BufferSize` is not an abstraction; it is just another name for an integer.
* If `BufferSize` is exposed in a public API, it can easily be misinterpreted to mean more than just `int`. Generally, domain types have multiple attributes to them and are not primitive types like `int`. This abbreviation violates that assumption.
* The casing of `BufferSize` (PascalCase) implies that this type holds more data.
* This alias does not offer increased clarity compared with providing a named argument to a function.
* The abbreviation will not manifest in compiled IL; it is just an integer and this alias is a compile-time construct.

```fsharp
module Networking =
    ...
    let send data (bufferSize: int) = ...
```

In summary, the pitfall with Type Abbreviations is that they are **not** abstractions over the types they are abbreviating. In the previous example, `BufferSize` is just an `int` under the covers, with no additional data, nor any benefits from the type system besides what `int` already has.

An alternative approach to using type abbreviations to represent a domain is to use single-case discriminated unions. The previous sample can be modeled as follows:

```fsharp
type BufferSize = BufferSize of int
```

If you write code that operates in terms of `BufferSize` and its underlying value, you need to construct one rather than pass in any arbitrary integer:

```fsharp
module Networking =
    ...
    let send data (BufferSize size) =
    ...
```

This reduces the likelihood of mistakenly passing an arbitrary integer into the `send` function, because the caller must construct a `BufferSize` type to wrap a value before calling the function.
