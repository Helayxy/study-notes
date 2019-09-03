# spring boot学习笔记

## 一、spring boot获取配置文件中的值的不同方式

### 1、获取全局配置文件中的属性值：

#### 1.1、实体类：

```java
@Component
@ConfigurationProperties(prefix = "person")
public class Person {   
    private String lastName;
    private Integer age;
    private Boolean boss; 
    private Date birthday;
    private Map<String, Object> map;
    private List<Object> list;
    private Dog dog;
```

#### 1.2、配置文件：

```properties
person.lastName=张三
person.age=12
person.boss=true
person.birthday=2018/10/12
person.map.k1=v1
person.map.k2=v2
person.list=a,b,c
person.dog.name=dog
person.dog.age=1
```

**说明：**

**@ConfigurationProperties(prefix = "person")：此注解用于将配置文件中的属性值与实体类中的属性进行绑定，其中prefix 的值为属性的前缀，即将person下的属性值与实体类进行绑定**

**@Component：此注解用于将实体类Person和配置文件处理器放入ioc容器**

**注意：**

**spring boot默认加载的是quanju配置文件，即以application为文件名的配置文件**

### 2、获取其他配置文件中的属性值：

```java
@PropertySource(value = {"classpath:person.properties"})
@Component
@ConfigurationProperties(prefix = "person")
public class Person {  
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birthday;
    private Map<String, Object> map;
    private List<Object> list;
    private Dog dog;
```

**说明：**

**@PropertySource(value = {"classpath:person.properties"})：此注解用于spring boot获取其他配置文件中的属性值，其中value的值为数组形式，即可以加载多个配置文件**

### 3、@Value注解进行属性值的注入：

```java
@PropertySource(value = {"classpath:person.properties"})
@Component
@ConfigurationProperties(prefix = "person")
@Validated  //这个注解用来进行数据校验
public class Person {
    /* @Value：这个注解相当于
    <bean id="" class="">
          <property name="" value="字面量/${从配置文件中、环境变量中获取的值}/#{SpEL表达式}">
     </bean>
     中的value属性
     */
    @Email	//lastName的值必须为邮箱格式，用于数据校验
    private String lastName;
    @Value("#{3+8}")	//SpEL表达式，将3+8的值付给age
    private Integer age;
    @Value("true")	//注入字面值，即boss的值为true
    private Boolean boss;
    @Value("${person.birthday}")	//获取配置文件中的值
    private Date birthday;
    private Map<String, Object> map;
    private List<Object> list;
    private Dog dog;
```

### 4、@Value注解和@ConfigurationProperties进行属性值注入的区别：

|                      | @ConfigurationProperties | @Value       |
| -------------------- | ------------------------ | ------------ |
| 功能                 | 批量注入                 | 一个一个注入 |
| 松散绑定（语法松散） | 支持                     | 不支持       |
| SpEL（spring表达式） | 不支持                   | 支持         |
| JSR303数据校验       | 支持                     | 不支持       |
| 复杂类型封装         | 支持                     | 不支持       |

**两个注解的使用场景：**

**这两个注解都可以获取.ylm和.properties配置文件中的属性值**

**如果在摸个功能块需要配置文件中的某个属性值时，使用@Value注解；**

**如果是某个实体类（Javabean）需要和某个配置文件映射时，使用@ConfigurationProperties注解；**

## 二、@ImportResource注解说明：

#### 使spring boot加载我们编写的xml配置文件

#### 第一种方式：

##### 1、配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="helloService" class="cn.lynu.xy.service.HelloService">
    </bean>
</beans>
```

##### 2、HelloService.java类：

```java
package cn.lynu.xy.service;

public class HelloService {
}
```

3、配置类：

```java
@SpringBootApplication
@ImportResource({"classpath:beans.xml"})
public class Application {
   public static void main(String[] args) {
      SpringApplication.run(Application.class, args);
   }
}
```

##### 4、测试类：

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class ApplicationTests {
    @Autowired
    Person person;
    @Autowired
    //将ioc容器的对象添加到本类中
    ApplicationContext ioc;
    @Test
    public void testHelloService() {
        //调用ioc容器中的方法判断容器中有没有一个叫helloService的bean
        boolean flag = ioc.containsBean("helloService");
        System.out.println(flag);
    }
}
```

#### 解释说明：

**当我们编写xml配置文件，需要在配置类上加上@ImportResource注解，并且指定配置文件的位置，这样spring boot才能加载到我们编写的xml配置文件**

#### 第二种方式：

##### 1、编写一个配置类：

```java
/*
 * @Configuration：表明这是一个配置类，代替之前的配置文件
 */
@Configuration
public class MyConfig {
    //@Bean：在spring的ioc容器中配置HelloService这个类的对象（bean），对象名（bean的id）就是方法名
    @Bean
    public HelloService helloService() {
        return new HelloService();
    }
}
```

**说明：第二种方式就不需要编写xml的配置文件了，spring boot推荐使用第二种方法,即全注解的方式**

## 三、配置文件占位符：

### 1、配置文件：

```properties
#随机数
person.lastName=赵雷${random.uuid}	
person.age=${random.int}	
person.boss=true
person.birthday=2018/10/12
person.map.k1=v1
person.map.k2=v2
person.list=a,b,c
#占位符
person.dog.name=${person.lastName}_dog	
person.dog.age=1
```

### 2、测试结果：

```java
Person{lastName='赵雷129bc1a0-b8ad-494d-b14b-ef0befa6bc8c', age=811922129, boss=true, birthday=Fri Oct 12 00:00:00 CST 2018, map={k1=v1, k2=v2}, list=[a, b, c], dog=Dog{name='赵雷349ff172-25f6-4c0d-865b-a1ff1aa3a340_dog', age=1}}
```

说明：我们可以使用${}来获取随机数或者其他的属性值，如果其他属性没有，可以用：来指定，即person.dog.name=${person.hello:hello}_dog，表示没有person.hello属性及其值，但是我们用：来指定，最后输出的结果为：

```java
Person{lastName='赵雷129bc1a0-b8ad-494d-b14b-ef0befa6bc8c', age=811922129, boss=true, birthday=Fri Oct 12 00:00:00 CST 2018, map={k1=v1, k2=v2}, list=[a, b, c], dog=Dog{name='hello_dog', age=1}}
```

## 四、profile（多环境支持）

**说明：**

**我们在编写全局配置文件的时候，配置文件名可以指定为application-dev.properties，这就表示开发环境的配置文件，或者application-prod.properties表示生产环境的配置文件，当我们需要哪个环境就激活哪个环境的配置文件，在主配置文件application.properties使用spring.profiles.active=dev进行激活开发环境的配置文件**

### yml多文档块：

```yml
server:
  port: 8081
spring:
  profiles:
    active: prod

---
server:
  port: 8082
spring:
  profiles: dev
---
server:
  port: 8083
spring:
  profiles: prod
```

**说明：使用---进行文档的分割，这里分成了三个文档块，在第一个文档块指定激活哪个文档块**





