---
title: Java中级
date: 2020-11-09 11:40:15
categories:
- Java
tags:
- 中级
---

#### Day 001

> <!-- Part 001 -->
>
> ※ 每加载一种`class`，`JVM`便会为其创建一个`Class`类型的实例，并将它们关联起来。在`Class`实例中，保存了该`class`的类名、包名、父类、实现的接口、所有方法、所有字段等，因此，若获取了某个`Class`实例，便可以通过该实例获取到该实例对应的`class`的所有信息。上述通过`Class`实例获取`class`信息的方法便称为<span style="color:blue">反射</span>。
>
> ※ 获取一个`class`的`Class`实例，存在以下三种方法：
>
> - 直接通过`class`的静态变量`class`获取：
>
> ```java
> Class cls = String.class;
> ```
>
> - 通过实例变量提供的`getClass()`方法获取：
>
> ```java
> String str = "chen";
> Class cls = s.getClass();
> ```
>
> - 通过静态方法`Class.forName()`获取（前提是知道`class`的完整类名）：
>
> ```java
> Class cls = Class.forName("java.lang.String");
> ```
>
> **注**：因为`Class`实例在`JVM`中是唯一的，因此上述方法获取的`Class`实例为同一个实例，可以直接用`==`进行比较。
>
> ※ 通常情况下，我们应该使用`instanceof`判断数据类型，因为面向抽象编程的时候，我们并不关心具体的子类型，只有在需要精确判断一个类型是不是某个`class`的时候，才会使用`==`判断。
>
> ※ `JVM`在执行`Java`程序的时候，并不是一次性把所有用到的`class`加载到内存，而是需要用到时才加载，这便是`JVM`<span style="color:blue">动态加载</span>`class`的特性。利用`JVM`的动态加载特性，可以在运行期间根据条件加载不同的实现类。

> <!-- Part 002 -->
>
> ※ 反射`API`提供的`Field`类封装了字段的所有信息。
>
> ※ 利用`Class`实例的`getField()`，`getFields()`，`getDeclaredField()`和`getDeclaredFields()`方法可以获取`Field`实例。通过`Field`实例，可以获取字段信息，如`getName()`（名称），`getType()`（类型），`getModifiers()`（修饰符）；也可以读取或设置某个对象的字段（若存在访问限制，则首先需要调用`setAccessible(true)`来访问非`public`字段）。

> <!-- Part 003 -->
>
> ※ 反射`API`提供的`Method`类封装了方法的所有信息。
>
> ※ 利用`Class`实例的`getMethod()`，`getMethods()`，`getDeclareMethod()`和`getDeclaredMethods()`方法可以获取`Method`实例。通过`Method`实例，可以获取方法信息，如`getName()`，`getModifiers()`，`getReturnType()`，`getParameterTypes()`；也可以调用某个对象的方法。**注**：通过反射调用方法时，仍然遵守多态原则。

> <!-- Part 004 -->
>
> ※ 反射`API`提供的`Constructor`类封装了构造方法的所有信息。
>
> ※ 利用`Class`实例的`getConstructor()`，`getConstructors()`等方法可以获取`Constructor`实例。通过`Constructor`实例，可以创建一个对象，如`newInstance(Object... parameters)`。**注**：其实也可以直接调用`Class`实例的`newInstance()`方法来创造实例，但是它只能调用该类的`public`无参数构造方法，若构造方法不是`public`或者带有参数，就无法通过`Class.newInstance`来创造。

> <!-- Part 005 -->
>
> ※ 利用`Class`实例可以获取继承关系，如`getSuperclass()`（获取父类类型），`getInterfaces()`（获取**当前类**实现的所有接口）。**注**：对所有`interface`的`Class`调用`getSuperclass()`返回的是`null`，获取接口的父接口要用`getInterfaces()`。
>
> ※ 利用`Class`实例的`isAssignableFrom()`方法可以判断一个向上转型是否可以实现。

> <!-- Part 006 -->
>
> ※ `Java`标准库提供了<span style="color:green">动态代理</span>功能，允许在运行期间动态创建一个接口的实例。<!--存疑-->

#### Day 002



#### Day 003

> <!-- Part 001 -->
>
> ※ <span style="color:blue">泛型</span>是指编写模板代码来适应任意类型，其好处是在使用时不必对类型进行强制转换，可以通过编译器对类型进行检查。
>
> ※ 可以把`ArrayList<Integer>`向上转型为`List<Integer>`（`T`不能变），但不能把`ArrayList<Integer>`向上转型为`ArrayList<Number>`（`T`不能变为父类）。

