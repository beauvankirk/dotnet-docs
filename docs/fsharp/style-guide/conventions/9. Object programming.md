## Object programming

F# has full support for objects and object-oriented (OO) concepts. Although many OO concepts are powerful and useful, not all of them are ideal to use. The following lists offer guidance on categories of OO features at a high level.

**Consider using these features in many situations:**

* Dot notation (`x.Length`)
* Instance members
* Implicit constructors
* Static members
* Indexer notation (`arr.[x]`)
* Named and Optional arguments
* Interfaces and interface implementations

**Don't reach for these features first, but do judiciously apply them when they are convenient to solve a problem:**

* Method overloading
* Encapsulated mutable data
* Operators on types
* Auto properties
* Implementing `IDisposable` and `IEnumerable`
* Type extensions
* Events
* Structs
* Delegates
* Enums

**Generally avoid these features unless you must use them:**

* Inheritance-based type hierarchies and implementation inheritance
* Nulls and `Unchecked.defaultof<_>`

### Prefer composition over inheritance

[Composition over inheritance](https://en.wikipedia.org/wiki/Composition_over_inheritance) is a long-standing idiom that good F# code can adhere to. The fundamental principle is that you should not expose a base class and force callers to inherit from that base class to get functionality.

### Use object expressions to implement interfaces if you don't need a class

[Object Expressions](../language-reference/object-expressions.md) allow you to implement interfaces on the fly, binding the implemented interface to a value without needing to do so inside of a class. This is convenient, especially if you _only_ need to implement the interface and have no need for a full class.

For example, here is the code that is run in [Ionide](https://ionide.io/) to provide a code fix action if you've added a symbol that you don't have an `open` statement for:

```fsharp
    let private createProvider () =
        { new CodeActionProvider with
            member this.provideCodeActions(doc, range, context, ct) =
                let diagnostics = context.diagnostics
                let diagnostic = diagnostics |> Seq.tryFind (fun d -> d.message.Contains "Unused open statement")
                let res =
                    match diagnostic with
                    | None -> [||]
                    | Some d ->
                        let line = doc.lineAt d.range.start.line
                        let cmd = createEmpty<Command>
                        cmd.title <- "Remove unused open"
                        cmd.command <- "fsharp.unusedOpenFix"
                        cmd.arguments <- Some ([| doc |> unbox; line.range |> unbox; |] |> ResizeArray)
                        [|cmd |]
                res
                |> ResizeArray
                |> U2.Case1
        }
```

Because there is no need for a class when interacting with the Visual Studio Code API, Object Expressions are an ideal tool for this. They are also valuable for unit testing, when you want to stub out an interface with test routines in an ad hoc manner.

