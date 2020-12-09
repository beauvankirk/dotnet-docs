## Overview

This document looks at some of the issues related to F# component design and coding. A component can mean any of the following:

* A layer in your F# project that has external consumers within that project.
* A library intended for consumption by F# code across assembly boundaries.
* A library intended for consumption by any .NET language across assembly boundaries.
* A library intended for distribution via a package repository, such as [NuGet](https://nuget.org).

Techniques described in this article follow the [Five principles of good F# code](index.md#five-principles-of-good-f-code), and thus utilize both functional and object programming as appropriate.

Regardless of the methodology, the component and library designer faces a number of practical and prosaic issues when trying to craft an API that is most easily usable by developers. Conscientious application of the [.NET Library Design Guidelines](../../standard/design-guidelines/index.md) will steer you towards creating a consistent set of APIs that are pleasant to consume.

