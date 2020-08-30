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


**3. Most random number generators can be initialized with a seed.
Implement a function that takes a seed, calls the random number
generator with that seed, and returns the result. Memoize that
function. Does it work?**


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

**5. How many different functions are there from Bool to Bool? Can
you implement them all?**

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
