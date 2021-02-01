# Chapter 8: Generic Programming #

---

[toc]

---

## Section 1: Why Generic Programming ##

### 1: The Advantage of Type Parameters

- Reuse withouot inheritance
- Easier to read
- Safer

### 2: There Layers of Generic Programming

1. Just Use
2. Solve the Generic problems
3. Realize your own Generic methods

---

## Section 2:  Defining a Simple Generic Class ##

> A generic class is a class with one or more type variables

- Class introduces a type variable ```T``` enclosed in angle brackets ```<>``` after the class name
- There can be more than one type variebles, declared like ``` public class name<T,U>{...}```
- Generally, we use
    - ```E``` for the element type of a collection
    - ```K``` for the key types of a table
    - ```V``` for the value types of a table
    - ```T``` for the AnType, besides, the neighboring letters ```U``` and ```S``` is also used if necessary



---

## Section 3: Generic Methods ##

- Definition
    - We can difine it inside an ordinary class, not only inside a generic class.
    - ``` public static <T> T methodName(T... a){...}``` 
        - ```public, static```: modifiers
        - ```<T>```: type parameter
        - ```T```: return Type
        - ```(T... a)```:  parameters
- Usage
    - ```className.<Anytype>methodName(parameters)```
    - Mostly, if there exists enough information to infer what type is needed, we can omit the type parameter.

---

## Section 4: Bounds for Type Variables ##

> There comes a question: what if we want the type parameters can only be instantialized as several specific types rather than any type.

- Solution: We set bounds for type variables
- Syntax: Use ```<T extends BoundingType>``` to substitute ```<T>```
    - BoundingType can be both **class** and **interfaces**, and use ```extends``` key word uniformly.
    - If BoundingType exists a class, the ```T``` can only be classes extends form the class. Obviously, more than one class in BoundingType is not allowed.
    - If BoundingType exists interfaces, the ```T``` can only be classes implements such interfaces.
    - Put class as the first BoundingType if there exists a class.
    - Among the class and interfaces in BoundingType, we use ```&``` to split them up, because we have used comma "```,``` " to separate two type variables like ```<T,U>```

---

## Section 5: Generic Code and the Virtul Machine ##

> This section is the most complicated one in the chapter. So take some time and read carefully!!!
>
> In summary, we must remember these facts about translation of Java generics:
>
> - There are no generics in the virtual machine, only ordinary classes and methods.
> - All type parameters are replaced by their bounds
> - Bridge methods are synthesized to preserve polymorphism.
> - Casts are inserted as necessary to preserve type safety.

### 1: Type Erasure ###

1. Compiler will replace ```T``` with the first BoundingType.
    1. If T there is a class, T will be replaced as the class
    2. If there exist several BoundingTypes, it will be replaced as the first one, and the compiler will insert casts to others when necessary. So put the interface with less methods at the end of BoundingType list for effciency.
2. Compiler will always replace ```T``` with ```Object``` if no BoundingType is assigned.

> We named generic type after type erasure as ***Raw Type***

### 2：Translating Generic Expressions ##

1. For generic methods
    1. Compiler convert it to ***Raw Method***
    2. When called, execute the call of ***Raw Method***
    3. Cast the return type(```Object``` or else) into type needed.
2. For generic fields, similar operations are also performed.

### 3: Translating Generic Methods ###

​	Let's get into the translation of generic methods deeply.

​	Suppose there exists a piece of codes like below:

```java
class A<T,T> {
    public func(){
    	//operation one
    }
}
class B extends A<T,T>{

}
```



---

## Section 6: Restrictions and Limitations ##

---

## Section 7: Inheritance Rules for Generic Types ##

---

## Section 8: Wildcard Types ##

---

## Section 9: Reflection and Generics ##

