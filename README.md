# Wart Remover

This project aims to clean up some of Scala's warts by shadowing
things or making implicits ambiguous.

## Usage

`WartRemover` is meant to be extended by a `package object` for your
project's package (via the special `package.scala`). For example:

    import org.brianmckenna.wartremover.WartRemover

    package object com.precog extends WartRemover

You don't have to do anything else to your project to get more safety!

## Warts

### Warnings for "advanced" features

SIP-18 introduced warnings for the following features:

* Existentials
* Higher-kinds
* Implicit conversions

They're "disabled" mostly because they're hard to understand. Let's
disable those warnings. These features are sane and we know what we're
doing.

### Disable postfix operators

Scala allowed postfix operators, like so:

    List(1, 2, 3) length

Notice the lack of a `.` before length? That means the operator is
"postfix". The problem is that postfix operators mess up semicolon
inference. Let's make postfix operators impossible to accidentally
use.

### any2stringadd

What do you expect the following to do?

    println({} + "test")

Print `()test`, of course... Scala has an implicit which will convert
anything to a String if the right side of `+` is a String. Get some
type safety back by making this not compile.