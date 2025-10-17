### Namespace

The default namespace of a piece of code is the filename it was written in
you can define your own namespace using "namespace" keyword
examples

```cs
// this namespace will apply to the entire file
namespace Math;
```

but you can namespace a portion of code like this
```cs
namespace Math
{
    // private by default
    public float abs(float number)
    {
        return number * -1 if number < 0 else number;
    }
}
```
or with :: operator
```cs
public float Math::abs(float number)
{
    return number * -1 if number < 0 else number;
}

// accessing namespace is using :: operator like in c++
float result = Math::abs(10);
```

you can defined nested namespace like this
```cs
namespace OperatingSystem::Console;

namespace OperatingSystem::Display
{

}

public float OperatingSystem::Console::beep()
{

}
```

importing namespaces
file `Math.astral`
```cs
public float abs(float number)
{
    return number * -1 if number < 0 else number;
}
```

file `Main.astral`
```cs
import Math;

void main()
{
    float result = Math::abs(10);
}
```

namespace alias to prevent namespace collisions

file `Main.astral`
```cs
import Math as Mathematic;
import OperatingSystem::Console as Console;

void main()
{
    Console::println(Mathematic::abs(10));
}
```

if there is a namespace clash it will error 
like for example
file `Main.astral`
```cs
import Math;

public float Math::abs(float number)
{
    // oops there is already abs in Math namespace this will error
}

void main()
{
    float result = Math::abs(10);
}
```

### Data Types

```cs
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

int32[] numbers = [1, 2, 3, 4];

List<string> names = ["Bob", "Steve", "Alex"];
names.add("robert"); 

Dictionary<int32, Person> people = heapalloc(){
    {
        2424525,
        Person("Bob", 24)
    },
    
    {
        32324,
        Person("Steve", 32)
    }
} 


enum Weapon
{
    Sword,
    Bow,
    Gun
}

// struct is a value types and it will be allocated on the stack by default
struct Color
{
    
}

// function pointers
int32 function(int32 number) mapper = (int32 number) 
{
    return number * number;
};

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
int32 width = null // Error

// to make it nullable use the ? operator
int32? maybe_height = null

// check if it null
if (maybe_height is int32 height)
{
    import OperatingSystem::Console as Console;

    Console::println(height);
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
    import OperatingSystem::Console as Console;

    Console::println(&number_ptr);
}

// WIP
```

### Memory Allocation
```cs
import ProgrammingLanguage::Utils as Utils;

// allocate on the stack using default allocator
int32* number_stack = Utils::stack_alloc(Utils::size_of(int));
*number_stack = 10;

// allocate on the heap using default allocator
int32* number_heap = Utils::heapalloc(Utils::size_of(int));
*number_heap = 20;
```