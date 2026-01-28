# Runtime & performance

**Tip 1 – Array.Empty for zero-allocation empties**
When you need to return an empty collection, use `Array.Empty<T>()` or `Enumerable.Empty<T>()` instead of creating a new empty array or list each time. This ensures the empty instance is allocated once and reused, which cuts down on allocations and GC pressure.
*Categories:* runtime-performance, collections
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (00:35)

**Tip 3 – SemaphoreSlim for async-safe locking**
The `lock` keyword cannot be used with `await`. Instead, use a `SemaphoreSlim` initialized with 1 to create an async-safe lock. Await `WaitAsync` before the critical section and always call `Release` in a `finally` block to prevent deadlocks.
*Categories:* threading, async, runtime-performance
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (02:15)

**Tip 4 – Avoid multiple LINQ enumerations**
Deferred execution means a LINQ query runs each time it is enumerated. Calling methods like `Count()` and `All()` on the same query triggers multiple executions. Materialize the query to a list or array first to avoid redundant processing.
*Categories:* LINQ, runtime-performance
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (03:11)

**Tip 6 – Use CollectionsMarshal.AsSpan on List**
`CollectionsMarshal.AsSpan(list)` gives you a `Span<T>` over a list’s internal array without copying. This allows for high-performance processing but is unsafe if the list size changes during iteration.
*Categories:* runtime-performance, collections, span
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (04:41)

**Tip 9 – Choose ToList vs ToArray deliberately**
If the consumer needs to add/remove items, return a `List<T>`. If the collection is fixed-size, return an array. Note that .NET 9 significantly improves `ToArray()` performance for large collections.
*Categories:* collections, runtime-performance
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (06:38)

**Tip 113 – Pre-size List capacity**
If you know roughly how many items you will add to a list, set the capacity in the constructor (e.g., `new List<int>(100)`). This prevents the list from resizing its internal array multiple times, saving CPU and memory.
*Categories:* runtime-performance, collections
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (06:16)

**Tip 116 – Use ConcurrentDictionary over manual locking**
Instead of manually locking a standard `Dictionary` in multi-threaded scenarios, use `ConcurrentDictionary`. It is built for concurrency, handles thread safety internally, and offers atomic operations like `GetOrAdd`.
*Categories:* threading, runtime-performance, collections
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (07:38)

**Tip 117 – Pre-size Dictionary capacity**
Just like lists, dictionaries resize and rehash when they grow beyond their capacity, which is expensive. Pass the estimated size to the constructor to allocate the memory once.
*Categories:* runtime-performance, collections
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (08:04)

**Tip 118 – Use TrimExcess to free memory**
If you allocated a large capacity for a list (e.g., 1,000 slots) but only used a few (e.g., 10), the unused memory is wasted. Call `TrimExcess()` to shrink the internal array to match the actual count.
*Categories:* runtime-performance, memory
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (08:25)

# Design & architecture

**Tip 2 – Rethrow exceptions properly**
Using `throw ex;` resets the stack trace to the current line. Always use `throw;` (without the variable) inside a catch block to preserve the original stack trace for debugging.
*Categories:* error-handling, design
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (01:23)

**Tip 7 – Structured logging templates**
Do not use string interpolation in logger messages. Use templates like `LogInformation("User {UserId}", id)`. This allows logging providers to capture named properties for filtering and avoids unnecessary string allocations.
*Categories:* logging, design
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (05:22)

**Tip 10 – Assembly markers for DI**
Instead of using `Program` or random classes as markers for assembly scanning (e.g., with MediatR), create a dedicated empty interface like `IAssemblyMarker`. This makes your registration intent explicit and refactor-safe.
*Categories:* architecture, DI, design
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (07:19)

**Tip 15 – Use built-in CancellationTokens in ASP.NET**
Don't create your own cancellation tokens in API controllers. Simply add `CancellationToken` as a parameter to your action method. ASP.NET Core will automatically inject a token linked to the client's request lifetime.
*Categories:* asp.net, api-design, async
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (10:23)

**Tip 24 – Enforce architecture with NetArchTest**
You don't need separate projects to enforce boundaries. Use `NetArchTest.Rules` to write unit tests that assert which namespaces can depend on others (e.g., "Domain cannot depend on Infrastructure").
*Categories:* architecture, testing
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (15:31)

**Tip 106 – Avoid boolean parameters**
Methods like `User.SetActive(true)` obscure intent. Prefer explicit methods like `User.Activate()` and `User.Deactivate()` to make the calling code self-documenting and readable.
*Categories:* clean-code, api-design
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (03:13)

