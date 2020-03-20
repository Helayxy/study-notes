##### Java反射介绍

**1、什么是反射？**

在运行状态中，对于任何一个类，我们都能够知道这个类有哪些方法和属性，对于任何一个对象，我们都能够对它的方法和属性进行调用。**我们把这种动态获取对象信息和调用对象方法的功能称之为反射机制。**

说明：反射中，要获取一个类或者调用某个类的方法，首先要获取到该类的 Class 对象。

**2、获取Class对象的三种方式**

```java
package reflectt_demo;

public class User {
    
    private String name;
    private int age;
    private String sex;
    
    public User() {
    }
    //get、set方法
}
```

- 调用Object的getClass()方法

```java
package reflectt_demo;

public class UserClass {
    public static void main(String[] args) {
        User user = new User();
        Class<? extends User> aClass = user.getClass();
        System.out.println(aClass);
    }
}
//输出结果：class reflectt_demo.User
```

- 直接对象.class

```java
package reflectt_demo;

public class UserClass {
    public static void main(String[] args) {
        Class<User> userClass = User.class;
        System.out.println(userClass);
    }
}
//输出结果：class reflectt_demo.User
```

- 通过Class.forName()

```java
package reflectt_demo;

public class UserClass {
    public static void main(String[] args) throws ClassNotFoundException {
        Class<?> name = Class.forName("reflectt_demo.User");//全限定类名（包名+类名）
        System.out.println(name);
    }
}
//输出结果：class reflectt_demo.User
```

**3、通过反射创建对象的两种方式**

- 通过类对象调用newInstance()方法，例如：String.class.newInstance()；
- 通过类对象调用getConstructor()或getDeclaredConstructor()方法获得构造器（Constructor）对象，然后调用其newInstance()方法创建对象，代码如下所示：

```java
package reflectt_demo;

import java.lang.reflect.Constructor;

public class Test {
    public static void main(String[] args) throws Exception {
        Constructor<String> constructor = String.class.getConstructor(String.class);
        String s = constructor.newInstance("hello world!");
        System.out.println(s);
    }
}
//输出结果：hello world!
```

说明：类对象就是类的字节码对象。