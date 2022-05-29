## 简介

![img](https://bkimg.cdn.bcebos.com/pic/63d0f703918fa0eccc4a0d17299759ee3d6ddb1f?x-bce-process=image/watermark,image_d2F0ZXIvYmFpa2U4MA==,g_7,xp_5,yp_5/format,f_auto)

1. 轻量级javaee框架
2. 主要特定为IOC和AOP

## IOC（控制反转）

通过new来创建会增加代码之间的耦合度，利用IOC可降低耦合度

获取实例步骤：解析xml，通过反射获取对象并实例化对象

### bean管理

1. 内容：创建对象、注入属性

2. 实现方式

   1. xml配置文件

      **创建对象**

      ```xml
      <bean id="user" class="com.spring5.User"></bean>
      ```

      id：唯一标识

      class：类全路径（包类路径）

      *创建对象时，默认执行无参构造*

      **注入属性**

      1. DI（依赖注入）

         1. set

            ```xml
            <property name="bname" value="yiyi"></property>
            ```

            name：类中属性名称

            value：向属性中注入的值

            *写在bean标签内*

         2. 有参构造

            ```xml
            <constructor-arg name="bname" value="yiyi"></constructor-arg>
            ```

            必须对全部属性进行赋值

         3. p名称空间注入

         4. 其他

            空值：

            ```xml
            <property name="bname">
                <null/>
            </property>
            ```

            特殊符号

            ```xml
            <property name="bname">
                <value><![CDATA[<<yiyi>>]]></value>
            </property>
            ```

            也可使用转义

      2. 注入外部bean：ref

      3. 内部bean和级联赋值：ref、property里嵌套bean

   2. 注解
   
      **创建对象**
   
      1. @Component：普通组件
      2. @Service
      3. @Controller
      4. @Repository
   
      **属性注入**
   
      1. @Autowired：根据属性类型自动装配
      2. @Qualifier：根据属性名称注入，需要和@Autowired一起使用
      3. @Resource：皆可

### bean分类

1. 普通bean
2. 工厂bean：配置文件中的bean类型可以和返回类型不同

### bean作用域

1. 默认情况下，bean为单实例对象
2. scope
   1. 默认值：singleton——单实例对象，加载spring配置文件时创建
   2. prototype——多实例对象，调用getBean()时创建对象

### 生命周期

## AOP

### 底层原理

1. 使用动态代理方式

2. 有接口时，使用JDK动态代理

   创建接口实现类的代理对象，增强类的方法

3. 无接口时，使用CGLIB动态代理

   创建当前类子类的代理对象

## AspectJ

### 实现方式

1. xml
2. 注解

### 切入点表达式

1. 语法结构

   execution(\[权限修饰符\]\[返回类型\]\[类全路径\]\[方法名称\](\[参数列表\]))

## 事务

### 概念

1. 概念：数据库操作的最基本单元，逻辑上一组操作，全部成功或全部失败
2. 典型场景：银行转账
3. 特性（ACID）
   1. 原子性
   2. 一致性
   3. 隔离性
   4. 持久性

### spring事务操作

1. 方式
   1. 编程式
   2. 声明式，底层使用AOP
      1. 注解
      2. xml
2. spring事务管理api，提供一个接口，针对不同的框架提供不同的实现类
3. 事务参数
   1. propagation：事务传播行为
      1. REQUIRED
      2. REQUIRED_NEW
   2. ioslation：事务隔离级别
      1. 读问题：脏读、不可重复读、幻读
   3. timeout：超时时间
   4. readOnly：是否只读
   5. rollbackFor：回滚
   6. noRollbackFor：不回滚

## Webflux

非阻塞框架，核心基于reactor的相关API实现

### 响应式编程