**Tip 114 – Structured logging over interpolation**
(Reiteration of Tip 7) String interpolation allocates memory even if logging is disabled. Use message templates to avoid allocation and enable structured data querying in tools like Seq or Kibana.
*Categories:* logging, design, performance
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (06:48)

**Tip 115 – Avoid async work in constructors**
Calling `.Result` or `.GetAwaiter().GetResult()` in a constructor to run async code blocks the thread and risks deadlocks. Use an asynchronous factory method (e.g., `CreateAsync`) instead.
*Categories:* design, async, best-practices
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (07:13)

**Tip 121 – Immutable properties with Init**
Properties with `get; set;` can be changed by anyone. Use `get; init;` to allow setting the value only during object initialization, enforcing immutability and safer API design.
*Categories:* design, immutability
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (10:03)

**Tip 123 – Dispose CancellationTokenSource**
`CancellationTokenSource` often uses a timer under the hood (especially with timeouts). If you don't dispose it, you risk leaking memory and timer resources.
*Categories:* async, memory-management, best-practices
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (10:42)

# Language & syntax

**Tip 8 – Empty types with semicolon**
C# 12 allows declaring empty classes or interfaces with a semicolon (e.g., `public class Marker;`) instead of empty braces, reducing boilerplate for marker types.
*Categories:* language, syntax
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (06:07)

**Tip 11 – StringSyntax attribute**
Use `[StringSyntax(StringSyntaxAttribute.Regex)]` (or JSON, Date) on string parameters to tell the IDE to provide syntax highlighting and validation for the string literal passed in.
*Categories:* language, tooling
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (08:00)

**Tip 12 – Primary Constructors**
C# 12 Primary Constructors let you define constructor parameters directly on the class declaration. Be aware that these parameters are not read-only fields by default; they are captured in the class scope.
*Categories:* language, syntax, csharp-12
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (08:35)

**Tip 14 – Smallest valid C# program**
Since top-level statements were introduced, the smallest valid C# program is an empty file (or a file with a single semicolon). The compiler generates the entry point for you.
*Categories:* language, trivia
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (09:53)

**Tip 16 – Collection expressions**
C# 12 introduced collection expressions (`[...]`). You can now initialize arrays, lists, and spans using simple square brackets: `int[] nums = [1, 2, 3];`.
*Categories:* language, syntax, csharp-12
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (10:57)

**Tip 22 – Alias any type**
C# 12 allows you to alias any type, including tuples and pointers, using the `using` directive (e.g., `using UserList = System.Collections.Generic.List<User>;`). This simplifies complex type names.
*Categories:* language, syntax
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (14:12)

**Tip 23 – DateTimeOffset over DateTime**
`DateTime` is ambiguous regarding time zones. Always prefer `DateTimeOffset` for absolute points in time, as it includes the offset from UTC, preventing conversion errors.
*Categories:* language, datetime, best-practices
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (14:50)

**Tip 101 – Discards for unused deconstruction**
Use the underscore `_` (discard) when deconstructing tuples or objects to ignore values you don't need. This avoids unused variable warnings and clarifies intent.
*Categories:* language, syntax, clean-code
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (01:03)

**Tip 102 – Raw string literals**
Use triple quotes `"""` to create raw string literals. They preserve whitespace and ignore escape characters, making them perfect for JSON, SQL, or XML.
*Categories:* language, syntax, strings
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (01:27)

**Tip 103 – SetsRequiredMembers attribute**
If you have `required` properties but initialize them via a custom constructor, use the `[SetsRequiredMembers]` attribute on the constructor to suppress compiler warnings about missing properties.
*Categories:* language, compiler
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (01:58)

**Tip 104 – Init keyword polyfill**
You can use the `init` keyword on older runtimes (like .NET Standard 2.0) by defining a dummy class `namespace System.Runtime.CompilerServices { internal static class IsExternalInit {} }`.
*Categories:* language, compatibility
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (02:22)

**Tip 105 – CollectionBuilder attribute**
You can enable C# 12 collection expressions (`[...]`) for your custom types by adding the `[CollectionBuilder]` attribute, pointing to a static factory method.
*Categories:* language, syntax
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (02:51)

**Tip 108 – String concatenation with null**
In C#, `"Hello" + null` results in `"Hello"`. It does not throw an exception; it treats null as an empty string. Be aware of this behavior to avoid masking missing data bugs.
*Categories:* language, strings, gotchas
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (04:07)

