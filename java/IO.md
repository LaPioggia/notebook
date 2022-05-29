## 文件

1. 作用：用来保存数据

2. 文件流：文件在程序中是以流的形式来操作的![image-20220227154555844](C:\Users\10337\AppData\Roaming\Typora\typora-user-images\image-20220227154555844.png)

3. 常用方法 

   ![image-20220227163133583](C:\Users\10337\AppData\Roaming\Typora\typora-user-images\image-20220227163133583.png)



1. 创建文件对象

   ```java
   new File(String pathname);  //根据路径构造一个File对象
   new File(File parent, String child);  //根据父目录文件+子路径构建
   new File(String parent, String child);  //根据父目录+子路径构建
   ```

2. 创建新文件

   ```java
   file.createNewFile();  //会抛出异常
   ```

3.  获取文件相关信息

   ```java
   file.getName();  //获取文件名
   file.getAbsolutePath();  //获取绝对路径
   file.getParent();  //获取文件父级目录
   file.length();  //获取文件大小（字节）
   file.exists();  //是否存在
   file.isFile();  //是否是文件
   file.isDirectory();  //是否是一个目录
   ```

4. 目录操作和文件删除

   ```java
   file.mkdir();  //创建一级目录
   file.mkdies();  //创建多级目录
   file,delete();  //删除空目录或文件
   ```

   


## IO流

1. 操作数据单位：字节流（8bit，二进制文件），字符流（文本文件）

   1. 字节流直接操作文件本身，字符流在操作是用到缓冲区，通过缓冲区再操作文件，在关闭字符流的时候会强制性将缓冲区中的内容进行输出，若未关闭，缓冲区内容无法输出，不关闭流但想输出时，可使用flush()方法

2. 流向：输入流，输出流

   | 抽象基类 | 字节流       | 字符流 |
   | -------- | ------------ | ------ |
   | 输入流   | InputStream  | Reader |
   | 输出流   | OutputStream | Writer |

3.  角色：节点流，处理流/包装流
   1. 节点流：对一个节点读写数据
   2. 处理流：对一个已存在的流的连接和封装，通过所封装的流的功能调用实现数据读写。采用装饰者模式。<br/>常见处理流：缓冲流、转换流、数据流

## 具体类

1. **InputStream**

   1. 所有字节输入流的超类，该类为抽象类，需通过子类实例化对象。

   2. 常用子类

      ​	![image-20220504163303016](C:\Users\10337\AppData\Roaming\Typora\typora-user-images\image-20220504163303016.png)

      1. FileInputStream：文件输入流
         1. read()：从该输入流读取一个字节的数据。若无输入，则该方法停止；若返回-1，则表示读取完毕
         2. read(bytep[] b)：从该输入流读取b.length字节的数据到字节数组。若读取正常，返回实际读取的字节数；若返回-1，表示读取完毕
         3. 需在最后将流关闭，释放资源
      2. BufferedInputStream：缓冲字节输入流
      3. ObjectInputStream：对象字节输入流

2. OutputStream

   1. FileOutputStream：将数据写入到文件中，如果文件不存在，则创建该文件
      1. new FileOutputStream(filePath)：覆盖原本内容
      2. new FileOutputStream(filePath, true)：追加内容

3. Reader

   1. FileReader：字符流

      ![image-20220508230907205](C:\Users\10337\AppData\Roaming\Typora\typora-user-images\image-20220508230907205.png)

      1. new FileReader(File/String)
      2. read()：每次读取单个字符，返回该字符，若到文件末尾返回-1
      3. read(char[])：批量读取多个字符到数组，返回读取到的字符数，若到文件末尾返回-1

4. Writer

   1. FileWriter

      ![image-20220508231549103](C:\Users\10337\AppData\Roaming\Typora\typora-user-images\image-20220508231549103.png)

      1. new FileWriter(File/String)：覆盖模式，相当于流的指针在首端

      2. new FileWriter(File.String, true)：追加模式，相当于流的指针在尾端

      3. write(int)：写入单个字符

      4. write(char[])：写入指定数组

      5. write(char[], off, len)：写入指定数组的指定部分

      6. write(String)：写入整个字符串

      7. write(string, off, len)：写入字符串的指定部分

         该类使用后，需要关闭(close)或刷新(flush)，否则写入不到

   

s
