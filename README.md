### summary of clean code

remember that you are an author, you have readers.

always use clean names that reveal context.
avoid names like list, int, num, i, j ...

---

## Chapter 1 : names

### clear names

avoid magic literal values like 1, 2, arr[2]' ...
instead of

```
if(arr[0] === 4) {
  do something...
}
```

write this:

```
const flagIndex = 0
const SPECIAL = 4

if(flagsList[flagIndex] === SPECIAL) {
  do something...
}
```

even better you can do this:
try to move logic related to a value and the value itself and put them in a class.

```
if(flag.isSpecial()) {
  ...
}
```

### avoid misinformation

- don't use wront container name for collections of data. if your container is not a real List, don't call
  the variable accountsList, use accounts.

- avoid incostistant names. avoid similar names that could confuse the users. avoid letters that look like number l,1 o o

### avoid noninformation

in the example below:

```
public static void copyChars(char a1[], char a2[]) {
  for (int i = 0; i < a1.length; i++) {
    a2[i] = a1[i];
  }
}
```

it's much better to use source instead of a1 and destination instead of a2. it makes the intent much clearer.

### distinct names

make sure the names you use in a scope are clearly distinguishable.
for example customer, customerInfo and customerAccount are not clearly distinguishable.

These methods are not clear enough:

```
getActiveAccount();
getActiveAccounts();
getActiveAccountInfo();
```

### pronounceable names

make sure all names are pronounceable! avoid names like: DtaRcrd102!.

### searcable names

#### The length of a name should correspond to the size of its scope

compare

```
for (int j=0; j<34; j++) {
  s += (t[j]_4)/5;
}
```

to

```
int realDaysPerIdealDay = 4;
const int WORK_DAYS_PER_WEEK = 5;
int sum = 0;

for (int j=0; j < NUMBER_OF_TASKS; j++) {
  int realTaskDays = taskEstimate[j] - realDaysPerIdealDay;
  int realTaskWeeks = (realdays / WORK_DAYS_PER_WEEK);
  sum += realTaskWeeks;
}
```

### class names

classes and objects should have a noun pharase as a name. exp: Customer, Account.
never use a verb as a class name.

### method names

method names should be verbs, exp: postPayment, deletePage, or save.

- getter should be prefixed with `get`: `getName`
- setters should be prefixed with `set`: `setName`
- predicates should be prefixed with `is`: `isPosted`

instead of one overloaded constructor use statuc factory methods and make the constructor private.

### Pick One Word per Concept

use only one pharase for the same action across the codebase.
it’s confusing to have fetch, retrieve, and get as equivalent methods of different classes.
Likewise, it’s confusing to have a controller and a manager and a driver in the same
code base.

- What is the essential difference between a DeviceManager and a ProtocolController?
- Why are both not controllers or both not managers? Are they both Drivers really?
- The name leads you to expect two objects that have very different type as well as having different classes.

pay attention to semantics of a method. add could mean arithamtics or concat string or appen to list. based on the
semantics of the operation in your mind, change the name to append or concat or push ...

Our goal, as authors, is to make our code as easy as possible to understand. We want
our code to be a quick skim, not an intense study.

### problem domain vs solution domain

Separating solution and problem domain concepts is part of the job of a good programmer and designer. The code that has more to do with problem domain concepts
should have names drawn from the problem domain.

---

## Chapter 2 : functions

The first rule of functions is that they should be small. The second rule of functions is that
they should be smaller than that.

try to keep your functions shorter than 8~10 lines of code.

The first rule of functions is that they should be small. The second rule of functions is that
they should be smaller than that.

The indent level of a function should not be greater than one or two.

FUNCTIONS SHOULD DO ONE THING. THEY SHOULD DO IT WELL.
THEY SHOULD DO IT ONLY.

So, another way to know that a function is doing more than “one thing” is if you can
extract another function from it with a name that is not merely a restatement of its implementation

Operations in a function should all be of the same level of abstraction. lower level stuff should be refactored into their own set of functions.

Mixing levels of abstraction within a function is always confusing.

Functions that do one thing cannot be reasonably divided into
sections.

Don’t be afraid to spend time choosing a name. Indeed, you should try several different names and read the code with each in place.

The ideal number of arguments for a function is
zero (niladic). Next comes one (monadic), followed
closely by two (dyadic). Three arguments (triadic)
should be avoided where possible. More than three
(polyadic) requires very special justification—and
then shouldn’t be used anyway.

