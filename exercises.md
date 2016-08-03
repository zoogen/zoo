
# Familiar Things

## http request handler

# Time/Concurrency

## Aynchronous 

The following runs asynchronously, by default.

    result1 = run job1
    result2 = run job2

Each of the `run job_` lines runs in parallel.

The executor may choose to run them on a single machine core concurrently, on different cores of the machine in parallel, or even different servers in parallel.

## Dependency-Order

The following runs asynchronously by default, but `combine` waits on the results because it depends on them.

    result1 = run job1
    result2 = run job2
    combine result1 result2

## Arbitrary Order

There are lots of constructs to define order. A simple one is `order`.
    
    order
        result1 = run job1
        result2 = run job2

After `run job1` ends, `run job2` will start.

Another construct is `when`.

    result1 = run job1
    when result1 (result2 = run job2)

## Sophisticated Order

Let's model a complex scenario. It will have several timelines that have triggers and relative time constraints. The constraints will also change during the scenario.

    Bike-Race


# Scaling

# Hot Swap

## Rejection

Deploy but have the system stop you if constraints not satisfied.

# Time

## order
## mutex
## race
## before
## after

# Persistence

 (Database)

# Communication

Different systems talk to each other through Zoo

## APIs

Interfacing with an API

Model an API in a non-brittle way... allow non-breaking changes, like renaming a field... or providing different syntax for the same semantics.

# Mingle Languages

You can model other languages in Zoo, then embed them in Zoo code.

    Site =

        . html = html
            <!DOCTYPE html>
            <html>
            <head>
                <title>HTML Example</title>
                <link rel="stylesheet" type="text/css" href="style.css">
                <script type="text/javascript" src="script.js"></script>
            </head>
            <body> 
                <h1>This is a heading</h1>
                <p>This is a paragraph.</p>
                <button onclick="myFunction()">Click Me!</button>
            </body> 
            </html>

        . css = css (filename="style.css")
            body { background-color: lightblue; }
            h1 { color: white; text-align: center; }
            p { font-family: verdana; font-size: 20px; }

        . js = javascript (filename="script.js")
            function myFunction() {
                var x = document.getElementById("demo"); 
                x.style.fontSize = "25px"; 
                x.style.color = "red"; 
            }


# Low-Level Modeling

# AST

## AST-to-code

## Version Conversion

# Language Profiles



# Other Ideas
(Nice modeling of something in the real world)
Model another language
Auto-implementation
Game: calculator, text editor, paint, Tetris, Pong, hangman, sudoku solver
http://rosettacode.org/wiki/Category:Programming_Tasks
http://www.spoj.com/problems/classical/