> <!-- Part 002 -->
>
> ※ 使用泛型时，可以把泛型参数`<T>`替换为需要的`class`类型，也可以省略编译器能自动推断出的类型。
>
> ※ 不指定泛型参数类型时，编译器将会给出警告，且只能将`<T>`视为`Object`类型。
>
> ※ 可以在接口中定义泛型类型，实现此接口的类必须实现正确的泛型类型。

> <!-- Part 003 -->
>
> ※ 编写泛型时，需要定义泛型类型`<T>`。
>
> ※ 泛型可以同时定义多种类型，例如`Map<K, V>`。
>
> ※ 静态方法不能引用泛型类型`<T>`，必须定义其它类型（如`<T>`）来实现静态泛型方法。

> <!-- Part 004 -->
>
> ※ `Java`的泛型是采用<span style="color:red">擦拭法</span>实现的，擦拭法决定了泛型`<T>`存在以下缺点：不能是基本类型；不能获取带泛型类型的`Class`；不能判断带泛型类型的类型，例如`x instanceof Pair<String>`；不能实例化`T`类型。
>
> ※ 泛型方法要防止重复定义方法，例如`public boolean equals(T obj)`。
>
> ※ 子类可以获取父类的泛型类型。<!--存疑-->

> <!-- Part 005 -->
>
> ※ 使用类似`<? extends Number>`通配符作为方法参数时：方法内部可以调用获取`Number`引用的方法，但不可以调用传入`Number`引用的方法（`null`除外），换句话说，使用`extends`通配符表示可以读，不能写。<span style="color:blue">上界通配符</span>
>
> ※ 使用类似`<? super Number>`通配符作为方法参数时：方法内部可以调用传入`Number`引用的方法，但不可以调用获取`Number`引用的方法（`Object`除外），换句话说，使用`super`通配符表示可以写，不能读。<span style="color:blue">下界通配符</span>

> <!-- Part 006 -->
>
> ※ 部分反射`API`为泛型，例如`Class<T>`、`Constructor<T>`。
>
> ※ 可以声明带泛型的数组，但不能直接创建带泛型的数组，必须强制转型。<!--存疑-->

#### Day 004

> <!-- Part 001 -->
>
> ※ 如果一个`Java`对象可以在内部持有若干其它`Java`对象，并对外提供访问接口，那么该对象被称之为集合。显然，数组便是一种集合。
>
> ※ `Java`主要提供了`3`种集合类：`List`、`Set`和`Map`，均定义在`java.util`包中。`Java`的集合设计有如下特点：接口与实现类分离；支持泛型；利用迭代器`Iterator`进行访问。
>
> ※ `Java`的集合设计比较久远，中间经历过大规模改进，有一些集合类不应该继续使用：`Hashstable`：一种线程安全的`Map`实现；`Vector`：一种线程安全的`List`实现；`Stack`：基于`Vector`实现的栈。还有一些接口也不应该继续使用：`Eumneration<E>`：已被`Iterator<E>`取代。

> <!-- Part 002 -->
>
> ※ 除使用`ArrayList`、`LinkedList`外，还可以通过`List`接口提供的`of()`方法，根据给定元素快速创建`List`：

```java
List<Integer> list = List.of(1, 2, 3)
```

> 但是，`List.of()`方法不接受`null`。若传入`null`，将会抛出`NullPointerException`异常。
>
> ※ 在遍历`List`时，完全可以利用`for`循环，但是并不推荐，一是代码比较复杂，二是索引对于`LinkedList`来说访问速度较慢。推荐利用`Iterator`，它是由`List`的实例调用`iterator()`方法时创建的对象，知道如何遍历`List`，<span style="color:red">总是具有最高的访问效率</span>：

```java
public class Main {
    public static void main(String[] args) {
        List<String> list = List.of("apple", "pear", "banana");
        for (String s : list) {
            System.out.println(s);
        }
    }
}
```

> ※ 将`List`转换为数组有三种方法：调用`toArray()`方法直接返回一个`Object[]`数组，该方法会导致类型信息丢失，因此实际应用较少；给`toArray(T[])`方法传入一个类型相同的数组，`List`内部自动将元素复制到传入的数组中；通过`List`接口定义的`T[] toArray(IntFunction<T[]> generator)`方法：

```java
List<Integer> list = List.of(12, 34, 56);
// Way 1:
Object[] array = list.toArray();
// Way 2:
Integer[] array = list.toArray(new Integer[3]);
// Way 3:
Integer[] array = list.toArray(Integer[]::new);
```

