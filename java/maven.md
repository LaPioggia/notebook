### 一、概念
项目管理工具，对软件项目提供构建和依赖管理
### 二、核心特性
1. 项目设置遵循统一的规则，保证不同开发环境的兼容性
2. 强大的依赖管理，项目依赖组建自动下载、自动更新
3. 可扩展的插件机制，使用简单，功能丰富
### 三、Maven坐标
1. GroupId：机构或团体的英文，采用“逆向域名”形式书写
2. ArtifactId：项目名称，说明其用途
3. Version：版本号，一般采用“版本+单词”形式
### 四、Maven项目标准结构
![maven项目结构](https://github.com/LaPioggia/notebook/blob/gh-pages/picture/maven项目标准结构.png)
|目录|用途|
|---|---|
|${basedir}|根目录|
|${basedir}/src/main/java|java源代码目录|
|${basedir}/src/main/resources|资源目录，保存配置文件、静态图片等|
|${basedir}/src/test/java|测试类的源代码|
|${basedir}/src/test/resources|测试时需要使用的资源文件|
|${basedir}/target|项目输出的目录，用于存储jar、war文件|
|${basedir}/target/classes|字节码的编译输出目录|
|${basedir}/pom.xml|项目对象模型文件|
### 五、依赖管理
1. Maven利用dependency（依赖）自动下载、管理第三方Jar
2. 在pom.xml文件中配置项目依赖的第三方组件
3. maven自动将依赖从远程仓库下载至本地仓库，并在工程中引用
``` xml
<dependencies>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.47</version>
    </dependency>
</dependencies>
```
### 六、项目打包
通过插件实现：maven-assembly-plugin
``` xml
<build>
    <plugins>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-assembly-plugin</artifactId> 
        <version>2.5.5</version>
        <configuration>
            <archive>
                <manifest>
                    <mainClass>程序入口</mainClass>
                </manifest>
            </archive>
            <descriptorRefs>
                <descriptorRef>jar-with-dependencies</descriptorRef>
            </descriptorRefs>
        </configuration>
    </plugins>
</build>
```