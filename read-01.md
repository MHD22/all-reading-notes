
##### [back-home](https://mhd22.github.io/all-reading-notes/main-table)

# Read: 01 - Java Basics


#### ______________________________________________________

## Variables:

### The Java programming language defines the following kinds of variables:

* Instance Variables (Non-Static Fields) 
* Class Variables (Static Fields) 
* Local Variables
* Parameters

### Naming (The rules and conventions for naming your variables can be summarized as follows):

* Variable names are case-sensitive
* Subsequent characters may be letters, digits, dollar signs, or underscore characters.
* If the name you choose consists of only one word, spell that word in all lowercase letters. If it consists of more than one word, capitalize the first letter of each subsequent word.

#### ______________________________________________________

## Operators:

| Operator | Precedence |
| -------  | ---------  |
| postfix  | expr++ expr-- |
|unary     |++expr --expr +expr -expr ~ ! |
|multiplicative | * / % |
|additive | + - |
|shift | << >> >>> |
|relational | < > <= >= instanceof |
|equality | == != |
|bitwise AND | & |
|bitwise exclusive OR | ^ |
|bitwise inclusive OR | `|` |
|logical AND | && |
|logical OR | `||` |
|ternary | ? : |
|assignment | = += -= *= /= %= &= ^= `|=` <<= >>= >>>= |

#### ______________________________________________________

## Expressions, Statements, and Blocks:

### Expressions:

***An expression is a construct made up of variables, operators, and method invocations, which are constructed according to the syntax of the language, that evaluates to a single value.***
* ex: ( 2 * 2 ) + 10

### Statements:

***Statements are roughly equivalent to sentences in natural languages. A statement forms a complete unit of execution. ***

* ex: 

```
// assignment statement
aValue = 8933.234;
// increment statement
aValue++;
// method invocation statement
System.out.println("Hello World!");
// object creation statement
Bicycle myBike = new Bicycle();

// declaration statement
double aValue = 8933.234;

```

### Blocks:

***A block is a group of zero or more statements between balanced braces and can be used anywhere a single statement is allowed.***

#### ______________________________________________________

## Control Flow Statements:

***Control flow statements, break up the flow of execution by employing decision making, looping, and branching, enabling your program to conditionally execute particular blocks of code. This section describes the decision-making statements (if-then, if-then-else, switch), the looping statements (for, while, do-while), and the branching statements (break, continue, return) supported by the Java programming language.***

### The if-then and if-then-else Statements:

ex: 

```
if (isMoving) {
        currentSpeed--;
} else {
        System.err.println("The bicycle has already stopped!");
    } 

```

### The switch Statement:

ex:
```
    int month = 8;
    String monthString;
    switch (month) {
        case 1:  monthString = "January";
                    break;
        case 2:  monthString = "February";
                    break;
        case 3:  monthString = "March";
                break;
        default: monthString = "Invalid month";
                     break;       
```

### The while and do-while Statements:

ex:
```
while (true){
    // your code goes here
}
--------------

do {
     statement(s)
} while (expression);

```

### The for Statement

* for (initialization; termination; increment) {
    statement(s)
}

ex:
```
for(int i=1; i<11; i++){
    System.out.println("Count is: " + i);
}
```

* **the enhanced for to loop :**
ex:
```
int[] numbers = 
    {1,2,3,4,5,6,7,8,9,10};
for (int item : numbers) {
    System.out.println("Count is: " + item);
}
```

### Branching Statements:

#### The break Statement:

***unlabeled break statement:***
```
for (i = 0; i < arrayOfInts.length; i++) {
    if (arrayOfInts[i] == searchfor) {
        foundIt = true;
        break;
    }
}
```
***labeled break statement:***
```
search:
    for (i = 0; i < arrayOfInts.length; i++) {
        for (j = 0; j < arrayOfInts[i].length;
                j++) {
            if (arrayOfInts[i][j] == searchfor) {
                foundIt = true;
                break search;
            }
        }
    }

```

#### The continue Statement:

```
 for (int i = 0; i < max; i++) {
    // interested only in p's
    if (searchMe.charAt(i) != 'p')
        continue;

    // process p's
    numPs++;
}
```

#### The return Statement:

* **The last of the branching statements is the return statement. The return statement exits from the current method, and control flow returns to where the method was invoked.** 
* The return statement has two forms: one that returns a value, and one that doesn't. To return a value, simply put the value (or an expression that calculates the value) after the return keyword.
`return ++count;`

* The data type of the returned value must match the type of the method's declared return value. When a method is declared void, use the form of return that doesn't return a value.

`return;`

#### ______________________________________________________

### This file wrote by [Mohamad Saad Eddin](https://github.com/MHD22).
***you can visit my profile and follow me.❤️😎***
### ______________________________________________


###### Thanks for your time, I hope that you have enjoyed it.