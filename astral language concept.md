### Namespaces

the default for the namespace is the filename
you can define your own namespace using "namespace" keyword
examples

```cs
// this namespace will apply to the entire file
namespace Program;
```

but you can namespace a portion of code like this
```cs
namespace Math
{
    // private by default
    float abs(float number)
    {
        return number * -1 if number < 0 else number;
    }
}

// accessing namespace is using :: operator like in c++

float result = Math::abs(10);
```