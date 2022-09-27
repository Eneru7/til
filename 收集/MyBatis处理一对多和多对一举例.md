MyBatis处理一对多和多对一举例

实际问题就是业务中经常出现的，实体中包含其他实体或者包含其他实体数组的情况

模拟场景：员工和部门

```java
class Emp{
    Integer eid;
    String name;
    Integer age;
    String sex;
    String email;
    Dept dept;
}
class Dept{
    Integer did;
    String deptName;
    List<Emp> emps;
}
```

```java
//EmpMapper
//查询员工以及员工所对应的部门信息
Emp getEmpAndDept(@Param("eid")) Integer id);
```

```xml
<resultMap id="empAndDeptResultMapOne" type="Emp">
    <id property="eid" column="eid"></id>
    <result property="empName" column="emp_name"></result>
    <result property="age" column="age"></result>
    <result property="sex" column="sex"></result>
    <result property="email" column="email"></result>
    <result property="dept.did" column="did"></result>
    <result property="dept.deptName" column="deptName"></result>
</resultMap>

<resultMap id="empAndDeptResultMapTwo" type="Emp">
    <id property="eid" column="eid"></id>
    <result property="empName" column="emp_name"></result>
    <result property="age" column="age"></result>
    <result property="sex" column="sex"></result>
    <result property="email" column="email"></result>
    <assosiation property="dept" javaType="Dept">
        <id property="did" column="did"></id>
        <result property="deptName" column="dept_name"></result>
    </assosiation>
</resultMap>



<select id ="getEmpAndDept" resultMap="">
    select * from t_emp left join t_dept on t_emp.eid = t_dept.did where t_emp.id=#{id}
</select> 
```

分步多对一查询

```java
// 通过分步查询员工以及员工对应的部门信息
// 第一步：查询员工信息
Emp getEmpAndDeptByStepOne(@Param("eid")Integet id);

// 通过分步查询员工以及员工对应的部门信息
// 第二步：查询部门信息
Emp getEmpAndDeptByStepTwo(@Param("eid")Integet id);
```

```xml
<resultMap id = "empAndDeptByStepResultMap" type="Emp">
    <id property="eid" column="eid"></id>
    <result property="empName" column="emp_name"></result>
    <result property="age" column="age"></result>
    <result property="sex" column="sex"></result>
    <result property="email" column="email"></result>
    <assosiation property="dept" 
                 select="DeptMapper.getEmpAndDeptByStepTwo"
                 column="did"
                 >
        <id property="did" column="did"></id>
        <result property="deptName" column="dept_name"></result>
    </assosiation>
</resultMap>

<select id = "getEmpAndDeptByStepOne" resultMap="empAndDeptByStepResultMap">
    select * from t_emp where eid = #{eid}
</select>
```

一对多

```java
// 查询部门以及部门中所有员工信息
Dempt getDeptAndEmps(@Param("did")Integer did);
```



```xml
<reslutMap id="deptAndEmpsResultMap" type="Dept">
    <id property="did" column="did"></id>
    <result property="deptName" column="dept_name"></result>
    <collection property="emps" ofType="Emp">
        <id property="edi" column = "eid"></id>
        <result property="empName" column="emp_name"></result>
        <result property="age" column="age"></result>
        <result property="sex" column="sex"></result>
        <result property="email" column="email"></result>
    </collection>
</reslutMap>

<select id="getDeptAndEmps" resultMap="deptAndEmpsResultMap">
    select * from t_dept left join t_emp on t_dept.did = t_emp.did where t_dept.did=#{did}
</select>
```

分步查询

```java
// 通过分步查询部门以及部门中所有员工信息
Dempt getDeptAndEmpsByStepOne(@Param("did")Integer did);

```

