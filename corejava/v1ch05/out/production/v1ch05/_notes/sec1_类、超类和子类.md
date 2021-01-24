# 类、超类和子类 #

---

## 1、定义子类 ##

- 使用extends关键字

- 只需要指出子类与超类不相同的地方

- 子类继承超类的字段和方法，即子类的对象也可以拥有定义在超类中的实例字段、调用超类的方法

- 以下说法等价
    - 超类superclass；基类baseclass；父类parentclass；
    - 子类subclass；派生类derivedclass；孩子类childrenclass；

---

## 2、覆盖方法 ##

当超类的某些方法不适用于子类的时候，我们可以在子类中定义同名方法来覆盖(override)超类中的方法

例如，我们想实现经理的salary是baseSalary和bonus的和，这时就可以重写超类中的getSalary方法
```java
    public class Manager extends Employee
    {
        // ...
       public double getSalary()
       {
          // override here
       }
    }
```

此时，会出现以下几个问题：

1. salary字段是Employee的私有字段，子类不能直接访问，因此以下写法错误
    ```java
    public double getSalary(){
        return salary + bonus;
    }
    ```
    > 始终牢记一个类的私有字段只有这个类的方法可以访问，哪怕是它的子类也需要通过提供的公共接口来访问

2. 既然如此，我们通过超类提供的公共接口getSalary来访问，但是如下写法也是错误的
    ```java
    public double getSalary(){
        double baseSalary = getSalary();// wrong here
        return baseSalary + bonus;
    }
    ```
    > 当我们重载getSalary的时候，调用getSalary()方法是在调用重载以后的方法，这会导致无限调用自己程序崩溃。

3. 正确写法如下
    ```java
    public double getSalary(){
        double baseSalary = super.getSalary();// use **super**
        return baseSalary + bonus;
    }
    ```

**总结：**  在子类中可以实现增加字段或方法，以及覆盖超类的方法；但是绝对不可能删除字段或者方法。

---

## 3、子类构造器 ##

```java
public Manager(String name, double salary, int year, int month, int day)
{
    super(name, salary, year, month, day);
    bonus = 0;
}
```

- 依然由于子类不能直接访问超类的私有字段，所以这里使用super关键字来调用父类中符合相应参数的构造器

- 这种利用super关键字的调用方式必须是子类构造器的第一条语句

- 如果子类没有显式的调用超类的构造器，那么编译器会自动的调用超类的无参构造器

- 如果上述情况中，超类没有无参构造器，编译器将报错


this关键字与super关键字的比较

||this|super|
|:-|:-|:-|:-|
|构造器|调用该类的其他构造器|调用超类的构造器|
|方法|指示隐式参数的引用|指示编译器调用超类方法的关键字<br>不是对象的引用<br>不能将值super赋给另外一个对象|
|共性|在构造器中调用的时候只能作为第一条语句|

---

## 4、继承层次 ##

- 由一个公共超类派生出来的所有类的集合称为继承层次(inheritance hierarchy)

- 从某个特定的类到其祖先的路径称为该类的继承链(inheritance chain)

- Java不支持多重继承，但是通过接口可以实现类似多重继承的效果

---

## 5、多态 ##

> "is - a"规则是用来判断是否应该将数据设计为继承关系的重要依据

这条规则指出了每个子类的对象也是超类的对象。  
因此也可以表述为替换规则，即：**程序中任何出现超类对象的地方都可以用子类对象来进行替换。**  
由此，产生了一个重要的特性：对象变量是多态的（polymorphic）

> **Warning!**  
> 在使用Java对象变量的多态时候需要特别特别小心，编译器可能无法帮你检查这里的错误    
> 所以永远记住一个原则：**仅仅用子类对象来替换超类对象，而不要用超类对象来替换子类对象**    
> 这听起来很简单，但是在方法调用、对象引用赋值和数组等复杂数据结构中，尤其容易出错  

```java
var boss = new Manager(...);
var staff = new Employee[3];
staff[0] = boss;
```

基于上述代码，我们给出一些常见误区的说明：

- 最基本的，我们可以通过子类的对象引用，调用子类的方法：  
`boss.setBonus(1000);//OK`

- 但是不能通过这样调用：  
`staff[0].setBonus(1000);//ERROR`  
因为staff[]的声明是Employee[]类型（用超类的对象引用调用了子类的方法）

- 也不能通过这样调用：  
`Manager m = staff[i];//ERROR`，  
同上（用超类的对象引用为子类的实例赋值）

- 子类引用的数组可以直接转换为超类引用的数组，而不需要强制类型转换:  
    ```java
    Manager[] managers = new Manager[10];
    Employee[] staffs = managers;//这里完全合法
    ```
    因为对任意managers[i]，它既是一个Manager子类的对象引用，也是一个Employee超类的对象引用

- 但是这种转换会带来一定的隐患：  
`staffs[i] = new Employee(...)`  
是可以被编译器接受的，因为staffs[i]和managers[i]是相同的引用，那么实质上我们就又犯了用超类的对象引用为子类的实例赋值的错误。  
**所以对任意的数组，我们都要牢记创建时的元素类型**

---

## 6、理解方法调用 ##

to be added here!

---

## 7、阻止继承：final类和方法 ##

>- 不能被继承、不允许扩展的类，可以用final关键字进行修饰
>- 在实例化后不能被改变的字段，可以用final关键字进行修饰
>- 不能被子类覆盖的方法，可以用final关键字进行修饰
>- final类中的所有方法自动地成为final方法，但是final类中的字段不会自动的声明为final

将方法或者类声明为final的主要原因是：确保不会在子类中改变语义；且任何一个final类的引用一定是这个final类的对象而不是其他类的对象。

---

## 8、强制类型转换 ##

> 不好的习惯，除了基本数据类型，任何需要强制类型转换的地方我们都要反思是不是应该重构超类

---

## 9、抽象类 ##

**基本思想：**




