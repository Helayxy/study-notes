sql语句中#{}的用法

```
1.parameterType="map"：参数为map类型时，#{}括号内写map的key值

2.parameterType="com.xy.pojo.CheckGroup"：参数为实体类，#{}括号内写实体类的属性名

3.parameterType="int"：参数类型为简单类型时，包括int、string、long等，#{}括号内内可以随意写
```

使用动态SQL时，必须写value，如下所示：

```xml
<!--分页查询检查组-->
<select id="findByCondition" parameterType="string" resultType="com.itheima.pojo.CheckGroup">
    select * from t_checkgroup
    <if test="value != null and value.length > 0">
        where code = #{value} or name = #{value} or helpCode = #{value}
    </if>
</select>
```