> ※ 将数组转换为`List`可以直接利用`List.of()`方法。**注**：返回的是一个只读`List`，对其调用`add()`等方法均会抛出`UnsupportedOperationException`异常。

> <!-- Part 003 -->
>
> ※ 要正确使用`List`的`contains()`、`indexOf()`方法，放入的实例必须正确覆写`equals()`方法，否则将查找不到。`equals()`方法必须满足以下条件：自反性；对称性；传递性；一致性。**注**：编写`equals()`方法时可以借助`Object.equals()`方法判断，两个引用类型都是`null`时它们也是相等的。
>
> ※ 覆写`equals()`方法的一般步骤：确定哪些属性相等，就认为实例相等；利用`instanceof()`判断传入待比较的`Object`是否为当前类型，若是则继续比较，否则返回`false`；对基本类型利用`==`进行比较，对引用类型利用`Ibject.equals()`方法进行比较：

```java
public boolean equals(Object o) {
    if (o instanceof Person) {
        Person p = (Person) o;
        return Objects.equals(this.name, p.name) && this.age == p.age;
    }
    return false;
}
```

> **注**：若不调用`List`的`contains()`、`indexOf()`这些方法，则无需覆写`equals()`方法。

> <!-- Part 004 -->
>
> ※ 对`Map`来说，遍历`Key`可以使用`for each`循环遍历`Map`对象的`KeySet()`方法返回的`Set`集合；同时遍历`key`和`value`可以使用`for each`循环遍历`Map`对象的`entrySet()`方法返回的`Set`集合：

```java
for (String key : map.keySet()) {
    Integer value = map.get(key);
    System.out.println(key + " = " + value);
}
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    String key = entry.getKey();
    Integer value = entry.getValue();
    System.out.println(key + " = " + value);
}
```

> <!-- Part 005 -->
>
> ※ `HashMap`初始化时默认的数组大小只有`16`，任何`key`，无论其`hashcode()`有多大，通过与`0x1f`进行与运算后均可将索引限制在`0~15`；当添加的`key`的数量超过`16`时，`HashMap`会在内部自动扩容，每次扩容一倍；相应地，需要重新借助与运算将索引限制在`0~31`。由于扩容会导致重新分布已有的`key-value`，因此对`HashMap`的性能影响很大。若事先已经确定`HashMap`的容量，更好的方式是初始化时便指定容量。
>
> ※ 不同`key`具有相同`hashcode()` 的情况称为<span style="color:blue">哈希冲突</span>。发生哈希冲突时，一种最简单的方法便是利用`List`存储`hashCode()`相同的`key-value`。显然，冲突的概率越大，`List`便越长，`Map`的`get()`方法效率便越低。
>
> ※ 正确使用`Map`必须保证：若实例`a`和实例`b`相等，则`a.equals(b)`必须为`true`，并且`a.hashCode()`必须等于`b.hashCode()`；反之，若实例`a`和实例`b`不等，则`a.equals(b)`必须为`false`，并且`a.hashCode()`和`b.hashCode()`尽量不要相等，否则会出现上面那种情况。

> <!-- Part 006 -->
>
> ※ 若`Map`的`key`为`enum`类型，则推荐使用`EnumMap`，其在内部以非常紧凑的数组存储`value`，并根据`enum`类型的`key`直接定位到内部数组的索引，无需计算`hashCode()`，不但效率最高，而且没有额外的空间浪费。

> <!-- Part 007 -->
>
> ※ 通常情况下，遍历`Map`的`Key`时顺序是不可预测的，但`SortedMap`可以。
>
> ※ `SortedMap`的实现类为`TreeMap`。使用`TreeMap`时，放入的`Key`必须实现`Comparable`接口；若没有实现，则在创建`TreeMap`时必须指定一个自定义排序算法：

```java
public class Main {
    public static void main(String[] args) {
        Map<Person, Integer> map = new TreeMap<>(new Comparator<Person>() {
            public int compare(Person p1, Person p2) {
                return p1.name.compareTo(p2.name);
            }
        });
        map.put(new Person("Tom"), 1);
        map.put(new Person("Bob"), 2);
        map.put(new Person("Lily"), 3);
        for (Person key : map.keySet()) {
            System.out.println(key);
        }
        // {Person: Bob}, {Person: Lily}, {Person: Tom}
        System.out.println(map.get(new Person("Bob"))); // 2
    }
}

class Person {
    public String name;
    Person(String name) {
        this.name = name;
    }
    public String toString() {
        return "{Person: " + name + "}";
    }
}
```

