# The Second Nature Style Guide

This style guide is taken from the [raywenderlich](https://github.com/raywenderlich/c-sharp-style-guide/blob/master/README.markdown) style guide, but with alterations for our specific needs. 

The overarching goals are __conciseness__, __readability__, __simplicity__, and promoting __good practices__.

## Table of Contents

- [Nomenclature](#nomenclature)
  + [Namespaces](#namespaces)
  + [Classes & Interfaces](#classes--interfaces)
  + [Methods](#methods)
  + [Fields](#fields)
  + [Parameters & Local Variables](#parameters--parameters)
  + [Delegates](#delegates--delegates)
  + [Events](#events--events)
  + [Properties](#properties)
  + [Misc](#misc)
- [Practices](#practices)
  + [Serialized Fields](#serialized-fields)
  + [Properties](#properties)
  + [Optimization](#optimization)
  + [Inheritence](#inheritence)
  + [Interfaces](#interfaces)
  + [Tooltips](#tooltips)
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
- [Version Control](#version-control)


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

Parameters and local variables within functions are written in __lowerCamelCase__.

__BAD:__

```c#
public void DoSomething(Vector3 Location)
{
    Vector3 TempLocation = Location + Location;
}
```
__GOOD:__

```c#
public void DoSomething(Vector3 location)
{
    Vector3 tempLocation = location + location;
}
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

## Practices

The following guidelines highlight important practices that should be followed. These rules are not hard-and-fast
and they may be broken from time to time, but generally they help to write better code earlier in the project 
without the need for extensive debugging.

### Serialized Fields

Fields in a class should be marked private and tagged with the `[SerializeField]` property to expose it in the Editor.

__BAD__:

```c#
public bool m_myBool;
```

__GOOD:__

```c#
[SerializeField]
private bool m_myBool;
```

This should only apply to information that needs to be exposed in the Editor. If a variable is truly private and
doesn't need to be shown in the Editor, then it does not need a `[SerializeField]` tag.

### Properties

Properties should be preferred over using access restrictions on variables. This has two distinct advantages:
It allows for specific read-only and/or write-only access to variables; and it allows extra code to be written
for variable modifications.

For example:

```c#
[SerializeField]
private float m_myReadWriteFloat;

[SerializeField]
private int m_myReadInt;

private int m_floatAccessCounter = 0;
private int m_floatModificationCounter = 0;
private int m_intAccessCounter = 0;

public float MyFloat 
{ 
    get 
    { 
        ++m_floatAccessCounter;
        return m_myReadWriteFloat; 
    } 
    set 
    { 
        ++m_floatModificationCounter;
        m_myReadWriteFloat = value; 
    } 
}

public int MyInt
{
    get
    {
        ++m_intAccessCounter;
        return m_myReadInt;
    }
}
```

In the above example, the floating point variable is both read and write while the integer variable is a read-only variable. 
By adding/removing the `get` and `set` methods, the accessability of a variable can be quickly updated, and other
code can accompany the accessing. The example is simplistic, keeping track of a counter, but it could for example
perform any initialization (lazy initialization), or compute a value to return.

### Optimization

Unity provides several methods such as `GameObject.Find()` and `GetComponent<T>()` which are commonly used 
and are very useful but are rather inefficient.
To prevent unnecessary calls to these kinds of functions, they should be used sparingly and only in initialization functions.

For example:

__BAD:__

```c#
public class MyClass
{
    [SerializeField]
    private string m_gameObjectName;

    private void Update ()
    {
        GameObject otherGameObject = GameObject.Find(m_gameObjectName);
        if(otherGameObject != null)
        {
            CoolComponent otherCoolComponent = otherGameObject.GetComponent<CoolComponent>();
            otherCoolComponent.DoCoolThing();
        }
    }
}
```

__GOOD:__

```c#
public class MyClass
{
    [SerializeField]
    private string m_gameObjectName;
    
    private CoolComponent m_coolComponent;
    
    private void Start ()
    {
        GameObject otherGameObject = GameObject.Find(m_gameObjectName);
        if(otherGameObject != null)
        {
            m_coolComponent = otherGameObject.GetComponent<CoolComponent>();
        }
        else
        {
            Debug.LogError("Could not find game object with name " + m_gameObjectName);
        }
    }

    private void Update ()
    {
        m_coolComponent.DoCoolThing();
    }
}
```

In the example above, the expensive operations `GameObject.Find()` and `GetComponent()` were moved into the `Start()` method
and the `CoolComponent` variable was cached as a member variable of the `MyClass` class. Since `Update()` is called every
frame, the first version would waste a lot of resources making function calls while the second option takes advantage
of the initialization MonoBehaviour methods. 

Alternatively, one could simply make the `CoolComponent` variable a serialized field, which exposes it in the Editor,
and the object in question could be dragged to the reference. Different implementation details depend on the particular
needs of the situation, but the basic idea still holds: keep expensive operations in the initialization code, cache variables
when appropriate, and declutter the various Update methods. 

### Inheritence

### Interfaces

### Tooltips

## Declarations

### Access Level Modifiers

Access level modifiers should be explicitly defined for classes, methods and
member variables. Always prefer the strictest version of access and use private
whenever possible. This helps declutter the auto-complete list with unneeded variables and methods.

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
