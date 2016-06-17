---
layout: post
title:  "Boolean Basics"
date:   2016-06-16 17:07:59 -0400
---


Booleans are a type of data in ruby. Booleans can either be true or false. In ruby, we use the terms 'truthy' and 'falsey' to describe whether something is true or false. 

> Only two values are false in ruby: false, and nil. Everything else is true.

You may be wondering, "How are booleans helpful?"

Booleans can help to control when and if certain things happen in a program. This is called control flow. We use booleans when we write if, else, and ternary statements, loops, the list goes on...

**There are three main boolean operators that we can use in ruby: `!`, `&&`, and `||`.**

`!` is the single-bang operator. It represents "not". In order for a statement containing ! to be truthy, the value after ! must be false. You can also place ! in front of a boolean in order to return the opposite value.

Example:

```
!true
=> false
```

This can come in handy in a variety of situations, one of those being if you are uncertain of the truthiness of something.


`&&` is the double-ampersand operator. It represents "and". In order for a statement containing && to be truthy, the values on either side of && must both be true.

Example:

```
true && true
=> true

false && true
=> false
```

`||` is the double-pipe operator. It represents "or". In order for a statement containing `||` to be truthy, at least one of the values on either side of `||` must be true.

Example:

```
true || false
=> true

false || false
=> false

true || true
=> true
```

> In place of these operators you may also use the words "not", "and", and "or", though they have lower precedence than their counterparts.


When used in if, else, and ternary statements, booleans can help determine what will happen in a program.

Example:

```
if true
 puts "truthy"
else
 puts "falsey"
end
```

This idea could be taken further, and in more complex programs it can determine which methods run, and when.

Example:

```
if condition == true
 method1
else
 method2
end
```

**Booleans and Looping**

When creating a loop (whether with the loop keyword or with until or while) we can use boolean expressions to create conditions that will control how long the loop will run until it breaks.

While loops continue while the condition remains true, and break as soon as soon as it becomes false.

Example:

```
counter = 0

while counter < 5
 puts "My condition is truthy! So I must loop!"
 counter +=1
end
puts "Now my condition is falsey! I've broken free from my loop!"

=>"My condition is truthy! So I must loop!"
=>"My condition is truthy! So I must loop!"
=>"My condition is truthy! So I must loop!"
=>"My condition is truthy! So I must loop!"
=>"Now my condition is falsey! I've broken free from my loop!"
```

Until loops are the opposite of while loops. They will continue as long as their condition remains false, and break when it becomes true.

Example:

```
counter = 0

until counter > 4
 puts "My condition is falsey! So I must loop!"
 counter +=1
end
puts "Now my condition is truthy! I've broken free from my loop!"

=>"My condition is falsey! So I must loop!"
=>"My condition is falsey! So I must loop!"
=>"My condition is falsey! So I must loop!"
=>"My condition is falsey! So I must loop!"
=>"Now my condition is truthy! I've broken free from my loop!"
```

Booleans are a wonderfully simple way to control our conditional logic and thus implement control flow in our programs. 
