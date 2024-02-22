# Template Method

The **template method pattern** is a generic computation mechanism that can be specialized for a particular application by redefining certain steps. 

To allow customization, the primary algorithm calls auxiliary functions known as **hooks** at designated steps of the process. These hooks can be overridden for specialized use cases.

An example is the [Euler Tour Tree Traverl](../Algorithms%20and%20Data%20Structures/Algorithms/tree%20traversals.md##Euler%20Tours)