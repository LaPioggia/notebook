### 一、 简介
1. 持久层框架
2. 支持自定义SQL、存储过程及高级映射
3. 免除了几乎所有的JDBC代码以及设置参数和获取结果集的工作
4. 可通过简单的XML或注解来配置和映射原始类型、接口和Java POJO到数据库记录
### 二、 使用
1. 利用maven安装
    将下述依赖置于pom.xml中   
    ```xml
    <dependency>
       <groupId>org.mybatis</groupId>
       <artifactId>mybatis</artifactId>
       <version>x.x.x</version>
    </dependency>
    ```
2. 构建SqlSessionFactory
    SqlSessionFactory可由SqlSessionFactoruBuilder获得。<br/>SqlSessionFactoryBuilder从XML配置文件或一个预先配置的Configuration实例构建获得SqlSessoionFactory实例。<br/>构建SqlSessionFactory实例如下（使用类路径下资源文件进行配置）：    
    ```xml
    String resource = "org/mybatis/example/mybatis-config.xml";
    InputStream inputStream = Resources.getResourceAsStream(resource);
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
    ```
      xml配置如下（mybatis-config.xml）：    
    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE configuration
       PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
       "http://mybatis.org/dtd/mybatis-3-config.dtd">
    <configuration>
       <environments default="development">
          <environment id="development">
            <!--事务管理-->
            <transactionManager type="JDBC"/>
            <!--数据源-->
            <dataSource type="POOLED">
               <property name="driver" value="${driver}"/>
               <property name="url" value="${url}"/>
               <property name="username" value="${username}"/>
               <property name="password" value="${password}"/>
           </dataSource>
         </environment>
       </environments>
       <!--映射器-->
       <mappers>
          <mapper resource="org/mybatis/example/BlogMapper.xml"/>
       </mappers>
    </configuration>
    ```
3. 获取SqlSession
    SqlSession提供了在数据库执行SQL命令所需的所有方法。
    
    ```java
    try (SqlSession session = sqlSessionFactory.openSession()) {
       BlogMapper mapper = session.getMapper(BlogMapper.class);
       Blog blog = mapper.selectBlog(101);
    }
    ```
    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE mapper
       PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
       "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace="org.mybatis.example.BlogMapper">
       <select id="selectBlog" resultType="Blog">
          select * from Blog where id = #{id}
       </select>
    </mapper>
    ```
### 三、 配置
1. 属性<br/>
    可在外部进行配置，并进行动态替换。<br/>
    既可在典型java属性文件中，也可在properties元素的子元素中设置。
   ```xml
   <properties resource="org/mybatis/example/config.properties">
   <property name="username" value="dev_user"/>
   <property name="password" value="F2Fa3!33TYyg"/>
    </properties>
   ```
   ```xml
   <dataSource type="POOLED">
     <property name="driver" value="${driver}"/>
     <property name="url" value="${url}"/>
     <property name="username" value="${username}"/>
     <property name="password" value="${password}"/>
   </dataSource>
   ```
   若一个属性在不只一个地方进行了配置，通过方法参数传递的属性具有最高优先级，resource/url属性中指定的配置文件次之，最低优先级为properties元素中指定的属性。  
2. 类型别名<br/>
   可为java类型设置一个别名，仅用于XML配置：
   ```xml
   <typeAliases>
      <typeAlias alias="Author" type="domain.blog.Author"/>
   </typeAliases>
   ```
    也可指定包名：
   

   ```xml
   <typeAliases>
      <package name="domain.blog"/>
   </typeAliases>
   ```
     ```java
   @Alias("author")
   public class Author {
       ...
   }
     ```
   mybatis会在包名下搜索需要的java bean，每一个在包domain.blog中的java bean，在没有注解的情况下，会使用bean的首字母小写的非限定类名作为别名，若有注解，则别名为其注解值。
3. 类型处理器（-）
     Mybatis在设置预处理语句中的参数或从结果集中取出一个值时，都会用类型处理器将获取到的值以合适的方式转换成java类型。
  4. 重写/创建类型处理器（-）
     可以重写已有的类型处理器或创建自己的类型处理器来处理不支持或非标准的类型，（可选：映射到一个JDBC类型）<br/>
     具体做法:
     
     1. 实现org.apache.ibatis.type.TypeHandler接口；
     2. 继承org.apache.ibatis.type.BaseTypeHandler。
     Mybatis不会通过检测数据库元信息来决定使用哪种类型，Mybatis直到语句被执行时才清楚数据类型，必须在参数和结果映射中指明字段类型，以使其能够绑定到正确的类型处理器中。<br/>通过类型处理器的泛型，Mybatis可以得知该类型处理器处理的Java类型，此行为可以通过两种方法改变：
     1. 在类型处理器的配置元素（typeHandler）上增加一个javaType属性（javaType="String"）；
     2. 在类型处理器的类上增加一个@MappedTypes注解，指定与其关联的java类型列表。（若在javaType属性中同时指定，则注解上的配置将被忽略）
     可通过两种方式指定关联的JDBC类型：
     1. 在类型处理器的配置元素上增加一个jdbcType属性（jdbcType="VARCHAR"）;
     2. 在类型处理器的类上增加一个@MappedJdbcTypes注解，指定与其关联的JDBC类型列表。（若在jdbcType属性中同时指定，则注解上的配置将被忽略）
  2. 枚举类型处理（-）
     会处理任意继承了Enum的类
