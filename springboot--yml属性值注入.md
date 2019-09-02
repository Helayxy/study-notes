# springboot--yml属性值注入

## 第一步（编写Javabean）

```java
/*
* 将配置文件中的值映射到这个类中
* 1.@ConfigurationProperties：配置文件处理器，用来将配置文件中的属性值和这个类中的属性进行绑定
*   prefix = "person"：表示将配置文件中person下的属性值进行映射
* 2.需要将配置文件处理器放入spring容器中，才能生效，所以需要加上@Component注解
* */
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birthday;
    private Map<String,Object> map;
    private List<Object> list;
    private Dog dog;
```

```java
public class Dog {
    private String name;
    private Integer age;
```

## 第二步（编写yml文件）

```yml
person:
  lastName: zhangsan
  age: 20
  boss: false
  birthday: 2017/10/22
  map:  {k1: v1,k2: v2}
  list: [lisi,wangwu]
  dog:
    name: 小狗
    age: 2
```

**注意：每个属性后一定要有冒号，属性和值之间一定要有空格**

## 第三步（编写测试用例）

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class ApplicationTests {
    @Autowired
    private Person person;

    @Test
    public void contextLoads() {
        System.out.println(person);
    }
}
```

## 第四步（启动Application主程序）

## 注意：@ConfigurationProperties此注解需要在pom.xml文件中添加配置文件处理器：

```xml
<!--配置文件处理器，绑定配置文件时会有提示-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