**Tip 109 – Static abstract members**
C# 11 interfaces can have `static abstract` members. This is the foundation of Generic Math, allowing you to define static contracts (like `Parse` or operators) on generic types.
*Categories:* language, generics, architecture
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (04:38)

**Tip 110 – The field keyword**
(C# 13 Preview) The `field` keyword allows you to access the compiler-generated backing field of a property within its accessor, removing the need to manually declare private fields for simple logic.
*Categories:* language, syntax, csharp-13
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (04:58)

**Tip 111 – Sealed classes**
Classes are open by default, but you should often mark them as `sealed` if they aren't designed for inheritance. This improves performance (JIT optimizations) and prevents misuse.
*Categories:* language, performance, design
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (05:20)

**Tip 112 – Method groups**
Instead of `x => Method(x)`, you can just pass `Method`. This is cleaner and, in recent C# versions, can be more efficient as the compiler avoids creating unnecessary delegate wrappers.
*Categories:* language, syntax, linq
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (05:51)

**Tip 120 – Default vs Class vs Struct**
`default(MyClass)` is `null`. `default(MyStruct)` is a valid struct with zeroed-out memory. Be careful when using `default` with generics, as the behavior differs drastically between reference and value types.
*Categories:* language, syntax, gotchas
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (09:29)

**Tip 122 – Swap values with tuples**
You can swap two variables without a temp variable using tuple deconstruction: `(a, b) = (b, a);`. This is syntactic sugar that is clean and readable.
*Categories:* language, syntax, tricks
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (10:18)

**Tip 124 – Pattern matching with variable declaration**
You can check a type and declare a variable in one line inside an `if` statement: `if (obj is User user && user.IsActive)`. This avoids casting and keeps scope tight.
*Categories:* language, pattern-matching
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (11:02)

**Tip 129 – Understanding Closures**
When a lambda captures a local variable, the compiler creates a hidden class to hold that variable. Be careful—keeping the lambda alive keeps the captured variable (and potentially large objects it references) alive too.
*Categories:* language, memory, gotchas
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (13:10)

**Tip 131 – Switch pattern matching**
Use tuples in switch expressions to match on multiple values at once: `(State, Action) switch { (Open, Close) => ... }`. It replaces complex nested `if` statements.
*Categories:* language, syntax, pattern-matching
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (14:02)

# Tooling & DX

**Tip 5 – C# REPL (csrepl)**
Use `dotnet-repl` or the C# Interactive window to test small snippets of code without creating a new Console Project. It supports NuGet packages and full IntelliSense.
*Categories:* tooling, productivity
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (04:03)

**Tip 13 – UUID v7 (Sortable GUIDs)**
.NET 9 introduces `Guid.CreateVersion7()`. Unlike standard GUIDs (v4), v7 is time-ordered, making it much friendlier for database indexing while maintaining uniqueness.
*Categories:* tooling, database, new-features
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (09:11)

**Tip 17 – dotnet-outdated**
Install the `dotnet-outdated` global tool to quickly scan your solution for outdated NuGet packages. It shows you exactly which version you have versus the latest available.
*Categories:* tooling, nuget, maintenance
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (11:21)

**Tip 18 – Waffle Generator**
Stop using "Lorem Ipsum". Use `WaffleGenerator` to generate realistic-looking text (or "waffle") for testing UIs. It mimics real sentence structure better than random Latin words.
*Categories:* tooling, testing, libraries
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (11:54)

**Tip 20 – NaughtyStrings**
Users will input garbage. Use the `NaughtyStrings` library to test your inputs against edge cases like SQL injection strings, zero-width characters, and massive Unicode blobs.
*Categories:* tooling, testing, security
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (13:05)

**Tip 21 – InterpolatedParser**
This library allows you to "reverse" string interpolation. You define a template like `$"Item: {name}, Price: {price}"` and it extracts the values from a string matching that format.
*Categories:* tooling, libraries
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (13:33)

**Tip 25 – Assertion libraries alternatives**
If you want to avoid FluentAssertions' new licensing fees, consider "Awesome Assertions" (a fork), "Shouldly" (a similar fluent library), or stick to the free version 7 of FluentAssertions.
*Categories:* tooling, testing, libraries
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (16:04)

**Tip 107 – Never block threads with Thread.Sleep**
Using `Thread.Sleep` blocks the thread entirely, killing scalability. In async apps, always use `await Task.Delay`, which frees the thread to do other work while waiting.
*Categories:* async, best-practices
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (03:35)

**Tip 119 – IsNullOrWhiteSpace vs IsNullOrEmpty**
`string.IsNullOrEmpty` allows strings with only spaces (which are often invalid inputs). Use `string.IsNullOrWhiteSpace` to catch nulls, empty strings, and strings that contain only spaces.
*Categories:* tooling, validation, best-practices
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (08:54)

**Tip 125 – Stopwatch.GetTimestamp for benchmarking**
Don't subtract `DateTime.Now` to measure time; it's imprecise. Use `Stopwatch.GetTimestamp()` and `Stopwatch.GetElapsedTime(start)` for high-precision, allocation-free timing.
*Categories:* tooling, benchmarking, performance
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (11:25)

# Language & Syntax

**Tip 26 – CallerArgumentExpression**
This attribute allows a method to capture the expression passed as an argument as a string. It is incredibly useful for validation libraries or logging, allowing you to print exactly which variable caused an error (e.g., `ArgumentNullException.ThrowIfNull(user)` knows the parameter was named "user").
*Categories:* language, attributes, validation
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (16:35)

**Tip 27 – CallerMemberName**
Automatically capture the name of the calling method or property. This is widely used in `INotifyPropertyChanged` implementations to avoid hardcoding property name strings, making refactoring safer.
*Categories:* language, attributes, mvvm
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (17:05)

**Tip 28 – Null Coalescing Assignment (??=)**
Use `variable ??= value` to assign a value to a variable only if it is currently null. It’s a concise way to initialize lazy-loaded fields or set defaults without verbose `if` statements.
*Categories:* language, syntax, clean-code
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (17:30)

**Tip 29 – Null Forgiving Operator (!)**
When you know a value isn't null but the compiler thinks it might be (e.g., after an external check), append `!` to the variable. This suppresses the nullable warning. Use cautiously!
*Categories:* language, syntax, safety
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (17:55)

**Tip 30 – Range Operator (..)**
Use `..` to slice arrays or lists easily. `array[1..^1]` gives you everything from index 1 to the second-to-last element, offering a cleaner syntax than `Skip` and `Take`.
*Categories:* language, syntax, collections
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (18:15)

**Tip 31 – Index from End (^)**
The hat operator `^` lets you access elements from the end of a collection. `array[^1]` gets the last item, `array[^2]` the second to last, etc.
*Categories:* language, syntax, collections
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (18:38)

**Tip 38 – Static Anonymous Functions**
Mark lambdas as `static` (e.g., `static x => x * x`) to prevent them from accidentally capturing local variables (closures). This avoids hidden memory allocations and unintended side effects.
*Categories:* language, performance, memory
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (22:45)

**Tip 40 – Using Declarations**
Instead of wrapping `using` blocks in braces, simply write `using var x = new X();`. The object is disposed when it goes out of the current scope (usually the method end). This reduces indentation levels.
*Categories:* language, syntax, clean-code
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (23:45)

**Tip 41 – File-scoped Namespaces**
In C# 10+, you can declare `namespace MyNamespace;` at the top of the file to apply it to the whole file. This saves one level of indentation for your entire class.
*Categories:* language, syntax, clean-code
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (24:10)

**Tip 42 – Global Usings**
Move common using statements (like `System`, `System.Linq`) to a separate file and mark them `global using`. They will apply to the entire project, decluttering individual files.
*Categories:* language, syntax, project-structure
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (24:35)

**Tip 126 – CountBy (NET 9)**
.NET 9 introduces `CountBy`, allowing you to count occurrences of keys in a collection without a complex `GroupBy` and `Select` chain. It’s cleaner and more efficient.
*Categories:* language, linq, csharp-13
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (11:55)

**Tip 127 – AggregateBy (NET 9)**
Similar to `CountBy`, `AggregateBy` lets you perform aggregations (like Sum or Max) on groups without the overhead of grouping the entire collection first.
*Categories:* language, linq, csharp-13
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (12:20)

**Tip 130 – Interceptors (C# 12)**
Interceptors allow you to swap out method calls at compile time. This is an advanced feature primarily for source generator authors to optimize code paths without changing the original source.
*Categories:* language, compiler, advanced
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (13:35)

**Tip 131 – Inline Arrays**
C# 12 introduces inline arrays, which are fixed-size struct arrays that don't incur heap allocations. They are useful for high-performance scenarios where you need a small buffer.
*Categories:* language, performance, memory
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (14:02)

# Runtime & Performance

**Tip 32 – StackAlloc**
For small arrays, use `stackalloc` to allocate memory on the stack instead of the heap. This avoids GC pressure entirely. Use with `Span<T>` for safe access.
*Categories:* runtime-performance, memory, unsafe
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (19:10)

**Tip 33 – SkipLocalsInit**
By default, C# zero-initializes all local variables. Using `[SkipLocalsInit]` bypasses this, offering a tiny performance boost in high-frequency loops where you overwrite the data immediately anyway.
*Categories:* runtime-performance, advanced, attributes
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (19:45)

**Tip 34 – MethodImplOptions.AggressiveInlining**
You can hint to the JIT compiler to inline a method using `[MethodImpl(MethodImplOptions.AggressiveInlining)]`. This removes the method call overhead but can increase binary size, so use it only for small, hot methods.
*Categories:* runtime-performance, jit, attributes
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (20:15)

**Tip 47 – ArrayPool**
Instead of creating new large arrays repeatedly, use `ArrayPool<T>.Shared.Rent(size)`. This reuses arrays from a pool, drastically reducing GC pressure in high-throughput apps. Don't forget to `Return`!
*Categories:* runtime-performance, memory, pooling
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (27:10)

**Tip 50 – Frozen Collections**
.NET 8 introduces `FrozenDictionary` and `FrozenSet`. These are immutable and optimized for read-heavy workloads, offering much faster lookups than standard dictionaries once initialized.
*Categories:* runtime-performance, collections, net8
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (29:15)

# Tooling & Debugging

**Tip 35 – Conditional Attribute**
Use `[Conditional("DEBUG")]` on methods that should only run during debugging (like logging). In Release mode, the compiler completely removes calls to these methods, so they cost zero performance.
*Categories:* debugging, compiler, attributes
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (20:50)

**Tip 36 – Obsolete Attribute**
Mark old methods with `[Obsolete("Message", true)]`. The `true` parameter causes a compiler error (not just a warning), forcing developers to stop using the deprecated code immediately.
*Categories:* tooling, maintenance, attributes
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (21:20)

**Tip 147 – Rate Limiting middleware**
.NET 7+ has built-in Rate Limiting middleware. You can define policies (like fixed window, token bucket) to protect your API from abuse without needing 3rd party libraries.
*Categories:* asp.net, security, middleware
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (21:10)

**Tip 148 – YARP (Yet Another Reverse Proxy)**
YARP is a Microsoft-supported library for building custom reverse proxies. It is highly performant and fully customizable in C#, making it a great alternative to Nginx for .NET shops.
*Categories:* architecture, networking, tools
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (21:45)

**Tip 150 – Health Checks**
Use the built-in Health Checks middleware to expose endpoints that load balancers can monitor. You can easily add checks for DB connectivity, disk space, or external APIs.
*Categories:* asp.net, devops, monitoring
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (22:30)

# Data & Entity Framework

**Tip 132 – AsNoTracking**
(Repeated for emphasis in video context) For read-only queries in EF Core, always use `AsNoTracking()`. It bypasses the change tracker, reducing memory usage and CPU time significantly.
*Categories:* ef-core, performance, database
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (14:23)

**Tip 133 – ExecuteUpdate**
EF Core 7+ supports `ExecuteUpdate`, allowing you to update multiple rows in a single SQL command without loading them into memory first.
*Categories:* ef-core, performance, sql
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (14:50)

**Tip 134 – ExecuteDelete**
Similar to `ExecuteUpdate`, `ExecuteDelete` lets you delete many rows directly in the database with one command, skipping the fetch-track-delete cycle.
*Categories:* ef-core, performance, sql
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (15:15)

**Tip 136 – DbContext Pooling**
Creating a `DbContext` is cheap but not free. Use `AddDbContextPool` to recycle context instances, which reduces object allocation overhead in high-traffic APIs.
*Categories:* ef-core, performance, dependency-injection
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (16:05)

**Tip 139 – Split Queries**
For queries with many joins (collections), standard EF behavior (Cartesian explosion) can be slow. Use `.AsSplitQuery()` to load related data in separate SQL queries, which is often faster and uses less bandwidth.
*Categories:* ef-core, performance, sql
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (17:30)

# Language & Pattern Matching

**Tip 51 – Switch Expressions**
Replace verbose `switch` statements with concise `switch` expressions. They return a value directly and force you to handle all cases (or use a discard `_`), making your code cleaner and safer.
*Categories:* language, syntax, clean-code
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (30:05)

**Tip 52 – Property Pattern Matching**
You can match on properties inside an object directly in a `switch` or `is` expression: `if (person is { Age: > 18, Name: "Eric" })`. This avoids nested `if` blocks.
*Categories:* language, pattern-matching
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (30:45)

**Tip 53 – Logical Patterns (and/or/not)**
Combine patterns using `and`, `or`, and `not`. Example: `if (input is not null and > 0)`. This reads almost like English.
*Categories:* language, pattern-matching
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (31:20)

**Tip 54 – Positional Patterns (Deconstruction)**
If a class implements `Deconstruct`, you can match based on the order of values: `if (point is (0, 0))`. Great for coordinates or simple data structures.
*Categories:* language, pattern-matching
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (31:55)

**Tip 61 – Records (Reference Types)**
Use `record` for immutable data objects. They give you value-based equality, `ToString()` formatting, and non-destructive mutation (`with` expressions) automatically.
*Categories:* language, architecture, clean-code
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (35:10)

**Tip 62 – Record Structs**
C# 10 introduced `record struct`. These are value types (stack-allocated) with the same benefits as records (equality, printing). Use `readonly record struct` for maximum performance and immutability.
*Categories:* language, performance, syntax
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (35:45)

# Async & Threading

**Tip 56 – ValueTask**
If your async method often returns a result immediately (synchronously), return `ValueTask<T>` instead of `Task<T>`. This avoids allocating a `Task` object on the heap when the operation completes synchronously.
*Categories:* async, performance, memory
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (33:10)

**Tip 57 – IAsyncEnumerable**
Don't return `Task<List<T>>` if you are streaming data. Use `IAsyncEnumerable<T>` with `yield return` to stream items one by one asynchronously, improving perceived performance and memory usage.
*Categories:* async, collections, performance
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (33:45)

**Tip 58 – CancellationTokens in Iterators**
When using `IAsyncEnumerable`, apply the `[EnumeratorCancellation]` attribute to your token parameter so that `WithCancellation` works correctly on the consumer side.
*Categories:* async, best-practices
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (34:20)

**Tip 153 – Channels for Background Jobs**
(Deep Dive) `System.Threading.Channels` is the best way to decouple producers (API requests) from consumers (background workers). It handles backpressure and concurrency much better than `BlockingCollection`.
*Categories:* threading, architecture, performance
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (24:15)

**Tip 154 – Parallel.ForEachAsync**
.NET 6 added `Parallel.ForEachAsync`. It allows you to run async work in parallel with a degree of parallelism (DOP) limit. It is much safer and easier than `Task.WhenAll` on a huge set of tasks.
*Categories:* async, performance, parallel
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (24:50)

# Security & Web API

**Tip 70 – User Secrets**
Never commit passwords or API keys to Git. Use the "User Secrets" tool in Visual Studio (`dotnet user-secrets`) to store sensitive config locally on your machine during development.
*Categories:* security, tooling, devops
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (40:10)

**Tip 158 – Minimal APIs**
Minimal APIs remove the MVC boilerplate (Controllers). They are faster to start up and easier to write for microservices. You can define endpoints directly in `Program.cs`.
*Categories:* asp.net, api-design, net6
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (26:40)

**Tip 160 – TypedResults**
In Minimal APIs, return `TypedResults.Ok(data)` instead of `Results.Ok(data)`. This allows unit tests to easily assert the result type and content without casting.
*Categories:* asp.net, testing, clean-code
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (27:30)

**Tip 165 – API Versioning**
Always version your APIs. Use the `Asp.Versioning.Http` package. It supports URL versioning, header versioning, and query string versioning to prevent breaking changes for clients.
*Categories:* api-design, architecture
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (30:10)

**Tip 168 – Global Exception Handling**
Use `IExceptionHandler` in .NET 8+ to handle exceptions globally. It replaces the older middleware approach and provides a standardized way to return "Problem Details" responses.
*Categories:* asp.net, error-handling, design
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (31:20)

**Tip 171 – OpenTelemetry**
Move away from proprietary logging agents. Use OpenTelemetry (OTEL) to collect metrics, logs, and traces in a vendor-neutral format that can be exported to Prometheus, Jaeger, or any observability tool.
*Categories:* devops, monitoring, architecture
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (32:45)

# Advanced & Architecture

**Tip 76 – Dependency Injection Scopes**
Understand `Transient` (new every time), `Scoped` (new per HTTP request), and `Singleton` (one forever). Mixing them wrong (e.g., injecting Scoped into Singleton) causes "Captive Dependencies" and memory leaks.
*Categories:* architecture, DI, best-practices
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (42:30)

**Tip 77 – Keyed Services**
.NET 8 added Keyed Services. You can now register multiple implementations of the same interface (e.g., `ICache`) and retrieve a specific one by key: `[FromKeyedServices("redis")] ICache cache`.
*Categories:* architecture, DI, net8
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (43:10)

**Tip 80 – Options Pattern**
Don't read `IConfiguration` directly in classes. Bind config sections to strongly-typed classes using `IOptions<T>`, `IOptionsSnapshot<T>` (reloads per request), or `IOptionsMonitor<T>` (real-time updates).
*Categories:* architecture, configuration, clean-code
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (44:20)

**Tip 81 – Options Validation**
Add validation to your options classes using DataAnnotations (e.g., `[Required]`). Call `ValidateDataAnnotations()` on registration so the app crashes at startup if config is missing, rather than runtime.
*Categories:* architecture, validation, safety
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (44:55)

**Tip 180 – MediatR Pipeline Behaviors**
Use Pipeline Behaviors in MediatR to implement cross-cutting concerns (logging, validation, caching) for all your commands/queries in one place, keeping your handlers clean.
*Categories:* architecture, patterns, clean-code
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (36:10)

**Tip 185 – Vertical Slice Architecture**
Consider Vertical Slice Architecture over Clean Architecture. Group code by feature (e.g., "CreateUser" folder containing API, Logic, and DB code) rather than technical layers. This reduces jumping between projects.
*Categories:* architecture, design
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (38:20)

**Tip 190 – REPR Pattern**
Request-Endpoint-Response pattern. Similar to Vertical Slices, this pattern treats every API endpoint as a standalone handler that takes a Request and returns a Response, simplifying API maintenance.
*Categories:* architecture, api-design
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (39:50)

# Final Polish & .NET 9

**Tip 90 – EditorConfig**
Add an `.editorconfig` file to your solution root. It enforces code style (braces, spacing, naming conventions) across the entire team, regardless of which IDE they use.
*Categories:* tooling, collaboration, consistency
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (48:00)

**Tip 91 – TreatWarningsAsErrors**
Set `<TreatWarningsAsErrors>true</TreatWarningsAsErrors>` in your CI/CD pipeline (Release build). This prevents technical debt from accumulating by forcing you to fix warnings immediately.
*Categories:* devops, quality, best-practices
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (48:30)

**Tip 195 – AOT Compilation**
Native AOT (Ahead-of-Time) compilation produces standalone binaries that start instantly and use less memory. Ideal for AWS Lambda or CLI tools, though it has limitations (no reflection/dynamic code).
*Categories:* performance, deployment, net8
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (42:00)

**Tip 199 – .NET Aspire**
A new stack for building cloud-native distributed applications. It handles orchestration, service discovery, and telemetry configuration for you, making microservice local dev much easier.
*Categories:* architecture, cloud-native, net8
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (44:30)

**Tip 200 – Keep Learning**
The ecosystem moves fast. The best tip is to stay curious, read the release notes for every .NET version, and don't be afraid to refactor old code when new, better patterns emerge.
*Categories:* career, mindset
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (45:00)

# Language & Syntax (Missing Items)

**Tip 66 – Extension Methods on Interfaces**
You can create extension methods on interfaces (e.g., `public static void Log(this ILogger logger)`). This allows you to add default behavior to an interface without breaking existing implementations.
*Categories:* language, architecture, patterns
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (37:35)

**Tip 67 – Local Functions vs Lambdas**
Prefer local functions (`void Helper() { }`) inside methods over `Func<>` delegates. Local functions are more performant (can be generic, struct-based, no heap allocation) and have access to the outer scope more efficiently.
*Categories:* language, performance, clean-code
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (38:00)

**Tip 68 – Static Local Functions**
Mark local functions as `static` to ensure they don't accidentally capture local variables from the enclosing method, preventing unintended memory allocations.
*Categories:* language, performance, best-practices
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (38:30)

**Tip 69 – Using Alias Directives**
C# 12 allows `using Point = (int X, int Y);`. You can alias complex generic types or tuples to make your code more readable without defining new types.
*Categories:* language, syntax, clean-code
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (39:00)

**Tip 94 – Module Initializers**
Use `[ModuleInitializer]` on a static method to run code automatically when an assembly is loaded. This is great for registering dependencies or licenses without user intervention.
*Categories:* language, advanced, startup
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (50:15)

**Tip 96 – Unsafe Code**
The `unsafe` keyword allows pointer arithmetic in C#. While rarely needed, it is essential for high-performance interop or memory manipulation where the GC overhead is unacceptable.
*Categories:* language, performance, advanced
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (51:30)

# Runtime & Performance (Missing Items)

**Tip 83 – Memory<T> and Span<T>**
`Span<T>` is stack-only and fast, but cannot be stored on the heap. Use `Memory<T>` when you need to store a "span-like" window of memory in a class field or across async calls.
*Categories:* runtime-performance, memory, advanced
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (45:45)

**Tip 84 – Ref Structs**
Declare a `ref struct` to force a type to stay on the stack. This is how `Span<T>` works and guarantees that the struct will never be garbage collected.
*Categories:* language, performance, advanced
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (46:10)

**Tip 85 – In Parameter Modifier**
Use `in` parameters (e.g., `void Method(in LargeStruct s)`) to pass large structs by reference (pointer) rather than copying them, while still preventing the method from modifying them.
*Categories:* language, performance, syntax
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (46:35)

**Tip 161 – Short Circuit**
In Minimal APIs, use `.ShortCircuit()` to end the request pipeline immediately for static files or favicon.ico, bypassing unnecessary middleware (like authentication) to save CPU cycles.
*Categories:* asp.net, performance
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (28:00)

**Tip 196 – SIMD (Single Instruction Multiple Data)**
Use `Vector<T>` to perform mathematical operations on multiple values simultaneously using CPU vector instructions (AVX/SSE). It can speed up math-heavy code by 4x-8x.
*Categories:* performance, hardware, math
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (42:40)

# Tooling & Architecture (Missing Items)

**Tip 71 – EditorConfig (Formatting)**
(Expansion) Use `.editorconfig` not just for whitespace, but to enforce language rules like "always use var" or "require this." qualifiers. It integrates with the build to warn on violations.
*Categories:* tooling, consistency
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (40:35)

**Tip 72 – Polyfills**
If you are stuck on older .NET versions, use "Polyfill" NuGet packages to gain access to newer C# language features (like `Index` or `Range`) that are syntactic sugar or library-based.
*Categories:* tooling, compatibility
*Video:* [100 Must Know Tips](https://www.youtube.com/watch?v=8F-Pb-SKO5g) (41:00)

**Tip 166 – HTTP Repl**
Use the `dotnet httprepl` tool to explore your API like a file system (`cd api/users`, `get`). It’s a CLI alternative to Swagger UI for quick testing.
*Categories:* tooling, testing, api
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (30:40)

**Tip 172 – Metrics (System.Diagnostics.Metrics)**
Don't use `Stopwatch` for production monitoring. Use `Meter` and `Counter` from .NET's built-in metrics API to record data that can be scraped by Prometheus/Grafana.
*Categories:* devops, monitoring, performance
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (33:10)

**Tip 186 – Clean Architecture**
(Alternative view) While Vertical Slices are great, Clean Architecture (Onion) is still valid for complex domains where strict separation of UI, Infrastructure, and Domain logic is required for testability.
*Categories:* architecture, design
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (38:50)

# Security & Best Practices (Missing Items)

**Tip 176 – User Secrets in Production? No!**
Never try to use User Secrets in production. They are unencrypted files on the disk. Use Azure Key Vault, AWS Secrets Manager, or HashiCorp Vault for real environments.
*Categories:* security, devops, gotchas
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (34:30)

**Tip 177 – OWASP Top 10**
Familiarize yourself with the OWASP Top 10 vulnerabilities (Injection, Broken Auth, etc.). There are .NET specific libraries and analyzers to help prevent these common attacks.
*Categories:* security, best-practices
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (35:00)

**Tip 194 – Fuzz Testing**
Use fuzzing tools to throw random, malformed data at your application to find crashes and security vulnerabilities that standard unit tests miss.
*Categories:* testing, security, advanced
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (41:30)

**Tip 169 – Problem Details (RFC 7807)**
Don't invent your own error formats. Use the standardized `ProblemDetails` class to return errors. It provides a consistent structure (Type, Title, Status, Detail) that allows clients to handle errors programmatically and predictably.
*Categories:* asp.net, api-design, standards
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (31:50)

**Tip 170 – Serilog**
The default ASP.NET Core logger is functional but basic. Replace it with **Serilog**. It offers superior structured logging capabilities, enrichment (adding context like RequestId to every log), and a massive ecosystem of "Sinks" to write logs to files, databases, or observability tools.
*Categories:* logging, libraries, tooling
*Video:* [Another 100 Tips](https://www.youtube.com/watch?v=V2uRyxffnHA) (32:15)