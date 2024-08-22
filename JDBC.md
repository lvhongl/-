[TOC]

# JDBC概述

Java DataBase Connectivity，  Java 数据库连接， Java语言操作数据库

* JDBC本质：其实是官方（sun公司）定义的一套操作所有关系型数据库的规则，即接口。各个数据库厂商去实现这套接口，提供数据库驱动jar包。我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动jar包中的实现类。

  ![image-20230215172306812](https://s2.loli.net/2023/07/31/UK4bpsgReWL7SEl.png)

# 快速入门：

步骤：

1. 导入驱动jar包 mysql-connector-java-8.0.w-bin.jar
	1.复制mysql-connector-java-8.0.22-bin.jar到项目的lib目录下
	2.右键-->Add As Library
2. 注册驱动
3. 获取数据库连接对象 Connection
4. 定义sql
5. 获取执行sql语句的对象 Statement
6. 执行sql，接受返回结果
7. 处理结果
8. 释放资源

代码示例：

```java
/**
 * jdbc快速入门
 */
public class Simple01 {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //1. 导入驱动jar包
        //2.注册驱动
        Class.forName("com.mysql.jdbc.Driver");
        //3.获取数据库连接对象
        //Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/DB_JDBC_01", "root", "root1234");
        //Connection conn =  DriverManager.getConnection("jdbc:mysql://localhost/DB_JDBC_01?user=root&password=root1234");
        Connection conn = DriverManager.getConnection("jdbc:mysql:///DB_JDBC_01", "root", "root1234");
        //4.定义sql语句
        String sql = "update account set balance = 500 where id = 1";
        //5.获取执行sql的对象 Statement
        Statement stmt = conn.createStatement();
        //6.执行sql
        int count = stmt.executeUpdate(sql);
        //7.处理结果
        System.out.println(count);
        //8.释放资源
        stmt.close();
        conn.close();
    }
}
```

# 详解各个对象1：

## DriverManager：驱动管理对象

功能：

1. 注册驱动：告诉程序该使用哪一个数据库驱动jar
    static void registerDriver(Driver driver) :注册与给定的驱动程序 DriverManager 。 
    	写代码使用：  Class.forName("com.mysql.jdbc.Driver");
    
    ​	查看源码发现：在com.mysql.jdbc.Driver类中存在静态代码块
    
    ```java
     static {
    	        try {
    	            java.sql.DriverManager.registerDriver(new Driver());
    	        } catch (SQLException E) {
	            throw new RuntimeException("Can't register driver!");
    	        }
    		}
    //注意：mysql5之后的驱动jar包可以省略注册驱动的步骤。
    //8.022数据库驱动程序改为：com.mysql.cj.jdbc.Driver
    ```
2. 获取数据库连接：

   * 方法：

     ```java
     static Connection getConnection(String url, String user, String password) 
     ```

- 参数：

  - url：指定连接的路径

    - 语法：jdbc:mysql://ip地址(域名):端口号/数据库名称

      例子：jdbc:mysql://localhost:3306/db3

    - 细节：如果连接的是本机mysql服务器，并且mysql服务默认端口是3306，则url可以简写为：jdbc:mysql:///数据库名称

    ​		例子：jdbc:mysql:///db7

  - user：用户名

  - password：密码 

## Connection：数据库连接对象

​			1. 功能：

​				1. 获取执行sql 的对象

​					* Statement createStatement()

​					* PreparedStatement prepareStatement(String sql)  

​				2. 管理事务：

​				   * 开启事务：setAutoCommit(boolean autoCommit) ：调用该方法设置参数为false，即开启事务

​					* 提交事务：commit() 

​					* 回滚事务：rollback() 

## Statement：数据库连接对象

1. 执行sql

   * boolean execute(String sql) ：可以执行任意的sql   这个不常用 了解

   * int executeUpdate(String sql) ：

     ​				执行DML（insert、update、delete）语句、

     ​    			执行DDL   (create，alter、drop)语句，很少用，都用sql语句创建。

     ​				返回值：int count = stmt.executeUpdate(sql);

     ​				count是影响的行数，可以通过这个影响的行数判断DML语句是否执行成功 返回值>0

     ​				的则执行成功，反之，则失败。

   * ​	ResultSet executeQuery(String sql)  ：
   
     ​				执行DQL（select)语句			



