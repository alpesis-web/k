---
layout: post_tech
title: "Notes on Programming in Scala"
date: 2016-01-04 15:15:52 +0800
comments: true
categories: [tech]
tags: [scala]
toc: true
---

Programming in Scala: http://www.artima.com/pins1ed/

## Funtional Language and Scala

### Functional Language


object object-orientaed programming

In principle, the motivation for object-oriented programming is very simple: all but
the most the trivial programs need some sort of structure. The most straightforward way
to do this is to put data and operations into some form of containers. The great idea of
object-oriented programming is to make these containers fully general, so that they can
contain operations as well as data, and that they are themselves values that can be stored
in other containers, or passed as paramters to operations. Such containers are called
objects.

Alan Kay, the inventor of Smalltalk, remarked that in this way the simplest object has
the same construction principle as a full computer: it combines data with operations
under a formalized interface. So objects have a lot to do with language scalability:
the same techniques apply to the construction of small as well as large programs.


### Scala

- compatibility
- brevity
- high-level abstractions
- advanced static typing

<img src="https://s-media-cache-ak0.pinimg.com/474x/52/c2/15/52c2157c0b2f25cfae2d295e67ef32f8.jpg" />

## Scala Elements

### Val vs Var

Prefer vals, immutable objects, and methods without side effects. Reach for them first. 

Use vars, mutable objects, and methods with side effects when you have a specific need and justification for them.

### Types and Operations

- types
- literals
- operations

<img src="https://s-media-cache-ak0.pinimg.com/736x/b3/98/ab/b398ab10483fd8a9892880e0fe324326.jpg" />

image source: Table 5.1 · Some basic types, Chpater 5, Programming in Scala, page 118

<img src="https://s-media-cache-ak0.pinimg.com/736x/fc/91/b6/fc91b687d2acfcf714bf5278ef37c4f9.jpg" />

image source: Rich types, Chapter 5, Programming in Scala, page 138


<img src="https://s-media-cache-ak0.pinimg.com/736x/cb/ce/46/cbce46c71c592e1fe97448e3a5fc1ed4.jpg" />

image source: Chapter 5, Programming in Scala


<img src="https://s-media-cache-ak0.pinimg.com/736x/b3/0e/50/b30e502bcc31787ad660c38ea4104b3c.jpg" />

image source: Operator precedence, Chapter 5, Programming in Scala, p135

<img src="https://s-media-cache-ak0.pinimg.com/736x/92/85/5b/92855b63af3053f3cd522150b3d1c7b4.jpg" />

image source: Rich Operations, Chapter 5, Programming in Scala, page 138

### Collections

- array
- list
- tuple
- set
- map


<img src="https://s-media-cache-ak0.pinimg.com/736x/1e/ac/60/1eac6035eaee581df8a0b46eecd14d3a.jpg" />

image source: Class hierarchy for Scala sets, Chapter 3, Programming in Scala, page 92



<img src="https://s-media-cache-ak0.pinimg.com/736x/d3/af/00/d3af004af2898246aee87b91006895fd.jpg" />

image source: Class hierarchy for Scala maps, Chapter 3, Programming in Scala, page 94


### Class and Objects

- class
- object
- app

<img src="https://s-media-cache-ak0.pinimg.com/736x/93/7c/06/937c06add4c4e06b27e77a096a5e0975.jpg" />

image source: Scala Class Hierarchy, Chapter 10, Programming in Scala

Trait or not trait

- If the behavior will not be reused, then make it a concrete class. It is not reusable behavior after all.
- If it might be reused in multiple, unrelated classes, make it a trait. Only traits can be mixed into different parts of the class hierarchy.
- If you want to inherit from it in Java code, use an abstract class, use an abstract class. Since traits with code do not have a close Java analog, it tends to be awkward to inherit from a trait in a Java class. Inheriting from a Scala class, meanwhile, is exactly like inheriting from a Java class. As one exception, a Scala trait with only abstract members translates directly to a Java interface, so you should feel free to define such traits even if you expect Java code to inherit from it. See Chapter 31 for more information on working with Java and Scala together.
- If you plan to distribute it in compiled form, and you expect outside groups to write classes inheriting from it, you might lean towards using an abstract class. The issue is that when a trait gains or loses a member, any classes that inherit from it must be recompiled, even if they have not changed. If outside clients will only call into the behavior, instead of inheriting from it, then using a trait is fine.
- If efficiency is very important, lean towards using a class. Most Java
runtimes make a virtual method invocation of a class member a faster oper- ation than an interface method invocation. Traits get compiled to interfaces and therefore may pay a slight performance overhead. However, you should make this choice only if you know that the trait in question constitutes a per- formance bottleneck and have evidence that using a class instead actually solves the problem.
- If you still do not know, after considering the above, then start by making it as a trait. You can always change it later, and in general using a trait keeps more options open.


<img src="https://s-media-cache-ak0.pinimg.com/736x/50/e0/b6/50e0b6173313209d85c1b08647f06709.jpg" />

image source: Collection hierarchy, Chapter 24, Programming in Scala, Page 536

