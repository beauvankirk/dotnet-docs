## Access control

F# has multiple options for [Access control](../language-reference/access-control.md), inherited from what is available in the .NET runtime. These are not just usable for types - you can use them for functions, too.

* Prefer non-`public` types and members until you need them to be publicly consumable. This also minimizes what consumers couple to.
* Strive to keep all helper functionality `private`.
* Consider the use of `[<AutoOpen>]` on a private module of helper functions if they become numerous.

