##### [back-home](https://mhd22.github.io/all-reading-notes/main-table)

# Read: 06 - Inheritance and Interfaces

#### ______________________________________________________

# What Is Inheritance?:

* Object-oriented programming allows classes to inherit commonly used state and behavior from other classes.
* In the Java programming language, each class is allowed to have one direct superclass, and each superclass has the potential for an unlimited number of subclasses.

## What You Can Do in a Subclass

* A subclass inherits all of the public and protected members of its parent, no matter what package the subclass is in. If the subclass is in the same package as its parent, it also inherits the package-private members of the parent. You can use the inherited members as is, replace them, hide them, or supplement them with new members:

- The inherited fields can be used directly, just like any other fields.
- You can declare a field in the subclass with the same name as the one in the superclass, thus hiding it (not recommended).
- You can declare new fields in the subclass that are not in the superclass.
- The inherited methods can be used directly as they are.
- You can write a new instance method in the subclass that has the same signature as the one in the superclass, thus overriding it.
- You can write a new static method in the subclass that has the same signature as the one in the superclass, thus hiding it.
- You can declare new methods in the subclass that are not in the superclass.
- You can write a subclass constructor that invokes the constructor of the superclass, either implicitly or by using the keyword super.

## Private Members in a Superclass

* A subclass does not inherit the private members of its parent class. However, if the superclass has public or protected methods for accessing its private fields, these can also be used by the subclass.

* A nested class has access to all the private members of its enclosing class—both fields and methods. Therefore, a public or protected nested class inherited by a subclass has indirect access to all of the private members of the superclass.

## Modifiers

* The access specifier for an overriding method can allow more, but not less, access than the overridden method. 
* For example, a protected instance method in the superclass can be made public, but not private, in the subclass.

- You will get a compile-time error if you attempt to change an instance method in the superclass to a static method in the subclass, and vice versa.

## Summary

**The following table summarizes what happens when you define a method with the same signature as a method in a superclass.**

*Defining a Method with the Same Signature as a Superclass's Method*

|               |      Superclass Instance Method | Superclass Static Method |
| ------------- |   ----------------------------- | -------------------------- |
| Subclass Instance Method | Overrides | Generates a compile-time error |
| Subclass Static Method | Generates a compile-time error | Hides |


#### ______________________________________________________

# What Is a Package?:

* A package is a namespace that organizes a set of related classes and interfaces. Conceptually you can think of packages as being similar to different folders on your computer. 

* The Java platform provides an enormous class library (a set of packages) suitable for use in your own applications.
* This library is known as the "Application Programming Interface", or `"API"` for short. 
* Its packages represent the tasks most commonly associated with general-purpose programming. 


#### ______________________________________________________


# Interfaces: 

## What Is an Interface?:

* an interface is a group of related methods with empty bodies. A bicycle's behavior, if specified as an interface, might appear as follows:

```
interface Bicycle {

    //  wheel revolutions per minute
    void changeCadence(int newValue);

    void changeGear(int newValue);

    void speedUp(int increment);

    void applyBrakes(int decrement);
}
```

* To implement this interface, the name of your class would change (to a particular brand of bicycle, for example, such as ACMEBicycle), and you'd use the `implements` keyword in the class declaration.

* Implementing an interface allows a class to become more formal about the behavior it promises to provide. 
* Interfaces form a contract between the class and the outside world, and this contract is enforced at build time by the compiler. 
* If your class claims to implement an interface, all methods defined by that interface must appear in its source code before the class will successfully compile.

## Interfaces in Java:

* In the Java programming language, an interface is a reference type, similar to a class, that can contain only constants, method signatures, default methods, static methods, and nested types. 
* Method bodies exist only for default methods and static methods. 
* Interfaces cannot be instantiated—they can only be implemented by classes or extended by other interfaces.

* An interface can extend other interfaces, just as a class subclass or extend another class. However, whereas a class can extend only one other class, an interface can extend any number of interfaces. 
* The interface declaration includes a comma-separated list of all the interfaces that it extends.

## Using an Interface as a Type:

* If you define a reference variable whose type is an interface, any object you assign to it must be an instance of a class that implements the interface.


## Summary of Interfaces:

* An interface declaration can contain method signatures, default methods, static methods and constant definitions. The only methods that have implementations are default and static methods.

* A class that implements an interface must implement all the methods declared in the interface.

* An interface name can be used anywhere a type can be used.

* ***Note: Static methods in interfaces are never inherited.***


#### ______________________________________________________

## Polymorphism:

* Subclasses of a class can define their own unique behaviors and yet share some of the same functionality of the parent class.

## Object as a Superclass:

* The Object class, in the java.lang package, sits at the top of the class hierarchy tree. 
* Every class is a descendant, direct or indirect, of the Object class. 
* Every class you use or write inherits the instance methods of Object. 
* You need not use any of these methods, but, if you choose to do so, you may need to override them with code that is specific to your class. The methods inherited from Object that are discussed in this section are:

* `protected Object clone() throws CloneNotSupportedException` => (***implements the Cloneable interface***)
  * Creates and returns a copy of this object.
* `public boolean equals(Object obj)` => (***Note: If you override equals(), you must override hashCode() as well.***)
  * Indicates whether some other object is "equal to" this one.
* `protected void finalize() throws Throwable`
  * Called by the garbage collector on an object when garbage collection determines that there are no more references to the object
* `public final Class getClass()`  => (***You cannot override getClass.***)
  * Returns the runtime class of an object.
* `public int hashCode()`
  * Returns a hash code value for the object.
* `public String toString()`
  * Returns a string representation of the object.


## Abstract Classes Compared to Interfaces

* Abstract classes are similar to interfaces. You cannot instantiate them, and they may contain a mix of methods declared with or without an implementation. 
* However, with abstract classes, you can declare fields that are not static and final, and define public, protected, and private concrete methods.
* With interfaces, all fields are automatically public, static, and final, and all methods that you declare or define (as default methods) are public. 
* In addition, you can extend only one class, whether or not it is abstract, whereas you can implement any number of interfaces.




#### ______________________________________________________

### This file wrote by [Mohamad Saad Eddin](https://github.com/MHD22).
***you can visit my profile and follow me.❤️😎***
### ______________________________________________


###### Thanks for your time, I hope that you have enjoyed it.