functions that accept a boolean flag should be converted to two functions with proper names
instead.

instead of

```
function doSomthing(flag) {
  if(flag) {...}
}
```

use

```
function doSomthingThisWay() {}
function doSomthingThatWay() {}
```

When a function seems to need more than two or three arguments, it is likely that some of
those arguments ought to be wrapped into a class of their own.

the function and argument should form a very nice verb/noun pair. For example,
write(name) is very evocative. Whatever this “name” thing is, it is being “written.”

avoid side-effects. they creates a temporal coupling. That is, the function can only be
called at certain times

avoid receiving an argument and mutating it and basically using it as an output rather than
an input

---

_command-query-separation_:
**Functions should either do something(mutation) or answer a question(drived state), but not both.**
for example a setter that returns true or false as the result of the operation, can be confusing
in some scenarios. like:

```
if(set('username', 'hamid')) {
 ...
}
```

does this mean the user is trying to set the username or is he/she asking is the username is set to hamid? it's ambigious. a better approach is to not return a boolean from the setter and:

```
if(attributeExists('username')) {
  setAttribute("username", "unclebob");
}
```

---

throwing errors has an advantage over returning them, and that is the caller can separate happy path from un-happy path by the use of try-catch blocks and not include error handing logic, right after function invocation. returning errors could violate command-query-separation too.

I's a good idea to extact the login inside try or catch block into their own functions to improve readability

**Functions should do one thing. Error handing is one thing. Thus, a function that handles errors should do nothing else.**

Functions should do one thing. Error handing is one thing. Thus, a function that handles
errors should do nothing else.

---

### Comments

comments are a necessary evil. Comments are always failures. We must
have them because we cannot always figure out how to express ourselves without them.

when you find yourself in a position where you need to write a comment, think it
through and see whether there isn’t some way to turn the tables and express yourself in
code.

Clear and expressive code with few comments is far superior to cluttered and complex
code with lots of comments. Rather than spend your time writing the comments that
explain the mess you’ve made, spend it cleaning that mess.

try to replace comments with code. make code more explanatory.

write comments only about the near scope. don't write anything about higher or lower scopes cause they might change any time.

comment must make the code obviouse. consider this example:

```
/*
* start with an array that is big enough to hold all the pixels
* (plus filter bytes), and an extra 200 bytes for header info
*/
this.pngBytes = new byte[((this.width + 1) * this.height * 3) + 200];
```

What is a filter byte? Does it relate to the +1? Or to the \*3? Both? Is a pixel a byte? Why
200?

---

### formatting

different parts of code should be separated by blank lines

Concepts that are closely related should be kept vertically close to each other

Variables should be declared as close to their usage as possible. Because our functions are very short, local variables should appear a the top of each
function

If one function calls another, they should be vertically close,
and the caller should be above the callee, if at all possible

These functions have a strong conceptual affinity because they share a common naming
scheme and perform variations of the same basic task. The fact that they call each other is
secondary. Even if they didn’t, they would still want to be close together. perhaps under a class.

As in newspaper articles, we expect the most important concepts to come first, and
we expect them to be expressed with the least amount of polluting detail. We expect the
low-level details to come last.

---

### Objects and Data structures

Hiding implementation is not just a matter of putting a layer of functions between
the variables. Hiding implementation is about abstractions! A class does not simply
push its variables out through getters and setters. Rather it exposes abstract interfaces
that allow its users to manipulate the essence of the data, without having to know its
implementation.

In both of the above cases the second option is preferable. We do not want to expose
the details of our data. Rather we want to express our data in abstract terms. This is not
merely accomplished by using interfaces and/or getters and setters. Serious thought needs
to be put into the best way to represent the data that an object contains. The worst option is
to blithely add getters and setters.

compare:

```
public interface Vehicle {
  double getFuelTankCapacityInGallons();
  double getGallonsOfGasoline();
}
```

to

```
public interface Vehicle {
  double getPercentFuelRemaining();
}
```

the first one leaks implementaion details

---

Objects could serve two purpose, first: be a data type, a wrapper around complex data types.
There is a distinction between DataTypes objects and objects that contain methods and logic.

OOP in general tries to put logic where the data lives. under the same class/object. in procedural or functional style of programming the data usally is dumb and you have functions/methods etc, that
performs operations on the data.

with the first approach(data and logic together), you can add new datatypes without changing the functions.

with the second approach(dumb data structures), you can add new functions types without chaning data types.

So, the things that are hard for OO are easy for procedures, and the things that are
hard for procedures are easy for OO!

