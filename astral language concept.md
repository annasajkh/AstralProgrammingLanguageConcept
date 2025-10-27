### Namespace

if you don't wrap variables, functions, class, etc in a namespace you cannot import it to other file but you can use it in the current file\
for example
```cs
import std::io

int8 number = 69;

void main()
{
    // Console is a static class or struct
    Console.println("Hello World");
    Console.println(number);
}
```

you can define your own namespace using "namespace" keyword
for example

```cs
// this namespace will apply to the entire file
namespace math;
```

but you can namespace a portion of code like this
```cs
namespace math
{
    float abs(float number)
    {
        return number * -1 if number < 0 else number;
    }
}
```

namespace are `private` by default so it can only be accessed in the same file\
there is `internal` like in c# to make it accessible on the current project, useful for libraries\
and there is also `public` to make it accessible everywhere
```cs
internal namespace engine_utils
{

}

public namespace cat
{

}
```

you can defined nested namespace like this
```cs
namespace std::io

namespace std::io
{

}
```

importing namespaces\
file `math.astral`
```cs
public namespace std::math
{
    float abs(float number)
    {
        return number * -1 if number < 0 else number;
    }
}
```

file `main.astral`
```cs
import std::math;

void main()
{
    float result = abs(-10);
}
```
`abs()` is now in the top level, but i think is not good in large codebases\
because it can collide with the stuff in the top level if they have the same name
so you should wrap it with namespace alias like this
```cs

import std::math as math;

void main()
{
    math::abs(-10);
}
```
or use static class/struct
```cs

import std::math;

void main()
{
    Math.abs(10);
}
```

if there is a clash it will error at compile time

### Variables
```cs
// all variables is immutable by default like in rust
int32 score = 10;
score = 20; // Error: Cannot modify immutable variable "score"

// make it mutable
mutable int32 size = 20;
size = 30; // this work

// all variables are also not null by default
int32 width = null; // error

// to make it nullable use the ? operator
int32? maybe_height = null;
// you can also leave it uninitialize and it will assign it to null (no default values)
string? maybe_name;
bool? maybe_enabled;

string name; // error

// check if it's null
if (maybe_height is int32 height)
{
    import std::io;

    Console.println(height);
}

// WIP
```

### Data Types

```cs
// primitive data types are all allocated on the stack by default

// signed int types
int8 number1 = 1;
int16 number2 = 2;
int32 number3 = 3;
int64 number4 = 4;
int128 number4 = 5;

// signed pointer sized int
intptr number1_ptr = (intptr)&number1;

// unsigned int types
uint8 number5 = 6;
uint16 number6 = 7;
uint32 number7 = 8;
uint64 number8 = 9;
uint128 number9 = 10;

// unsigned pointer sized int
uintptr number5_ptr = (intptr)&number1;

float scale = 3.5f;
double pi = 2.71828;
decimal pi = 3.14159265359;

bool enabled = true;

char a = 'a';
string name = "Bob";

// you can also allocate primitives on the heap by doing this
int8 number_heap = heapalloc 69;
defer delete number_heap;

// with array, collections, and user types you need to specify where to allocate
int32[] numbers_heap = heapalloc [1, 2, 3, 4];
defer delete numbers_heap;

import std::collections;

// inside List<T> there is collection expressions like in c#
// so you can make your own custom collections
List<string> names = heapalloc ["Bob", "Steve", "Alex"];
names.add("robert"); 

defer delete names;

Dictionary<int32, Person> people = heapalloc {
    {
        2424525,
        heapalloc Person("Bob", 24)
    },
    
    {
        3232224,
        heapalloc Person("Steve", 32)
    }
}

defer delete people;

import std::io;

class Person
{
    public Person(string name = name, uint8 age = age);

    public void say_hi()
    {
        Console.println($"Hi my name is {name}, and i'm {age} years old");
    }
}

Person lisa = heapalloc Person(name: "Lisa" age: 34);
lisa.say_hi();

// function type
int32 function(int32 number) double_it = (int32 number) 
{
    return number * number;
};

Console.println(double_it(10));

// events
event void function() say;

say += () {
    Console.println("Hello");
}

say += () {
    Console.println("World");
}

say();
// will print
// Hello
// World

// WIP
```

### Pointers
Make something a pointer by adding * to the end of a type
```cs
int32 number = 32;
int32* number_ptr = &number;
```

Pointers are non null by default to make it nullable add ? operator
```cs
int32*? maybe_number_ptr = nullptr;

// and then you can use the same check to access it
if (maybe_number_ptr is int32* number_ptr)
{
    import std::io;

    Console.println(&number_ptr);
}

// nullable pointer to a nullable pointer to a nullable pointer
int32*?*?*? something = null;

// WIP
```

### Memory Allocation
```cs

// allocate on the stack using default allocator
int32 number_stack = stackalloc 8;

// allocate on the heap using default allocator
int32 number_heap = heapalloc 8;
defer delete number_heap;
```