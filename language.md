
# Language

This is the aspirational design of the Zoo Programming Language. It's a work in progress.

Feedback is welcome at
<a href="mailto:lexads@zxoo.xai"
    onmouseover="this.href=this.href.replace(/x/g,'');" class="reverse">ia.ooz@sdael</a>


## Goals

The primary goals of the Zoo Language are --

* Safe hot-swap
* Definition-based ("executable specifications")
* Automatic verification of definition satisfaction
* Simple semantics
* Tasteful syntax that's expressive, readable, predictable
* Model any system at any level of granularity
* Backwards and forwards compatible
* Simple AST that converts to readable code
* No line between program and database
* Anonymous Types
* Strongly typed runtime metaprogramming

## Features

What's relatively unique --

* Design-by-definition
* Asynchronous-by-default
* Term rewriting
* Time semantics
* Multi-typing (one thing can satisfy multiple types)
* Language profiles
* Flow typing
* Prototype-based (not class-based)
* Automatic concurrency
* Automatic thread safety
* Predicate dispatch

Zoo also tends to fit these characterizations --

* Declarative
* Strongly-typed
* Stateful
* Lexically scoped
* Type inference
* Structurally typed
* Dependent types
* Call-by-sharing
* Automatic memory management
* Constraint-programming
* Safe-by-default
* Immutable-by-default
* Private-by-default
* Reflective, metaprogramming
* Deterministic, non-deterministic
* High-level, low-Level, any-level


# Definitions

Definitions are a single construct that model what types, functions, and state do in other languages.

A *definition* takes the form `signature = meaning`. The meaning substitutes for the signature.

A *usage* takes the form `signature`.

The *meaning* of a definition is composed of usages. 

Everything in Zoo is a definition or a usage. 

```
x = 1    // definition
print x  // usage
```

(Use `//` for single-line comments)

For `x = 1`, `x` is the signature, `1` is the meaning. So, `1` substitutes for `x`.

`1` is actually a usage of a pre-defined definition. 


## Parameters

Definitions can have parameters.

    inc (a) = a + 1

Usages can have arguments that replace the parameters.

    print inc 2  // 3

`inc (a)` is the definition *signature*.

`a` is a signature *parameter*.

`a + 1` is the definition *meaning*.

`inc 2` is a *usage*.

`2` is a usage *argument*.


### Parentheses

In signatures, parentheses are required to indicate parameters. In signatures and usages, they also demarcate precedence, if only for clarity.

```
a = 1
print inc a    // 1
print inc (a)  // 1
print (inc a)  // 1
```


### Multiple

You can have multiple parameters and arguments. Separate them by spaces.

    add (a b) = a + b
    print (add 1 2)


### Named

You can name the parameters in a usage.

    add (a=1) (b=2)

You can also specify arguments (using names or not) in the inner scope.

    add
        a = 1
        b = 2

### Inference

The definitions of the parameters are inferred by the usages applied to them in the definition.

```
add (a b) = a + b  // + requires Numbers
```

In the definition of `add`, `+` takes arguments of type `Number`, so `inc`'s parameters `a` and `b` must be `Number`s too.

### Where

You can also further define `a` and `b` in the indented inner scope.

```
add (a b) = a + b
    a : Number
    b : Number
```

Read that as: "`add (a b) = a + b` *where* `a` and `b` are both `Number`s".

You can also define parameters in the signature, using `:`.

```
add (a:Number) (b:Number) = a + b
```

### Variadic

Use `...` for a variable number of parameters.

    print (params...) = ()
        p over params
            print p
        

    print 1 2    // 1 2
    print 1 2 3  // 1 2 3



## Indentation

Indentation is used to further define the meaning of the definition.

```
inc (a) = b
    b = a + 1
```

Indentation also creates an inner scope, which hides its definitions from outer scopes.

Imagine adding *where* at the end of the definition line so that it reads `inc (a) = b` *where* `b = a + 1`.

