
# Zoo

Zoo is an experimental programming language, a software ecosystem, and a community of creative builders. We want to level-up the craft of software development and are exploring these ideas --

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

Zoo is a new kind of open source ecosystem where code automatically floats to running systems that it (safely) improves.

## Low Friction

In the current open source community, there is tremendous propagation friction. For code to be adopted in a system, developers have to discover, learn, integrate, deploy, monitor, and maintain it. That chain is time-intensive and requires a lot of expertise. Zoo tries to automates it all.

When one person submits code, it propagates immediately to systems where it's a better solution than whatever that system is currently using. What's "better" in a given system is defined by that system's architects in the Zoo language. Code in the ecosystem competes to satisfy the architect's definitions.

## Open

Ecosystem code is open source, and the behavior of the ecosystem itself is open. Developers can watch their contributions make their way into running systems immediately. The impact is measured and developers get credit.

## Safe

The Zoo language is definition-based, which enables automated formal verification of most code. Those strong guarantees can make it safe for even third-party code to integrate automatically into your system. This can dramatically accelerate the development of robust software.



# Language

The Zoo language has the noble goal of elevating the art of software development for teams and individuals to feel --

| More Like     | Less Like
| ------------- |:-------------:|
| having superpowers | dying a thousand deaths |
| being the gods of your own universe | navigating a minefield in the dark |
| designing a cathedral with your mind | wrestling the machine into submission |
| inventing with legos | untangling a coiled mess of rope |
| being in a state of flow | running in a hamster wheel |
| painting on a canvas | gluing together someone's broken vase |

We'd like to push the state of the art in practical general-purpose language design. We're going for simple, orthogonal, composable semantic constructs that can model anything with tasteful syntax.

Zoo selectively borrows much from academic work and many existing languages, and adds a few ideas of its own.


## Definitions

Definitions are the basis for the syntax and semantics of the Zoo language. They share similarities with the constructs in design-by-contract and constraints-based languages, but are different enough to deserve their own nomenclature.

Zoo's definitions allows a high level of abstraction that maps closely with the minds of developers, rather than forcing their thinking into a paradigm that's convenient for the machine. This doesn't mean Zoo is slow, and it doesn't mean Zoo can't operate on a low level of abstraction -- just that the priority is convenience for the human, not the machine.

In Zoo, you don't typically model common data structures or algorithms. You'll usually pick a predefined definition (e.g. "List", "Sort"), and the runtime will automatically pick an implementation that satisfies that definition (e.g. "LinkedList", "QuickSort"). The runtime may dynamically change what implementation is used, depending on how the current state of the system compares to its resource limits and performance goals.

Zoo developers don't typically need to think about RAM vs Disk memory either, or execution efficiency. The runtime is resource-aware and auto-optimizing.


## Human-Centric

Zoo tries to move code a bit closer towards a codified form of conceptual composition. It goes for "flexibility with discipline" where it can model any system with whatever constraints.

There are lots of ideas we like for making code more readable and predictable while still remaining sufficiently precise and executable. You should [read about the language](/language.html#top) to get more of a flavor.



# Community

Please share your feedback.

The lead developers can be reached at
<a href="mailto:lexads@zxoo.xai"
    onmouseover="this.href=this.href.replace(/x/g,'');" class="reverse">ia.ooz@sdael</a>






