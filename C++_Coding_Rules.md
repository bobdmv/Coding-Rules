# C++ Coding Rules

Coding rules are a set of practices that are designed to improve the quality of the code, as well as its readability through the use of a unified code style. Do not fit your code to the rules just for the sake of compliance with them. If this impairs readability, then it’s better to leave it as it is, do not make it worse. Coding rules is a living document, the team should update it regularly with discovered/learned good practices.

This document is a compilation of various recommendations from the following sources:
- [C++ Core Guidelines](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)
- [Bjarne Stroustrup's C++ Style and Technique](https://www.stroustrup.com/bs_faq2.html)
- [LLVM Coding Standards](https://llvm.org/docs/CodingStandards.html)
- [Qt Coding Style](https://wiki.qt.io/Qt_Coding_Style)

Also, this document was developed taking into account the fact that other popular programming languages have official style guides, for example:
- [C# Coding Conventions](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- [Java Code Conventions](https://www.oracle.com/java/technologies/javase/codeconventions-introduction.html)
- [Kotlin Coding conventions](https://kotlinlang.org/docs/coding-conventions.html)
- [Effective Dart: Style](https://dart.dev/guides/language/effective-dart/style)

They are quite similar to each other. Therefore, the code style guidelines in this document try to be as similar as possible to them, to make it easier for a developer to switch to another language and participate in projects that use multiple languages at once.

## 1. Names

Choose simple names that match the semantics and roles of the underlying entities, within reason. Avoid abbreviations unless they are commonly known.

### 1.1. Types

Type names (including classes, structs, enums, typedefs, etc) should be nouns in camel case, and start with an upper-case letter.

```C++
class Foo // Do
{
  ...
}

class bar // Don't
{
  ...
}

class foo_Bar : public Foo, public bar // Don't
{
  ...
}
```

#### 1.1.1 Enumerators

Enumerators should start with an upper-case letter, just like types. Unless the enumerators are defined in their own small namespace or inside a class, enumerators should have a prefix corresponding to the enum declaration name.

```C++
class Foo
{
public:
  enum class Flags // Do
  {
    One,
    Two,
    Three
  }

  enum Mode // Allowed, not recommended
  {
    Mode_One,
    Mode_Two,
  };

  enum Option // Don't
  {
    One   = 1,
    Two   = 2,
    Three = 4
  };
}
```

### 1.2 Variables

Variable names should be nouns (as they represent state). The name should be in camel case, and start with a lower-case letter.

```C++
int calcSomething(const int userVal) // Do
{
  const int firstVal = 1;   // Do
  const int second_val = 2; // Don't

  const int Result = firstVal + second_val * userVal; // Don't
}
```

#### 1.2.1. Protected and private fields

Protected and private fields should follow the "Variables names" rule and they should have “m_” prefix.

```C++
class Foo
{
protected:
  int m_protectedField; // Do
  int protected_field;  // Don't
  int protectedField;   // Don't

private:
  int m_privateField; // Do
  int PrivateField;   // Don't
  int PRIVATE_FIELD   // Don't
};
```

#### 1.2.2. Global variables

Global variables should follow the the "Variables names" rule and if global variable is used across multiple files it must be prefixed with “g_”. If it is used in single file it should be prefixed with “s_”.

### 1.3 Functions

Function names should be verb phrases (as they represent actions), and command-like function should be imperative. The name should be in camel case, and start with a lower-case letter.

```C++
class Foo
{
public:
  int publicMethod();  // Do
  int PublicMethod();  // Don't
  int publicmethod();  // Don't
  int public_method(); // Don't
};
```

#### 1.3.1. Getters and setters

Getters and setters should follow the above rule. Also getter should be prefixed with “get” or “is”. There are a few alternatives to the “is” prefix that fit better in some situations. These are the “has”, “can” and “should” prefixes. Try not to use these prefixes for functions that look like simple getters, but actually do some calculations inside to return a value.

It is allowed to write getters without a prefix, but the presence of a prefix allows getters to be grouped in the IDE autocompletion list.

Setters should be prefixed with “set”.

```C++
class Foo
{
public:
  int  getValue();        // Good, recommended
  int  value();           // Allowed, not recommended
  void setValue(int val); // Good

protected:
  bool isSomethingEnabled();                  // Good
  void setSomethingEnabled(const bool value); // Good
  bool hasLicense();                          // Good
  bool canEvaluate();                         // Good
  bool shouldSort();                          // Good
};
```

### 1.4. Preprocessor macros

Preprocessor macros must be all uppercase with words separated by underscores.

```C++
#define NB_COLORS  256 // Do
#define nbColors   256 // Don't
#define m_nbColors 256 // Don't
#define g_nbColors 256 // Don't
#define nb_colors  256 // Don't
```

Try to avoid using macros to define constant primitives. It is better to use "constexpr" variables.

### 1.5 Files

Since most files usually contain a declaration or implementation of only one type, then name the files according to the "Types names" rule.

Use the following as file extensions:
- .cpp - for files that contain implementations;
- .h - for files that contain a declarations;
- .hpp - for header-only types which contain declaration and implementation in a single file.

## 2. Formatting

### 2.1. Indentation

Use only spaces, and indent 2 spaces at a time. Do not use tabs in your code. You should set your editor to emit spaces when you hit the tab key.

### 2.2. Lines length

Each line of text in your code should be at most 80 characters long. However, if trimming your code leads to worse readability, this coding rule can be tolerated, but the line of text still must not exceed 120 characters long. Consider setting up guidelines in your IDE.

### 2.3. New lines

#### 2.3.1. Open Braces

Opening brace must be always on a new line for namespaces, types, functions, control statements (if, else, for, while, etc.). Control statements should always have braces, even if they contain only one line in their body.

Do:
```C++
void Solver::checkAndExec()
{
  if (m_checker.isFirstParameterOk(m_firstParam))
  {
    m_executor.doSomethingWithFirstParameter(m_firstParam);
  }
  m_executor.doAdditionalActionWithFirstParameter(m_firstParam);

  if (m_options.isSomeOptionEnabled()
      && m_checker.isSecondParameterOk(m_secondParam))
  {
    m_executor.doSomethingWithSecondParameter(m_secondParam);
  }
  m_executor.doAdditionalActionWithSecondParameter(m_secondParam);
}
```

Don't:
```C++
void Solver::checkAndExec()
{
  if (m_checker.isFirstParameterOk(m_firstParam)) m_executor.doSomethingWithFirstParameter(m_firstParam);
  m_executor.doAdditionalActionWithFirstParameter(m_firstParam);

  if (m_options.isSomeOptionEnabled()
      && m_checker.isSecondParameterOk(m_secondParam))
    m_executor.doSomethingWithSecondParameter(m_secondParam);
  m_executor.doAdditionalActionWithSecondParameter(m_secondParam);
}
```

#### 2.3.2. Closing Braces

Closing curly braces must be always on a new line. Empty types or empty function bodies can have closing brace on same line as open brace.

#### 2.3.3. Keywords

Keywords “else”, “while” and “catch” should be always on a new line.

#### 2.3.4. Switch statements

Switch statements should follow above rules. "break" keyword should be inside braces.

```C++
switch (condition)
{
  case 1:
  {
    ...
    break;        // Do
  }
  case 2:
  {
    ...
  }
  break;          // Don't
  case 3:
    ...
    break;        // Don't
  default: break; // Do
}
```

In some cases rules can be tolerated for better readability.

```C++
bool Document::upgradeFormat(int oldVersion)
{
  bool isOk = true;
  switch (oldVersion)
  {
    case 0: isOk &= upgradeFrom0To1();
    case 1: isOk &= upgradeFrom1To2(); break;
    //case 2: isOk &= upgradeFrom2To3(); remove break from case 1 and put it here
    default: return false;
  }
  return isOk;
}
```

#### 2.3.5. Operators

Operators start at the beginning of the new lines. An operator at the end of the line is easy to miss.

```C++
//Do
int result = longExpression
             + otherLongExpression
             + otherOtherLongExpression
// Don't
int result = longExpression +
             otherLongExpression +
             otherOtherLongExpression

// Do
if (longCondition
    && otherLongCondition
    && otherOtherLongCondition)
{
  ...
}

// Don't
if (longCondition &&
    otherLongCondition &&
    otherOtherLongCondition)
{
  ...
}
```

### 2.4. Whitespaces

#### 2.4.1. Operators

Operators should be surrounded by a space character.

```C++
int a = 1 + 2 * 3; // Do
int b=4*5-6;       // Don't
```

#### 2.4.2. Control statements

Control statements should include space character before opening parenthesis. Don't put space character after opening or before closing parenthesis. Semicolon characters in "for" statements should be followed by a space character.

```C++
// Do
if (condition1)
{
  // Do
  while (condition2)
  {
    doSomething();
  }
}

// Don't
if( condition3 )
{
  // Don't
  for(int i=0;i<10;++i)
  {
    doSomethingElse();
  }
}
```

#### 2.4.3. Functions arguments

Don't put space character before or after opening/closing parenthesis in functions. Comma characters in functions arguments should be followed by a space character.

```C++
doSomething(a, b, c, d);   // Do
doSomething (a, b, c, d);  // Don't
doSomething( a, b, c, d ); // Don't
doSomething(a,b,c,d);      // Don't
```

### 2.5. Class members order

When defining the class use the following order:

1. Public types and aliases;
2. Static constants;
3. Static functions;
4. Constructors and assignment operators;
5. Destructor;
6. Public functions;
7. Protected functions;
8. Private functions;
9. Public fields;
10. Protected fields;
11. Private fields.

Also it is good to group getter with setter. Don’t separate them to only getters block and only setters block. Try to declare and define functions in the workflow order. It is better to understanding how to work with class and better for refactoring.

```C++
class Foo
{
public:
  enum Mode
  {
    Mode_One,
    Mode_Two,
  };

  enum Option
  {
    Option_One   = 1,
    Option_Two   = 2,
    Option_Three = 4
  };

public:
  Foo();

public:
  Mode getMode() const;
  void setMode(const Mode mode);

  int getOptions() const;
  void setOptions(const int options = Option_One | Option_Two);

  bool perform();

  int result() const;

protected:
  bool doPreparations();

  bool calculateResult();

private:
  Mode m_mode;
  int  m_options;
  int  m_result;
};
```

### 2.6. Includes

#### 2.6.1. Order of includes

Organize includes alphabetically.

#### 2.6.2. Grouping

Organize includes in logical groups using comment lines to give group names.

Do:
```C++
// My includes.
#include <MyClass.h>
#include <MyAnotherClass.h>

// 3rd-party includes.
#include <Some_Builder.hxx>
#include <Some_Shape.hxx>

// Standard includes.
#include <vector>
```

Don't:
```C++
#include <Some_Builder.hxx>
#include <Some_Shape.hxx>
#include <vector>
#include <MyClass.h>
#include <MyAnotherClass.h>
```

#### 2.6.3. Order of groups

Includes belonging to the current library go first. Includes belonging to related libraries go next. Includes belonging to external libraries go last.

## 3. Documentation

### 3.1. General

Try to document all types, functions and their parameters. Documentation should be meaningful, not just copy-pasting function and variables names.

Documentation should be in the header files, do not put it in the implementation files.

Documentation should follow [Doxygen C++ style](https://doxygen.nl/manual/docblocks.html).

Use `//` for regular comments and `///` for documentation.

Use at-sign (`@`) for [Doxygen commands](https://www.doxygen.nl/manual/commands.html) to avoid potential problems with other tools.

Use [@see](https://www.doxygen.nl/manual/commands.html#cmdsee) command to cross-references to classes, functions, methods, variables, files or URLs. Two names joined by either :: or # are understood as referring to a class and one of its members. One of several overloaded methods or constructors may be selected by including a parenthesized list of argument types after the method name.

### 3.1 Classes

Use [@code](https://www.doxygen.nl/manual/commands.html#cmdcode) command to put inline usage examples of your class in its description. By default the language that is assumed for syntax highlighting is based on the location where the `@code` block was found. If it is unclear from the context which language is meant (for instance the comment is in a .txt or .markdown file) then you can also explicitly indicate the language, by putting the file extension typically that doxygen associated with the language in curly brackets after the code block: `@code{.cpp}`

Examples:
```C++
/// Brief description of the class on one line and in one sentence.
/// A more detailed description of the class. Preferably with examples of use.
/// Usage:
/// @code
///   TestClass test(param1, param2);
///   test.prepare();
///   int res = test.getResult();
/// @endcode
class TestClass
{
  ...
};
```

### 3.2 Functions

Use [@param](https://www.doxygen.nl/manual/commands.html#cmdparam) command for parameter description.

Use [@returns](https://www.doxygen.nl/manual/commands.html#cmdreturns) command for a function return value description.

Examples:
```C++
/// Copies bytes from a source memory area to a destination memory area,
/// where both areas may not overlap.
/// @param[out] dest the memory area to copy to.
/// @param[in]  src  the memory area to copy from.
/// @param[in]  n    the number of bytes to copy.
void memcpy(void* dest, const void* src, const size_t n);
```

```C++
/// A normal member taking two arguments and returning an integer value.
/// @param a[in] an integer argument.
/// @param s[in] a  constant character pointer.
/// @returns test results
/// @see testMeToo()
/// @see publicVar()
int testMe(const int a, const char* s);
```

Note that you can also document multiple parameters with a single `@param` command using a comma separated list.

Example:
```C++
/// Sets the position.
/// @param x,y,z coordinates of the position in 3D space.
void setPosition(double x, double y, double z);
```

#### 3.2.1 Getters/Setters

Don't document getters and setters if they are simple enough. This kind of comment doesn’t bring more information than the function prototype, and must be potentially maintained. It should be clear what these methods do.

Do:
```C++
/// Foo is the adjustment factor used in the Bar-calculation. It has
/// a default value depending on the Bar type, but can be adjusted
/// on a per-case base.
///
/// @param[in] foo must be greater than 0 and not greater than MAX_FOO.
void setFoo(const float foo);
```

Allowed, if getter and setter are simple enough:
```C++
Solver* getSolver() const;
void setSolver(const Solver* solver);
```

Don't:
```C++
/// @return solver
Solver* getSolver() const;

/// Sets new solvers.
/// @param[in] solver new solver to set.
void setSolver(const Solver* solver);
```

### 3.3 Members

If you want to document the members of a file, struct, union, class, or enum, it is sometimes desired to place the documentation block after the member instead of before. For this purpose you have to put an additional `<` marker in the comment block. Note that this also works for the parameters of a function.

Examples:
```C++
int var; ///< Brief description after the member.
```

## 4. Miscellaneous

### 4.1. #pragma once

Use "#pragma once" instead of include guards. "#pragma once" serves the same purpose as include guards, but with several advantages, including: less code, avoidance of name clashes, and sometimes improvement in compilation speed.

### 4.2. Separate declaration and definition

Try not to put the implementation of methods, even simple ones, in the header files, unless absolutely necessary. Modern compilers are smart enough to optimize them on their own.

### 4.3. Methods override

For overridden methods you must use “override” keyword. This will prevent you from painfully debugging when the someone changed base class behaviour and your child class “suddenly” stopped working.

### 4.4. Functions arguments

Arguments that will not be modified inside function should have “const” keyword. Even if you pass them as copy.

### 4.5. Exit early

Don’t stack conditions blocks inside each other. It increases code complexity and limits space for code (remember about the "Lines length" rule).

Do:

```C++
std::string performSomeEquation()
{
  if (!condition1)
  {
    return "condition1 failed";
  }

  if (!condition2)
  {
    return "condition2 failed";
  }

  if (!condition3)
  {
    return "condition3 failed";
  }

  return "OK";
}
```

Don't
```C++
std::string performSomeEquation()
{
  if (condition1)
  {
    if (condition2)
    {
      if (condition3)
      {
        return "OK";
      }
      else
      {
        return "condition3 failed";
      }
    }
    else
    {
      return "condition2 failed";
    }
  }
  else
  {
    return "condition1 failed";
  }

  return "failed";
}
```

### 4.6. "auto" keyword

There is no strict rule for using "auto" keyword. Use it when you feel that it will improve readability. For example, when the type of an object is not particularly important, such as an iterator. Or when the type is obvious from the context, for example, when getting an object from a collection. In other cases, prefer to explicitly specify the type.

### 4.7 "int* p;" or "int *p;"

Use "int* p;" because it is easer to read, since the type of p is int*.

To avoid potential error:
```C++
int* p, p1;	// probable error: p1 is not an int*
```
follow the rule: declare one name per line.