There is actually a *where* operator, `\`, that you can use to make the definition a one-liner.

```
inc (a) = b \ b = a + 1
```

In this example, the definition `b` is used to "wire up" the dependency chain from `a` to the return expression `b`. Breaking up a dependency chain into smaller pieces can make code more readable.





## Signatures

Signatures come in many forms --

Prefix.

    inc (a) = a + 1

Infix.

    (a) plus (b) = a + b

Postfix.

    (amount) kg = Kilograms amount

Mad Libs.

    add (a) to (b) = a + b


## Meanings

The *meaning* (the right-hand-side of an `=`) come in three forms --

*Function-style*, where the signature substitutes for another expression.

    inc (a) = a + 1

*Procedure-style*, where the signature substitutes for nothing.

    print (a) = ()

*Record-style*, where the signature substitutes for the thing itself.

    Person (name) =
        . name = name


## Context Signatures

A *context signature* is a signature defined in the inner scope of a record-style definition, which refers to the definition.

    Person (name) =
        (this) name = name
        name-of (this) =
            print (this name)

    p = Person (name="Mel")
    name-of p  // "Mel"

A `.` is sugar for `(this)`, so you could do this --

    Person (name) =
        . name = name
        name-of . =
            print . name

Two different definitions can each have context signatures that share a name (e.g. `name-of`) without colliding in the namespace.

    name-of (Person "Mel" "68")
    name-of (Building "Empire State" (1_454 ft) 1930)


## Anonymous

Definitions can be anonymous if their signature is just parameters.

```
anon = ((a)=a+1)
```

```
map ((a)=a+1) (List 1 2 3)  // (a)=a+1 is anonymous
```


## Multiples

Commas are used for repetition.

    x, y = Integer  // x and y are both int
    z = Integer, 1  // z is both int and 1

It may seem strange that you can do `i = Integer, 1`, where you define `i` as both a "type" `Integer` and an "instance" `1` of that type.

The definition `Integer` essentially means "`i` can have, at most, one other definition that's a sequence of digits 0-9."

The definition `1` essentially means "I am substitutable for parameters that take `Integer`."

Having definitions specified in this way is powerful, flexible, and uniform -- which make them compositional.



# Primitives

| Primitive     | Examples              |
| ------------- |:---------------------:|
| String        | "A thousand pardons!" |
| Boolean       | True, False           |
| Number        | -1, 0, 3.14159        |
| Integer       | -1, 0, 1              | 

*Numbers* are infinite-precision to prevent loss of information during arithmetic. Internally, a floating-point number is a symbolic representation -- often a ratio of integers. They can be rendered as a floating-point.

    a = 1 / 3
    print a  // 0.33333…

    b = a + 2 / 3
    print b  // 1

Every `Integer` is, of course, a number too. Any definition that accepts a `Number` argument will also accept an `Integer`.


## Math

Base-2 math is ordinary.

    True and False  // False
    True or False   // True
    not True        // False

Base-10 math is ordinary. Operations take `Number`s and return `Number`s, so there's no loss of informtion during, say, integer division. Internally, an operation like `1 / 3` is represented as the ratio of the integers `1` and `3`. It can be "float-ified" for printing as needed.

    1 + 2    // 3
    1 - 2    // -1
    1 * 2    // 2
    1 / 2    // 1/2
    1 % 2    // 1
    2 ** 8   // 256

Number comparison is ordinary.

    1 == 2   // False
    1 < 2    // True
    1 <= 2   // True
    1 > 2    // False
    1 >= 2   // False


## Strings

Strings are unicode.

```
str = "What did you say?!?"

// concatenation
hw = "Hello" ++ " World"  // "Hello World"

// formatting
dys = "Well, {hw}."       // "Well, Hello World."

