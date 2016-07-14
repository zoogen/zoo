
# Zoo

Zoo is an experimental programming language, a software ecosystem, and a community of creative builders. We want to level-up the craft of software development and are exploring these ideas:

__DRAMATIC AUTOMATION__

 * Software that auto-improves
 * Software that auto-adapts
 * Software that auto-optimizes
 * Software that auto-migrates

__MODEL-ANYTHING SEMANTICS__

 * Any level of abstraction
 * Human-centric (not machine-centric) syntax
 * Runnable specifications
 * Flexible temporal definitions

__WORLD-SCALE ORCHESTRATION__

 * Multi-site
 * Multi-platform
 * Safe interoperability
 
__RUNTIME FORMAL VERIFICATION__
 
 * Safe hot-swapping without downtime
 * Automated verification for reliability & security
 
Software is becoming more foundational to how the world works. Improving software quality and accelerating its development is extremely high-leverage.


# Ecosystem

Zoo is a new kind of open source ecosystem where code automatically floats into running systems that it (safely) improves.

## Low Friction

In the current open source community, there is tremendous propagation friction. For code to be adopted in a system, developers have to discover, learn, integrate, deploy, monitor, and maintain it. That chain is time-intensive and requires a lot of expertise. Zoo tries to automates it all.

When one person submits code, it propagates immediately to systems where it's a better solution than whatever that system is currently using. What's "better" in a given system is defined by that system's architects in the Zoo language. Code in the ecosystem competes to satisfy the architect's definitions. They'll become gradually more useful and more sophisticated.

## Open

Ecosystem code is open source, and the behavior of the ecosystem itself is open. Developers can watch their contributions make their way into running systems immediately. The impact is measured and developers get credit.

## Safe

The Zoo language is definition-based, which enables automated formal verification of most code. Those strong guarantees can make it safe for even third-party code to integrate automatically into your system. This can dramatically accelerate the development of robust software.



# Language

The Zoo language has the noble goal of elevating the art of software development to feel --

| More Like     | Less Like
| ------------- |:-------------:|
| having superpowers | dying a thousand deaths |
| being the god of your own universe | navigating a minefield in the dark |
| designing a cathedral with your mind  | wrestling the machine into submission |
| inventing with legos | untangling a coiled mess of rope |
| being in a state of flow | running in a hamster wheel |
| painting on a canvas | gluing together someone's broken vase |

We'd like to push the state of the art in practical general-purpose language design, and have a chance of broad adoption. We're going for simple, orthogonal, composable semantic constructs that can model anything with tasteful syntax.

Zoo selectively borrows much from academic work and many existing languages, and adds a few ideas of its own.


## Definitions

Definitions are the basis for the syntax and semantics of the Zoo language. They share similarities with the constructs in design-by-contract and constraints-based languages, but are different enough to deserve their own nomenclature.

Zoo's definitions allows a high level of abstraction that maps closely with the minds of developers, rather than forcing their thinking into a paradigm that's convenient for the machine. This doesn't mean Zoo is slow, and it doesn't mean Zoo can't operate on a low level of abstraction -- just that the priority is convenience for the human, not the machine.

In Zoo, you don't typically model common data structures or algorithms. You'll usually pick a predefined definition (e.g. "List", "Sort"), and the runtime will automatically pick an implementation that satisfies that definition (e.g. "LinkedList", "QuickSort"). The runtime may dynamically change what implementation is used, depending on how the current state of the system compares to its resource limits and performance goals.

Zoo developers don't typically need to think about RAM vs Disk memory, or execution efficiency. The runtime is resource-aware and auto-optimizing.

Here's an example of the definition of `sort`.

```
sort (list) = ()
    list = List
    i over 1..#list-1
        (list after)[i] <= (list after)[i+1]
```

## Human-Centric Syntax

Zoo moves code a bit closer towards a writen form of idea composition with flexible expression of time dynamics, rather than the imperative tradition of strictly branching sequences of instructions or the functional tradition of strictly nested functions.

There are lots of ideas we like for making code more readable and predictable while still remaining sufficiently precise and executable: definitions, separation of concerns, pattern-based syntax, literate programming. You should [read about the syntax]() to get more of a flavor.


## Modeling Time (Order)

Most programming languages express "when code executes" in an explicit way. 

Imperative languages model control flow as a sequence of commands.

Functional languages model computation by the "applicative" procession of (nested) function calls.

There's also flow-based, the dataflow/reactive tradition, term-rewriting, time-based (like a metronome), and others.

Most are deterministic.

Any turing-complete can simulate any other model, but how convenient is their expression?

Zoo's goal is to be able to express all these, but will normally have the best one chosen for you by the runtime, based on the definitions in your code.



 instruction order (the imperative tradition) or function nesting (the functional tradition). Flow is scheduled by the runtime and usually determined by explicit processes, threads, and blocking.

There are some systems that are better modeled with different order semantics - like modeling an electronic circuit or GUI with a dataflow/reactive-style constructs.

| Tradition     | Construct |
| ------------- |:-------------:|
| Imperative | instruction order |
| Functional | function nesting |
| Dataflow | .. |

In Zoo, order is modeled through definitions, which encompass all these traditions.

Zoo can do the imperative/functional-style "expressions are eval'd when the definition is defined"

Zoo can also do dataflow-style "expressions are eval'd whenever definitions are maintained forever"

## Auto-Verify

Let's say you want to sort a list with the expression, `sort list`.

The Zoo ecosystem already has `MergeSort`, `BubbleSort`, `QuickSort`, and others. Each of these can be automatically proven to satisfy the definition, `sort`.

## Auto-Improve

If a new algorithm is introduced into the ecosystem which satisfies the definition of “sort” and better fits your system’s situation (uses less RAM or CPU), it’s used. Your system improves automatically.

## Auto-Adapt

When system resources change (e.g. low RAM or CPU), a different algorithm or data structure representation might be swapped in dynamically. Your system adapts automatically.

Some algorithms and data structures use more or less CPU and RAM, and so the runtime will choose among them based on which best fits your system resources and performance goals. They'll be swapped in dynamically.

## Auto-Optimize

Zoo automatically picks the best implementation that fits the performance goals of a system, which are usually "use all allocated RAM and as few CPU cycles as possible".

When a more optimized component is introduced into the ecosystem, it's tested against all the definitions it satisfies. If it's more optimized for a given system (usually uses less RAM, CPU, or disk), it floats in.

## Auto-Concurrency

Zoo code is auto-concurrent by default. The choice of concurrency strategy is chosen by the runtime, and that's configurable.

## Auto-Scaling

Because Zoo code is conveniently composable and concurrent, it can be scaled automatically too. The scaling strategy is chosen by the runtime, and is configurable.



# Community

Please share your feedback.

The lead developers can be reached at
<a href="mailto:lexads@zxoo.xai"
    onmouseover="this.href=this.href.replace(/x/g,'');" class="reverse">ia.ooz@sdael</a>






