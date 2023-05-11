---
title: "Basic Scala"
datePublished: Sun Nov 07 2021 19:27:23 GMT+0000 (Coordinated Universal Time)
cuid: ckvpmnnkq0dki4as11ezchgjc
slug: basic-scala
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1683817700416/780b50b6-f65f-49f2-89b4-33b9e16b81dd.png
tags: scala

---

### Variables

Two kinds of variables: val (immutable, can't be reassigned once initialized) and var (mutable).  
// for comments.

```scala
val two: Int = 2
var a: String = "massy"

// you can also use type inference
val one = 1

// println to print
println(one)
```

### Functions

```scala
// define function to see if the variable hand is greater than 21
def bust(hand: Int) = {
   hand > 21
}

// call the function
println(bust(20))
```

### Arrays and Lists

Mutable collections of values, can be updated or extended.

```scala
// I use val, so names is immutable
val names = Array("Massy", "Ceci", "Cooper")

// but arrays are mutable, so I can change a value to another of the same type
names(2) = "Alvise"

// you can use the supertype Any if you want an array with different element types
val MixedTypes = new Array[Any](3)
MixedTypes(0) = "Hello world!"
MixedTypes(1) = 2
MixedTypes(2) = true
```

Lists are immutable collections, can't be changed.

```scala
val OtherNames = List("Massy", "Ceci", "Cooper")

// Lists are immutable, if I want to add an element I have to create a new list
// :: is the cons operator to add a new element to the beginning of an existing List
val NewOtherNames = "Alvise" :: OtherNames 

// Nil is an empty List in Scala, so a common way to create a new list is this:
val AllNames = "Massy" :: "Ceci" :: "Cooper" :: Nil

// ::: to concatenate Lists
Val NamesNames = OtherNames ::: NewOtherNames
```

### Control Structures

**if - else**

```scala
// If this hand busts, print to output
if (hand > 21) {
   println("This hand busts")
}

// If we have and else condition, no { are needed
if (handA > handB) println(handA)
else println(handB)

// we can use also elseifs
if (bust(handA) & bust(handB)) println(0)
else if (bust(handA)) println(handB)
else if (bust(handB)) println(handA)
else if (handA > handB) println(handA)
else println(handB)
```

*Relational operators:*  
Greater than: &gt;  
Less than: &lt;  
Greater than or equal to: &gt;=  
Less than or equal to: &lt;=  
Equal to: ==  
Not equal to: !=

*Logical operators:*  
And: &&  
Or: ||  
Not: !

**While Loops**

```scala
// define the counter
var i = 0

// repetition
var NumRepetitions = 3

// while Loop
while (i < NumRepetition) {
   println("Hip Hip Hooray!")
   i = i+1
}

// loop over a collection (bust is our previous defined function)
var hands = Array(19,21,22)

while (i < hands.length) {
   println(bust(hands(i)))
   i = i+1
}
```

**Foreach Loops**

foreach is a method

```scala
var hands = Array(19,21,22)

// bust is the function we have defined over
hands.foreach(bust)
```