4. 对象工厂（-）
    每次Mybatis创建结果对象的实例时，都会使用对象工厂实例来完成实例化工作。<br/>默认的对象工厂仅仅是实例化目标类，如果想覆盖对象工厂的默认行为，可创建自己的对象工厂   
5. 插件
    Mybatis允许在映射语句执行过程中的某一点进行拦截调用    
6. 环境配置
    Mybatis可配置多个环境，每个SqlSessionFactory实例只能选择一个环境。<br/>SqlSessionFactoryBuilder可接受环境配置参数：    
    ```java
    SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment);
    SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment, properties);
    ```
     若忽略环境参数，则加载默认环境：    
    ```java
    SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader);
    SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, properties);
    ```
    environments元素定义了如何配置环境：    
    ```xml
    <environments default="development">
       <environment id="development">
          <transactionManager type="JDBC">
            <property name="..." value="..."/>
          </transactionManager>
          <dataSource type="POOLED">
            <property name="driver" value="${driver}"/>
            <property name="url" value="${url}"/>
            <property name="username" value="${username}"/>
            <property name="password" value="${password}"/>
          </dataSource>
       </environment>
    </environments
    ```
      1. 事务管理器 
         type="[JDBC|MANAGED]"<br/>JDBC：直接使用JDBC的回滚和提交，依赖从数据源获取的连接来管理事务作用域；<br/>MANAGED：从不回滚或提交一个连接，而是让容器来管理整个事务的生命周期，默认会关闭连接，可通过设置closeConnection来改变：   
         ```xml
         <transactionManager type="MANAGED">
           <property name="closeConnection" value="false"/>
         </transactionManager>
         ```
         spring+mybatis，无需配置事务管理器，spring模块会使用自带的管理器来覆盖之前的配置。    
      2. 数据源    
         dataSource元素使用标准JDBC数据源接口来配置JDBC连接对象的资源。
### 四、 XML映射
1. select
   ```xml
   <select id="selectPerson" parameterType="int" resultType="hashmap">
      SELECT * FROM PERSON WHERE ID = #{id}
   </select>
   ```
   属性
   |属性|描述|默认值|
   |---|---|---|
   |id|命名空间中唯一标识符，可被用来引用这条语句|
   |parameterType|（可选）将会传入参数的类全限定名或别名，Mybatis可通过类处理器推断具体传入语句的参数|unset|
   |resultType|期望这条语句返回结果的类全限定名或别名，若返回的是集合，则应设置为集合包含的类型。*resultType和resultMap只能用一个*||
   |resultMap|对外部resultMap的命名引用||
   |flushCache|值为true时，||
   ||||
   ||||
   ||||
### 五、 缓存
1. 启用缓存
   默认情况下，只启用本地会话缓存，仅对一个会话中的数据进行缓存。<br/>启用全局二级缓存，需要在SQL映射文件中增加：
   ```xml
   <cache/>
   ```
   效果：
   1. 映射语句文件中的所有select语句的结果将被缓存；
   2. 映射语句文件中的所以insert,update,delete语句将刷新缓存；
   3. 缓存使用最近最少使用算法（LRU）来清除不需要的缓存；
   4. 缓存不会定时刷新；
   5. 会保存列表或对象的1024个引用；
   6. 会被视为读/写缓存，获取到的对象非共享
   属性可通过cache元素的属性更改
   ```xml
   <cache
      eviction="FIFO"
      flushInterval="60000"
      size="512"
      readOnly="true"/>
   ```
2. 自定义缓存

### 六、 动态SQL
1. if
   ```xml
   <select id="findActiveBlogLike"
      resultType="Blog">
      SELECT * FROM BLOG WHERE state = ‘ACTIVE’
      <if test="title != null">
         AND title like #{title}
      </if>
      <if test="author != null and author.name != null">
         AND author_name like #{author.name}
      </if>
   </select>
   ```
2. choose
   ```xml
   <select id="findActiveBlogLike"
      resultType="Blog">
      SELECT * FROM BLOG WHERE state = ‘ACTIVE’
      <choose>
         <when test="title != null">
            AND title like #{title}
         </when>
         <when test="author != null and author.name != null">
            AND author_name like #{author.name}
         </when>
         <otherwise>
            AND featured = 1
         </otherwise>
      </choose>
   </select>
   ```
   *where只会在子元素返回内容时才插入where子句，若子句开头为and或or，也会将其去除*
3. trim
4. foreach
   ```xml
   <select id="selectPostIn" resultType="domain.blog.Post">
      SELECT *
      FROM POST P
      WHERE ID in
      <foreach item="item" index="index" collection="list"
         open="(" separator="," close=")">
         #{item}
      </foreach>
   </select>
   ```
5. bind
   运行在OGNL表达式以外创建一个变量，并将其绑定到当前上下文
   ```xml
   <select id="selectBlogsLike" resultType="Blog">
      <bind name="pattern" value="'%' + _parameter.getTitle() + '%'" />
      SELECT * FROM BLOG
      WHERE title LIKE #{pattern}
   </select>
   ```

### 七、 API


### 八、 日志
mybatis通过内置的日志工厂实现日志功能。<br/>在应用服务器的类路径中已包含Common Logging时，将会忽略原有配置，将Common Logging作为日志工具。