// slice
slice = dys[1..4]         // "Well"
```


## Functions

So far, a definition looks a lot like a function, but it's really a substitution pattern. `A` and `B` are semantically equivalent.

```
// A
inc (a) = a + 1
print inc 123
```

```
// B
print 123 + 1
```


## State

A definition that models state uses "record-style" definition and looks like this. 

    Person (name) =
        . name = name

You can read `Person (name) =` as "`Person` returns an instance of itself".


### Containment

Consider this --

    Person (age) =
        min-age = 0
        age > min-age
        . age = age

The definitions contained in the indented inner scope, like `age` and `min-age`, are private to that scope.

To make them available from outside, you can create a signature like `. age`. The `.` defines a signature that uses the containing definition as a parameter. You can model containment this way. 

    p = Person 25
    print p age      // works
    print p min-age  // fails


### Immutable

State is immutable by default. If you want mutability, apply the `Variable` definition.

```
a : Integer = 1
a = 2  // fails!

b : Integer, Variable = 1
b = 2  // works!
```

`a = Integer` means "`a` may have at most one other definition that's an `Integer`, like `1`."

`1` means "`a`, as an `Integer`, fits the parameters of definitions that accept `Integer`s, like `+`, `-`, etc."

`Variable` means "For `a`'s other definitions that allow only one of something, that something can be replaced."


## Containers

The common containers are predefined for you.

    b = Bag [1 2 3]
    s = Set [1 2 3]
    os = OrderedSet [1 2 3]
    l = List [1 2 3]

| Container   | Ordered | Unique Members  |
| ----------- |---------|-----------------|
| Bag         | no      | no              |
| Set         | no      | yes             |
| OrderedSet  | yes     | yes             |       
| List        | yes     | no              |

Maps (a.k.a.associative arrays, dictionaries) are here too.

    m = Map (a=1) (b=2)
    mm = MultiMap (a=1) (b=2)
    om = OrderedMap (a=1) (b=2)
    omm = OrderedMultiMap (a=2) (b=2)

| Container         | Ordered | Unique Keys |
| ----------------- |---------|-------------|
| Map               | no      | yes         |
| MultiMap          | no      | no          |
| OrderedMap        | yes     | yes         |
| OrderedMultiMap   | yes     | no          |

Using the indentation for arguments can be more readable.

    b = Bag
        contains
            1
            2
            3

    d = Map
        contains
            a = 1
            b = 2

### Member Access

To refer to a member of an ordered container by index, use `[index]`.

    list[1]

To refer to a member of a Map by key, use `{key}`.

    map{key}

### Size

Want to know the size of a container? Use `#`.

    list = List 1 2 3
    print #list  // 3

### Mutability

Containers are immutable by default. To make one mutable, use `Variable`.

```
l = Variable (List 1 2 3)
```

### Constraints

Here are some ways to constraint a container --

    l = List [1 2 3]
        each this > 0

    l = List of Integer [1 2 3]

## Conditions

Zoo has `if` expressions. They can have multiple cases and return things. They can take multiple forms.

    a = 1
    
    b = if a > 0 then True else False
    
    c = a > 0 ? True ! False

    d = if 
        a < 0 then -1
        a = 0 then 0
        a > 0 then 1

    e = if a ==
        1 ? 10
        2 ? 20
        3 ? 30
        _ ? 0

`_` here means *else*.

An `if` can also match multiple conditions at the same time.

    show (i) = if
        i % 2 == 0   ? 2
        i % 10 == 0  ? 10
        i % 100 == 0 ? 100
        _              ? 0

    show 100  // 2 10 100
    show 100  // 10 2 100
    show 100  // 100 2 10

Zoo defaults to asychrony unless order is made explicit, so matching is not ordered. If you need an ordered `if`, use `if-ordered`.

    show (i) = if-ordered
        i % 2 == 0   ? 2
        i % 10 == 0  ? 10
        i % 100 == 0 ? 100
        _              ? 0

    show 100  // 2 10 100
    show 100  // 2 10 100
    show 100  // 2 10 100


## Patterns