Mature programmers know that the idea that everything is an object is a myth. Sometimes you really do want simple data structures with procedures operating on them.

---

#### The law Demeter

Inner data or state of an object should not be exposed if the object is not a data structure, and contains logic.

Objects that contain behavoiur should hide their data. getters and setters are just syntax sugar.
hiding means not exposing the data at all, just exposing methods for behaviour.
the user should not know anything about what is inside this object.
we should not be able to navigate through them.
How then would we get access to inner data?
we don't! objects should only contain behaviour not accessable data. they can have their own
internal state which is not exposeable. they should do things.
**private variables and public functions**

objects that are used as data structure, on the other hand, should expose their inner value easily.
the user must know exactly what is inside this object
these are what we call data transfer objects.
active record is a type of DTO but they typically have navigational methods like save and find.
**public variables and no functions**

avoid a hybrid approach.

#### conclusion

Objects expose behavior and hide data
Data structures expose data and have no significant behavior

---

### Error handing

returning errors instaed of throwing them in not a good idea, because the caller needs to handle the error exactly when it calls the functions which clutter the code.

by throwing errors you allow the caller to separate the concern of handling errors from the actual logic.

#### try-catch-finally

try is like a transaction.

Create informative error messages and pass them along with your exceptions. Mention the operation that failed and the type of failure. If you are logging in your application,
pass along enough information to be able to log the error in your catch.

don't return null in exceptions.
If you are tempted to return null from a method, consider throwing an exception or returning a SPECIAL CASE object instead. If you are calling a null-returning method from a third-party API, consider wrapping that method with a method that either throws an exception or returns a special case object.

In many cases, special case objects are an easy remedy

instead of returning plain null, return null object. object that contains neutral data. for example an empty list, or 0, or thing like this.

Returning null from methods is bad, but passing null into methods is worse. Unless you
are working with an API which expects you to pass null, you should avoid passing null in
your code whenever possible.

---

### Boundries

There are boundries in our code. libraries, yet-to-be-written parts of code base, ...

when a part of the app is not ready, we can define a interface and develope our side of things based on that interface and once the actual api is ready, write an adapter for it.

Passing generic data types like arrays/lists/maps etc might cause some problems. any user can
mutate them. if you are using OOP style your data types are not immutable, you might end up with some really wierd behaviours. The solution to this is to wrap the generic data type and only provide a limited api. This also imporves encapsulation.

contact points to boundries should be kept minimal. we may wrap them or use an adapter to communicate with them. this promotes loose coupling in the system.

---

### Unit tests

tests must be kept as clean as the rest of the program, simply because the must change whenever a part of our code changes.
It is unit tests

that keep our code flexible, maintainable, and reusable

If you have tests, you do not fear making changes to the code! Without tests every change is a possible bug.

No matter how flexible your architecture is, no matter how nicely partitioned your
design, without tests you will be reluctant to make changes because of the fear that you
will introduce undetected bugs.

we want to test a single concept in each test function. We don’t
want long test functions that go testing one miscellaneous thing after another.

#### F.I.R.S.T.

#### Clean

tests follow five other rules that form the above acronym:
Fast Tests should be fast. They should run quickly. When tests run slow, you won’t want
to run them frequently. If you don’t run them frequently, you won’t find problems early
enough to fix them easily. You won’t feel as free to clean up the code. Eventually the code
will begin to rot.

####Independent
Tests should not depend on each other. One test should not set up the conditions for the next test. You should be able to run each test independently and run the tests in
any order you like. When tests depend on each other, then the first one to fail causes a cascade of downstream failures, making diagnosis difficult and hiding downstream defects.

#### Repeatable

Tests should be repeatable in any environment. You should be able to run the
tests in the production environment, in the QA environment, and on your laptop while
riding home on the train without a network. If your tests aren’t repeatable in any environment, then you’ll always have an excuse for why they fail. You’ll also find yourself unable
to run the tests when the environment isn’t available.

#### Self-Validating

The tests should have a boolean output. Either they pass or fail. You
should not have to read through a log file to tell whether the tests pass. You should not have
to manually compare two different text files to see whether the tests pass. If the tests aren’t
self-validating, then failure can become subjective and running the tests can require a long
manual evaluation.

#### Timely

The tests need to be written in a timely fashion. Unit tests should be written just
before the production code that makes them pass. If you write tests after the production
code, then you may find the production code to be hard to test. You may decide that some
production code is too hard to test. You may not design the production code to be testable.