> <span style="color:red">**注**</span>：`Person`类并未覆写`equals()`和`hashCode()`方法，因为`TreeMap`利用`Comparable`进行比较！
>
> ※ `TreeMap`在比较两个`Key`是否相等时，依赖于`Key`的`compareTo()`方法或者`Comparator.compare()`方法。当两个`Key`相等时，<span style="color:red">必须</span>返回`0`；或者直接借助`Integer.compare(int, int)`方法。

> <!-- Part 008 -->
>
> ※ 利用`Properties`读取配置文件共分为如下三步：创建`Properties`实例；调用`load()`方法读取文件；调用`getProperty()`获取配置：

```java
String f = "setting.properties";
Properties props = new Properties();
props.load(new java.io.FileInputStream(f));
String filepath = props.getProperty("last_open_file");
String interval = props.getProperty("auto_save_interval", "120");
```

> 调用`getProperty()`获取配置时，若`Key`不存在，则返回`null`或者事先设定的默认值。**注**：可以从文件系统，`classpath`或其它任何地方读取配置文件。
>
> ※ 如果通过`setProperty()`修改了`Properties`实例，可以利用`store()`方法将配置写入文件，方便下次读取：

```java
Properties props = new Properties();
props.setProperty("language", "Java");
props.store(new FileOutputStream("setting.properties"), "这是写入的properties注释");
```

> ※ 如果存在多个配置文件，则后读取的`key-value`会覆盖先读取的`key-value`。

> <!-- Part 009 -->
>
> ※ `Set`相当于只存储`Key`、不存储`value`的`Map`。
>
> ※ 最常用的`Set`实现类为`HashSet`，它是对`HashMap`的一个简单封装。
>
> ※ 与`SortedMap`类似，`SortedSet`可以实现对`Key`的顺序访问。

> <!-- Part 010 -->
>
> ※ `Queue`常用方法：
>
> |                    | Throw Exception | E or False or Null   |
> | ------------------ | --------------- | -------------------- |
> | 添加元素到队尾     | `add(E e)`      | `boolean offer(E e)` |
> | 取队首元素并删除   | `E remove()`    | `E poll()`           |
> | 取队首元素但不删除 | `E element()`   | `E peek()`           |
>
> **注**：切忌将`null`添加到队列中，否则`poll()`方法返回`null`时，很难确定是取到了`null`元素还是队列为空。
>
> ※ `LinkedList`既实现了`List`接口，又实现了`Queue`接口。

> <!-- Part 011 -->
>
> ※ `PriorityQueue`与`Queue`的区别在于：前者的出队顺序与元素的优先级有关，在对其调用`remove()`或`pull()`方法时，返回的总是优先级最高的元素。
>
> ※ 放入`PriorityQueue`中的元素，必须实现`Comparable`接口；若没有实现，则在创建`PriorityQueue`时必须提供一个`Comparator`对象：

```java
public class Main {
    public static void main(String[] args) {
        Queue<User> q = new PriorityQueue<>(new UserComparator());
        // 添加3个元素到队列:
        q.offer(new User("Bob", "A1"));
        q.offer(new User("Alice", "A2"));
        q.offer(new User("Boss", "V1"));
        System.out.println(q.poll()); // Boss/V1
        System.out.println(q.poll()); // Bob/A1
        System.out.println(q.poll()); // Alice/A2
        System.out.println(q.poll()); // null,因为队列为空
    }
}

class UserComparator implements Comparator<User> {
    public int compare(User u1, User u2) {
        if (u1.number.charAt(0) == u2.number.charAt(0)) {
            // 如果两人的号都是A开头或者都是V开头,比较号的大小:
            return u1.number.compareTo(u2.number);
        }
        if (u1.number.charAt(0) == 'V') {
            // u1的号码是V开头,优先级高:
            return -1;
        } else {
            return 1;
        }
    }
}

class User {
    public final String name;
    public final String number;

    public User(String name, String number) {
        this.name = name;
        this.number = number;
    }

    public String toString() {
        return name + "/" + number;
    }
}
```