To make syntax repetition more succinct while still very readable, use `$` to separate the pattern from the unique parts. Put an index after the `$`.

```
c = if color==$1 then $2
    red   #FF0000
    green #00FF00
    blue  #0000FF
    _     #FFFFFF
```


## Importing

To import definitions into your local scope, use `import`.

```
save (s=string) = ()
    import io
    write s
```


## Allowing

To allow another scope to define in your local definition, use `allow`.

```
(a) times (b) = a * b
    . precedence = Integer \ allow Precedences
(a) minus (b) = a - b
    . precedence = Integer \ allow Precedences

Precedences =
    times precedence = 1
    minus precedence = 2
```

This allows separation of concerns while maintaining the definition as the single place to go to know about all its behavior.


## Ranges

Ranges can be bounded.

    r = 1..10

Ranges can be unbounded.

    r = 1..inf  // to infinity and beyond!




## Repetition

`over` is the main operator for repetition and takes the form `var over params`, where each `param` substitutes for `var` where it's used.

    i over [1 2 3]
        print i

It's as if this was happening --

    print 1
    print 2
    print 3

### Order

In Zoo, "ordered asychrony" is the default. That means "things start in source-code order and run concurrently."

Consider each inner scope of `over` to be a different *iteration*. There are two levels of order --

1. How iterations are ordered.
2. How the contents of iterations are ordered.

Both of these have "ordered asychrony". Iterations will start in order and run in parallel, and the contents of an iteraton will start in order and run in parallel.

If you need ordered iterations, where each iteration finishes before the next one starts, use `over-ordered`.

    i over-ordered 1 2 3
        print (i + 0.1)
        print (i + 0.2)

Note that `over-ordered` only orders the iterations. It does *not* order the  *contents* of an iteration. If you want that, you have to do this --

    i over-ordered 1 2 3
        order
            print (i + 0.1)
            print (i + 0.2)

If applied to a container, `over-ordered` iterates in the order of the container members. If the container isn't ordered (e.g. `Set`), then `over-ordered` works the same as `over`.

When you have a definition like the following, `a` will be a `List` containing `1 2 3`.

    a = v
        v over-ordered 1 2 3


### Flipping

You can have the `over` expression be in the scope above or below the definition where it's variable appears.

    print i
        i over 1 2 3

    i over 1 2 3
        print i


### Over Containers

`over` works with containers.

    list = List 1 2 3
    i over list
        print i

In the following example, `a` will be the (unordered) `Set` of `1 2 3`.

    a = v
        v over 1 2 3

### Unbounded

You can have unbounded iteration.

    // print numbers every second
    a over-ordered 1..inf
        order
            print i
            wait 1s

### Recursion

Definitions can be defined in terms of themselves.

    fn (a) = if 
        a > 0  then a + fn(a - 1) 
        _      then 0

### Looping

If you need to define a more complicated repetitive control flow, you can use `loop`, `break`, and `continue`.

    a = 1, Variable
    loop
        order
            a = a + 1
            if a == 100 then continue
            if a % 100 == 0 then break

Each inner scope of a `loop` is finished before the next one begins, though the definitions inside a `loop` are *not* ordered by default with-respect-to each other.


## Typing

### Union Types

When you want to define something as "one of these other things", use `|`.

    color = red | blue | green

### Intersection Types

    dad = Male, Father

### Enumerated Types

    colors = List [red blue green]
    color in colors

### Product type (record type)

    Person (name) = 
        name = String
        . name = name


## Conventions

### Indexes

Indexes of containers start at 1, not 0.

    list = List 1 2 3
    print list[1]  // 1

### Compound Words

There are 3 common naming schemes --

* camelCaseStyle
* underscore_case_style
* hyphenation-style

We suggests the following --

Hyphenate most identifiers. It's readable (e.g. `red-wagon`) and makes acronyms in names nicer (e.g. `HTML-to-JSON`). Don't worry, the language senses conflicts with subtraction, which will rarely happen.

