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

>  [Back to catalogue](#Chapter 8: Generic Programming#)

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

>  [Back to catalogue](#Chapter 8: Generic Programming#)

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

>  [Back to catalogue](#Chapter 8: Generic Programming#)

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

>  [Back to catalogue](#Chapter 8: Generic Programming#)

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

​	Generally, we think a generic method like ```public static <T extends Comparable> T min(T[] a)``` is a set of methods. After type erasure, there only left one method ``` public static Comparable min(Comparable[] a)```.

​	The translation bring up a couple of complexities. Let's get into the translation of generic methods deeply.

​	Suppose there exists a piece of codes like below:

```java
class A<T> {
    public void func(T t){
    	//operation one
    }
}
class B extends A<T>{
	public void func(T t){
        //operation two
    }
}
```

​	Obviously, we want to override the method ```func(T t)```, but , we must take care of the trap of type erasure. 

```java
// after erasure
class A{
    public void func(Object t){
        //opeartion one
    }
}
class B extends A{
    public void func(T t){
        //operation two
    }
}
```

​	Notice that generic method ```func(...)``` will be transleted in ```class A``` while unchanging in ```class B```. We've learned that ```B``` will inherit all ```public``` methods in ```A```. 

​	So we hadn't override the ```func``` method, there actually exist two deffferent ```func``` in ```B```

​	We may have thought that same return type, same method name and different parameters, seems great for Polymorphism. But, take one more look at the parameters, we can find that one parameter type is the RAW type for the other, which means that if an input fits the more strict parameter, it works for the other.

​	To resolve the problem, compiler will generates a *bridge method* in ```B``` like this:

``` java
public void func(Object t){
    // insert a forced type conversion
    func((T) t);
}
```

​	Through the *Bridge Method*, we can easily call the right method.

​	And what if generic type shows in return type? 

> [Go to Supplement 1](##Supplement 1: Generic Inheritance##)
>
>  [Back to catalogue](#Chapter 8: Generic Programming#)

---

## Section 6: Restrictions and Limitations ##

---

## Section 7: Inheritance Rules for Generic Types ##

---

## Section 8: Wildcard Types ##

---

## Section 9: Reflection and Generics ##

---

## Supplement 1: Generic Inheritance ##

​	When we enxtend a class ```subClass``` from a generic class ``` superClass```, ```subClass``` may introduce extra generic type which wasn't declared in ``` superClass``` 

​	we can distinguish as four situations.

1. ```subClass``` inherited all the generic type from ```superClass```

    ```java
    public class totalInheritance{
    
        public static void main(String[] args) {
            Father<Integer,String> father = new child<>(1, "2");
            child<String,String ,Boolean> child = new child<>("1","2");
    
        }
    }
    class Father<T1, T2>{
        T1 t1;
        T2 t2;
    
        public Father(T1 t1,T2 t2){
            this.t1 = t1;
            this.t2 = t2;
            System.out.println(this.t1.getClass());
            System.out.println(this.t2.getClass());
        }
    
    
    }
    class child<T1, T2, T3> extends Father<T1, T2> {
    
        public child(T1 t1, T2 t2) {
            super(t1, t2);
        }
    }
    /**
    * output:
    * class java.lang.Integer
    * class java.lang.String
    * class java.lang.String
    * class java.lang.String
    */
    ```

2. ```subClass``` inherited  part of the generic type from ```superClass```, generics that are not inherited should be defined as concrete types 

    ```java
    public class partialInheritance {
    
        public static void main(String[] args) {
            Father<Integer,String> father = new child<>(1, "2");
            child<String,String ,Boolean> child = new child<>("1","2");
    
        }
    }
    class Father<T1, T2>{
        T1 t1;
        T2 t2;
    
        public Father(T1 t1,T2 t2){
            this.t1 = t1;
            this.t2 = t2;
            System.out.println(this.t1.getClass());
            System.out.println(this.t2.getClass());
        }
    
    
    }
    class child<T1, T2, T3> extends Father<T1,String> {
    // child didn't intended to inherit the second genetric type parameter form father
    // so we have to define it as a concrete type like ```String```
        public child(T1 t1, String t2) {
            super(t1, t2);
        }
    }
    /**
    * output
    * class java.lang.Integer
    * class java.lang.String
    * class java.lang.String
    * class java.lang.String
    */
    ```

3.  ```subClass``` inherited  no  generic type from ```superClass```, and ```subClass``` may introduce extra generic type which wasn't declared in ``` superClass```

    1. ```subClass``` implements all the generic parameters in the ```superClass```, so that no generic is inherited in ```subClass```

        ```java
        public class Test{ 
        	public static void main(String[] args) {
        		child<String,String ,String> child = new child<>(1,"2");
                 //no matter how to declare, it was determined by the parameters input
        	}
        }
        class Father<T1, T2>{
        	T1 t1;
        	T2 t2;
        	
        	public Father(T1 t1,T2 t2){
        		this.t1 = t1;
        		this.t2 = t2;
        		System.out.println(this.t1.getClass());
        		System.out.println(this.t2.getClass());
        	}
        	
        	
        }
        class child<T1, T2, T3> extends Father<Integer,String> {
         
        	public child(Integer t1, String t2) {
        		super(t1, t2);
        	}
        }
        
        /**
        * output
        * class java.lang.Integer
        * calss java.lang.String
        */
        ```

    2. ```subClass``` implements no generic parameters in the ```superClass```, in which case, ```<>``` should not show after the ```superClass```, all generic type in ```subClass``` could be regarded as additional.

        ```java
        public class test {
        
            public static void main(String[] args) {
                child<String,String ,String> child = new child<>("1",false);
                 //no matter how to declare, it was determined by the parameters input
        
            }
        }
        class Father<T1, T2>{
            T1 t1;
            T2 t2;
        
            public Father(T1 t1,T2 t2){
                this.t1 = t1;
                this.t2 = t2;
                System.out.println(this.t1.getClass());
                System.out.println(this.t2.getClass());
            }
        
        
        }
        class child<T1, T2, T3> extends Father {
        
            public child(Object t1, Object t2) {
                super(t1, t2);
                // TODO Auto-generated constructor stub
            }
        }
        
        /**
        * output decided by the input
        * now we construct the child with an Integer and a Boolen objects
        * so such result will show
        * 
        * class java.lang.Integer
        * class java.lang.Boolen
        *
        * no matter what we have declared before, eg:<String,String.String>
        */
        ```

>  [Back to catalogue](#Chapter 8: Generic Programming#)

