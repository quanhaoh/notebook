## 动态sql语句

### if标签

只使用if标签需要使用where 1 = 1

```xml
<select ...>
	select * from user where 1 = 1
	<if test = "userName != null">
       and userName = #{userName}
    </if>
    <if test = "userAge != null">
        and userAge = #{userAge}
    </if>
</select>
```

### where标签

使用where标签即可省略where 1 = 1

```xml
<select ...>
	select * from user
	<where>
        <if test = "userName != null">
           and userName = #{userName}
        </if>
        <if test = "userAge != null">
            and userAge = #{userAge}
        </if>
     </where>
</select>
```

### foreach标签

```xml
<foreach collection="ids" item="id" open="and id in (" close=")" separator=",">
    #{id}
</foreach>
```

