# C++ Coding Guidelines

This covers the basic coding guidline for all c++ code that is submitted to the project. 

## Files
Files should be named according to their class names:

Ex. 
class **Terrain** = **Terrain**.h + **Terrain**.cpp

## Naming
| Behaviour | Convention |
| --- | --- |
| Classes, Structs, Enum Classes and Typedefs | PascalCase |
| Functions | under_case |
| Private + Protected member variables | _camelCase |
| Public member variables | camelCase |
| Local variables | camelCase |
| Constants | UNDER_CASE |

## Class Layout
```cpp
#pragma once

#include <memory>

namespace gameplay
{
namespace example
{

class Bar;

class GP_API Foo
{
public:

    Foo();
    
    ~Foo();
    
    Bar* get_bar() const;
    
    void set_bar(Bar* bar);

private:
    std::unique_ptr<Bar> _bar;
};

}
}
```

## Classes
- Each access modifier appears no more than once in a class, in the order:
**public**, **protected**, **private**.
- All **public** member variables live at start of the class.
- All **private** member variables live at the end of the class.
- Use **protected** member variables judiciously.
- Constructors and destructors are the first methods in a class after **public** member variables.
- The method implementations in cpp should appears in the order which they are declared in the class.
- Avoid **inline** implementations unless absolutely needed and are trivial and short.
- Add GP_API on all classes for export into shared library

## Enums
- Should be declared in classes directly after constructor and destructors.
- Keep the naming simple and short-and-sweet.
```cpp
namespace gameplay
{
class GP_API Camera
{
public:

    Camera();
 
    ~Camera();

    enum class Mode : uint32_t
    {
        PERSPECTIVE,
        ORTHOGRAPHIC
    };

    Mode get_mode() const;
```

## Flags (bitwise)
- We do not use enums for flags. They are instead flags, hints, etc:
```cpp
namespace gameplay
{
typedef uint32_t SpecialFlags;
constexpr SpecialFlags SPECIAL_FLAG_NONE = 0;
constexpr SpecialFlags SPECIAL_FLAG_THIS = 1 << 0;
constexpr SpecialFlags SPECIAL_FLAG_THAT = 1 << 1;
    
```

## Auto
- Use **auto** sparingly and when needed to abstract a complex type.
- Great for use on **std::** stl containers and smart pointers.
- Be careful about accidental _copy-by-value_ when you meant _copy-by-reference_.
- Understand the difference between **auto** and **auto&**.

## Lambdas
- Lambdas are acceptable where they make sense.
- Focus use around anonymity.
- Avoid over use, but for std algorithms Ex. **std::sort**, etc) they are great.