conclusion
If you let the tests rot, then your code will rot too. Keep your tests clean.

---

### Classes

Classes should not have minimal public method and no public variables.

classes should be as small as possible

If we cannot derive a concise name for a class, then it’s likely too large. The more ambiguous the class name, the more
likely it has too many responsibilities.

For example, class names including weasel words like Processor or Manager or Super often hint at unfortunate aggregation of responsibilities.

We should also be able to write a brief description of the class in about 25 words,
without using the words “if,” “and,” “or,” or “but.”

The Single Responsibility Principle states that a class or module should have one,and only one, reason to change

To restate the former points for emphasis: We want our systems to be composed of many small classes, not a few large ones. Each small class encapsulates a single responsibility, has a single reason to change, and collaborates with a few others to achieve the desired system behaviors.

A class in which each variable is used by each method is maximally cohesive

Consider a large function with many variables declared within it. Let’s say you
want to extract one small part of that function into a separate function. However, the code
you want to extract uses four of the variables declared in the function. Must you pass all
four of those variables into the new function as arguments?
Not at all! If we promoted those four variables to instance variables of the class, then
we could extract the code without passing any variables at all. It would be easy to break
the function up into small pieces.
Unfortunately, this also means that our classes lose cohesion because they accumulate
more and more instance variables that exist solely to allow a few functions to share them.
But wait! If there are a few functions that want to share certain variables, doesn’t that
make them a class in their own right? Of course it does. When classes lose cohesion, split
them!

interfaces contain the knowledge that a class must have in order to work on another class, without coupling or knowing the interals on that class.

In essence, the dependency injection says that our classes should depend upon abstractions, not on concrete details (interfaces)

---

### Systems

A program is made up of different systems, just like a city is. we have power distribution systems, transportation systems, etc...

Construction of an object is a separate concern from consuming it. imagine a hotel, when it's being built, it looks a lot different and people working there are dress very differently from when the hotel is finished.

One way of doing this is to construct all dependencies in a Main() function.

sometimes we want to create objects on demand, for this situations we use abstract factories to keep the details of construction away from the main program.

#### Dependency injection

it's powerful way of separating creation from consumption of objects. It's a way to implement inversion on control in objects. it moves secondary responsibilities to other objects that are specialized for that action.

In the context of dependency management, an object should not take responsibility for instantiating dependencies itself. Instead, it should pass this responsibility to another “authoritative”mechanism, thereby inverting the control. Because setup is a global concern, this authoritative
mechanism will usually be either the “main” routine or a special-purpose container.

Systems need domain specific language

---

### Simple design

there are a few rules that lead to a simple design

• Runs all the tests
• Contains no duplication
• Expresses the intent of the programmer
• Minimizes the number of classes and methods

#### Runs all the tests

The more tests we write, the more we’ll continue to push toward things that are simpler to test.
So making sure our system is fully testable helps us create better designs.

Tight coupling makes it difficult to write tests. So, similarly, the more tests we write,
the more we use principles like DIP and tools like dependency injection, interfaces, and
abstraction to minimize coupling. Our designs improve even more.

following a simple and obvious rule that says we need to have tests and run them continuously impacts our system’s adherence to the primary OO goals of low coupling and high cohesion. Writing tests leads to better designs.

#### Refactoring

For each few lines of code we add, we pause and reflect on the new design. Did we just degrade it? If so, we clean it up and run our tests to demonstrate that we haven’t broken anything. The fact that we have these tests eliminates the fear that cleaning up the code will break it!

During this refactoring step, we can apply anything from the entire body of knowledge about good software design. We can increase cohesion, decrease coupling, separate concerns, modularize system concerns, shrink our functions and classes, choose better names, and so on. This is also where we apply the final three rules of simple design: Eliminate duplication, ensure expressiveness, and minimize the number of classes and methods

Duplication is the primary enemy of a well-designed system.

No code should be duplicated across 2 or more methods, it should be extracted to it's own method.

Code shared between methods that are mostly the same with little difference can be refactored into a super-class and we can extend that class with some minute changes in behavior.

All too often we get our code working and then move on to the next problem without giving sufficient thought to making that code easy for the next person to read. Remember, the most likely next person to read the code will be you. So take a little pride in your workmanship. Spend a little time with each of your functions and classes. Choose better names, split large functions into smaller functions, and generally just take care of what you’ve created. Care is a precious resource.

---

### Code Smells

#### Comments

