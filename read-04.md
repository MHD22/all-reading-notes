##### [back-home](https://mhd22.github.io/all-reading-notes/main-table)

# Read: 04 - OOP

#### ______________________________________________________

## What Is an Object?

* Software objects are conceptually similar to real-world objects: they too consist of state and related behavior.
* An object stores its state in fields (variables in some programming languages) and exposes its behavior through methods (functions in some programming languages). 
* Methods operate on an object's internal state and serve as the primary mechanism for object-to-object communication. 
* Hiding internal state and requiring all interaction to be performed through an object's methods is known as data `encapsulation` — a fundamental principle of object-oriented programming.

***Bundling code into individual software objects provides a number of benefits, including:***

* Modularity: The source code for an object can be written and maintained independently of the source code for other objects. Once created, an object can be easily passed around inside the system.
* Information-hiding: By interacting only with an object's methods, the details of its internal implementation remain hidden from the outside world.
* Code re-use: If an object already exists (perhaps written by another software developer), you can use that object in your program. This allows specialists to implement/test/debug complex, task-specific objects, which you can then trust to run in your own code.
* Pluggability and debugging ease: If a particular object turns out to be problematic, you can simply remove it from your application and plug in a different object as its replacement. This is analogous to fixing mechanical problems in the real world. If a bolt breaks, you replace it, not the entire machine.

#### ______________________________________________________

## What Is a Class?

* In the real world, you'll often find many individual objects all of the same kind. 
* There may be thousands of other bicycles in existence, all of the same make and model. 
* Each bicycle was built from the same set of blueprints and therefore contains the same components. 
* In object-oriented terms, we say that your bicycle is an instance of the class of objects known as bicycles. 
* A class is the blueprint from which individual objects are created.

#### ______________________________________________________

## Classes:

### Declaring Classes:

* You've seen classes defined in the following way:

```

class MyClass {
    // field, constructor, and 
    // method declarations
}

```

* The preceding class declaration is a minimal one. It contains only those components of a class declaration that are required. 
* You can provide more information about the class, such as the name of its superclass, whether it implements any interfaces, and so on, at the start of the class declaration. For example,

```

class MyClass extends MySuperClass implements YourInterface {
    // field, constructor, and
    // method declarations
}
```
***means that MyClass is a subclass of MySuperClass and that it implements the YourInterface interface.***

***In general, class declarations can include these components, in order:***

1. Modifiers such as public, private, and a number of others that you will encounter later.
2. The class name, with the initial letter capitalized by convention.
3. The name of the class's parent (superclass), if any, preceded by the keyword extends. A class can only extend (subclass) one parent.
4. A comma-separated list of interfaces implemented by the class, if any, preceded by the keyword implements. A class can implement more than one interface.
5. The class body, surrounded by braces, {}.

#### ______________________________________________________

### Declaring Member Variables:

**There are several kinds of variables:**

* Member variables in a class—these are called fields.
* Variables in a method or block of code—these are called local variables.
* Variables in method declarations—these are called parameters.

#### ______________________________________________________

### Access Modifiers:

**The first (left-most) modifier used lets you control what other classes have access to a member field. For the moment, consider only public and private. Other access modifiers will be discussed later.**

* public modifier—the field is accessible from all classes.
* private modifier—the field is accessible only within its own class.

#### ______________________________________________________

### Defining Methods:

**Here is an example of a typical method declaration:**

```

public double calculateAnswer(double wingSpan, int numberOfEngines,
                              double length, double grossTons) {
    //do the calculation here
}
```

* The only required elements of a method declaration are the method's return type, name, a pair of parentheses, (), and a body between braces, {}.

**More generally, method declarations have six components, in order:**

1. Modifiers—such as public, private, and others you will learn about later.
2. The return type—the data type of the value returned by the method, or void if the method does not return a value.
3. The method name—the rules for field names apply to method names as well, but the convention is a little different.
4. The parameter list in parenthesis—a comma-delimited list of input parameters, preceded by their data types, enclosed by parentheses,(),If there are no parameters, you must use empty parentheses.
5. An exception list—to be discussed later.
6. The method body, enclosed between braces—the method's code, including the declaration of local variables, goes here.

#### ______________________________________________________

### Overloading Methods

* The Java programming language supports overloading methods, and Java can distinguish between methods with different method signatures
* This means that methods within a class can have the same name if they have different parameter lists.

***Note: Overloaded methods should be used sparingly, as they can make code much less readable.***

#### ______________________________________________________

### Providing Constructors for Your Classes

