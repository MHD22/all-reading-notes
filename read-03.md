##### [back-home](https://mhd22.github.io/all-reading-notes/main-table)

# Read: 03 - Maps, primitives, File I/O

#### ______________________________________________________

## Primitives vs Objects

* the primitive type variables live in the stack and hence are accessed fast.
* The reference types are objects, they live on the heap and are relatively slow to access. They have a certain overhead concerning their primitive counterparts.

### Default Values:

* Default values of the primitive types are 0 for numeric types, false for the boolean type, \u0000 for the char type.
* For the wrapper classes, the default value is null.

### Conclusion

* ***the objects in Java are slower and have a bigger memory impact than their primitive analogs.***

#### ___________________________________

## Exceptions:

### What Is an Exception:

***The term exception is shorthand for the phrase "exceptional event."***

'Definition: An exception is an event, which occurs during the execution of a program, that disrupts the normal flow of the program's instructions.'

#### throwing an exception:

It's the event when the error occur during the runtime, then the method which is executing will create an object called exception and send it to the runtime system with the state of the object before an error has occured.


### The Catch or Specify Requirement

* Valid Java programming language code must honor the Catch or Specify Requirement. This means that code that might throw certain exceptions must be enclosed by either of the following:

  * A try statement that catches the exception. The try must provide a handler for the exception.
  * A method that specifies that it can throw the exception. The method must provide a throws clause that lists the exception.

* Code that fails to honor the Catch or Specify Requirement will not compile.

#### ______________________________________________________

## Three kind of Exceptions:

1. *checked exception*: These are exceptional conditions that a well-written application should anticipate and recover from.
 All exceptions are checked exceptions, except for those indicated by Error, RuntimeException, and their subclasses.
2. *the error*: These are exceptional conditions that are external to the application, and that the application usually cannot anticipate or recover from.
3. *the runtime exception*: These are exceptional conditions that are internal to the application, and that the application usually cannot anticipate or recover from.

***Errors and runtime exceptions are collectively known as unchecked exceptions.***

#### ______________________________________________________

### Catching and Handling Exceptions:

**The try Block**:
The first step in constructing an exception handler is to enclose the code that might throw an exception within a try block. In general, a try block looks like the following:

```

try {
    code
}

catch and finally blocks . . .

```

If an exception occurs within the try block, that exception is handled by an exception handler associated with it. To associate an exception handler with a try block, you must put a catch block after it.

**The catch Block**:

You associate exception handlers with a try block by providing one or more catch blocks directly after the try block. No code can be between the end of the try block and the beginning of the first catch block.

```

try {

} catch (ExceptionType name) {

} catch (ExceptionType name) {

}

```

#### Catching More Than One Type of Exception with One Exception Handler

In Java SE 7 and later, a single catch block can handle more than one type of exception. This feature can reduce code duplication and lessen the temptation to catch an overly broad exception.

In the catch clause, specify the types of exceptions that block can handle, and separate each exception type with a vertical bar (|):

```

catch (IOException|SQLException ex) {
    logger.log(ex);
    throw ex;
}

```

**The finally Block**
The finally block always executes when the try block exits. This ensures that the finally block is executed even if an unexpected exception occurs.

***Important***: The finally block is a key tool for preventing resource leaks. When closing a file or otherwise recovering resources, place the code in a finally block to ensure that resource is always recovered.

#### ______________________________________________________

## Scanning:

* Objects of type Scanner are useful for breaking down formatted input into tokens and translating individual tokens according to their data type.

### Breaking Input into Tokens

**Breaking Input into Tokens**:
By default, a scanner uses white space to separate tokens.

```

Scanner s = null;

try {
    s = new Scanner(new BufferedReader(new FileReader("xanadu.txt")));

    while (s.hasNext()) {
        System.out.println(s.next());
    }
} finally {
    if (s != null) {
        s.close();
    }
}

```

***To use a different token separator, invoke `useDelimiter()`, specifying a regular expression.***





#### ______________________________________________________

### This file wrote by [Mohamad Saad Eddin](https://github.com/MHD22).
***you can visit my profile and follow me.❤️😎***
### ______________________________________________


###### Thanks for your time, I hope that you have enjoyed it.
