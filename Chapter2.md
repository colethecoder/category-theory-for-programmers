# Chapter 2: Types and Functions

**1. Define a higher-order function (or a function object) memoize in
your favorite language. This function takes a pure function f as
an argument and returns a function that behaves almost exactly
like f, except that it only calls the original function once for every
argument, stores the result internally, and subsequently returns
this stored result every time it’s called with the same argument.
You can tell the memoized function from the original by watching its performance. For instance, try to memoize a function that
takes a long time to evaluate. You’ll have to wait for the result
the first time you call it, but on subsequent calls, with the same
argument, you should get the result immediately.**

```csharp
public static Func<A,B> memoize<A, B>(Func<A, B> f)
{
    var remembered = new Dictionary<A, B>();

    return x => {
        if (remembered.TryGetValue(x, out var rememberedResult))
        {
            return rememberedResult;
        }
        else
        {
            var result = f(x);
            remembered.Add(x, result);
            return result;
        }
    };
}
```

**2. Try to memoize a function from your standard library that you
normally use to produce random numbers. Does it work?**

I created a very basic Unit type so that the function can have an input:

```csharp
public struct Unit {
    public static Unit unit => default;
}
```

Then wrapped up System.Random

```csharp
public static int Random(Unit _) => new Random().Next();
```

Two calls without memoizing:

```csharp
Console.WriteLine(Random(Unit.unit)); // 293662137
Console.WriteLine(Random(Unit.unit)); // 1621155432
```

Then memoized:

```csharp
var memRand = memoize<Unit, int>(Random);

Console.WriteLine(memRand(Unit.unit)); // 540638266
Console.WriteLine(memRand(Unit.unit)); // 540638266
```

**3. Most random number generators can be initialized with a seed.
Implement a function that takes a seed, calls the random number
generator with that seed, and returns the result. Memoize that
function. Does it work?**

```csharp
public static int Random(int seed) => new Random(seed).Next();
```

```csharp
Console.WriteLine(Random(123)); // 2114319875
Console.WriteLine(Random(123)); // 2114319875
Console.WriteLine(Random(456)); // 2044805024

var memRand = memoize<int, int>(Random);

Console.WriteLine(memRand(123)); // 2114319875
Console.WriteLine(memRand(123)); // 2114319875
Console.WriteLine(memRand(456)); // 2044805024
```

**4. Which of these C++ functions are pure? Try to memoize them
and observe what happens when you call them multiple times:
memoized and not.**

**(a) The factorial function from the example in the text.**

**(b)**
```cpp
std::getchar()
```
**(c)** 
```cpp
bool f() {
std::cout << "Hello!" << std::endl;
return true;
}
```
**(d)** 
```cpp
int f(int x) {
static int y = 0;
25
y += x;
return y;
}
```

Only (a) is pure. (b) and (c) contain IO and therefore the IO would only apply on the initial all not when memo-ed. (d) uses a static variable that will increment when used normally but not when memo-ed. 

**5. How many different functions are there from Bool to Bool? Can
you implement them all?**

The cardinality of Bool (number of possible values) is 2. The cardinality of a function (number of possible pure functions) of the form `a -> b` is the cardinality of type `b` to the power of the cardinality of type `a` i.e.

`b^a`

`Bool -> Bool` therefore has the cardinality:

`2^2`

The 4 functions are:

```csharp
public static bool always(bool _) => true;
public static bool never(bool _)  => false;
public static bool not(bool x)    => !x;
public static bool id(bool x)     => x;
```

**6. Draw a picture of a category whose only objects are the types
Void, () (unit), and Bool; with arrows corresponding to all possible functions between these types. Label the arrows with the
names of the functions.**

![Alt text](https://g.gravizo.com/source/custom_mark1?https%3A%2F%2Fraw.githubusercontent.com%2Fcolethecoder%2Fcategory-theory-for-programmers%2Fmaster%2FChapter2.md)
<details> 
<summary></summary>
custom_mark1
  digraph d {
    { Bool [height="1.3"] }
    void -> void [label = "id", headport = n, tailport = n];
    void -> unit [label = "  absurd  "];
    void -> Bool [label = "  absurd  "];
    unit -> unit [label = "  id  ", headport = s, tailport = s];
    unit -> Bool [label = "  true  "];
    unit -> Bool [label = "  false  "];
    Bool -> unit [label = "unit"];
    Bool -> Bool [label = "   id   "];
    Bool -> Bool [label = "   always   "];
    Bool -> Bool [label = "   never   "];
    Bool -> Bool [label = "   not   "];
  }
custom_mark1
</details>
