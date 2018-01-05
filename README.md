# Chummy Objective-C Style Guide

## Introduction

Here are some of the documents from Apple that informed the style guide. If something isn’t mentioned here,
it’s probably covered in great detail in one of these:

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)


## Table of Contents

* [Principles](#principles)
   + [Optimize for the reader, not the writer](#optimize-for-the-reader--not-the-writer)
   + [Be consistent](#be-consistent)
* [Code Organization](#code-organization)
* [Spacing and Formatting](#spacing-and-formatting)
   + [Spaces vs. Tabs](#spaces-vs-tabs)
	+ [Line Length](#line-length)
	+ [Method Declarations and Definitions](#method-declarations-and-definitions)
	+ [Conditionals](#conditionals)
	+ [Ternary operator](#ternary-operator)
	+ [Expressions](#expressions)
	+ [Method Invocations](#method-invocations)
	+ [Function Calls](#function-calls)
	+ [Function Length](#function-length)
	+ [Vertical Whitespace](#vertical-whitespace)
* [Naming](#naming)
	+ [File Names](#file-names)
	+ [Class Names](#class-names)
	+ [Category Names](#category-names)
	+ [Objective-C Method Names](#objective-c-method-names)
	+ [Function Names](#function-names)
	+ [Variable Names](#variable-names)
		- [Common Variable Names](#common-variable-names)
		- [Constants](#constants)
* [Types and Declarations](#types-and-declarations)
	+ [Local Variables](#local-variables)
	+ [Unsigned Integers](#unsigned-integers)
* [Macros](#macros)
* [Cocoa and Objective-C Features](#cocoa-and-objective-c-features)
	+ [Identify Designated Initializer](#identify-designated-initializer)
	+ [Override Designated Initializer](#override-designated-initializer)
	+ [Overridden NSObject Method Placement](#overridden-nsobject-method-placement)
	+ [Initialization](#initialization)
	+ [Keep the Public API Simple](#keep-the-public-api-simple)
	+ [Avoid Messaging the Current Object Within Initializers and `-dealloc`](#avoid-messaging-the-current-object-within-initializers-and---dealloc-)
	+ [Setters copy NSStrings](#setters-copy-nsstrings)
	+ [Use Lightweight Generics to Document Contained Types](#use-lightweight-generics-to-document-contained-types)
	+ [BOOL Pitfalls](#bool-pitfalls)
	+ [Interfaces Without Instance Variables](#interfaces-without-instance-variables)
	+ [Case Statements](#case-statements)
	+ [Error Handling](#error-handling)
	+ [Literals](#literals)
	+ [Private Properties](#private-properties)
	+ [Singletons](#singletons)
	+ [Protocols](#protocols)
	+ [Golden Path](#golden-path)
* [Delegate](#delegate)
* [Imports](#imports)
* [Xcode project](#xcode-project)
* [Based on Objective-C Style Guides](#based-on-objective-c-style-guides)

## Principles

### Optimize for the reader, not the writer

Codebases often have extended lifetimes and more time is spent reading the code
than writing it. We explicitly choose to optimize for the experience of our
average software engineer reading, maintaining, and debugging code in our
codebase rather than the ease of writing said code. For example, when something
surprising or unusual is happening in a snippet of code, leaving textual hints
for the reader is valuable.

### Be consistent

When the style guide allows multiple options it is preferable to pick one option
over mixed usage of multiple options. Using one style consistently throughout a
codebase lets engineers focus on other (more important) issues. Consistency also
enables better automation because consistent code allows more efficient
development and operation of tools that format or refactor code. In many cases,
rules that are attributed to "Be Consistent" boil down to "Just pick one and
stop worrying about it"; the potential value of allowing flexibility on these
points is outweighed by the cost of having people argue over them.

### No duplication

If you should implement the same functionality on two different places at the code, please,
move this functionality to the common class/category/method. Don't duplicate the code

## Code Organization

Use `#pragma mark -` to categorize methods in functional groupings and protocol/delegate implementations following this general structure.

```objc
#pragma mark - Lifecycle

- (instancetype)init {}
- (void)dealloc {}
- (void)viewDidLoad {}
- (void)viewWillAppear:(BOOL)animated {}
- (void)didReceiveMemoryWarning {}

#pragma mark - Custom Accessors

- (void)setCustomProperty:(id)value {}
- (id)customProperty {}

#pragma mark - IBActions

- (IBAction)submitData:(id)sender {}

#pragma mark - Public

- (void)publicMethod {}

#pragma mark - Private

- (void)privateMethod {}

#pragma mark - Protocol conformance
#pragma mark - UITextFieldDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate

#pragma mark - NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject

- (NSString *)description {}
```

## Spacing and Formatting 

### Spaces vs. Tabs 

Use only spaces, and indent 4 spaces at a time. We use spaces for indentation.
Do not use tabs in your code.

You should set your editor to emit spaces when you hit the tab key, and to trim
trailing spaces on lines.

### Line Length 

The maximum line length for Objective-C files is 100 columns.

You can make violations easier to spot by enabling *Preferences > Text Editing >
Page guide at column: 100* in Xcode.

### Method Declarations and Definitions 

One space should be used between the `-` or `+` and the return type, and no
spacing in the parameter list except between parameters.

Methods should look like this:

```objectivec 
- (void)doSomethingWithString:(NSString *)theString {
    ...
}
```

The spacing before the asterisk is optional. When adding new code, be consistent
with the surrounding file's style.

If you have too many parameters to fit on one line, giving each its own line is
preferred. If multiple lines are used, align each using the colon before the
parameter.

```objectivec 
- (void)doSomethingWithFoo:(GTMFoo *)theFoo
                      rect:(NSRect)theRect
                  interval:(float)theInterval {
  ...
}
```

When the second or later parameter name is longer than the first, indent the
second and later lines by at least four spaces, maintaining colon alignment:

```objectivec
- (void)short:(GTMFoo *)theFoo
          longKeyword:(NSRect)theRect
    evenLongerKeyword:(float)theInterval
                error:(NSError **)theError {
  ...
}
```

### Conditionals 

Include a space after `if`, `while`, `for`, and `switch`, and around comparison
operators.

**Good:**

```objectivec
for (int i = 0; i < 5; ++i) {
}

while (test) {};
```

Braces may be omitted when a loop body or conditional statement fits on a single
line.

**Good:**

```objectivec
if (hasSillyName) LaughOutLoud();

for (int i = 0; i < 10; i++) {
    BlowTheHorn();
}
```
**Not:**

```objectivec
if (hasSillyName)
    LaughOutLoud();               

for (int i = 0; i < 10; i++)
    BlowTheHorn();                
```

If an `if` clause has an `else` clause, both clauses should use braces.

**Good:**

```objectivec
if (hasBaz) {
    foo();
} else {
    bar();
}
```
**Not:**

```objectivec
if (hasBaz) foo();
else bar();        

if (hasBaz) {
    foo();
} else bar();      
```

Intentional fall-through to the next case should be documented with a comment
unless the case has no intervening code before the next case.

**Good:**

```objectivec
switch (i) {
    case 1:
        ...
        break;
    case 2:
        j++;
        // Falls through.
    case 3: {
        int k;
        ...
        break;
    }
    case 4:
    case 5:
    case 6: break;
}
```
### Ternary operator

The intent of the ternary operator, `?` , is to increase clarity or code neatness.
The ternary SHOULD only evaluate a single condition per expression.
Evaluating multiple conditions is usually more understandable as an if statement or refactored into named variables.

**Good:**

```objectivec
result = a > b ? x : y;
```

**Not:**

```objectivec
result = a > b ? x = c > d ? c : d : y;
```

### Expressions 

Use a space around binary operators and assignments. Omit a space for a unary
operator. Do not add spaces inside parentheses.

**Good:**

```objectivec
x = 0;
v = w * x + y / z;
v = -y * (x + z);
```

Factors in an expression may omit spaces.

**Good:**

```objectivec
v = w*x + y/z;
```

### Method Invocations 

Method invocations should be formatted much like method declarations.

When there's a choice of formatting styles, follow the convention already used
in a given source file. Invocations should have all arguments on one line:

**Good:**

```objectivec
[myObject doFooWith:arg1 name:arg2 error:arg3];
```

or have one argument per line, with colons aligned:

**Good:**

```objectivec
[myObject doFooWith:arg1
               name:arg2
              error:arg3];
```

**Not:**

```objectivec
[myObject doFooWith:arg1 name:arg2  // some lines with >1 arg
              error:arg3];

[myObject doFooWith:arg1
               name:arg2 error:arg3];

[myObject doFooWith:arg1
          name:arg2  // aligning keywords instead of colons
          error:arg3];
```

As with declarations and definitions, when the first keyword is shorter than the
others, indent the later lines by at least four spaces, maintaining colon
alignment:

**Good:**

```objectivec
[myObj short:arg1
          longKeyword:arg2
    evenLongerKeyword:arg3
                error:arg4];
```

Invocations containing multiple inlined blocks may have their parameter names
left-aligned at a four space indent.

### Function Calls 

Function calls should include as many parameters as fit on each line, except
where shorter lines are needed for clarity or documentation of the parameters.

Continuation lines for function parameters may be indented to align with the
opening parenthesis, or may have a four-space indent.

```objectivec
CFArrayRef array = CFArrayCreate(kCFAllocatorDefault, objects, numberOfObjects,
                                 &kCFTypeArrayCallBacks);

NSString *string = NSLocalizedStringWithDefaultValue(@"FEET", @"DistanceTable",
    resourceBundle,  @"%@ feet", @"Distance for multiple feet");

UpdateTally(scores[x] * y + bases[x],  // Score heuristic.
            x, y, z);

TransformImage(image,
               x1, x2, x3,
               y1, y2, y3,
               z1, z2, z3);
```

Use local variables with descriptive names to shorten function calls and reduce
nesting of calls.

```objectivec
double scoreHeuristic = scores[x] * y + bases[x];
UpdateTally(scoreHeuristic, x, y, z);
```

### Function Length 

Prefer small and focused functions.

Long functions and methods are occasionally appropriate, so no hard limit is
placed on function length. If a function exceeds about 40 lines, think about
whether it can be broken up without harming the structure of the program.

Even if your long function works perfectly now, someone modifying it in a few
months may add new behavior. This could result in bugs that are hard to find.
Keeping your functions short and simple makes it easier for other people to read
and modify your code.

When updating legacy code, consider also breaking long functions into smaller
and more manageable pieces.

### Vertical Whitespace 

Use vertical whitespace sparingly.

To allow more code to be easily viewed on a screen, avoid putting blank lines
just inside the braces of functions.

Limit blank lines to one or two between functions and between logical groups of
code.

## Naming 

Names should be as descriptive as possible, within reason. Follow standard
[Objective-C naming
rules](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html).

Avoid non-standard abbreviations. Don't worry about saving horizontal space as
it is far more important to make your code immediately understandable by a new
reader. For example:

**Good:**

```objectivec
// Good names.
int numberOfErrors = 0;
int completedConnectionsCount = 0;
tickets = [[NSMutableArray alloc] init];
userInfo = [someObject object];
port = [network port];
NSDate *gAppLaunchDate;
```

**Not:**

```objectivec
// Names to avoid.
int w;
int nerr;
int nCompConns;
tix = [[NSMutableArray alloc] init];
obj = [someObject object];
p = [network port];
```

Any class, category, method, function, or variable name should use all capitals
for acronyms and
[initialisms](https://en.wikipedia.org/wiki/Initialism)
within the name. This follows Apple's standard of using all capitals within a
name for acronyms such as URL, ID, TIFF, and EXIF.

Names of C functions and typedefs should be capitalized and use camel case as
appropriate for the surrounding code.

### File Names 

File names should reflect the name of the class implementation that they
contain—including case.

Files containing code that may be shared across projects or used in a large
project should have a clearly unique name, typically including the project or
class prefix.

File names for categories should include the name of the class being extended,
like GTMNSString+Utils.h or NSTextView+GTMAutocomplete.h

### Class Names 

Class names (along with category and protocol names) should start as uppercase
and use mixed case to delimit words.

When designing code to be shared across multiple applications, prefixes are
required (e.g. GTMSendMessage). Prefixes are also recommended
for classes of large applications that depend on external libraries.

### Category Names 

Category names should start with a 2-3 character prefix identifying the category
as part of a project or open for general use.

The category name should incorporate the name of the class it's extending. For
example, if we want to create a category on `NSString` for parsing, we would put
the category in a file named `NSString+GTMParsing.h`, and the category itself
would be named `GTMNSStringParsingAdditions`. The file name and the category may
not match, as this file could have many separate categories related to parsing.
Methods in that category should share the prefix
(`gtm_myCategoryMethodOnAString:`) in order to prevent collisions in
Objective-C's global namespace.

There should be a single space between the class name and the opening
parenthesis of the category.

```objectivec
/** A category that adds parsing functionality to NSString. */
@interface NSString (GTMNSStringParsingAdditions)
- (NSString *)gtm_parsedString;
@end
```

### Objective-C Method Names 

Method and parameter names typically start as lowercase and then use mixed case.

Proper capitalization should be respected, including at the beginning of names.

**Good:**

```objectivec
+ (NSURL *)URLWithString:(NSString *)URLString;
```

The method name should read like a sentence if possible, meaning you should
choose parameter names that flow with the method name. Objective-C method names
tend to be very long, but this has the benefit that a block of code can almost
read like prose, thus rendering many implementation comments unnecessary.

Use prepositions and conjunctions like "with", "from", and "to" in the second
and later parameter names only where necessary to clarify the meaning or
behavior of the method.

**Good:**

```objectivec
- (void)addTarget:(id)target action:(SEL)action;                          // no conjunction needed
- (CGPoint)convertPoint:(CGPoint)point fromView:(UIView *)view;           // conjunction clarifies parameter
- (void)replaceCharactersInRange:(NSRange)aRange
            withAttributedString:(NSAttributedString *)attributedString;  
```

A method that returns an object should have a name beginning with a noun
identifying the object returned:

**Good:**

```objectivec
- (Sandwich *)sandwich;      
```
**Not:**

```objectivec
- (Sandwich *)makeSandwich;  
```

An accessor method should be named the same as the object it's getting, but it
should not be prefixed with the word `get`. For example:

**Good:**

```objectivec
- (id)delegate;
```
**Not:**

```objectivec
- (id)getDelegate;
```

Accessors that return the value of boolean adjectives have method names
beginning with `is`, but property names for those methods omit the `is`.

Dot notation is used only with property names, not with method names.

**Good:**

```objectivec
@property(nonatomic, getter=isGlorious) BOOL glorious;

- (BOOL)isGlorious;

BOOL isGood = object.glorious;
BOOL isGood = [object isGlorious];
```
**Not:**

```objectivec
BOOL isGood = object.isGlorious;
```

**Good:**

```objectivec
NSArray<Frog *> *frogs = [NSArray<Frog *> arrayWithObject:frog];
NSEnumerator *enumerator = [frogs reverseObjectEnumerator];
```
**Not:**

```objectivec
NSEnumerator *enumerator = frogs.reverseObjectEnumerator;
```

See [Apple's Guide to Naming
Methods](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingMethods.html#//apple_ref/doc/uid/20001282-BCIGIJJF)
for more details on Objective-C naming.

### Function Names

Because Objective-C does not provide namespacing, non-static functions should
have a prefix that minimizes the chance of a name collision.

```objectivec
extern NSTimeZone *GTMGetDefaultTimeZone();
extern NSString *GTMGetURLScheme(NSURL *URL);
```

### Variable Names 

Variable names typically start with a lowercase and use mixed case to delimit
words.

Instance variables have leading underscores. File scope or global variables have
a prefix `g`. For example: `myLocalVariable`, `_myInstanceVariable`,
`gMyGlobalVariable`.

#### Common Variable Names 

Readers should be able to infer the variable type from the name, but do not use
Hungarian notation for syntactic attributes, such as the static type of a
variable (int or pointer).

File scope or global variables (as opposed to constants) declared outside the
scope of a method or function should be rare, and should have the prefix g.

```objectivec
static int gGlobalCounter;
```

#### Constants 

Constant symbols (const global and static variables and constants created
with #define) should use mixed case to delimit words.

Global and file scope constants should have an appropriate prefix.

```objectivec
extern NSString *const GTLServiceErrorDomain;

typedef NS_ENUM(NSInteger, GTLServiceError) {
    GTLServiceErrorQueryResultMissing = -3000,
    GTLServiceErrorWaitTimedOut       = -3001,
};
```

Because Objective-C does not provide namespacing, constants with external
linkage should have a prefix that minimizes the chance of a name collision,
typically like `ClassNameConstantName` or `ClassNameEnumName`.

## Types and Declarations 

### Local Variables 

Declare variables in the narrowest practical scopes, and close to their use.
Initialize variables in their declarations.

**Good:**

```objectivec
CLLocation *location = [self lastKnownLocation];
for (int meters = 1; meters < 10; meters++) {
    reportFrogsWithinRadius(location, meters);
}
```

Occasionally, efficiency will make it more appropriate to declare a variable
outside the scope of its use. This example declares meters separate from
initialization, and needlessly sends the lastKnownLocation message each time
through the loop:

**Not:**

```objectivec
int meters;                                         
for (meters = 1; meters < 10; meters++) {
    CLLocation *location = [self lastKnownLocation];  
    reportFrogsWithinRadius(location, meters);
}
```

Under Automatic Reference Counting, pointers to Objective-C objects are by
default initialized to `nil`, so explicit initialization to `nil` is not
required.

### Unsigned Integers 

Avoid unsigned integers except when matching types used by system interfaces.

Subtle errors crop up when doing math or counting down to zero using unsigned
integers. Rely only on signed integers in math expressions except when matching
NSUInteger in system interfaces.

**Good:**

```objectivec
NSUInteger numberOfObjects = array.count;
for (NSInteger counter = numberOfObjects - 1; counter > 0; --counter)
```
**Not:**

```objectivec
for (NSUInteger counter = numberOfObjects - 1; counter > 0; --counter)  
```

Unsigned integers may be used for flags and bitmasks, though often NS_OPTIONS or
NS_ENUM will be more appropriate.


## Macros

Avoid macros, especially where `const` variables, enums, XCode snippets, or C
functions may be used instead.

Macros make the code you see different from the code the compiler sees. Modern C
renders traditional uses of macros for constants and utility functions
unnecessary. Macros should only be used when there is no other solution
available.

Where a macro is needed, use a unique name to avoid the risk of a symbol
collision in the compilation unit. If practical, keep the scope limited by
`#undef` the macro after its use.

Macro names should use `SHOUTY_SNAKE_CASE`—all uppercase letters with
underscores between words. Function-like macros may use C function naming
practices. Do not define macros that appear to be C or Objective-C keywords.

**Good:**

```objectivec
#define GTM_EXPERIMENTAL_BUILD ...      // GOOD

// Assert unless X > Y
#define GTM_ASSERT_GT(X, Y) ...         // GOOD, macro style.

// Assert unless X > Y
#define GTMAssertGreaterThan(X, Y) ...  // GOOD, function style.
```

**Not:**

```objectivec
#define kIsExperimentalBuild ...        // AVOID

#define unless(X) if(!(X))              // AVOID
```

Avoid macros that expand to unbalanced C or Objective-C constructs. Avoid macros
that introduce scope, or may obscure the capturing of values in blocks.

Avoid macros that generate class, property, or method definitions in
headers to be used as public API. These only make the code hard to
understand, and the language already has better ways of doing this.

Avoid macros that generate method implementations, or that generate declarations
of variables that are later used outside of the macro. Macros shouldn't make
code hard to understand by hiding where and how a variable is declared.

**Not:**

```objectivec
#define ARRAY_ADDER(CLASS) \
    -(void)add ## CLASS ## :(CLASS *)obj toArray:(NSMutableArray *)array

ARRAY_ADDER(NSString) {
    if (array.count > 5) {              // AVOID -- where is 'array' defined?
        ...
    }
}
```

Examples of acceptable macro use include assertion and debug logging macros
that are conditionally compiled based on build settings—often, these are
not compiled into release builds.

## Cocoa and Objective-C Features 

### Identify Designated Initializer 

Clearly identify your designated initializer.

It is important for those who might be subclassing your class that the
designated initializer be clearly identified. That way, they only need to
override a single initializer (of potentially several) to guarantee the
initializer of their subclass is called. It also helps those debugging your
class in the future understand the flow of initialization code if they need to
step through it. Identify the designated initializer using comments or the
`NS_DESIGNATED_INITIALIZER` macro. If you use `NS_DESIGNATED_INITIALIZER`, mark
unsupported initializers with `NS_UNAVAILABLE`.

### Override Designated Initializer 

When writing a subclass that requires an `init...` method, make sure you
override the designated initializer of the superclass.

If you fail to override the designated initializer of the superclass, your
initializer may not be called in all cases, leading to subtle and very difficult
to find bugs.

### Overridden NSObject Method Placement 

Put overridden methods of NSObject at the top of an `@implementation`.

This commonly applies to (but is not limited to) the `init...`, `copyWithZone:`,
and `dealloc` methods. The `init...` methods should be grouped together,
followed by other typical `NSObject` methods such as `description`, `isEqual:`,
and `hash`.

Convenience class factory methods for creating instances may precede the
`NSObject` methods.

### Initialization 

Don't initialize instance variables to `0` or `nil` in the `init` method; doing
so is redundant.

All instance variables for a newly allocated object are [initialized
to](https://developer.apple.com/library/mac/documentation/General/Conceptual/CocoaEncyclopedia/ObjectAllocation/ObjectAllocation.html)
`0` (except for isa), so don't clutter up the init method by re-initializing
variables to `0` or `nil`.


### Keep the Public API Simple 

Keep your class simple; avoid "kitchen-sink" APIs. If a method doesn't need to
be public, keep it out of the public interface.

Unlike C++, Objective-C doesn't differentiate between public and private
methods; any message may be sent to an object. As a result, avoid placing
methods in the public API unless they are actually expected to be used by a
consumer of the class. This helps reduce the likelihood they'll be called when
you're not expecting it. This includes methods that are being overridden from
the parent class.

Since internal methods are not really private, it's easy to accidentally
override a superclass's "private" method, thus making a very difficult bug to
squash. In general, private methods should have a fairly unique name that will
prevent subclasses from unintentionally overriding them.

### Avoid Messaging the Current Object Within Initializers and `-dealloc`

Code in initializers and `-dealloc` should avoid invoking instance methods.

Superclass initialization completes before subclass initialization. Until all
classes have had a chance to initialize their instance state any method
invocation on self may lead to a subclass operating on uninitialized instance
state.

A similar issue exists for `-dealloc`, where a method invocation may cause a
class to operate on state that has been deallocated.

One case where this is less obvious is property accessors. These can be
overridden just like any other selector. Whenever practical, directly assign to
and release ivars in initializers and `-dealloc`, rather than rely on accessors.

**Good:**

```objectivec
- (instancetype)init {
    self = [super init];
    if (self) {
        _bar = 23;  
    }
    return self;
}
```

Beware of factoring common initialization code into helper methods:

-   Methods can be overridden in subclasses, either deliberately, or
    accidentally due to naming collisions.
-   When editing a helper method, it may not be obvious that the code is being
    run from an initializer.
    
**Not:**

```objectivec
- (instancetype)init {
    self = [super init];
    if (self) {
        self.bar = 23;  
        [self sharedMethod];   Fragile to subclassing or future extension.
    }
    return self;
}
```

**Good:**

```objectivec
- (void)dealloc {
    [_notifier removeObserver:self];  
}
```

**Not:**

```objectivec
- (void)dealloc {
    [self removeNotifications];  
}
```

### Setters copy NSStrings 

Setters taking an `NSString` should always copy the string it accepts. This is
often also appropriate for collections like `NSArray` and `NSDictionary`.

Never just retain the string, as it may be a `NSMutableString`. This avoids the
caller changing it under you without your knowledge.

Code receiving and holding collection objects should also consider that the
passed collection may be mutable, and thus the collection could be more safely
held as a copy or mutable copy of the original.

```objectivec
@property(nonatomic, copy) NSString *name;

- (void)setZigfoos:(NSArray<Zigfoo *> *)zigfoos {
    // Ensure that we're holding an immutable collection.
    _zigfoos = [zigfoos copy];
}
```

### Use Lightweight Generics to Document Contained Types 

All projects compiling on Xcode 7 or newer versions should make use of the
Objective-C lightweight generics notation to type contained objects.

Every `NSArray`, `NSDictionary`, or `NSSet` reference should be declared using
lightweight generics for improved type safety and to explicitly document usage.

```objectivec
@property(nonatomic, copy) NSArray<Location *> *locations;
@property(nonatomic, copy, readonly) NSSet<NSString *> *identifiers;

NSMutableArray<MyLocation *> *mutableLocations = [otherObject.locations mutableCopy];
```

If the fully-annotated types become complex, consider using a typedef to
preserve readability.

```objectivec
typedef NSSet<NSDictionary<NSString *, NSDate *> *> TimeZoneMappingSet;
TimeZoneMappingSet *timeZoneMappings = [TimeZoneMappingSet setWithObjects:...];
```

Use the most descriptive common superclass or protocol available. In the most
generic case when nothing else is known, declare the collection to be explicitly
heterogenous using id.

```objectivec
@property(nonatomic, copy) NSArray<id> *unknowns;
```

### BOOL Pitfalls 

Be careful when converting general integral values to `BOOL`. Avoid comparing
directly with `YES`.

`BOOL` in OS X and in 32-bit iOS builds is defined as a signed `char`, so it may
have values other than `YES` (`1`) and `NO` (`0`). Do not cast or convert
general integral values directly to `BOOL`.

Common mistakes include casting or converting an array's size, a pointer value,
or the result of a bitwise logic operation to a `BOOL` that could, depending on
the value of the last byte of the integer value, still result in a `NO` value.
When converting a general integral value to a `BOOL` use ternary operators to
return a `YES` or `NO` value.

You can safely interchange and convert `BOOL`, `_Bool` and `bool` (see C++ Std
4.7.4, 4.12 and C99 Std 6.3.1.2). Use `BOOL` in Objective-C method signatures.

Using logical operators (`&&`, `||` and `!`) with `BOOL` is also valid and will
return values that can be safely converted to `BOOL` without the need for a
ternary operator.

**Not:**

```objectivec
- (BOOL)isBold {
    return [self fontTraits] & NSFontBoldTrait;  
}
- (BOOL)isValid {
    return [self stringValue];  
}
```

**Good:**

```objectivec
- (BOOL)isBold {
    return ([self fontTraits] & NSFontBoldTrait) ? YES : NO;
}
- (BOOL)isValid {
    return [self stringValue] != nil;
}
- (BOOL)isEnabled {
    return [self isValid] && [self isBold];
}
```

Also, don't directly compare `BOOL` variables directly with `YES`. Not only is
it harder to read for those well-versed in C, but the first point above
demonstrates that return values may not always be what you expect.

**Not:**

```objectivec
BOOL great = [foo isGreat];
if (great == YES) {  
    // ...be great!
}
```

**Good:**

```objectivec
BOOL great = [foo isGreat];
if (great) {         
    // ...be great!
}
```

### Interfaces Without Instance Variables 

Omit the empty set of braces on interfaces that do not declare any instance
variables.

**Good:**

```objectivec
@interface MyClass : NSObject
// Does a lot of stuff.
- (void)fooBarBam;
@end
```

**Not:**

```objectivec
@interface MyClass : NSObject {
}
// Does a lot of stuff.
- (void)fooBarBam;
@end
```

### Case Statements

Braces are not required for case statements, unless enforced by the complier.
When a case contains more than one line, braces should be added.

```objc
switch (condition) {
    case 1:
        // ...
        break;
    case 2: {
        // ...
        // Multi-line example using braces
        break;
    }
    case 3:
        // ...
        break;
    default:
        // ...
        break;
}

```

There are times when the same code can be used for multiple cases, and a fall-through should be used.
A fall-through is the removal of the 'break' statement for a case thus allowing the flow of execution to pass to the next case value.
A fall-through should be commented for coding clarity.

```objc
switch (condition) {
    case 1:
        // ** fall-through! **
    case 2:
        // code executed for values 1 and 2
        break;
    default:
        // ...
        break;
}

```

When using an enumerated type for a switch, 'default' is not needed.   For example:

```objc
RWTLeftMenuTopItemType menuType = RWTLeftMenuTopItemMain;

switch (menuType) {
    case RWTLeftMenuTopItemMain:
        // ...
        break;
    case RWTLeftMenuTopItemShows:
        // ...
        break;
    case RWTLeftMenuTopItemSchedule:
        // ...
        break;
}
```

### Error Handling

When methods return an error parameter by reference, code MUST switch on the returned value and MUST NOT switch on the error variable.

**Good:**

```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
    // Handle Error
}
```

**Not:**

```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
    // Handle Error
}
```

Some of Apple’s APIs write garbage values to the error parameter (if non-NULL) in successful cases,
so switching on the error can cause false negatives (and subsequently crash).

### Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals SHOULD be used whenever creating immutable instances of those objects.
Pay special care that `nil` values not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.

**Good:**

```objc
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```

**Not:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```
### Private Properties

Private properties SHALL be declared in class extensions (anonymous categories) in the implementation file of a class.

```objc
@interface NYTAdvertisement ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```


### Singletons

Singleton objects SHOULD use a thread-safe pattern for creating their shared instance.

```objectivec
+ (instancetype)sharedInstance {
    static id sharedInstance = nil;

    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        sharedInstance = [[[self class] alloc] init];
    });

    return sharedInstance;
}
```

This will prevent [possible and sometimes frequent crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

### Protocols

In a [delegate or data source protocol](https://developer.apple.com/library/ios/documentation/General/Conceptual/CocoaEncyclopedia/DelegatesandDataSources/DelegatesandDataSources.html),
the first parameter to each method SHOULD be the object sending the message.

This helps disambiguate in cases when an object is the delegate for multiple similarly-typed objects,
and it helps clarify intent to readers of a class implementing these delegate methods.

**Good:**

```objc
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath;
```

**Not:**

```objc
- (void)didSelectTableRowAtIndexPath:(NSIndexPath *)indexPath;
```

### Golden Path

When coding with conditionals, the left hand margin of the code should be the "golden" or "happy" path.
That is, don't nest `if` statements.  Multiple return statements are OK.

**Good:**

```objc
- (void)someMethod {
    if (![someOther boolValue]) {
        return;
    }

    //Do something important
}
```

**Not:**

```objc
- (void)someMethod {
    if ([someOther boolValue]) {
        //Do something important
    }
}
```

## Delegate

Delegates, target objects, and block pointers should not be retained when doing
so would create a retain cycle.

To avoid causing a retain cycle, a delegate or target pointer should be released
as soon as it is clear there will no longer be a need to message the object.

If there is no clear time at which the delegate or target pointer is no longer
needed, the pointer should only be retained weakly.

Block pointers cannot be retained weakly. To avoid causing retain cycles in the
client code, block pointers should be used for callbacks only where they can be
explicitly released after they have been called or once they are no longer
needed. Otherwise, callbacks should be done via weak delegate or target
pointers.

## Imports

If there is more than one import statement, statements MUST be grouped [together](http://ashfurrow.com/blog/structuring-modern-objective-c).
Groups MAY be commented.

Note: For modules use the [@import](http://clang.llvm.org/docs/Modules.html#using-modules) syntax.

```objc
// Frameworks
@import QuartzCore;

// Models
#import "NYTUser.h"

// Views
#import "NYTButton.h"
#import "NYTUserView.h"
```

## Xcode project

The physical files SHOULD be kept in sync with the Xcode project files in order to avoid file sprawl.
Any Xcode groups created SHOULD be reflected by folders in the filesystem.
Code SHOULD be grouped not only by type, but also by feature for greater clarity.


# Based on Objective-C Style Guides


* [Google](https://github.com/google/styleguide/blob/gh-pages/objcguide.md)
* [NYTimes](https://github.com/NYTimes/objective-c-style-guide)
* [Raywenderlich](https://github.com/raywenderlich/objective-c-style-guide)