* A class contains constructors that are invoked to create objects from the class blueprint. Constructor declarations look like method declarations—except that they use the name of the class and have no return type. For example, Bicycle has one constructor:

```

public Bicycle(int startCadence, int startSpeed, int startGear) {
    gear = startGear;
    cadence = startCadence;
    speed = startSpeed;
}
```

#### ______________________________________________________

### Arbitrary Number of Arguments
* You can use a construct called varargs to pass an arbitrary number of values to a method. 
* You use varargs when you don't know how many of a particular type of argument will be passed to the method. 
* It's a shortcut to creating an array manually (the previous method could have used varargs rather than an array).


* To use varargs, you follow the type of the last parameter by an ellipsis (three dots, ...), then a space, and the parameter name. The method can then be called with any number of that parameter, including none.

```

public Polygon polygonFrom(Point... corners) {
    int numberOfSides = corners.length;
    double squareOfSide1, lengthOfSide1;
    squareOfSide1 = (corners[1].x - corners[0].x)
                     * (corners[1].x - corners[0].x) 
                     + (corners[1].y - corners[0].y)
                     * (corners[1].y - corners[0].y);
    lengthOfSide1 = Math.sqrt(squareOfSide1);

    // more method body code follows that creates and returns a 
    // polygon connecting the Points
}
```

* You can see that, inside the method, corners is treated like an `array`. *The method can be called either with an array or with a sequence of arguments*. The code in the method body will treat the parameter as an array in either case.

* You will most commonly see `varargs` with the printing methods; for example, this printf method:

```

public PrintStream printf(String format, Object... args)
allows you to print an arbitrary number of objects. It can be called like this:

System.out.printf("%s: %d, %s%n", name, idnum, address);
or like this

System.out.printf("%s: %d, %s, %s, %s%n", name, idnum, address, phone, email);
or with yet a different number of arguments.

```

#### ______________________________________________________

## Objects

* A typical Java program creates many objects, which as you know, interact by invoking methods.
* Through these object interactions, a program can carry out various tasks, such as implementing a GUI, running an animation, or sending and receiving information over a network.
* Once an object has completed the work for which it was created, its resources are recycled for use by other objects.

### Creating Objects

* As you know, a class provides the blueprint for objects; you create an object from a class. 
* Each of the following statements taken from the CreateObjectDemo program creates an object and assigns it to a variable:

```

Point originOne = new Point(23, 94);
Rectangle rectOne = new Rectangle(originOne, 100, 200);
Rectangle rectTwo = new Rectangle(50, 100);
```

* The first line creates an object of the Point class, and the second and third lines each create an object of the Rectangle class.

* Each of these statements has three parts (discussed in detail below):

**Declaration**: The code set in bold are all variable declarations that associate a variable name with an object type.
**Instantiation**: The `new` keyword is a Java operator that creates the object.
**Initialization**: The `new` operator is followed by a call to a constructor, which initializes the new object.

### The Garbage Collector

* Some object-oriented languages require that you keep track of all the objects you create and that you explicitly destroy them when they are no longer needed. Managing memory explicitly is tedious and error-prone. 
* The Java platform allows you to create as many objects as you want (limited, of course, by what your system can handle), and you don't have to worry about destroying them. 
* The Java runtime environment deletes objects when it determines that they are no longer being used. This process is called garbage collection.

* An object is eligible for garbage collection when there are no more references to that object. 
* References that are held in a variable are usually dropped when the variable goes out of scope. 
Or, you can explicitly drop an object reference by setting the variable to the special value null.
* Remember that a program can have multiple references to the same object; all references to an object must be dropped before the object is eligible for garbage collection.

***The Java runtime environment has a garbage collector that periodically frees the memory used by objects that are no longer referenced. The garbage collector does its job automatically when it determines that the time is right.***


### Returning a Class or Interface

* If this section confuses you, skip it and return to it after you have finished the lesson on interfaces and inheritance.

* When a method uses a class name as its return type, such as whosFastest does, the class of the type of the returned object must be either a subclass of, or the exact class of, the return type. 

***`Note`: You also can use interface names as return types. In this case, the object returned must implement the specified interface.***


### Using the this Keyword:

* Within an instance method or a constructor, this is a reference to the current object — the object whose method or constructor is being called. 
* You can refer to any member of the current object from within an instance method or a constructor by using this.


#### ______________________________________________________

### This file wrote by [Mohamad Saad Eddin](https://github.com/MHD22).
***you can visit my profile and follow me.❤️😎***
### ______________________________________________


###### Thanks for your time, I hope that you have enjoyed it.
