**java集合框架详细介绍**

**Collection接口：**

**1、继承体系**

```java
Collection<interface>

​	—Set<interface>

​		—HashSet（实现类）

​		—LinkedHashSet（实现类）

​		—SortedSet<interface>

​			—TreeSet（实现类）

​	—List<interface>

​		—ArrayList（实现类）

​		—Vector（实现类）

​		—LinkedList（实现类）

​	—Queue<interface>

​		—LinkedList（实现类）

​		—PriorityQueue（实现类）
```

注意：

Set、List、Queue属于同一级别，均继承自Collection接口；

LinkedList同时实现了List接口和Queue接口；

SortedSet是个接口，只有一个实现类（TreeSet），元素唯一。

**2、总结**

List：元素有序、可重复

- ArrayList：底层数据结构是数组，查询快，增删慢，线程不安全，效率高；
- Vector：底层数据结构是数组，查询快，增删慢，线程安全，效率低；
- LinkedList：底层数据结构是链表，查询慢、增删快，线程不安全，效率高。

Set：元素唯一

- HashSet：底层数据结构是哈希表（无序、唯一），依赖hashCode()和equals()方法保证元素唯一性；
- LinkedHashSet：底层数据结构是链表+哈希表（有序、唯一），链表保证元素有序，哈希表保证唯一；
- TreeSet：底层数据结构是红黑树（有序、唯一），利用自然排序和比较器排序保证元素有序，根据返回值是否为0判断元素是否唯一。

说明：元素有序是指插入和读取的顺序是否一致！

**3、到底使用谁？**

```java
元素唯一吗？

​	唯一：Set

​		有序吗？

​			有序：TreeSet或LinkedHashSet

​			无序：HashSet

​			如果不知道用什么，就用HashSet

​	不唯一：List

​		线程安全吗？

​			安全：Vector

​			不安全：ArrayList或LinkedList

​				查询多：ArrayList

​				增删多：LinkedList

​				如果不知道用什么，就用ArrayList

如果只知道拥挤和，不知道用哪个，就用ArrayList
```

**Map接口**

Map接口有三个比较重要的实现类，分别是HashMap、TreeMap和HashTable。

- HashMap：线程不安全，效率高，允许存在null值（K、V都允许），无序；
- HashTable：线程安全，效率低，不允许存在null值，无序；

- TreeMap：有序。

- HashTable一般不用，如果实在要求线程安全，那就使用ConcurrentHashMap好了，Concurrent包下的类均为线程安全的。