- Inappropriate Information: information better held in a different kind of system such as your source code control system
- Obsolete Comments: old comments
- Redundant Comment: `i++ // increment`
- Poorly Written Comment: Don’t ramble. Don’t state the obvious. Be brief.
- Commented-Out Code: no one will delete it because everyone assumes someone else needs it or has plans for it. delete it!

#### Environment

- Building The Project Requires More Than One Step
- Tests Require More Than One Step: Tests Require More Than One Step
- Tests Require More Than One Step: Functions should have a small number of arguments. No argument is best, followed by one, two, and three. More than three is very questionable
- Flag Arguments: Boolean arguments loudly declare that the function does more than one thing. They are confusing and should be eliminated.
- Dead Function

### General

- Multiple Languages in One Source File
- Obvious Behavior Is Unimplemented: all expected behaviors of a function should always be implemented
- Duplication: Every time you see duplication in the code, it represents a missed opportunity for abstraction. A more subtle form is the switch/case or if/else chain that appears again and again in various modules, always testing for the same set of conditions. These should be replaced with polymorphism.
- Code at Wrong Level of Abstraction: For example, constants, variables, or utility functions that pertain only to the detailed implementation should not be present in the base class. The base class should know nothing about them.
- Base Classes Depending on Their Derivatives
- Too Much Information: Hide your data. Hide your utility functions. Hide your constants and your temporaries. Don’t create classes with lots of methods or lots of instance variables. Don’t create lots of protected variables and functions for your subclasses. Concentrate on keeping interfaces very tight and very small. Help keep coupling low by limiting information
- Vertical Separation: Variables and function should be defined close to where they are used. Local variables should be declared just above their first usage and should have a small vertical scope. We don’t want local variables declared hundreds of lines distant from their usages
- Inconsistency: If within a particular function you use a variable named response to hold an HttpServletResponse, then use the same variable name consistently in the other functions that use HttpServletResponse objects. If you name a method processVerificationRequest, then use a similar name, such as processDeletionRequest, for the methods that process other kinds of requests.
- Misplaced Responsibility: Where should some value live? The principle of least surprise comes into play here. Code should be placed where a reader would naturally expect it to be.
- Static methods: when use a static method ? when the method is never expected to be polymorphic. when the behavior is fixed for all the instances and derivitive classes, then the method can be static. In general you should prefer nonstatic methods to static methods. When in doubt, make the function nonstatic. If you really want a function to be static, make sure that there is no chance that you’ll want it to behave polymorphically.
- Use Explanatory Variables: when doing a calculation, use intermediary variables to clarify your intent
- Function Names Should Say What They Do: `Date newDate = date.add(5);`. what does this line of code do? add 5 days, hours, weeks? does it mutate the date or does it return a new one?
- Prefer Polymorphism to If/Else or Switch/Case
- Replace Magic Numbers with Named Constants
- Structure over Convention: try to enforce structure rather than just defining some conventions.
- Avoid Negative Conditionals: `if (!buffer.shouldNotCompact())`. WTF ?
- Functions should do only one thing: break functions into separate functions
- Hidden Temporal Couplings: enforce order of calling functions or any other temporal coupling obvious by creating intermediary variables.
- Functions should have one level of abstraction, at the level of it's name: in a function all operations must be of the same level of abstraction. the current function should not know anything about the internals of the other functions it is calling including how it works, what data structure it uses, how it calculates the result, etc.
- Keep Configurable Data at High Levels
- Avoid Transitive Navigation: we don’t want a.getB().getC().doSomething(), making sure that modules know only about their immediate collaborators and do not know the navigation map of the whole system

- Names must be at the appropriate level of abstraction:

```
public interface Modem {
  boolean dial(String phoneNumber);
  boolean disconnect();
  boolean send(char c);
  char recv();
  String getConnectedPhoneNumber();
}
```

this might seem ok, but "dial" is too low level, what if the modem is always connected. the better approach is to convert "dial" to "connect"

- Use long names for long scopes: The length of a name should be related to the length of the scope. You can use very short variable names for tiny scopes, but for big scopes you should use longer names.

- names should show side-effects: Don’t hide side effects with a name. Don’t use a simple verb to describe a function that does more than just that simple action.

```
public ObjectOutputStream getOos() throws IOException {
  if (m_oos == null) {
    m_oos = new ObjectOutputStream(m_socket.getOutputStream());
  }

  return m_oos;
}
```

This function does a bit more than get an “oos”; it creates the “oos” if it hasn’t been created already. Thus, a better name might be createOrReturnOos