> <!-- Part 012 -->
>
> ※ `Queue`与`Deque`出、入队方法对比：
>
> |                    | Queue                    | Deque                             |
> | ------------------ | ------------------------ | --------------------------------- |
> | 添加元素到队尾     | `add(E e)`/`offer(E e)`  | `addLast(E e)`/`offerLast(E e)`   |
> | 取队首元素并删除   | `E remove()`/`E poll()`  | `E removeFirst()`/`E pollFirst()` |
> | 取队首元素但不删除 | `E element()`/`E peek()` | `E getFirst()`/`E peekFirst()`    |
> | 添加元素到队首     | -                        | `addFirst(E e)`/`offerFirst(E e)` |
> | 取队尾元素并删除   | -                        | `E removeLast()`/`E pollLast()`   |
> | 取队尾元素但不删除 | -                        | `E getLast()`/`E peekLast()`      |
>
> ※ `Deque`接口的实现类有`ArrayDeque`和`LinkedList`。

> <!-- Part 013 -->
>
> ※ 在`Java`中，有个遗留类叫做`Stack`，出于兼容性考虑，并没有创建`Stack`接口，因此只能用`Deque`接口模拟`Stack`。**注**：当我们把`Deque`作为`Stack`使用时，只调用`push()`、`pop()`和`peek()`方法。

> <!-- Part 014 -->
>
> ※ `Java`编译器并不清楚如何遍历`List`，利用`for each`循环遍历`List`之所以能够编译通过，是因为编译器把`for each`循环通过`Iterator`改写为了普通的`for`循环：

```java
for (Iterator<String> it = list.iterator(); it.hasNext(); ) {
    String s = it.next();
    System.out.println(s);
}
```

> 我们把这种通过`Iterator`对象遍历集合的模式称为迭代器。
>
> ※ 使用迭代器的好处在于：调用方法总是以统一的方式遍历各种集合类型，而不必关心它们内部的存储结构。
>
> ※ 如果自己编写了一个集合类，并且想要使用`for each`循环，只需满足以下条件：集合类实现`Iterable`接口，该接口要求返回一个`Iterator`对象；用`Iterator`对象迭代集合内部数据。例如：

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        ReverseList<String> rlist = new ReverseList<>();
        rlist.add("Apple");
        rlist.add("Orange");
        rlist.add("Pear");
        for (String s : rlist) {
            System.out.println(s);
        }
    }
}

class ReverseList<T> implements Iterable<T> {

    private List<T> list = new ArrayList<>();

    public void add(T t) {
        list.add(t);
    }

    @Override
    public Iterator<T> iterator() {
        return new ReverseIterator(list.size());
    }

    class ReverseIterator implements Iterator<T> {
        int index;

        ReverseIterator(int index) {
            this.index = index;
        }

        @Override
        public boolean hasNext() {
            return index > 0;
        }

        @Override
        public T next() {
            index--;
            return ReverseList.this.list.get(index);
        }
    }
}
```

> ※ 在编写`Iterator`的时候，通常用一个内部类来实现`Iterator`接口，这个内部类可以直接访问对应的外部类的所有字段和方法。例如，上述代码中，内部类`ReverseIterator`可以用`ReverseList.this`获得当前外部类的`this`引用，然后通过该`this`引用访问`ReverseList`的所有字段和方法。

> <!-- Part 015 -->
>
> ※ `Collections`是`JDK`提供的工具类，同样位于`java.util`包中。它提供了一系列静态方法，能够更加方便地操作各种集合。
>
> ※ 创建空`List`：`List<T> emptyList()`；创建空`Map`：`Map<K, V> emptyMap()`；创建空`Set`：`Set<T> emptySet()`。**注**：返回的空集合为不可变集合，无法向其中添加或删除元素。此外，也可以通过各个集合接口提供的`of(T ...)`方法创建空集合。
>
> ※ 创建单元素`List`：`List<T> singletonList(T o)`；创建单元素`Map`：`Map<K, V> singletonMap(K key, V value)`；创建单元素`Set`：`Set<T> singleton(T o)`。**注**：返回的单元素集合同样为不可变集合，无法向其中添加或删除元素。此外，也可以通过各个集合接口提供的`of(T ...)`方法创建单元素集合。
>
> ※ `Collections`可以对`List`进行排序。由于排序会直接修改`List`元素的位置，因此必须传入可变`List`。
>
> ※ 将`List`封装成不可变集合：`List<T> unmodifiableList(List<? extends T> list)`；将`Map`封装成不可变集合：`Map<K, V> unmodifiableMap(Map<? extends K, ? extends V> m)`；将`Set`封装成不可变集合：`Set<T> unmodifiableSet(Set<? extends T> set)`。**注**：该封装实际上是通过创建一个代理拦截掉所有修改方法实现的。然而，若继续对原始可变集合进行增删，同样会影响到封装后的不可变集合。为此，如果我们希望把一个可变集合封装成不可变集合，最好在返回不可变集合后扔掉对可变集合的引用。

#### Day 005
