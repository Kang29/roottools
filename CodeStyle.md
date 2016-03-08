# Introduction #

In order to maintain a clean codebase, there are guidelines that must be adheared to so that all the code has a uniform style and formatting. Code style for RootTools mirrors the code style laid out by the [Android Open Source Project](http://source.android.com/source/code-style.html). You should read the entire specification, but for your convenience, highlights are laid out below.

# Code Style Guidelines #

The following is taken directly from the AOSP website linked to above. Note that not all existing code follows these rules, but all new code is expected to.

## Java Language Rules ##
We follow standard Java coding conventions. We add a few rules:

  1. [Deprecation](CodeStyle#Deprecation.md): do it
  1. Exceptions: Never catch and ignore them without explanation.
  1. Exceptions: do not catch generic Exception, except in library code at the root of the stack.
  1. Finalizers: generally don't use them.
  1. [Imports](CodeStyle#Imports.md): Fully qualify imports

## Java Style Rules ##
Programs are much easier to maintain when all files have a consistent style. We follow the standard Java coding style, as defined by Sun in their [Code Conventions for the Java Programming Language](http://java.sun.com/docs/codeconv/html/CodeConvTOC.doc.html), with a few exceptions and additions. This style guide is comprehensive and detailed and is in common usage in the Java community.

In addition, we enforce the following style rules:

  1. Comments/Javadoc: write it; use standard style
  1. Short methods: don't write giant methods
  1. [Fields](CodeStyle#Field_Names.md): should either be at the top of the file, or immediately before the methods that use them
  1. Local variables: limit the scope
  1. Imports: android; third party alphabetical; java(x)
  1. Indentation: 4 spaces, no tabs.
  1. Line length: 100 columns
  1. Field names: Non-public, non-static fields start with m.
  1. [Braces](CodeStyle#Braces.md): Opening braces don't go on their own line.
  1. Annotations: Use the standard annotations.
  1. Acronyms are words: Treat acronyms as words in names, yielding XmlHttpRequest, getUrl(), etc.
  1. [TODO style](CodeStyle#TODO_style.md): "TODO: write this description"
  1. Consistency: Look at what's around you!
  1. Logging: Be careful with logging. It's expensive.

---

# Notes #

Below are clarifications of some of the above rules. Not all rules are clarified here, but you can check the link to the AOSP code style guidelines for clarification of anything else you need.

Again, most of the text below is taken directly from the AOSP website.

---

## Deprecation ##
If you change or delete a method that was available in a public (numbered) release, be sure to leave the old method in and deprecate it. This is done by putting a `@deprecated` as well as when it was deprecated and a link to what replaced it in the javadoc, and by putting a `@Deprecated` just before the method. For example:
```
/**       
 * @return <code>true</code> if BusyBox was found
 *
 * @deprecated As of release 0.7, replaced by {@link #isBusyboxAvailable()}       
 */
@Deprecated
public static boolean busyboxAvailable() {
   return isBusyboxAvailable();
}
```
If at all possible, retain the original functionality of the method that is deprecated.

Methods that have been deprecated will be removed from the code eventually, although no timeline is given yet for how long to keep deprecated methods around.

---

## Imports ##
### Wildcards in imports ###
What it is: When you want to use class Bar from package foo,there are two possible ways to import it:

  1. `import foo.*;`
  1. `import foo.Bar;`
Pros of #1: Potentially reduces the number of import statements.

Pros of #2: Makes it obvious what classes are actually used. Makes code more readable for maintainers.

Decision: Use style #2 for importing all Android code. An explicit exception is made for java standard libraries (`java.util.*, java.io.*`, etc.) and unit test code (`junit.framework.*`).

---

## Indentation ##
We use 4 space indents for blocks. We never use tabs. When in doubt, be consistent with code around you.

We use 8 space indents for line wraps, including function calls and assignments. For example, this is correct:

```
Instrument i =
        someLongExpression(that, wouldNotFit, on, one, line);
```
and this is not correct:
```
Instrument i =
    someLongExpression(that, wouldNotFit, on, one, line);
```

---

## Field Names ##
  * Non-public, non-static field names start with m.
  * Static field names start with s.
  * Other fields start with a lower case letter.
  * Public static final fields (constants) are ALL\_CAPS\_WITH\_UNDERSCORES.
For example:
```
public class MyClass {
    public static final int SOME_CONSTANT = 42;
    public int publicField;
    private static MyClass sSingleton;
    int mPackagePrivate;
    private int mPrivate;
    protected int mProtected;
}
```

---

## Braces ##
Braces do not go on their own line; they go on the same line as the code before them. So:
```
class MyClass {
    int func() {
        if (something) {
            // ...
        } else if (somethingElse) {
            // ...
        } else {
            // ...
        }
    }
}
```
We require braces around the statements for a conditional. Except, if the entire conditional (the condition and the body) fit on one line, you may (but are not obligated to) put it all on one line. That is, this is legal:
```
if (condition) {
    body(); // ok 
}
if (condition) body(); // ok
```
but this is still illegal:
```
if (condition)
    body(); // bad
```

---

## TODO style ##
Use TODO comments for code that is temporary, a short-term solution, or good-enough but not perfect.

TODOs should include the string TODO in all caps, followed by a colon:
```
// TODO: Remove this code after the UrlTable2 has been checked in.

// TODO: Change this to use a flag instead of a constant.
```
If your TODO is of the form "At a future date do something" make sure that you either include a very specific date ("Fix by November 2005") or a very specific event ("Remove this code after all production mixers understand protocol V7.").