Start record-style definitions with uppercase.

    Person =

Start instances of things with lowercase.
 
    hodor = Person (name="Hodor")

Start function-style definitions with lowercase.

    inc (a) = a + 1

Make cross-cutting definitions (i.e. more global in scope) all uppercase.

    DEFAULT-KEY-LENGTH = 256

### Whitespace

Use whitespace liberally, which usually improves readability.

Put spaces between things.

    inc (a) = a + 1

Use indentation.

    inc (a) = a + 1
        a = Number


# Abstraction

Zoo is meant to model a wide variety of systems and uses primarily encapsulation and composition for modeling.

## Encapsulation

Scopes are nested by indentation level and determine what definitions can access which other definitions.

Definitions in the same scope can access each other.

```
a = 123
b = a + 1  // can access a
```
 
Definitions in an inner scope can access definitions in outer scopes, but not vice versa.
 
```
// this is the outer scope
a = 123
b = 
    // this is the inner scope
    c = a + 1  // can access a

print c // fail! cannot access c
```

To make a definition accessible, you can do an `import`, or you can create a signature involving the definition itself.

```
Thing =
    a = 1 ** 10, Variable
    . b = (a + 1), Variable

Thing a = 1 ** 11  // fails
Thing b = a + 2    // works

t = Thing
print t.a  // fails
print t.b  // works
```

To summarize the default rules --

| Is a definition accessible to... |   |
|------------------|----------------|
| ... definitions in the outer scope?  | no |
| ... definitions in the same scope?   | yes |
| ... definitions in inner scopes? | yes |

For the definition line `inc (a) = b`, what parts are in what scope?

| Part   | Identity                | Scope       |
|--------|---------------------|-------------|
| `inc`  | definition name     | same scope  |
| `a`    | signature parameter | inner scope |
| `b`    | meaning definition  | inner scope |

## Composition

Definitions are compositional. Two or more usages are additive to the definition.

    HasName (name) =
        #name > 0
        .name = name, String

    Hasage (age) =
        age >= 0
        .age = age, Integer

    Person (name age) = HasName name, HasAge age

`Person` gets the members of `HasName` and `HasAge`. If there are conflicts, you must resolve them.

    LivingThing (name) =
        #name > 0
        .name = name, string

    Tree (name) =
        .name = name, string

    AppleTree (name) = LivingThing name, Tree name
        .name = LivingThing.name  // resolution



## Modeling

### Data Structure

Let's model a Queue.

    Queue = 
        future = Future
        . push (item) = ()
            future.add item
        . pop = future.next

`push` and `pop` are defined relative to each other, through the `Future`.

`Future` is not like a *Future* in other languages. It's a special built-in construct for treating "future time" like a data structure of *moments* that you can accumulate. `Future` has these member operations --

* `add (moment)` adds a new farthest-away moment
* `set (moment)` sets (and replaces) all future moments with this one
* `next` gets (and removes) the next moment in the future

Want a version of Queue that doesn't block on pop? Use `else` after `future.next`.

    Queue = 
        future = Future
        . push (item) = ()
            future.add item
        . pop = future.next else ()


### Implementations

Implementations of a definition like a Queue can vary. You can certainly pick a particular one, but you should usually leave it to the executor to determine what implementation best suits your system -- especially because that might change dynamically during runtime.

For example, if you're using a `List`, but only referring to far-apart sparse indexes (e.g. 250, 2053, 30682), your implementation would probably be better as a hash-based dict with Integer keys. Then again, if your system have lots of RAM and lookup speed is paramount, an array implementation might be best. Let the executor figure that out, guided by the performance goals you can express.


## Metaprogramming

Metaprogramming is where code produces code, which can be evaluated. The code in Zoo is mainly organized as nested dictionaries, which are straightforward to inspect.

If the dependencies of metaprogramming code can be satisfied at compilation time, the code is evaluated then, like a macro. Otherwise it's evaluated at runtime.

