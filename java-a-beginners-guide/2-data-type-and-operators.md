# Chapter 2: Introducing Data Types and Operators

(Book: Java a Beginner’s Guide Sixth Edition)

## Integers

All of the integer types are signed positive and negative values.

- Byte: 8 bits (x-small coffee) 正负 128 范围 - 2 的 8 次方
- Short: 16 bits (small coffee) 正负 3 万范围 - 2 的 16 次方
- Int: 32 bits (medium coffee) 正负 21 亿范围 - 2 的 32 次方
- Long: 64 bits (large coffee) 超过正负 21 亿范围 - 2 的 64 次方

## Floating-Point Types

- Float: single-precision numbers, 32 bits
- Double: double-precision numbers, 64 bits
- Double is the most commonly used (e.g. Math.sqrt() method returns a double value)

## Characters

- In java, characters are not 8-bit quantities, because instead of using the standard 8-bit ASCII char set, Java uses Unicode, which is an unsigned 16-bit type (range of 0 - 65,536)
- A char variable can be assigned a value by enclosing the character in single quotes, such as `char ch = ‘X’`
- A char can be incremented and can be assigned an integer value. Such as `ch++`

## Literals

- A fixed value that represented in their human-readable form, commonly called a constant
- Java literal can be of any of the primitive data types
- By default, integer literals are of type int. To specify a long literal, append an l or an L. For example, 12 is an int, 12L is a long
- By default, floating-point literals are of type double. To specify a float literal, append an F or f to the constant. For example, 10.19F is of type float.
- Beginning with JDK7, you can insert underscores into an integer or floating-point literals to make it easier to read values consisting of many digits. For example, 123_45_1234 is equivalent to 123,45,1234. When the literal is compiled, the underscores are simply discarded

## Hexadecimal, Octal, and Binary Literals

- Octal: number system based on 8. In octal it uses the digits 0 - 7.
- Hexadecimal: number system based on 16, it uses digits 0 - 9 plus the letters A to F, which stands for 10, 11, 12, 13, 14 and 15.
- Specify integer literals in hexadecimal: must begin with 0x or 0X (e.g. hex = 0xFF // 255 in decimal, 16 的 2 次方)
- Specify integer literals in octal: must begins with a zero (e.g. octal = 011 // 9 in decimal)
- Specify integer literals in binary: must begins with 0b or 0B (e.g. bi = 0b1100)

## The Scope and Lifetime of Variables

See HeadFirst Java (chapter 9 Life and death of an Object)

- Local variable: live within the method (removed when the current stack frame is popped off)
- Instance variable: live within the object, removed by GC (Garbage Collection)

* Some other languages define two general categories of scopes: local and global
* In java, the most important scopes are: by class and by method
* As a general rule, variables declared inside a scope (a block of code) are not visible (accessible) to code that is defined outside that scope
* When you declare a variable within a scope (including parameters) you are localizing that variable and protecting it from unauthorized access and/or modification. (Think about closure!!)
* Scope can be nested. Each time you create a block of code, you are creating a new nested scope. The outer scope encloses the inner scope - this means that objects declared in the outer scope will be visible to code within the inner scope. However, objects declared within the inner scope will not be visible outside it.
* Variables are created when their scope is entered, and destroyed when their scope is left (e.g. a for loop)

## Difference between x++ and ++x

- Both x++ and ++x increment the variable x by 1
- When an increment/decrement operator precedes its operand (++x), the increment operation of x takes place first, then the incremented x is assigned to y. In the case of `x = 10; y = ++x;` y is set to 11.
- If the operator follows its operand, Java will get the operand’s value first, then increment x. In the case of `x = 10; y = x++` y is set to 10.
- In both cases, x is set to 11.

## Logical Operators (page 52)

- &: AND. p = false, q = true, p & q = false
- |: OR. p = false, q = true, p | q = true
- ^: XOR. the outcome of an exclusive OR operation is true when exactly one and only one operand is true. p = true, q = false, p ^ q = true
- &&: short-circuit AND. If the first operand is false, the outcome is false no matter what value the second operand has.
- ||: short-circuit OR. If the first operand is true, the outcome of the operation is true no matter what the value of the second operand.
- The normal operands will always evaluate each operand, but short-circuit version will evaluate the 2nd operand only when necessary
- The non-short-circuit forms of operations are required if your code expects the right-hand operand of an AND/OR operation to be evaluated. For example `if (false & (++I < 100))` would still increment i even though the if statement fails

## Type Conversion in Expressions

- All char, byte, and short values are promoted to int.
- If two byte type variables are added, the result is an int type, which can be assigned to an int automatically; but cast is needed to assign the result to a byte type, for example `byte i = (byte) (b * b)`
- If one operand is a long, the whole expression is promoted too long. Same applies to float and double.
