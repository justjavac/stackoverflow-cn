Why does Math.round(0.49999999999999994) return 1
====================================================

http://stackoverflow.com/questions/9902968/why-does-math-round0-49999999999999994-return-1

In the following program you can see that for each value slightly less that `.5` is rounded down, except for `0.5`.

```java
for (int i = 10; i >= 0; i--) {
    long l = Double.doubleToLongBits(i + 0.5);
    double x;
    do {
        x = Double.longBitsToDouble(l);
        System.out.println(x + " rounded is " + Math.round(x));
        l--;
    } while (Math.round(x) > i);
}
```

prints

```
10.5 rounded is 11
10.499999999999998 rounded is 10
9.5 rounded is 10
9.499999999999998 rounded is 9
8.5 rounded is 9
8.499999999999998 rounded is 8
7.5 rounded is 8
7.499999999999999 rounded is 7
6.5 rounded is 7
6.499999999999999 rounded is 6
5.5 rounded is 6
5.499999999999999 rounded is 5
4.5 rounded is 5
4.499999999999999 rounded is 4
3.5 rounded is 4
3.4999999999999996 rounded is 3
2.5 rounded is 3
2.4999999999999996 rounded is 2
1.5 rounded is 2
1.4999999999999998 rounded is 1
0.5 rounded is 1
0.49999999999999994 rounded is 1
0.4999999999999999 rounded is 0
```

I am using Java 6 update 31.

----------------- answer ---------------------

http://stackoverflow.com/a/9903075/343194

According to the Java 6 docs, `round(x)` is implemented as `floor(x+0.5)`.<sup>1</sup> 
But `0.5+0.49999999999999994` is exactly 1 in double precision:

```java
static void print(double d) {
    System.out.printf("%016x\n", Double.doubleToLongBits(d));
}

public static void main(String args[]) {
    double a = 0.5;
    double b = 0.49999999999999994;

    print(a);
    print(b);
    print(a+b);
    print(1.0);
}
```

gives:

```
3fe0000000000000
3fdfffffffffffff
3ff0000000000000
3ff0000000000000
```

This is because `0.49999999999999994` has a smaller exponent than `0.5`, so when they're added, its mantissa is shifted, and the ULP gets bigger.

--------------

1. At least, this is the definition given in the Java 6 docs. 
It's not given in the Java 7 docs, which may explain why people are seeing different behaviour when they run in Java 7. 

UPDATE: According to Simon Nickerson's answer, it's a [known bug][1], so this almost certainly explains the difference in the docs and the observed behaviour between versions.

[1]: http://bugs.sun.com/bugdatabase/view_bug.do?bug_id=6430675
