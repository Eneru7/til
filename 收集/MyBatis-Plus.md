# 一、MyBatis-Plus简介

## 1. 简介

MyBatis-Plus（简称 MP）是一个 MyBatis的增强工具，在 MyBatis 的基础上只做增强不做改变，为
简化开发、提高效率而生。

## 2. 特性

略

## 3. 支持数据库

> 任何能使用MyBatis进行 CRUD, 并且支持标准 SQL 的数据库，具体支持情况如下

## 4. 框架结构

略

## 5. 代码及文档地址

官方地址: http://mp.baomidou.com
代码发布地址:
Github: https://github.com/baomidou/mybatis-plus
Gitee: https://gitee.com/baomidou/mybatis-plus
文档发布地址: https://baomidou.com/pages/24112f





# 二、入门案例



## 1. 开发环境

## 2. 创建数据库及表

```sql
CREATE DATABASE `mybatis_plus` /*!40100 DEFAULT CHARACTER SET utf8mb4 */;
use `mybatis_plus`;
CREATE TABLE `user` (
`id` bigint(20) NOT NULL COMMENT '主键ID',
`name` varchar(30) DEFAULT NULL COMMENT '姓名',
`age` int(11) DEFAULT NULL COMMENT '年龄',
`email` varchar(50) DEFAULT NULL COMMENT '邮箱',
PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

```sql
INSERT INTO user (id, name, age, email) VALUES
(1, 'Jone', 18, 'test1@baomidou.com'),
(2, 'Jack', 20, 'test2@baomidou.com'),
(3, 'Tom', 28, 'test3@baomidou.com'),
(4, 'Sandy', 21, 'test4@baomidou.com'),
(5, 'Billie', 24, 'test5@baomidou.com');
```



## 3. 创建SpringBoot工程

## 4. 编写代码

1. 配置application.yml

2. 在Spring Boot启动类中添加@MapperScan注解，扫描mapper包

3. 添加实体

4. 添加mapper

   ```java
   public interface UserMapper extends BaseMapper<User> {
   }
   ```

   > BaseMapper是MyBatis-Plus提供的模板mapper，其中包含了基本的CRUD方法，泛型为操作的
   > 实体类型

# 三、 基本CRUD

## 1. BaseMapper

MyBatis-Plus中的基本CRUD在内置的BaseMapper中都已得到了实现，我们可以直接使用

## 2. 插入

```java
@Test
public void testInsert(){
User user = new User(null, "张三", 23, "zhangsan@atguigu.com");
//INSERT INTO user ( id, name, age, email ) VALUES ( ?, ?, ?, ? )
int result = userMapper.insert(user);
System.out.println("受影响行数："+result);
//1475754982694199298
System.out.println("id自动获取："+user.getId());
}
```

> 最终执行的结果，所获取的id为1475754982694199298
> 这是因为MyBatis-Plus在实现插入数据时，会默认基于雪花算法的策略生成id

## 3. 删除

1. 通过id删除

   ```java
   @Test
   public void testDeleteById(){
   //通过id删除用户信息
   //DELETE FROM user WHERE id=?
   int result = userMapper.deleteById(1475754982694199298L);
   System.out.println("受影响行数："+result);
   }
   ```

2. 通过id批量删除记录

   ```java
   @Test
   public void testDeleteBatchIds(){
   //通过多个id批量删除
   //DELETE FROM user WHERE id IN ( ? , ? , ? )
   List<Long> idList = Arrays.asList(1L, 2L, 3L);
   int result = userMapper.deleteBatchIds(idList);
   System.out.println("受影响行数："+result);
   }
   ```

3. 通过map条件删除记录

   ```java
   @Test
   public void testDeleteByMap(){
   //根据map集合中所设置的条件删除记录
   //DELETE FROM user WHERE name = ? AND age = ?
   Map<String, Object> map = new HashMap<>();
   map.put("age", 23);
   map.put("name", "张三");
   int result = userMapper.deleteByMap(map);
   System.out.println("受影响行数："+result);
   }
   ```

## 4. 删除

```java
@Test
public void testUpdateById(){
User user = new User(4L, "admin", 22, null);
//UPDATE user SET name=?, age=? WHERE id=?
int result = userMapper.updateById(user);
System.out.println("受影响行数："+result);
}
```

## 5. 查询

1. 根据id查询用户信息

   ```java
   @Test
   public void testSelectById(){
   //根据id查询用户信息
   //SELECT id,name,age,email FROM user WHERE id=?
   User user = userMapper.selectById(4L);
   System.out.println(user);
   }
   ```

2. 根据多个id查询多个用户信息

   ```java
   @Test
   public void testSelectBatchIds(){
   //根据多个id查询多个用户信息
   //SELECT id,name,age,email FROM user WHERE id IN ( ? , ? )
   List<Long> idList = Arrays.asList(4L, 5L);
   List<User> list = userMapper.selectBatchIds(idList);
   list.forEach(System.out::println);
   }
   ```

3. 通过map条件查询用户信息

   ```java
   @Test
   public void testSelectByMap(){
   //通过map条件查询用户信息
   //SELECT id,name,age,email FROM user WHERE name = ? AND age = ?
   Map<String, Object> map = new HashMap<>();
   map.put("age", 22);
   map.put("name", "admin");
   List<User> list = userMapper.selectByMap(map);
   list.forEach(System.out::println);
   }
   ```

4. 查询所有数据

   ```java
   @Test
   public void testSelectList(){
   //查询所有用户信息
   //SELECT id,name,age,email FROM user
   List<User> list = userMapper.selectList(null);
   list.forEach(System.out::println);
   }
   ```

> 通过观察BaseMapper中的方法，大多方法中都有Wrapper类型的形参，此为条件构造器，可针
> 对于SQL语句设置不同的条件，若没有条件，则可以为该形参赋值null，即查询（删除/修改）所
> 有数据

## 6. 通用Service

> 通用 Service CRUD 封装IService接口，进一步封装 CRUD。
>
> get 查询单行 
>
> remove 删除 
>
> list 查询集合 
>
> page 分页 
>
> 前缀命名方式区分 Mapper 层避免混淆，
> 泛型 T 为任意实体对象
> 建议如果存在自定义通用 Service 方法的可能，请创建自己的 IBaseService 继承
> Mybatis-Plus 提供的基类
> 官网地址：https://baomidou.com/pages/49cc81/#service-crud-%E6%8E%A5%E5%8F%
> A3

1. IService

   MyBatis-Plus中有一个接口 IService和其实现类 ServiceImpl，封装了常见的业务层逻辑
   详情查看源码IService和ServiceImpl

2. 创建Service接口和实现类

   ```java
   /**
   * UserService继承IService模板提供的基础功能
   */
   public interface UserService extends IService<User> {
   }
   ```

   ```java
   /**
   * ServiceImpl实现了IService，提供了IService中基础功能的实现
   * 若ServiceImpl无法满足业务需求，则可以使用自定的UserService定义方法，并在实现类中实现
   */
   @Service
   public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements
   UserService {
   }
   ```

3. 测试查询记录数

   ```java
   @Autowired
   private UserService userService;
   @Test
   public void testGetCount(){
   long count = userService.count();
   System.out.println("总记录数：" + count);
   }
   ```

4. 测试批量插入

   ```java
   @Test
   public void testSaveBatch(){
   // SQL长度有限制，海量数据插入单条SQL无法实行，
   // 因此MP将批量插入放在了通用Service中实现，而不是通用Mapper
   ArrayList<User> users = new ArrayList<>();
   for (int i = 0; i < 5; i++) {
   User user = new User();
   user.setName("ybc" + i);
   user.setAge(20 + i);
   users.add(user);
   }
   //SQL:INSERT INTO t_user ( username, age ) VALUES ( ?, ? )
   userService.saveBatch(users);
   }
   ```

> 总结：
>
> 层次关系
>
> mapper层（或者叫dao层），mybatis-plus提供了基础的一些CRUD操作，可以直接entityMapper.method()调用
>
> service层
>
> MyBatis-Plus中有一个接口 IService和其实现类 ServiceImpl，封装了常见的业务层逻辑

# 四、常用注解

## 1. @TableName

> 经过以上的测试，在使用MyBatis-Plus实现基本的CRUD时，我们并没有指定要操作的表，只是在
> Mapper接口继承BaseMapper时，设置了泛型User，而操作的表为user表
> 由此得出结论，MyBatis-Plus在确定操作的表时，由BaseMapper的泛型决定，即实体类型决
> 定，且默认操作的表名和实体类型的类名一致

1. 问题

   > 若实体类类型的类名和要操作的表的表名不一致，会出现什么问题？
   >
   > 我们将表user更名为t_user，测试查询功能
   > 程序抛出异常，Table 'mybatis_plus.user' doesn't exist，因为现在的表名为t_user，而默认操作
   > 的表名和实体类型的类名一致，即user表

2. 通过@TableName解决问题

   > 在实体类类型上添加@TableName("t_user")，标识实体类对应的表，即可成功执行SQL语句

3. 通过全局配置解决问题

   > 在开发的过程中，我们经常遇到以上的问题，即实体类所对应的表都有固定的前缀，例如t_或tbl_
   > 此时，可以使用MyBatis-Plus提供的全局配置，为实体类所对应的表名设置默认的前缀，那么就
   > 不需要在每个实体类上通过@TableName标识实体类对应的表

   ```yaml
   mybatis-plus:
   configuration:
   # 配置MyBatis日志
   log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
   global-config:
   db-config:
   # 配置MyBatis-Plus操作表的默认前缀
   table-prefix: t_
   ```

   

## 2. @TableId

> 经过以上的测试，MyBatis-Plus在实现CRUD时，会默认将id作为主键列，并在插入数据时，默认
> 基于雪花算法的策略生成id

1. 问题

   > 若实体类和表中表示主键的不是id，而是其他字段，例如uid，MyBatis-Plus会自动识别uid为主
   > 键列吗？
   > 我们实体类中的属性id改为uid，将表中的字段id也改为uid，测试添加功能。
   >
   > 程序抛出异常，Field 'uid' doesn't have a default value，说明MyBatis-Plus没有将uid作为主键赋值

2. 通过@TableId解决问题

   > 在实体类中uid属性上通过@TableId将其标识为主键，即可成功执行SQL语句

3. @TableId的value属性

   > 若实体类中主键对应的属性为id，而表中表示主键的字段为uid，此时若只在属性id上添加注解
   > @TableId，则抛出异常Unknown column 'id' in 'field list'，即MyBatis-Plus仍然会将id作为表的主键操作，而表中表示主键的是字段uid此时需要通过@TableId注解的value属性，指定表中的主键字段，@TableId("uid")或@TableId(value="uid")

4. @TableId的type属性

   > type属性用来定义主键策略

   | 值                       | 描述                                                         |
   | ------------------------ | ------------------------------------------------------------ |
   | IdType.ASSIGN_ID（默认） | 基于雪花算法的策略生成数据id，与数据库id是否设置自增无关     |
   | IdType.AUTO              | 使用数据库的自增策略，注意，该类型请确保数据库设置了id自增，否则无效 |

   配置全局主键策略：

   ```yaml
   mybatis-plus:
   configuration:
   # 配置MyBatis日志
   log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
   global-config:
   db-config:
   # 配置MyBatis-Plus操作表的默认前缀
   table-prefix: t_
   # 配置MyBatis-Plus的主键策略
   id-type: auto
   ```



## 3. @TableField

> 经过以上的测试，我们可以发现，MyBatis-Plus在执行SQL语句时，要保证实体类中的属性名和
> 表中的字段名一致
> 如果实体类中的属性名和字段名不一致的情况，会出现什么问题呢？

1. 情况1

   > 若实体类中的属性使用的是驼峰命名风格，而表中的字段使用的是下划线命名风格
   > 例如实体类属性userName，表中字段user_name
   > 此时MyBatis-Plus会自动将下划线命名风格转化为驼峰命名风格
   > 相当于在MyBatis中配置

2. 情况2

   > 若实体类中的属性和表中的字段不满足情况1
   > 例如实体类属性name，表中字段username
   > 此时需要在实体类属性上使用@TableField("username")设置属性所对应的字段名



## 4. @TableLogic

1. 逻辑删除

   * 物理删除：真实删除，将对应数据从数据库中删除，之后查询不到此条被删除的数据
   * 逻辑删除：假删除，将对应数据中代表是否被删除字段的状态修改为“被删除状态”，之后在数据库中仍旧能看到此条数据记录
     使用场景：可以进行数据恢复

2. 实现逻辑删除

   1. 数据库中创建逻辑删除状态列，设置默认值为0

   2. 实体类中添加逻辑删除属性

   3. > 测试删除功能，真正执行的是修改
      > UPDATE t_user SET is_deleted=1 WHERE id=? AND is_deleted=0
      > 测试查询功能，被逻辑删除的数据默认不会被查询
      > SELECT id,username AS name,age,email,is_deleted FROM t_user WHERE is_deleted=0



# 五、条件构造器和常用接口

## 1. wapper介绍

* Wrapper ： 条件构造抽象类，最顶端父类
  * AbstractWrapper ： 用于查询条件封装，生成 sql 的where 条件
    * QueryWrapper ： 查询条件封装
    * UpdateWrapper ： Update 条件封装
    * AbstractLambdaWrapper ： 使用Lambda 语法
      * LambdaQueryWrapper ：用于Lambda语法使用的查询Wrapper
      * LambdaUpdateWrapper ： Lambda 更新封装Wrapper

## 2. QueryWrapper

1. 例1：组装查询条件

   ```java
   @Test
   public void test01(){
   //查询用户名包含a，年龄在20到30之间，并且邮箱不为null的用户信息
   //SELECT id,username AS name,age,email,is_deleted FROM t_user WHERE
   is_deleted=0 AND (username LIKE ? AND age BETWEEN ? AND ? AND email IS NOT NULL)
   QueryWrapper<User> queryWrapper = new QueryWrapper<>();
   queryWrapper.like("username", "a")
   .between("age", 20, 30)
   .isNotNull("email");
   List<User> list = userMapper.selectList(queryWrapper);
   list.forEach(System.out::println);
   }
   ```

2. 例2：组装排序条件

   ```java
   @Test
   public void test02(){
   //按年龄降序查询用户，如果年龄相同则按id升序排列
   //SELECT id,username AS name,age,email,is_deleted FROM t_user WHERE
   is_deleted=0 ORDER BY age DESC,id ASC
   QueryWrapper<User> queryWrapper = new QueryWrapper<>();
   queryWrapper
   .orderByDesc("age")
   .orderByAsc("id");
   List<User> users = userMapper.selectList(queryWrapper);
   users.forEach(System.out::println);
   }
   ```

3. 例3：组装删除条件

   ```java
   @Test
   public void test03(){
   //删除email为空的用户
   //DELETE FROM t_user WHERE (email IS NULL)
   QueryWrapper<User> queryWrapper = new QueryWrapper<>();
   queryWrapper.isNull("email");
   //条件构造器也可以构建删除语句的条件
   int result = userMapper.delete(queryWrapper);
   System.out.println("受影响的行数：" + result);
   }
   ```

4. 例4：条件的优先级

   ```java
   @Test
   public void test04() {
   QueryWrapper<User> queryWrapper = new QueryWrapper<>();
   //将（年龄大于20并且用户名中包含有a）或邮箱为null的用户信息修改
   //UPDATE t_user SET age=?, email=? WHERE (username LIKE ? AND age > ? OR
   email IS NULL)
   queryWrapper
   .like("username", "a")
   .gt("age", 20)
   .or()
   .isNull("email");
   User user = new User();
   user.setAge(18);
   user.setEmail("user@atguigu.com");
   int result = userMapper.update(user, queryWrapper);
   System.out.println("受影响的行数：" + result);
   }
   ```

5. 例5：组装select子句

   ```java
   @Test
   public void test05() {
   //查询用户信息的username和age字段
   //SELECT username,age FROM t_user
   QueryWrapper<User> queryWrapper = new QueryWrapper<>();
   queryWrapper.select("username", "age");
   //selectMaps()返回Map集合列表，通常配合select()使用，避免User对象中没有被查询到的列值
   为null
   List<Map<String, Object>> maps = userMapper.selectMaps(queryWrapper);
   maps.forEach(System.out::println);
   }
   ```

6. 例6：实现子查询

   ```java
   @Test
   public void test06() {
   //查询id小于等于3的用户信息
   //SELECT id,username AS name,age,email,is_deleted FROM t_user WHERE (id IN
   (select id from t_user where id <= 3))
   QueryWrapper<User> queryWrapper = new QueryWrapper<>();
   queryWrapper.inSql("id", "select id from t_user where id <= 3");
   List<User> list = userMapper.selectList(queryWrapper);
   list.forEach(System.out::println);
   }
   ```

   

## 3. UpdateWrapper



# 六、插件

## 1. 分页插件

> MyBatis Plus自带分页插件，只要简单的配置即可实现分页功能

1. 添加配置类

   ```java
   @Configuration
   @MapperScan("com.atguigu.mybatisplus.mapper") //可以将主类中的注解移到此处
   public class MybatisPlusConfig {
   @Bean
   public MybatisPlusInterceptor mybatisPlusInterceptor() {
   MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
   interceptor.addInnerInterceptor(new
   PaginationInnerInterceptor(DbType.MYSQL));
   return interceptor;
   }
   }
   ```

2. 测试

   ```java
   @Test
   public void testPage(){
   //设置分页参数
   Page<User> page = new Page<>(1, 5);
   userMapper.selectPage(page, null);
   //获取分页数据
   List<User> list = page.getRecords();
   list.forEach(System.out::println);
   System.out.println("当前页："+page.getCurrent());
   System.out.println("每页显示的条数："+page.getSize());
   System.out.println("总记录数："+page.getTotal());
   System.out.println("总页数："+page.getPages());
   System.out.println("是否有上一页："+page.hasPrevious());
   System.out.println("是否有下一页："+page.hasNext());
   }
   ```



## 2. xml自定义分页

1. UserMapper中定义接口方法

   ```java
   /**
   * 根据年龄查询用户列表，分页显示
   * @param page 分页对象,xml中可以从里面进行取值,传递参数 Page 即自动分页,必须放在第一位
   * @param age 年龄
   * @return
   */
   IPage<User> selectPageVo(@Param("page") Page<User> page, @Param("age")Integer age);
   ```

2. UserMapper.xml中编写SQL

   ```java
   <!--SQL片段，记录基础字段-->
   <sql id="BaseColumns">id,username,age,email</sql>
   <!--IPage<User> selectPageVo(Page<User> page, Integer age);-->
   <select id="selectPageVo" resultType="User">
   SELECT <include refid="BaseColumns"></include> FROM t_user WHERE age > #
   {age}
   </select>
   ```

   