### Reflection

Let's use `inspect` to examine the definition `inc (a) = a + 1`.

    def = inspect inc
    def signature                      // (a Signature)
    def signature name                 // "inc"
    def signature params               // "a"
    def signature left-params          // "a"
    def signature right-params         // 
    def meaning                        // (an AST root node)
    def meaning signature name         // "+"
    def meaning signature left-params  // "a"
    def meaning signature right-params // 1

Note: in `inspect inc`, `inc` isn't treated as a usage. It's not evaluated. This is because inspect's parameter is a `Definition`. That special definition tells Zoo to not eval the expression.

    inspect (d=Definition) = ...

### Construction

To construct `inc (a) = a + 1`, use `define`.

    err = define
        p = Parameter "a"
        signature = Signature
            name = "inc"
            right-params = p
        meaning = Usage
            definition = +
            left-params = p
            right-params = 1

    // an error happens if the definition is inconsistent
    if err != none
        ? print "error {err}"
        ! print "all set!"

    // go ahead and use it now
    print inc 1  // 2

The definition has the scope it was created in.



# Execution

Zoo code typically defines *what* (definitions) and *when* (order). An executor for Zoo code must respect the definitions of the code, but there is tremendous freedom in how it treats *where* and *how*.

* *Where* might mean the execution is spread across cores, or even machines on a network.
* *How* means picking implementations for definitions, and potentially changing them during runtime.

An executor also has to deal with --

* Definition verification
* Definition substitution
* Concurrency / parallelism
* Memory management
* Caching

An executor is composed of Zoo code itself, and it can be parameterizable. You might, for example, define performance goals like "minimize latency variance". The default performance goals are --

* Satisfy all definitions
* Runs as fast as possible
* Use all allocated RAM
* Use all allocated Disk
* Schedule concurrent code fairly
* Soft-realtime


## Adaptation

When a better solution to a definition is available in the ecosystem, the executor does a verification and adoption process.

When the system resources (e.g. RAM, CPU, disk, network bandwidth) change, the executor may change its behavior. It might swap different (but definitionally equivalent) implementations in and out. As a system runs out of RAM, RAM-intensive algorithms might be swapped for slower and generally more CPU-intensive versions. There's generally a space-time exchange rate.

A clever executor will sense what code should be parallelized. It will also decide whether it's more efficient to ship code or the data it operates on.


## Concurrency

In Zoo, the only order is dependency-order, and the language makes it clear to the executor what the chains of dependencies (called *flows*) are.

```
// order is by dependency, not source code line
print y
y = inc x
x = 1
inc (a) = a + 1
```

The dependencies in a *flow* must be evaluated in sequence, but the results of one step can be continued elsewhere if, say, another core or machine is better suited to the next task.

Flows that have no common dependencies are *independent*. Independent flows have "ordered asychrony", where they start in source-code order and may run in parallel.

```
printAB =
    print "A"
    print "B"
```

You might get "AB" or "BA". So, if the print statement enforces mutual exclusion, the above code will deterministically produce "AB".

Independent flows can be executed in 4 main ways --

 * just run one at a time (event-loop-style)
 * concurrently on one core
 * parallel across cores
 * parallel across machines

A smart executor will determine what independent flows are efficient to parallize, and how.

### `order`, `when`

If you want to create arbitrary order, you can use `order`.

```
order
    print "like water"
    print "for chocolate"
```

You can also use `when`.

```
likeWater = 
    print "like water"
when likeWater
    print "for chocolate"
likeWater
```

```
when time = 2017-01-01 00:00:00 UTC
    print "kiss me you fool!"
```

`when` is pretty powerful. It enables a `pull-based` definition of execution in addition to the typical *push-driven* call strategy. This let's you write reactive dataflow-style behavior.

You can get map-reduce-like behavior like so --

```
phase1 =
    .results1 = doJob 1
    .results2 = doJob 2
order
    phase1
    print "results {phase1.results1} {phase1.results2}"
```

