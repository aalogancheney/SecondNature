# The Second Nature Style Guide

This style guide is taken from the [raywenderlich](https://github.com/raywenderlich/c-sharp-style-guide/blob/master/README.markdown) style guide, but with alterations for our specific needs. 

The overarching goals are __conciseness__, __readability__, __simplicity__, and promoting __good practices__.

## Table of Contents

- [Nomenclature](#nomenclature)
  + [Namespaces](#namespaces)
  + [Classes & Interfaces](#classes--interfaces)
  + [Methods](#methods)
  + [Fields](#fields)
  + [Parameters](#parameters--parameters)
  + [Delegates](#delegates--delegates)
  + [Events](#events--events)
  + [Properties](#properties)
  + [Misc](#misc)
- [Declarations](#declarations)
  + [Access Level Modifiers](#access-level-modifiers)
  + [Fields & Variables](#fields--variables)
  + [Classes](#classes)
  + [Interfaces](#interfaces)
- [Spacing](#spacing)
  + [Indentation](#indentation)
  + [Line Length](#line-length)
  + [Vertical Spacing](#vertical-spacing)
- [Brace Style](#brace-style)
- [Switch Statements](#switch-statements)
- [Language](#language)


## Nomenclature

Naming should be well thought out and should clearly indicate the purpose of a variable/function. 
Prefer verbosity to brevity, although too much detail is also frowned upon.

### Namespaces

Namespaces are all __UpperCamelCase__, multiple words concatenated together,
without
hypens or underscores:

__BAD__:

```c#
secondnature.ai.behavior
```

__GOOD__:

```c#
SecondNature.AI.Behavior
```

### Classes & Interfaces

Written in __UpperCamelCase__. For example `GameController`. 

### Methods

Public, protected, and private methods are written in __UpperCamelCase__. For example `DoSomething`. 

### Fields

Written in __lowerCamelCase__ with a leading __m___ to indicate a member variable.

Static fields should be written in __lowerCamelCase__ with a leading __s___ to indicate a static member variable:

```c#
public static int s_theAnswer = 42;
```

All non-static fields are written __lowerCamelCase__ with a leading __m___. Per Unity convention, this includes __public fields__ as well.

For example:

```c#
public class MyClass 
{
    public static int s_staticPublic;
    public int m_publicField;
    protected int m_myProtected;
    private int m_myPrivate;
}
```

All fields should start with a __m___.

__BAD:__

```c#
private int _myPrivateVariable
private int myPrivateVariable
```

__GOOD:__

```c#
private int m_myPrivateVariable
```


### Parameters & Local Variables

Parameters are written in __lowerCamelCase__.

__BAD:__

```c#
public void DoSomething(Vector3 Location)
```
__GOOD:__

```c#
public void DoSomething(Vector3 location)
```

Single character values to be avoided except for temporary looping variables.

### Delegates

Delegats are written in __UpperCamelCase__.

When declaring delegates, DO add the suffix __EventHandler__ to names of delegates that are used in events. 

__BAD:__

```c#
public delegate void Click()
```
__GOOD:__

```c#
public delegate void ClickEventHandler()
```
DO add the suffix __Callback__ to names of delegates other than those used as event handlers.

__BAD:__

```c#
public delegate void Render()
```
__GOOD:__

```c#
public delegate void RenderCallback()
```
### Events

Prefix event methods with the prefix __On__.

__BAD:__

```c#
public static event CloseCallback Close;
```
__GOOD:__

```c#
public static event CloseCallback OnClose;
```

### Properties

Properties are written in __UpperCamelCase__ with the same name as the associated member variable. 

__BAD:__

```c#
private int m_myInt;
public int myInt { get { return m_myInt; } set { m_myInt = value; } }
```

__GOOD:__

```c#
private int m_myInt;
public int MyInt { get { return m_myInt; } set { m_myInt = value; } }
```

### Misc

In code, acronyms should be capitalized in full. For example:

__BAD:__

```c#
XmlHttpRequest
String url
findPostById
```
__GOOD:__

```c#
XMLHTTPRequest
String URL
findPostByID
```

## Declarations

### Access Level Modifiers

Access level modifiers should be explicitly defined for classes, methods and
member variables.

### Fields & Variables

Prefer single declaration per line.

__BAD:__

```c#
string m_username, m_twitterHandle;
```

__GOOD:__

```c#
string m_username;
string m_twitterHandle;
```

### Classes

Exactly one class per source file, although inner classes are encouraged where
scoping appropriate.

### Interfaces

All interfaces should be prefaced with the letter __I__. 

__BAD:__

```c#
RadialSlider
```

__GOOD:__

```c#
IRadialSlider
```

## Spacing

Spacing is especially important, as code needs to be easily readable. 

### Indentation

Indentation is using spaces - never tabs.

#### Blocks

Indentation for blocks uses 4 spaces:

__BAD:__

```c#
for (int i = 0; i < 10; i++) 
{
  Debug.Log("index = " + i);
}
```

__GOOD:__

```c#
for (int i = 0; i < 10; i++) 
{
    Debug.Log("index = " + i);
}
```

#### Line Wraps

Indentation for line wraps should use 8 spaces:

__BAD:__

```c#
CoolUIWidget widget =
    SomeIncrediblyLongExpression(that, reallyWouldNotFit, on, aSingle, line);
```

__GOOD:__

```c#
CoolUIWidget widget =
        SomeIncrediblyLongExpression(that, reallyWouldNotFit, on, aSingle, line);
```

### Line Length

Lines should be no longer than 100 characters long.


### Vertical Spacing

There should be exactly one blank line between methods to aid in visual clarity 
and organization. Whitespace within methods should separate functionality, but 
having too many sections in a method often means you should refactor into
several methods.


## Brace Style

All braces should appear on their own lines. This helps to visually line up the 
braces and clearly identify code blocks:

__BAD:__

```c#
class MyClass {
    void DoSomething() {
        if (m_someTest) {
            // ...
        }
        else {
            // ...
        }
    }
}
```

__GOOD:__

```c#
class MyClass 
{
    void DoSomething() 
    {
        if (m_someTest) 
        {
            // ...
        } else 
        {
            // ...
        }
    }
}
```

Conditional statements are always required to be enclosed with braces,
irrespective of the number of lines required.

__BAD:__

```c#
if (m_someTest)
    DoSomething();
if (m_someTest) DoSomethingElse();
```

__GOOD:__

```c#
if (m_someTest) 
{
    DoSomething();
}
if (m_someTest) { DoSomethingElse(); }
```


## Switch Statements

Switch statements fall-through by default, but this can be unintuitive. Do __not__ use fall-through behavior. 

Alway include the `default` case.

## Language

Use US English spelling.

__BAD:__

```c#
string m_colour = "red";
```

__GOOD:__

```c#
string m_color = "red";
```
