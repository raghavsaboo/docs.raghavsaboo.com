# Methods

The main unit of organization in a Java program is the **class**. A class is a collection of methods.

There are two types of methods: **static** and **non-static** methods.

**Factoring** is the process of re-organizing code into different files - classes, methods, libraries, or functions.

## Static Methods

The same as ordinary functions in other languages.

```java
class CircleArea {
    public static void main(String args[]) {
        double area, pi, r;
        r = 6.0;
        pi = 3.141592654;
        area = pi * r * r;
        System.out.println(area)
    }
}
```