`phase1` runs two jobs in parallel until both are finished, then prints.



## Verification

Verification is deciding whether a usage satisfies another definition. 

In simple cases, this is equivalent to type-checking.

    inc (a) = a + 1  // a is implicitly a number
    inc 10           // passes type checking
    inc "blah"       // fails type checking

In more complex cases, it amounts to *model checking* (exhaustive test cases), or *formal verification* (proving code like a math theorem).

    sort = ...

    merge-sort = ...

If a usage cannot be proven to be satisfied or unsatisfied, the developer must handle both cases.

    ...

### Trust Models

To accept code into your system automatically, there are different trust models. You might want to manually inspect and approve everything that is suggested from the Zoo ecosystem. Or you might want to automatically accept everything. Or anywhere in between.

You might use multiple trust models --

* auto-approve code that's formally verified
* auto-approve security code that's approved by a trusted organization
* require manual approval of everything else

Here is a run down of some trust models you could mix and match.

#### Trust the Proof

| Model           | Description                                   |
|-----------------|-----------------------------------------------|
| FULL PROOF | formal verification (automated or assisted), or exhaustive model check |
| PARTIAL MODEL CHECK | passes automated unit testing, but non-exhaustive |
| PARTIAL PROOF     | parts of the definition proven, but not all |

#### Trust the People

| Model           | Description                                   |
|-----------------|-----------------------------------------------|
| MY TEAM         | approve internal code |
| MY INSPECTION   | approve if i've inspected and approved |
| AUTHORITY       | approve if the experts approve |
| SOCIAL PROOF    | approve if a critical mass of certain others use it safely |

#### Trust the Measurements

| Model           | Description                                   |
|-----------------|-----------------------------------------------|
| SIMULATION      | the simulation (using historial data?) demonstrates the properties we want |
| INNOCUOUS TRIAL  | measure the code with live data in parallel to current running code |
| PRODUCTION TRIAL | measure the code in production on a small part of the whole |

#### Trust the Algorithm

| Model            | Description                                   |
|------------------|-----------------------------------------------|
| BOT              | a bot's trust score crosses our threshold |


## Safety

The Zoo language prevents most semantic vulnerabilities. A Zoo executor will typically have similar guarantees and vulnerabilities to those of other languages.

### Semantic Safety

Zoo prevents --

 * divide by zero
 * loss of precision in floating-point arithmetic
 * array index out of bounds
 * undefined container member
 * arithmetic overflow
 * arithmetic underflow
 * exceptions (they're not in the language)
 * undefined
 * deadlock
 * livelock
 * simultaneous dependence on mutable state
 
In the divide-by-zero case, if the division is guaranteed to have a non-zero denominator, then no problem.

    a = 1
    b = 2
    c = a / b 

If that cannot be guaranteed, then the developer has to handle that case.

    a = 1
    b = fn (a)
    c = a / b  // c = Number | NaN
    if c = NaN 
        ? "whew, we're good."
        ! "div by zero!"

Zoo does not prevent:

 * non-termination (but you can sense it and respond)
 * blocking forever (but you can sense it and respond)

Any state accessed or depended-upon by two otherwise independent flows is effectively locked while each flow is evaluated.


### Runtime Safety

The default Zoo executor does not prevent these runtime issues:

 * out of memory
 * stack overflow
 * heap overflow
 * process killed
 * thread killed



# Source Files

## Main

Somewhere in your root scope, import the default language and executor definitions.

    = std-0.1 // syntax
    = executor-1.0 // executor

## Scoping

Scoping is by folder. Files in the same folder contain definitions that are all in the same scope. For example --

    /main.zoo
    /other.zoo
    /package/web.zoo
    /package/net.zoo

The definitions in `main` and `other` share the same scope. Call this scope *root*.

`web` and `net` share the same scope, *package*. *package* is an inner scope of *root*.


