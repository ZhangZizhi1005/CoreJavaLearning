# Chapter 6 :Interfaces, Lambada Expressions, and Inner Classes

---

[toc]

---



## Section 1: Iterfaces ##

### 1: The Interface concept

​	In the Java programming language, an interface is not a class but a set of *requirements* for the classes that want to conform to the interface.

​	eg: If you want to sort an array of objects with ```sort``` methods of ```Array``` class, you must promise the class which the objects belong to implement the ```Comparable``` interface.

#### 1.1 What can we do in an interface

- All methods of an interface automatically become ```public```. we can save the key words in interface declaration.

- Imoplements can define constants, but they never have any instance fields.

- when you want to supply a method in an interface, using ```default``` key word.

#### 1.2 How to implements an interface

1. Declare that the class intends to implement an interface with ```implement``` key word
2. Supply all definition of all methods in the interface
3. Use ```public``` key word to modify the methods in class
4. Supply ```<Type parameter>``` for **generic** iterfaces

> [**Back to Catalogue**](# Chapter 6 :Interfaces, Lambada Expressions, and Inner Classes)

### 2: Properties of Interfaces

#### 2.1 Three Key Principles

- Interfaces are **not** classes, so that we can never use a ```new``` operator to instantiate an interface
- We can still declare interface variables, even though we can not construct interface objects
- An interface variable must refer to an object of classes that implements the interface

####  2.2 Extension of interfaces

- As we extands a subclass from a superclass, we can also use ``` extends``` key word to extend interfaces
- With same logic, the sub-interface has all constants and methods of the super-interface

#### 2.3 Implement multiple interfaces

- A class can only extend from one superclass
- A class can implement multiple interfaces (Achieve similar effects multiple inheritance )
- Use commas to separate the interfaces that you want to implement

> [**Back to Catalogue**](# Chapter 6 :Interfaces, Lambada Expressions, and Inner Classes)

###  3: Interfaces and Abstract Classes

- [ ] to do here

> [**Back to Catalogue**](# Chapter 6 :Interfaces, Lambada Expressions, and Inner Classes)

### 4: Static and Private Methods

- [ ] to do here

> [**Back to Catalogue**](# Chapter 6 :Interfaces, Lambada Expressions, and Inner Classes)

### 5: Default Methods

#### 5.1 How to use default methods

- Tag the methods with the modifier  ``` dfault```
- Define the methods in the interface,
- Other methods in same interface can be called even though they are not defined

#### 5.2 Why we need default methods

- Case 1: If we implement a ```Iterator``` interface in a read-only class,  Unrealized ```remove``` method will be wrong
- Case 2: For any collection, ```isEmpty``` method realize by same way
- Case 3: If we add method into an interface, tagging new methods as ```default``` avoid us to add the method realizition into all classes that implements the interface.

> [**Back to Catalogue**](# Chapter 6 :Interfaces, Lambada Expressions, and Inner Classes)

### 6: Resolving Default Method Conflicts

#### 6.1 Superclasses win

If there exists same methods defined seprately in superclasses and interfaces, compiler will choose method in superclasses

#### 6.2 Iterfaces clash

- If at least one method with the name is realized, which means the realized method is default, we must override the method to resolve the conflict.
- If none of thoes methods are realized, we can realize it in the class or tag the class as ```abstract``` and just ignore the confiliction.

> [**Back to Catalogue**](# Chapter 6 :Interfaces, Lambada Expressions, and Inner Classes)

### 7: Interfaces and Callback

#### 7.1 What is callback

​	Callback is a common pattern in programming. It's hard to comprehand because we use mostly the expansion of the literal meaning. The analogy given below is going to show the problem.

​	If I called somebody named A, and told him to call me back after his work finished, this procedure is callback. As an extention, if  I told him to call someone else like B, we also consider the procedure as callback.

​	Back to programing, supose there exist two functions named A and B, I call function A by ```A(...,B)```. Intresting things hanppend, function B will be called when function A finish or function B is explicit called. That means I called A, then A called B. This procedure is call back.

#### 7.2 Callback in Java

​	We can see, if we have passed a function B as parameter to function A, it is the callback pattern. In java, we can furtherly think what if we passed an objcet as parameter.

​	In fact, we have down this thousands of times like ```A(..., String str){...; str.toArray();}```. An object of a class can carry much more informations than barely a function. all ```public``` instance fields and functions can be called through the object reference passed.

​	Last but not the seldom used, the function can be **constructor**, which means we can do much more!!!

#### 7.3 Why we need interfaces to inplement callback

​	Consider a question, if we want to pass object references of different classes, how could we declare the function? We may hope these classes extend from one superclass, so that we can pass the communal superclass as formal parameter. But what if these classes extend from different superclass.

​	Fortunately, we have learned that we can declare interface variables, and these variables can be instantiate by object reference of class that implements the interface. So we can pass interface as formal parameter when we want to implement callback.

​	Actually, whenever we want multiple inheritance, this method works!!!

> [**Back to Catalogue**](# Chapter 6 :Interfaces, Lambada Expressions, and Inner Classes)

### 8: The ```comparator``` Interface

​	The other way to implement interface ```comparator<string>``` and to pass a formal parameter ```comparator<T> t``` into method ```Arrays.sort(String str, comparator<T> t)``` is an application of 7.3

> [**Back to Catalogue**](# Chapter 6 :Interfaces, Lambada Expressions, and Inner Classes)

### 9: Object cloning

- [ ] to do later

> [**Back to Catalogue**](# Chapter 6 :Interfaces, Lambada Expressions, and Inner Classes)

 

---

## Section 2: Lambda Expression ##

### 1: Why Lambdas? ###

​	If we only need a snippet of code to be executed later, we might have to difine a class that implement specific interfaces, then create a instace of this class, and finally pass it to other object ot method.

​	To introduct Lambda Expression can simplify the procedure.

> [**Back to Catalogue**](# Chapter 6 :Interfaces, Lambada Expressions, and Inner Classes)

### 2: The Syntax of Lambda Expressions ###

```(parameters) -> {expressions}```

​	A Lambda Expression consists of three parts. First, several fomal parameters in parenthese; seccond, an arrow consists of ```-``` and ```>```; third, some codes in brackets.

​	Acorrding to the basic syntax and Java language regulation, we can write it in following forms.

1. If the Lambda Expression needs no parameters, we should still supply empty parentheses, just as what we do with a parameterless method
2. We should not do this, although it's legal to omit parameter types if it can be inferred from context
3. We also should not do this, although it's still legal to omit parentheses if there only exists one parameter, and whose types is omited
4. If the computaion can be fit in single expression, we can omit brackets and ```return``` key words

> [**Back to Catalogue**](# Chapter 6 :Interfaces, Lambada Expressions, and Inner Classes)

### 3: Functional Interfaces ###

​	Any interface that have a single ```abstract``` method can be called sa functional interfaces, which means that the interface and Lambda Expression can be transformed into each other.

​	However, the most frequent usage is to transform a functional interface into a Lambda Expression, because it's hard to decide which interface to be transformed into, unless we figured which interface supply meaningful transformation.

> [**Back to Catalogue**](# Chapter 6 :Interfaces, Lambada Expressions, and Inner Classes)

### 4: Method References ###

### 5: Constructer Rerferences ###

### 6: Variable Scope ###

### 7: Processing Lambda Expressions ###

### 8: More About ```comparator``` ###

- [ ] to do later



---

## Section 3: Inner Classes ##



​	An *inner class* is class that is defined inside another class.

​	There are two reasons to use inner class:

1. Inner classes can be hidden from other classes in the same packge.
2. Inner classes can access the data from the scope in which they are defined, including the date would otherwise  be ```private```.

### 1: Use of an Inner Class to Access Object State ###

- To access the instance fields of *outer* class

> [**Back to Catalogue**](# Chapter 6 :Interfaces, Lambada Expressions, and Inner Classes)

### 2: Special Syntax Rules for Inner class ###

- To access *outer* in *inner*, ```outer_class_name.this``` denotes the outer class reference
- Conversely, to access *inner* in *outer*, ```outer_object_name.this new inner_class(construction parameters)``` explicitly call the constructor of inner class
- If the inner class is ```public```, we can use the syntax to construct a inner class instance for any objects of outer class
- Static fields of inner class must be declared as ```final```, and there exists no static methods in inner class

> [**Back to Catalogue**](# Chapter 6 :Interfaces, Lambada Expressions, and Inner Classes)

### 3: Are Inner Class Useful? Actually neccesary? Secure?

- [ ] to do later

### 4: Local Inner Class ###

- If an inner class only use in one specific method of outer class, then we can define it in the method.
- We can never access to this class unless the method is called, so they are never declared with an access specifier(```public``` for example).

### 5: Accessing Variables from Outer Methods

- this is an great advantage for only **Local Inner Class **
- Recall how **General Inner Class** access to the instance fields, they simply access from their outer class, So why can not the **Local Inner Class** access to the variables from outer methods?
- Sure it can, but you have to do this very carefully with an ensurance that those variable will never change once assigned.
- So a guaranteed approach is to access the parameters of the method.
- Thanks to the feature, we can move some instance fields of outer class to parameters of outer method, which simplify the code and offers great flexibility.

> [**Back to Catalogue**](# Chapter 6 :Interfaces, Lambada Expressions, and Inner Classes)

### 6: Anonymous Inner class ###

> ***Inner class -> Local Inner Class -> Anonymous Inner class***

- [ ] to do later

### 7: Static Inner Class

- [ ] to do later

---

## Section 4 ##

---

## Section 5 ##







