# 接口 #

---

## 一、接口的概念 ##

> 接口不是**类**，而是对希望符合这个接口的类的一组**需求**。

- 例如，底层对上层说，你的类如果符合某个特定的接口，那么就可以调用特定的服务。
- Array类的sort()方法承诺，可以对对象数组进行排序，只要对象所属的类满足一个条件:实现Comparable接口：
    ```java
    public interface Comparable<T>
    {
        int compareTo  (T t)
    }
    //例如 在实现Comparable<Employee>接口的类中，必须提供int compareTo(Employee a)方法。
    ```
- 接口所有方法都自动的成为public方法，因此，在接口中声明方法的时候可以省略关键字public

