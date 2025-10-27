### Namespace

The default namespace of a piece of code is project name + file path of the file + the file name it was written in <br>
for example
the file path is `/src/main.astral` and project name is test_project
```cs
import system::console as console

// the namespace for the main function would be test_project::src::main
void main()
{
    console::println("Hello World");
}
```

you can define your own namespace using "namespace" keyword
for examples

```cs
// this namespace will apply to the entire file
namespace math;
```

but you can namespace a portion of code like this
```cs
namespace math
{
    // private by default
    public float abs(float number)
    {
        return number * -1 if number < 0 else number;
    }
}
```

you can defined nested namespace like this
```cs
namespace system::console;

namespace system::display
{

}

public float system::console::beep()
{

}
```

importing namespaces
file `math.astral`
```cs
public float abs(float number)
{
    return number * -1 if number < 0 else number;
}
```

file `main.astral`
```cs
import math;

void main()
{
    float result = math::abs(10);
}
```

namespace alias to prevent namespace collisions

file `main.astral`
```cs
import math as mathematic;
import system::console as console;

void main()
{
    console::println(mathematic::abs(10));
}
```

if there is a namespace clash it will error in compile time
like for example
file `main.astral`
```cs
import math;

public float math::abs(float number)
{
    // oops there is already abs in math namespace this will error at compile time
}

void main()
{
    float result = math::abs(10);
}
```

### Data Types

```cs
//primitive data types are all defaulted to be allocated on the stack

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

// this is allocated on the stack by default
int32[] numbers = [1, 2, 3, 4];

import programming_language::collections;

// this is allocated on the stack by default
List<string> names = ["Bob", "Steve", "Alex"];
names.add("robert"); 

// this is allocated on the stack by default
Dictionary<int32, Person> people = {
    {
        2424525,
        heapalloc Person("Bob", 24)
    },
    
    {
        3232224,
        heapalloc Person("Steve", 32)
    }
}

// or you can specify where you want to allocate
var people = heapalloc Dictionary<int32, Person>()
{
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

enum Weapon
{
    Sword,
    Bow,
    Gun
}

struct Color
{
    
}

// function pointers
int32 function(int32 number) double_it = (int32 number) 
{
    return number * number;
};

// WIP
```

### Variables
```cs
// all variables is immutable by default like rust
int32 score = 10;
score = 20; // Error: Cannot modify immutable variable "score"

// make it mutable
mutable int32 size = 20;
size = 30; // This work

// all variables are also not null by default
int32 width = null; // Error

// to make it nullable use the ? operator
int32? maybe_height = null;
// you can also leave it uninitialize and it will assign it to null (no default values)
string? maybe_name;
bool? maybe_enabled;

// check if it null
if (maybe_height is int32 height)
{
    import system::console as console;

    console::println(height);
}

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
    import system::console as console;

    console::println(&number_ptr);
}

// nullable pointer to a nullable pointer to a nullable pointer
int32*?*?*? something = null;

// WIP
```

### Memory Allocation
```cs

// allocate on the stack using default allocator
int32* number_stack = stackalloc int;
*number_stack = 10;

// allocate on the heap using default allocator
int32* number_heap = heapalloc int;
*number_heap = 20;
```