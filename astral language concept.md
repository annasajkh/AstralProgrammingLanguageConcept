### Namespace

The default namespace of a piece of code is project name + file path of the file + the file name it was written in (idk if this a good idea but it's better than putting it into the global scope by default)\
for example
the file path is `/src/main.astral` and project name is test_project
```cs
import std::io

// the namespace for the main function would be test_project::src::main
void main()
{
    // Console is a static class or struct
    Console.println("Hello World");
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
internal namespace math
{
    float abs(float number)
    {
        return number * -1 if number < 0 else number;
    }
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
    float result = abs(10);
    // abs is now in the this file namespace, but i think is not good in large codebases 
    // so you should wrap it with namespace alias "import std::math as math" so you can use it like this "math::abs(10);"
    // or use static class/struct "import std::math" so you can use it like this "Math.abs(10);"
}
```

namespace alias to prevent collisions\
file `main.astral`
```cs
import std::math as mathematic;
import std::io;

void main()
{
    Console.println(mathematic::abs(10));
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

// function pointers
int32 function(int32 number) double_it = (int32 number) 
{
    return number * number;
};

Console.println(double_it(10));

// WIP
```

### Pointers
Make something a pointer by adding * to the end of a type
```cs
int32 number = 32;
int32* number_ptr = &number;
```

Pointers are not null by default to make it nullable add ? operator
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
defer delete number_heap 
```