## Range-based loops
- They're great, use them.
```cpp
```cpp
// Suggests dev might be of type Device.
for (const auto& device : devices) 
```

## Friend
- Avoid using **friend** unless it is needed to restrict access to inter-class interop only.
- It easily leads to _difficult-to-untangle_ interdependencies that are hard to maintain.

## size_t
- Use **size_t** for all size counts and accessing **std::size()** results.

## int
- We use **int** for all dimensions and lengths.

## uint32_t
- Use **uint32_t** instead of **unsigned int** recommended only for bitwise flags, masks, data, etc.

## uint16_t
- Use **uint16_t** instead of **unsigned short** recommended only for data.

## uint8_t
- Use **uint8_t** instead of **unsigned char**.

## float
- The **float** type is the primary precision for all data in the 2D and 3D world.
- Used for low precision time intervals.
- Use **double** is _only_ used when absolutely necessary for higher precision.

## nullptr
- Use **nullptr** instead of _0_ or _NULL_.

## Errors and Logging
- Use **GP_LOG_ERROR** macro for all errors that will stop the program in release and debug modes.
```cpp
GP_LOG_ERROR("Invalid json base64 string for propertyName: {}", propertyName);
```
- Use **GP_LOG_INFO** for one time logging events such as initialization or unexpected singularities.
- Avoid excessive logging which can impact game engine performance, such as logging every frame.

## GP_ Macros and Global Definitions
- Use existing **GP_XXX** for various compile time change functionality.
- Use **GP_XXX** as a prefix for all gameplay scoped macros and global constants.

## Assertions
- Use **GP_ASSERT** for quick danger checks in start of impl that is checked in debug mode.
```cpp
size_t MyReader::read_xxxx(const char* propertyName, float** data)
{
    GP_ASSERT(propertyName);
    GP_ASSERT(data);
```



## Public member variable initialization
- Initialize public member variables directly in .h file for awareness.
```cpp
class GP_API Rectangle
{
public:
    float x = 0.0f;
    float y = 0.0f;
    float width = 0.0f;
    float height = 0.0f;
```

## Protected + private member variable initialization
- Use static initializers as a first line of de
- Initialize all **protected** and **private** member variables in constructor in cpp to obscure.
- Initialize all **protected** and **private** member variables in the order they are declared.
```cpp
MyClass::MyClass() :
    _fooX(0),
    _fooY(0)
     ...
```

## Commenting Headers
- Avoid spelling and grammatical errors.
- Header comments use _doxygen_ format. We are not too sticky on _doxygen_ formatting policy.
- We dont need [in] and [out] on params. It should be obvious in comments anyhow.
- All public functions and variables must be documented.
- The level of detail for the comment is based on the complexity for the api.
- Most important is that comments are simple and have clarity on how to use the api.
- **@brief** is not required and automatic assumed on first line of code summary. Easier to read too.
- **@details** is not dropped and automatic assumed proceeding the summary line.
- **@param** and **@return** are followed with a space after summary or details.

```cpp
    /**
     * Checks whether this cube intersects the specified cube.
     *
     * You would add any specific details that may be needed here. This is
     * only necessary if there is complexity to the user of the function.
     *
     * @param cube The cube to test intersection with.
     * @return true if the specified cube intersects this cube;
     *     false otherwise.
     */
    bool intersects(const Cube& cube) const;
```

## Commenting Implementation
- Clean simple code is the best form of commenting.
- Do not add comments above function definitions in .cpp they are already in header.
- Used to comment necessary non-obvious implementation details not the api.
- Only use // line comments on the line above the code you plan to comment with lower case comments.
- Avoid /* */  block comments. Preventing others from easily doing their own block comments when testing, debugging, etc.
- Avoid explicitly referring to identifiers in comments, since that's an easy way to make your comment outdated when an identifier is renamed.

## Formatting
- You should set your ide or editor to follow the formatting guidelines.
- Keep all code less than **120** characters per line.

## Include
- **#pragma** once is first line of the .h class header.
- Include the corresponding class .h on the first line of the .cpp
- All the other headers, sorted by **local** to **distant** directories. (Ex. #include <vector> stl headers come last)

## Tabs
- Insert **4** spaces for tabs to avoid avoid tab wars.
- Change your ide or editor to replace tabs for spaces.

## Line Spacing
- One line between gameplay namespace.
- One line of space between functions declarations in source and header.
- One line after each class scope section in header.
- Function call spacing:
  - No space before bracket.
  - No space just inside brackets.
  - One space after each comma separating parameters.
```cpp
foo->do_this(x, y, z);
```
- Conditional statement spacing:
  - One space after conditional keywords.
  - No space just inside the brackets.
  - One space separating commas, colons and condition comparison operators.

- **Do not** align blocks of member variables to match spacing causing unnecessary line changes when new variables are introduced.
```cpp
private:
    int          foo;
    Bar          bar;
    Ray          noonoo;
    unsigned int dirtyBits;
```

- Align indentation space for parameters when wrapping lines to match the initial bracket.

**Example 1:**
```cpp
CrazyData::CrazyData(float a0, float b0, float c0, float d0,
                     float a1, float b1, float c1, float d1,
                     float a2, float b2, float c2, float d2,
                     float a3, float b3, float c3, float d3)
```
Generally you should use a struct value by reference or pointer to data instead here anyhow.

**Example 2:**
```cpp
return sqrt((point.x - sphere.center.x) * (point.x - sphere.center.x) +
            (point.y - sphere.center.y) * (point.y - sphere.center.x) +
            (point.z - sphere.center.z) * (point.z - sphere.center.x));
```

- Use a line of space within .cpp implementation function to help organize blocks of code.

```cpp
// Lookup device surface extensions
 ...
 ...

// Create the platform surface connection
 ...
 ...
 ...
```

## Indentation
- Indent next line after all braces { }.
- Braces should always align vertically.
- Always indent the next line of any condition statement line.
- Indent 1 space after each condition or loop before the brackets.

**Example 1**
```cpp
if (box.is_empty())
{
    return;
}
```

**Example 2**
```cpp
for (size_t i = 0; i < count; i++)
{
    if (distance(sphere, points[i]) > sphere.radius)
    {
        return false;
    }
}
```

- Never leave conditional code statements on same line as condition test:
```cpp
if (box.is_mpty()) return;
```