# 执行DML语句

- account表 添加一条记录

- account表 修改一条记录

- account表 删除一条记录


## 添加一条记录

```java
/**
 * account表
 * 添加一条记录 insert
 *
 * @author KOBE
 */
public class Insert02 {

    public static void main(String[] args) {
        Statement stmt = null;
        Connection conn = null;

        try {
            // 1.注册驱动
            // 2. 定义sql
            String sql = "insert into account values(null,'王五' , 23000)";
            // 3.获取连接数据库的对象 Connection
            conn = DriverManager.getConnection("jdbc:mysql:///DB_JDBC_01", "root", "root1234");
            // 4.获取执行sql的对象 Statement
            stmt = conn.createStatement();
            // 5.执行sql语句 影响行数
            int count = stmt.executeUpdate(sql);
            // 6.处理结果
            System.out.println(count);// 影响行数
            if (count > 0)
                System.out.println("添加成功");
            else {
                System.out.println("添加失败");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // 7 释放资源
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## 		修改一条记录

```java
/**
 * account表
 * 添加一条记录 update
 *
 * @author KOBE
 */
public class Update03 {

    public static void main(String[] args) {
        Statement stmt = null;
        Connection conn = null;

        try {
            // 1.注册驱动
            // 2. 定义sql
            String sql = "update account set balance = 500 where id = 1";
            // 3.获取连接数据库的对象 Connection
            conn = DriverManager.getConnection("jdbc:mysql:///DB_JDBC_01", "root", "root1234");
            // 4.获取执行sql的对象 Statement
            stmt = conn.createStatement();
            // 5.执行sql语句 影响行数
            int count = stmt.executeUpdate(sql);
            // 6.处理结果
            System.out.println(count);// 影响行数
            if (count > 0)
                System.out.println("添加成功");
            else {
                System.out.println("添加失败");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // 7 释放资源
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## 删除一条记录

```java
/**
 * account表
 * 添加一条记录 delete
 *
 * @author KOBE
 */
public class Delete04 {

    public static void main(String[] args) {
        Statement stmt = null;
        Connection conn = null;

        try {
            // 1.注册驱动
            // 2. 定义sql
            String sql = "delete from account  where id = 3";
            // 3.获取连接数据库的对象 Connection
            conn = DriverManager.getConnection("jdbc:mysql:///DB_JDBC_01", "root", "root1234");
            // 4.获取执行sql的对象 Statement
            stmt = conn.createStatement();
            // 5.执行sql语句 影响行数
            int count = stmt.executeUpdate(sql);
            // 6.处理结果
            System.out.println(count);// 影响行数
            if (count > 0)
                System.out.println("删除成功");
            else {
                System.out.println("删除失败");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // 7 释放资源
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

# 执行DDL语句

代码示例：

```java
/**
 * 执行DDL语句
 * 创建表
 *
 * @author KOBE
 */
public class CreateTable05 {
    // DDL:数据定义语言：创建一个表、视图、操作列
    // DML:数据操作语言：对表进行增 删 改。insert delete update
    // DQL:数据查询语言：对表进行查询 select from where
    // DCL:数据控制语言：对数据库进行授权或者控制

    public static void main(String[] args) {
        Statement stmt = null;
        Connection conn = null;
        try {
            // 1.注册驱动
            // 2.定义sql
            String sql = "create table student(id int , name varchar(20))";
            // 3.获取连接数据库的对象 Connection
            conn = DriverManager.getConnection("jdbc:mysql:///DB_JDBC_01", "root", "root1234");
            // 4.获取操作sql的对象 Statement
            stmt = conn.createStatement();
            // 5.执行sql影响的行数
            int count = stmt.executeUpdate(sql);
            // 6.处理结果
            System.out.println(count);// DDL 返回结果就是0 不需要判断

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // 7.释放资源
            // stmt.close();
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## 详解各个对象2：

### ResultSet：结果集对象,封装查询结果

​	boolean next(): 游标向下移动一行，判断当前行是否是最后一行末尾(是否有数据)，

​	如果是最后一行末尾，则返回false，如果不是最后一行末尾则返回true

​			* getXxx(参数):获取数据

​				* Xxx：代表数据类型  如： int getInt() ,	String getString()

​				* 参数：

​					1. int：代表列的编号,从1开始  如： getString(1)   getString(“name”)

​					2. String：代表列名称。 如： getDouble("balance")  getDouble(1)

​			* 注意：

​				* 使用步骤：

​					1. 游标向下移动一行

​					2. 判断是否有数据

​					3. 获取数据

```java
//循环判断游标是否是最后一行末尾。
while(rs.next()){
//获取数据
//6.2 获取数据
int id = rs.getInt(1);
String name = rs.getString("name");
double balance = rs.getDouble(3);
System.out.println(id + "---" + name + "---" + balance);
}
```

# 执行DQL语句：

代码示例：

```java
/**
 * ResultSet
 * 结果集对象,封装查询结果
 *
 * @author KOBE
 */
public class Select06 {

    public static void main(String[] args) {
        Statement stmt = null;
        Connection conn = null;
        ResultSet rs = null;

        try {
            // 1.注册驱动
            // 2.定义sql
            String sql = "select * from account";
            // 3.获取连接数据库的对象 Connection
            conn = DriverManager.getConnection("jdbc:mysql:///DB_JDBC_01", "root", "root1234");
            // 4.获取操作sql的对象 Statement
            stmt = conn.createStatement();
            // 5.执行sql影响的行数
            // int count = stmt.executeUpdate(sql);
            rs = stmt.executeQuery(sql);
            // 6.处理结果
            // 循环判断游标是否是最后一行
            while (rs.next()) {
                // 获取数据
                // 6.1 让光标向下移动一行
                // 6.2获取数据
               /* int id = rs.getInt("id");
                String name = rs.getString("name");
                double balance = rs.getDouble("balance");*/

                int id = rs.getInt(1);
                String name = rs.getString(2);
                double balance = rs.getDouble(3);
                System.out.println(id + "--" + name + "--" + balance);
            }

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // 7.释放资源
            if (rs != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            // stmt.close();
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## 查询练习：

 定义一个方法，查询emp表的数据将其封装为对象，然后装载集合，返回。

​					1. 定义Emp类

​					2. 定义方法 public List**<Emp>** findAll(){}

​					3. 实现方法 select * from emp;

代码示例：

```sql
-- 员工表
CREATE TABLE emp (
  id INT PRIMARY KEY, -- 员工id
  ename VARCHAR(50), -- 员工姓名
  job_id INT, -- 职务id
  mgr INT , -- 上级领导
  joindate DATE, -- 入职日期
  salary DECIMAL(7,2) -- 工资

);
-- 添加员工
INSERT INTO emp(id,ename,job_id,mgr,joindate,salary) VALUES 
(1001,'孙悟空',4,1004,'2000-12-17','8000.00'),
(1002,'卢俊义',3,1006,'2001-02-20','16000.00'),
(1003,'林冲',3,1006,'2001-02-22','12500.00'),
(1004,'唐僧',2,1009,'2001-04-02','29750.00'),
(1005,'李逵',4,1006,'2001-09-28','12500.00'),
(1006,'宋江',2,1009,'2001-05-01','28500.00'),
(1007,'刘备',2,1009,'2001-09-01','24500.00'),
(1008,'猪八戒',4,1004,'2007-04-19','30000.00');

SELECT * FROM emp;
```

```java

/**
 * 封装Emp表数据的实体类
 *
 * @author KOBE
 */
public class Emp {

    private int id;
    private String ename;
    private int job_id;
    private int mgr;
    private Date joinDate;
    private double salary;
    //getter setter
    //toString();
}
```

```java
/**
 * 练习题 查询emp表，
 * 将其封装成对象,然后装载集合，并返回
 *
 * @author KOBE
 */
public class FindAllEmp07 {
    public static void main(String[] args) {
        List<Emp> list = new FindAllEmp07().findAll();
        System.out.println(list);
        System.out.println(list.size());
    }

    /**
     * 查询所有emp对象
     *
     * @return
     * @throws ClassNotFoundException
     */
    public List<Emp> findAll() {
        Statement stmt = null;
        Connection conn = null;
        ResultSet rs = null;
        List<Emp> list = null;

        r
            // 1.注册驱动
            // 2.获取连接
            conn = DriverManager.getConnection("jdbc:mysql:///DB_JDBC_01", "root", "root1234");
            // 3.定义sql语句
            String sql = "select * from emp";
            // 4.获取执行sql语句的对象
            stmt = conn.createStatement();
            // 5.执行sql语句
            rs = stmt.executeQuery(sql);
            // 6.遍历结果集 封装对象 装载集合
            Emp emp = null;
            list = new ArrayList();

            while (rs.next()) {
                int id = rs.getInt("id");
                String ename = rs.getString("ename");
                int job_id = rs.getInt("job_id");
                int mgr = rs.getInt("mgr");
                Date joindate = rs.getDate("joindate");
                double salary = rs.getDouble("salary");
                // 创建对象
                emp = new Emp();

                // 封装对象
                emp.setId(id);
                emp.setEname(ename);
                emp.setJob_id(job_id);
                emp.setMgr(mgr);
                emp.setJoinDate(joindate);

                // 装载集合
                list.add(emp);
            }

        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            // 7.释放资源
            if (rs != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            // stmt.close();
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
        return list;
    }
}
```

# JDBC工具类

##  JDBCUtils

抽取JDBC工具类 ： JDBCUtils

```java
/**
 * 工具类
 *
 * @author KOBE
 *
 */
public class JDBCUtils {

    private static String url;
    private static String user;
    private static String password;
    private static String driver;

    /**
     * 文件读取 只需要读取一次 即可 使用静态代码块
     */
    static {
        try {
            // 1.创建Properties集合类
            Properties pro = new Properties();
            // 2. 加载文件
            // 获取src路径下的文件的方式--->ClassLoader 类加载器
            ClassLoader classLoader = JDBCUtils.class.getClassLoader();
            // URL 统一资源标识定位符
            URL res = classLoader.getResource("jdbc.properties");// 默认是src根目录
            // 获取字符串的路径
            String path = res.getPath();
            System.out.println("path:" + path);

            //相对路径
            pro.load(new FileReader(path));// 路径有中文是识别不出来的
            //绝对路径
            //pro.load(new FileReader("/Users/kobe/Documents/java/develop/Project/4_DataBase/02_JDBC/code/jdbcdemo/src/jdbc.properties"));// 路径有中文是识别不出来的
            //pro.load(new FileReader("E:\\Java\\JDBC_Demo\\src\\jdbc.properties"));
            // 3.获取数据 赋值
            url = pro.getProperty("url");
            user = pro.getProperty("user");
            password = pro.getProperty("password");
            driver = pro.getProperty("driver");// 驱动
            // 4.获取驱动
            Class.forName(driver);
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取连接
     *
     * @return 连接对象
     * @throws SQLException
     */
    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url, user, password);
    }

    /**
     * 释放资源
     *
     * @param stmt
     * @param conn
     */
    public static void close(Statement stmt, Connection conn) {

        if (stmt != null) {
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 释放资源
     *
     * @param stmt
     * @param conn
     * @param rs
     */
    public static void close(ResultSet rs, Statement stmt, Connection conn) {
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        // stmt.close();
        if (stmt != null) {
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

}
```

## 用户登入练习：

​		* 需求：

​			1. 通过键盘录入用户名和密码

​			2. 判断用户是否登录成功

​				* select * from user where username = "" and password = "";

​				* 如果这个sql有查询结果，则成功，反之，则失败

 代码示例：

```sql
-- 创建数据库表 user

r
```

```java
/**
 * 练习： 需求：
 * 1. 通过键盘录入用户名和密码
 * 2. 判断用户是否登录成功
 *
 * @author KOBE
 */
public class Login08 {
    public static void main(String[] args) {
        // 1.键盘录入，接受用户名和密码
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入用户名：");
        String username = sc.nextLine();
        System.out.println("请输入密码：");
        String password = sc.nextLine();
        // 2.调用方法
        boolean flag = new Login08().login(username, password);
        // 3.判断结果，输出不同语句
        if (flag)
            System.out.println("登录成功！");
        else
            System.out.println("用户名或密码错误！");

    }

    /**
     * 登入方法
     *
     * @param username
     * @param password
     * @return
     */
    public boolean login(String username, String password) {
        if (username == null || password == null)
            return false;
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        // 和数据库进行验证
        try {
            // 1.建立连接
            conn = JDBCUtils.getConnection();
            // 2.定义sql
            String sql = "select * from user where username  ='" + username + "' and password= '" + password + "' ";
            System.out.println("sql:" + sql);
            // select * from user where username ='s' and password = 'a' or 'a' = 'a'
            // 3.获取执行sql的对象
            stmt = conn.createStatement();
            // 4.执行查询
            rs = stmt.executeQuery(sql);
            // 5.判断
            return rs.next();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.close(rs, stmt, conn);
        }
        return false;
    }
}
```

# SQL注入问题

## PreparedStatement

​	PreparedStatement：执行sql的对象

​			1. SQL注入问题：在拼接sql时，有一些sql的特殊关键字参与字符串的拼接。会造成安全性问题

​				1. 输入用户随便，输入密码：a' or 'a' = 'a

​				2. sql：select * from user where username = 'fhdsjkf' and password = 'a'  or 'a' = 'a' 

 					由于前面的用户名和密码不匹配结果是false 大事后面的or 是true，结果就是true

​			2. 解决sql注入问题：使用PreparedStatement对象来解决

​			3. 预编译的SQL：参数使用?作为占位符

​			4. 步骤：

​				1. 导入驱动jar包 mysql-connector-java-8.0.22-bin.jar

​				2. 注册驱动

​				3. 获取数据库连接对象 Connection

​				4. 定义sql

​                      注意：sql的参数使用？作为占位符。

 								如：select * from user where username = ? and password = ?;

​				5. 获取执行sql语句的对象 PreparedStatement  Connection.prepareStatement(String sql) 

​				6. 给？赋值：

​					* 方法： setXxx(参数1,参数2)

​						* 参数1：？的位置编号 从1 开始

​						* 参数2：？的值

​				7. 执行sql，接受返回结果，不需要传递sql语句

​				8. 处理结果

​				9. 释放资源

代码示例：

```Java
/**
 * 练习： 需求：
 * 1. 通过键盘录入用户名和密码
 * 2. 判断用户是否登录成功
 * 解决sql注入问题
 *
 * @author KOBE
 */
public class Login09 {
    public static void main(String[] args) {
        // 1.键盘录入，接受用户名和密码
        Scanner sc = new Scanner(System.in);
        System.out.println("请输入用户名：");
        String username = sc.nextLine();
        System.out.println("请输入密码：");
        String password = sc.nextLine();
        // 2.调用方法
        boolean flag = new Login09().login(username, password);
        // 3.判断结果，输出不同语句
        if (flag)
            System.out.println("登录成功！");
        else
            System.out.println("用户名或密码错误！");

    }

    /**
     * 登入方法
     *
     * @param username
     * @param password
     * @return
     */
    public boolean login(String username, String password) {
        if (username == null || password == null)
            return false;
        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        // 和数据库进行验证
        try {
            // 1.建立连接
            conn = JDBCUtils.getConnection();
            // 2.定义sql
            String sql = "select * from user where username  = ? and password= ? ";
            // 3.获取执行sql的对象
            pstmt = conn.prepareStatement(sql);
            // 3.1.依次给 ？号赋值
            pstmt.setString(1, username);
            pstmt.setString(2, password);
            // 4.执行查询
            rs = pstmt.executeQuery();
            // 5.判断
            return rs.next();
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            JDBCUtils.close(rs, pstmt, conn);
        }
        return false;
    }
}
```

# JDBC控制事务：

## 概念：

 1.  事务：

     一个包含多个步骤的业务操作，如果这个业务操作被事务管理，则这多个步骤要么同时成功，要么同时失败。

​	2. 操作：

​		1. 开启事务

​		2. 提交事务

​		3. 回滚事务

​	3. 使用Connection对象来管理事务

​		* 开启事务：setAutoCommit(boolean autoCommit) ：调用该方法设置参数为false，即开启事务

​			* 在执行sql之前开启事务

​		* 提交事务：commit() 

​			* 当所有sql都执行完提交事务

​		* 回滚事务：rollback() 

​			* 在catch中回滚事务

代码示例：

```java
package com.codingfuture.simple;

import com.codingfuture.util.JDBCUtils;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

/**
 * 事务
 * 案例：张三给李四转钱
 *
 * @author KOBE
 */
public class Transaction {

    public static void main(String[] args) {
        Connection conn = null;
        PreparedStatement pstmt1 = null;
        PreparedStatement pstmt2 = null;

        try {
            // 1.数据库连接
            conn = JDBCUtils.getConnection();
            // 开启事务
            conn.setAutoCommit(false);

            // 2.定义sql语句
            // 2.1张三 转钱500
            String sql1 = "update account set balance = balance - ? where id = ?";
            // 2.2李四 得到500
            String sql2 = "update account set balance = balance + ? where id = ?";
            // 3.获取执行sql语句对象
            pstmt1 = conn.prepareStatement(sql1);
            pstmt2 = conn.prepareStatement(sql2);
            // 4.设置参数
            pstmt1.setDouble(1, 500);
            pstmt1.setInt(2, 1);
          
            pstmt2.setDouble(1, 500);
            pstmt2.setInt(2, 2);
            // 5.执行sql语句crS
            pstmt1.executeUpdate();

            int a = 3 / 0;// 会有异常

            pstmt2.executeUpdate();
            // 提交
            conn.commit();
        } catch (Exception e) {
            if (conn != null) {
                System.out.println("异常");
                try {
                    conn.rollback();//事务回滚
                } catch (SQLException ex) {
                    ex.printStackTrace();
                }
            }
        } finally {
            JDBCUtils.close(pstmt1, conn);
            JDBCUtils.close(pstmt2, null);
        }
    }
}
```

# 数据库连接池

## 概念：

1. ​    其实就是一个容器(集合)，存放数据库连接的容器。

   ​    当系统初始化好后，容器被创建，容器中会申请一些连接对象，当用户来访问数据库时，从容器中获取	连接对象，

   ​	用户访问完之后，会将连接对象归还给容器。

![image-20230216162331338](https://s2.loli.net/2023/07/31/VE23ICGpkAS8RWL.png)

![image-20230216162435750](https://s2.loli.net/2023/07/31/SYPh9uVesgwxWrK.png)

### 好处：

1. 节约资源
2. 用户访问高效

## 实现：

### 标准接口：

标准接口：DataSource   javax.sql包下的，这是官方提供的，提供了获取数据库连接的方法

* 方法

  获取连接：getConnection()

  归还连接：Connection.close()。
  
  ​	如果连接对象Connection是从连接池中获取的，那么调用Connection.close()方法，
  
  ​	则不会再关闭连接了。而是归还连接

### 数据库厂商实现

一般我们不去实现它，有数据库厂商来实现

* C3P0：数据库连接池技术，相对比较老旧
* Druid：数据库连接池实现技术，由阿里巴巴提供的

## C3P0

C3P0数据库连接池技术

步骤：

1. 导入jar包 (两个) c3p0-0.9.5.2.jar和mchange-commons-java-0.2.12.jar ，

   ​	不要忘记导入数据库驱动jar包，mysql-connector-java-8.0.22-bin

2. 定义配置文件：

   ​	名称： c3p0.properties 或者 c3p0-config.xml，二者都可以

   ​	路径：直接将文件放在src目录下即可。

3. 创建核心对象 数据库连接池对象 ComboPooledDataSource

4. 获取连接：getConnection

代码示例：

```java
public class C3P0Demo {
    public static void main1(String[] args) throws SQLException {
        //1.创建数据库连接池对象
        DataSource ds = new ComboPooledDataSource();
        //2.获取连接对象
        Connection conn = ds.getConnection();
        //3.输出打印
        System.out.println(conn);
    }

    public static void main2(String[] args) throws SQLException {
        //1.获取数据库连接池对象  使用默认配置
        DataSource ds = new ComboPooledDataSource();
        //3.测试获取11个连接池对象，超过2秒超时
        for (int i = 1; i <= 10; i++) {
            //2.获取连接
            Connection conn = ds.getConnection();
            System.out.println(i + ":" + conn);
            if (i == 5) {//
                conn.close();//归还到连接池中
            }
            //conn.close();
            //c3p0默认不会把连接 回收到连接池中，需要手动归还close
        }
    }

    public static void main(String[] args) throws SQLException {
        //1.1获取数据库连接池对象  使用指定名称配置
        DataSource ds = new ComboPooledDataSource("otherc3p0");
        //2.获取连接
        for (int i = 1; i <= 9; i++) {
            Connection conn = ds.getConnection();
            System.out.println(i + " :: " + conn);
        }
    }


}

```

##  Druid

 Druid：数据库连接池实现技术，由阿里巴巴提供的

步骤：

1. 导入jar包 druid-1.0.9.jar
2. 定义配置文件：
   * 是properties形式的
   * 可以叫任意名称，可以放在任意目录下
3. 加载配置文件 Properties
4. 获取数据库连接池对象：通过工厂来来获取  DruidDataSourceFactory
5. 获取连接：getConnection

代码示例：

```java
/**
 * Druid基本使用
 */
public class Simple01 {
    public static void main(String[] args) throws Exception {
        //1.导入jar包
        //2.导入配置文件
        //3.加载配置文件
        Properties pro = new Properties();
        InputStream is = Simple01.class.getClassLoader().getResourceAsStream("druid.properties");
        pro.load(is);
        //4.获取连接池对象
        DataSource ds = DruidDataSourceFactory.createDataSource(pro);
        //5.获取连接
        Connection conn = ds.getConnection();
        //
        System.out.println(conn);
    }
}
```

# 定义工具类

1. 定义一个类 JDBCUtils
2. 提供静态代码块加载配置文件，初始化连接池对象
3. 提供方法
   * 获取连接方法：通过数据库连接池获取连接
   * 释放资源
   * 获取连接池的方法

代码示例：

```java
package com.util;


import com.alibaba.druid.pool.DruidDataSourceFactory;

import javax.sql.DataSource;
import java.io.InputStream;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

/**
 * 连接池JDBCUtils工具类
 */
public class JDBCUtils {

    //1.定义成员变量 DataSource
    private static DataSource ds;

    static {
        try {
            //1.加载配置文件
            Properties pro = new Properties();
            InputStream is = JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties");
            pro.load(is);
            //2.获取DateSource
            ds = DruidDataSourceFactory.createDataSource(pro);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取连接
     */
    public static Connection getConnections() throws SQLException {
        return ds.getConnection();
    }


    /**
     * 释放资源
     *
     * @param stmt
     * @param conn
     */
    public static void close(Statement stmt, Connection conn) {
        close(null, stmt, conn);
    }

    /**
     * 释放资源
     *
     * @param rs
     * @param stmt
     * @param conn
     */
    public static void close(ResultSet rs, Statement stmt, Connection conn) {
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if (stmt != null) {
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if (conn != null) {
            try {
                conn.close();//归还连接
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```





