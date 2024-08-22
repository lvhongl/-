[TOC]

# Object

## Object类型的概述

1、Object类是所有类型的顶层父类，所有类型的直接或者间接的父类；所有的类型中都含有Object类中的所   有方法。
​2、随意定义一个类型，不手动显式定义其父类，那么这个类的父类就是Object类
​3、Object类的构造方法：Object()
 			1、可以自己创建对象
 			2、让子类访问，所有子类都会直接或者间接的访问到这个顶层父类的构造方法
 			3、Object类在执行构造方法时，不去访问自己的父类，因为没有父类了

## toString方法

1. 返回当前对象的字符串表示

2. 对象返回一个地址值的字符串，没有什么意义

3. 最终操作：不需要手动重写，可以直接使用快捷键生成：Alt+Insert 

4. 使用打印语句打印一个对象，其实打印的就是这个对象的toString内容

## equals

1、用于比较两个对象是否相等的方法，比较的就是“调用者”和“参数”这两个对象

 boolean equals(Object obj)

2、在Object类型中，是比较两个对象的地址值是否相同。

3、实际生活中，比较两个对象的内存地址，没有什么意义。因此在自定义的子类中，都要重写这个方法。

​		按照自定义的方式比较两个对象是否相同

4、重写原则：

 一般比较两个对象中的所有属性，是否全部相同，

<比如两个对象的age值相同，那么两个对象就相同>

代码示例：

```java
public class Simple04 {
    public static void main(String[] args) {
        Cat cat1 = new Cat();
        Cat cat2 = new Cat();
        System.out.println(cat1.equals(cat2));//true

    }
}

class Cat {
    int age = 23;

    @Override
    public boolean equals(Object o) {
        if (this == o)
            return true;
        if (o == null || getClass() != o.getClass())
            return false;
        Cat cat = (Cat) o;
        return age == cat.age;
    }
}
```

## ==和equals方法的区别

1、==和equals都是用于比较数据是否相等的方式
		2、不同点：
 			1、比较内容的不同：
 	 			==可以比较任意数据类型，既可以比较基本数据类型，也可以比较引用数据类型
 	 			equals方法只能比较引用数据类型
 			2、比较规则不同：
 	 			==在比较基本类型的时候，比较的是数据的值；比较引用类型时，比较的是地址值
 	 			equals方法在重写之前，比较的是两个对象的地址值；在重写之后，比较的属性值
     		   也就是类里面的属性值的大小。

# 内部类

## 内部类的概述

1. 定义在内部的类，就是内部类。可以定义在方法的内部，可以定义在类的内部。

2. 根据定义的位置不同，可以分为：

   *  成员内部类

     *  普通的成员内部类
     *  私有的成员内部类
     *  静态的成员内部类

   *  局部内部类

   * 匿名内部类

     

## 普通的成员内部类

1、定义在成员位置上的类，就是成员内部类
		2、定义格式：
  	class 内部类类名 {
    	内部类成员
		}
	3、成员内部类的说明：
 	1、内部类可以直接访问外部类的所有成员，包括私有成员
 	2、外部类访问内部类的成员，必须先创建内部类的对象
 	3、在外部类以外，创建内部类的对象的格式： 	
 	 	外部类名.内部类名 内部类对象名 = new 外部类名().new 内部类名();

代码示例：

```java
package cn.lesson.simple1;

import java.security.KeyStore;

/**
 * 普通成员内部类
 */
public class Simple01 {
    public static void main(String[] args) {
        Body.Heart bh = new Body().new Heart();
        bh.show();
        bh.beats = 1;

        Body b = new Body();
        b.test();
    }
}

//类嵌套类
class Body {
    private double hight = 149.0;

    class Heart {
        int beats = 80;

        public void show() {
            System.out.println(hight + "..." + beats + "...扑通扑通的跳");
        }
    }

    public void test() {
        Heart h = new Heart();
        h.show();
    }
}
```

## 私有的成员内部类

1、也是一个成员内部类，在成员内部类前面加上一个private关键字
2、访问方式说明：
 	1、在外部类以外，不能直接访问外部类中的私有成员内部类
 	2、可以在外部类中，定义一个访问私有内部类的公有方法，让外界可以调用公有方法，间接的访问外部类中的私有成员内部类。

代码示例：

```java
public class Simple02 {
    public static void main(String[] args) {
        Body2 b = new Body2();
        //Body.Shen bs = b.new Shen(); 访问不到私有内部类Shen
        b.zouShen();//可以访问到
    }
}
class Body2 {
    private double hight = 149.0;

    private class Shen {
        int age = 40;

        public void show() {
            System.out.println("该走心的时候别找我..." + hight + "..." + age);
        }
    }

    public void zouShen() {
        Shen s = new Shen();
        s.show();
    }
}
```

## 静态的成员内部类

1、也是一个成员内部类，在成员内部类前面加上一个static关键字
2、访问特点：
 	1、成员内部类是外部类的静态成员，所以可以通过外部类名.内部类名的方式直接访问，而不需要创建外部类的对象
 	2、静态内部类中的非静态成员，需要将所在的内部类对象创建出来之后，才能被调用。
 	3、总结：一个类是否需要创建对象，不取决于该类本身是否是静态，而取决于要访问		的成员是否是静态
3、静态成员内部类的对象创建格式： 	
 	外部类名.内部类名 内部类对象名 = new 外部类名.内部类名();

代码示例：

```java
package cn.lesson.simple4;

import jdk.internal.dynalink.beans.StaticClass;

public class Simple03 {

    public static void main(String[] args) {
        Body3.Gan bg = new Body3.Gan();
        bg.show();//可以访问到
        System.out.println(bg.color);//可以访问到
        System.out.println(Body3.Gan.name);//可以访问到

    }
}

class Body3 {
    private double hight = 149.0;

    static class Gan {
        String color = "black";
        static String name = "tom";

        public void show() {
            Body3 b = new Body3();
            System.out.println(b.hight + "..." + color + "求你了哥，放过我吧别喝了");
        }
    }

    public static void test() {
    }
}
```

## 匿名内部类【常用】

1、没有名字的内部类
2、匿名内部类的使用前提：
 	匿名类：继承一个类
 	匿名类：实现一个接口
3、格式：
 	new 父类类名或者接口名() {
    	父类方法的重写或者是接口内容的实现
}
4、本质：创建了一个类的子类对象、接口的实现类对象

代码示例：

```java

public class Simple03 {
    public static void main(String[] args) {
        IPlay ipi = new IPlayImpl();
        ipi.playGame();

        IPlay ip = new IPlay() {
            @Override
            public void playGame() {
                System.out.println("匿名内部类");
            }
        };
        ip.playGame();

    }
}

interface IPlay {
    public abstract void playGame();
}

class IPlayImpl implements IPlay {
    @Override
    public void playGame() {
        System.out.println("玩游戏");
    }
}
```



# String

## 概述

1. String就是字符串类型，属于java.lang包，不需要导包
2. 字符串字面值属于常量，存储在方法区的常量池中。
3. String类型是一个常量，在创建之后就无法更改（是一个不可变的字符序列）。

4. 不可变的原因是String类型只提供了构造方法，没有提供set方法，因此只能在创建对象的时候，初始化成员变量，将来对象创建完成之后，无法通过方法来修改。

代码示例：

```java
public class Simple01 {
    public static void main(String[] args) {
        String str2 = "abc";
        System.out.println(str2);//abc
        str2 = "xyz";
        System.out.println(str2);//xyz
    }
}
```

## String类型的构造方法

1、String()：创建一个空字符串
2、String(String original)：创建参数字符串的一个副本（字符串是在常量池中，构造方法创建的字符串是在堆内存中）
3、String(byte[] arr)：将一个字节数组转成一个字符串
 	将我们不认识的字节数组，转成了我们认识的字符串，过程叫做【解码】
 	查询的是当前平台默认的编码表
4、String(byte[] arr, int offset, int length)：将字节数组的一部分转成字符串
5、String(char[] arr)：将字符数组转成字符串

既不是编码，也不是解码，只不过是把字符串成了串
6、String(char[] arr, int offset, int length)：将字符数组的一部分转成字符串

代码示例：

```java
public class Simple02 {

    public static void main(String[] args) {
        //test1_空参构造();
        //test2_原始字符串转换();
        //test3_字节数组转字符串();
        test4_字符数组转成字符串();
    }

    private static void test4_字符数组转成字符串() {
        char[] arr = {'a', 'b', 'X', 'Y'};
        String str1 = new String(arr);
        System.out.println(str1);//abXY  只是组成字符串而已

        //将字符 数组的一部分转成字符串，
        String str2 = new String(arr, 1, 2);
        System.out.println(str2);//bX
    }

    private static void test3_字节数组转字符串() {
        byte[] arr = {98, 49, 65, 66, 97, 98};
        //将整个字节数组转成了字符串
        String str = new String(arr);
        System.out.println(str);//b 1 A B a b
        /*将字节数组的一部分转成字符串，
         * 其中offset是从哪个索引开始，length表示要将多少个字节是转成字符串
         */
        String str2 = new String(arr, 1, 4);
        System.out.println(str2);
    }

    private static void test2_原始字符串转换() {
        String str = new String("abc");//这个对象，在堆内存中
        //"abc"常量，在方法区的常量池
        //new String("abc")这个对象，在堆内存中
        //str指向了堆内存中的对象
        System.out.println(str);//abc
        System.out.println(str == "abc");//false
        System.out.println("abc" == "abc");//true
    }

    public static void test1_空参构造() {
        String str = new String();
        System.out.println(str);
    }
}
```

## String类型的判断功能

1. equals(Object obj)：判断调用者和参数对象描述的字符串内容是否相同
2. equalsIgnoreCase(String otherStr)：忽略大小写判断两个字符串内容是否相同
3. contains(String str)：判断调用者是否包含了str这个子串
4. startsWith(String prefix)：判断调用者是否以prefix开头
5. endsWith(String suffix)：判断调用者是否以suffix结尾
6. isEmpty()：判断调用者是否是空串

代码示例：

```java
public class Simple03 {

    public static void main(String[] args) {
//        test1_equals();
//        test2_equalsIgnoreCase();
//        test3_包含();
//        test4_5_判断开始和结束();
//        test6_判断为空();
    }
    private static void test6_判断为空() {
        String str = "abc";
        System.out.println(str.isEmpty());//false  字符串不为null
    }

    private static void test4_5_判断开始和结束() {
        String str = "abcdefg";
        System.out.println(str.startsWith("abc"));//true
        System.out.println(str.startsWith("abd"));//fasle
        System.out.println(str.startsWith("cde"));//false
        System.out.println(str.endsWith("efg"));//true
    }

    private static void test3_包含() {
        String str1 = "abcdefg";
        System.out.println(str1.contains("abc"));//true
        System.out.println(str1.contains("efg"));//true
        System.out.println(str1.contains("cde"));//true
        System.out.println(str1.contains("ace"));//false 必须是连续的
    }

    private static void test2_equalsIgnoreCase() {
        String str1 = "abcDEF";
        String str2 = "abcdef";
        System.out.println(str1.equals(str2));//false 字符串不相等
        //忽略大小写判断两个字符串内容是否相同
        System.out.println(str1.equalsIgnoreCase(str2));//true
    }

    private static void test1_equals() {
        String str1 = new String("abc");//在堆中
        String str2 = new String("abc");
        System.out.println(str1 == str2);//false 两个对象地址不一样 new了两次
        System.out.println(str1.equals(str2));//true  指向同一个对象
    }
}
```

## String类型的获取功能

1、length()：获取字符串字符的个数

2、charAt(int index)：返回调用者字符串中索引为index的字符（和length方法结合之后可以遍历字符串）

3、substring(int beginIndex)：获取一个字符串，内容是从当前字符串的beginIndex索引开始，一直到最后一个字符机截止

4、substring(int beginIndex, int endIndex)：获取一个指定索引范围的子串

 注意事项：1、包含头不包含尾，返回的结果中，不包含endIndex索引指向的字符；2、所有的方法都无法修改字符串对象本身，一般都是返回一个新的字符串对象

5、indexOf家族：

 indexOf(int ch)：返回ch字符在当前调用者字符串中，第一次出现的索引

 indexOf(int ch, int fromIndex)：从fromIndex索引开始寻找，找到ch字符在当前字符串中第一次出现的索引

 indexOf(String str)：返回的是str这个字符串在调用者字符串中第一次出现的索引

 indexOf(String str, int fromIndex)：从fromIndex索引开始寻找，找到str字符串在当前字符串中第一次出现的索引（注意：无论从哪个位置开始找，所有字符的索引都不会变化）

6、lastIndexOf家族：

 和IndexOf基本一样，只不过是从后往前找，所有字符和字符串的索引也都不会发生变化

代码示例：

```java
public class Simple04 {

    public static void main(String[] args) {
        test1_length();
        test2_charAt();
        test3_字符串遍历();
        test4_获取子串();
        test5_indexOf();
        test6_lastIndexOf();
    }
    private static void test6_lastIndexOf() {
        String str = "abcdabcd";
        System.out.println(str.lastIndexOf('c'));
        System.out.println(str.lastIndexOf('c', 5));

        System.out.println(str.lastIndexOf("cd"));
        System.out.println(str.lastIndexOf("cd", 5));
    }

    private static void test5_indexOf() {
        String str = "abcdabcd";
        //返回第一个c出现的索引
        System.out.println(str.indexOf('c'));
        System.out.println(str.indexOf('c', 3));

        System.out.println(str.indexOf("cd"));//2
        System.out.println(str.indexOf("cd", 3));
    }

    private static void test4_获取子串() {
        String str = "abcdefg";
        String result = str.substring(3);
        //str本身不会变化，因为字符串对象是一个不可变的字符序列
        System.out.println(str);
        //substring可以返回一个新的字符串对象
        System.out.println(result);
        //通过两个参数的substring获取子串
        System.out.println(str.substring(2, 5));
    }

    private static void test3_字符串遍历() {
        String str = "abcdefg";
        //遍历字符串
        for (int i = 0; i < str.length(); i++) {
            System.out.println(str.charAt(i));
        }
    }

    private static void test2_charAt() {
        String str = "abcdefg";
        char c = str.charAt(3);
        System.out.println(c);
    }

    private static void test1_length() {
        String str = "abcdefg";
        System.out.println(str.length());
    }
}
```

## String类型的转换功能

1、byte[] getBytes()：将当前字符串，转成字节数组(编码)
2、char[] toCharArray()：将当前的字符串，转成字符数组
3、toUpperCase()：将当前的字符串，转成全大写形式
4、toLowerCase()：将当前的字符串，转成全小写形式
5、concat(String str)：将当前调用者，和参数str进行拼接，返回拼接后的长字符串（不常用，因为更多使用的是运算符+）
6、valueOf家族：可以将任意数据类型的数据，转换成字符串

代码示例：

```java
public class Simple05 {
    public static void main(String[] args) {
        test1_getBytes();
        test2_toCharArray();
        test3_upperAndLower();
        test4_concat();
        test5_valueOf();
    }

    private static void test5_valueOf(){
        //可以将任意数据类型的数据，转换成字符串
        System.out.println(String.valueOf(false));
        System.out.println(String.valueOf('x'));
        System.out.println(String.valueOf(new char[] {'a', 'b'}));
        System.out.println(String.valueOf(100));
        Object obj = null;
        System.out.println(String.valueOf(obj));
    }

    private static void test4_concat() {
        String str = "abc";
        String result = str.concat("xyz");
        System.out.println(result);
        System.out.println(str + "xyz");
        //将当前调用者，和参数str进行拼接，返回拼接后的长字符串（不常用，因为更多使用的是运算符+）
    }

    private static void test3_upperAndLower() {
        String str = "aBCdEfG";
        String upper = str.toUpperCase();//：将当前的字符串，转成全大写形式
        System.out.println(str);
        System.out.println(upper);
        String lower = str.toLowerCase();//：将当前的字符串，转成全小写形式
        System.out.println(lower);
    }

    private static void test2_toCharArray() {
        String str = "abcdefg";
        char[] arr = str.toCharArray();//将当前的字符串，转成字符数组
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + "\t");
        }
        System.out.println();

    }

    private static void test1_getBytes() {
        String str = "abc";
        byte[] arr = str.getBytes();//将当前字符串，转成字节数组(编码)
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);
        }
    }
}
```

## String类型的其他方法

1、replace(String oldStr, String newStr)：将调用者中的老串替换成新串
2、trim()：去掉字符串左右两边的空格、制表符

代码示例：

```java
public class Simple06 {
    public static void main(String[] args) {
        test2_trim();
        test1_replace();
    }

    //去掉字符串左右两边的空格、制表符
    private static void test2_trim() {
        String str = "  I love java,  java is good ";
        String result = str.trim();
        System.out.println(str);
        System.out.println(result);
    }

    //将调用者中的老串替换成新串
    private static void test1_replace() {
        String str = "I love java, java is good";
        String result = str.replace("java", "kobe");
        System.out.println(str);
        System.out.println(result);
    }
}
```

# StringBuilder

## 概述

1、StringBuilder是一个可变的字符序列，因为在类中提供了修改私有成员变量的方法

 常用的方法是append和insert，就是在StringBuilder对象本身上，进行修改操作

2、String类型和StringBuilder的关系：都是用于描述字符串

 1、String是不可变的字符序列，没有提供修改私有成员的方法；StringBuilder是可变的字符序列，因为提供了修改成员变量的方法；

 2、String长度本身也不可以变化，StringBuilder长度可以变化，可以认为StringBuilder就像一个可以伸缩的容器，用于存储字符

## 构造方法

1、构造方法作用：创建当前对象、将其他类型的数据，转换成当前类型
2、StringBuilder的构造方法：
 	StringBuilder()：创建一个生成器，初始容量为16个字符
 	StringBuilder(int capacity)：创建一个生成器，初始容量为capacity大小
 	StringBuilder(String str)：创建一个生成器，初始值就是str这些字符，初始大小是str+16
3、获取容积的方法：
 	capacity()：获取当前生成器的容器大小
 	length()：获取当前生成器中的字符个数

代码示例：

```java
public class Simple01 {
    public static void main(String[] args) {
        StringBuilder sb1 = new StringBuilder();
        System.out.println(sb1.capacity());//16

        StringBuilder sb2 = new StringBuilder(10);
        System.out.println(sb2.capacity());//10

        StringBuilder sb3 = new StringBuilder("Hello");
        System.out.println(sb3.capacity());//21
        System.out.println(sb3.length());//字符个数是5
    }
}
```

## 添加功能

1、append(任意类型)：可以将任意数据类型，转成字符，添加到生成器中
        2、insert(int index, 任意数据类型)：可以将任意数据类型，添加到指定的位置
 	说明：

1、index的范围是0~当前缓冲区的大小；

2、插入数据之后，数据本身的索引就是参数中指定的index

代码示例：

```java
    private static void test1insert() {
        StringBuilder sb1 = new StringBuilder();//"1"
        sb1.insert(0, "a");
        System.out.println(sb1);//1
        sb1.insert(0, "b");
        System.out.println(sb1);//ba
        sb1.insert(1, "xyz");
        System.out.println(sb1);//bxyza
   }
```

## 删除功能

1、deleteCharAt(int index) ：删除指定索引的字符
		2、delete(int start, int end)：删除指定范围的字符，被删除的部分包含头不包含尾

代码示例：

```java
public class Simple03 {
    public static void main(String[] args) {
        //test1_deleteCharAt();
        test2_delete();
    }

    private static void test2_delete() {
        StringBuilder sb1 = new StringBuilder("abcdef");
        //删除cd
        sb1.delete(2, 4);
        System.out.println(sb1);//abef
    }

    private static void test1_deleteCharAt() {
        StringBuilder sb1 = new StringBuilder("abcdefg");
        sb1.deleteCharAt(2);
        System.out.println(sb1);//abdefg  删除指定索引的字符
        sb1.deleteCharAt(6);//String索引越界异常
        System.out.println(sb1);
    }
}
```

## 替换和反转功能

1、replace(int start, int end ,String str)：
 	将字符串缓冲区中的从start开始到end-1这部分内容，替换成str
2、reverse()：将原有字符序列进行反转

代码示例：

```java
public class Simple04 {
    public static void main(String[] args) {
        test1_replace();
        test2_reverse();
    }

    private static void test2_reverse() {
        StringBuilder sb1 = new StringBuilder("abcdefg");
        sb1.reverse();
        System.out.println(sb1);
    }

    private static void test1_replace() {
        StringBuilder sb1 = new StringBuilder("abcdefg");
        sb1.replace(2, 5, "qq");
        System.out.println(sb1);
    }
}
```



## StringBuffer和StringBuilder的区别

1、共同点：
 	都是字符串的缓冲区，都是字符串的生成器，都是可变的字符序列
2、不同点：
 	1、出现版本不同：
 	 	StringBuffer在jdk1.0出现的
 	 	StringBuilder在jdk1.5出现的
 	2、线程安全性不同：
 	 	StringBuffer是线程安全的，在多线程环境下仍然保证数据安全
 	 	StringBuilder是线程不安全，在多线程环境下无法保证数据安全
 	3、效率不同：
 	 	StringBuffer效率低
 	 	StringBuilder效率高

# 包装类

## 概述

1. 基本数据类型有八种，都是非常简单的数据类型

2. 在这些类型中，只能表示简单的数据，不能包含一些操作数据的方法，没有办法存储描述这些数据的内容。需要准备一个引用数据类型，能将基本数据类型进行包装，提供一些操作基本类型的方法，定义一些描述数据的数据。

3. 基本类型的包装类：
       byte 				Byte
         short				Short
         int					Integer类型
         long				Long
         float				Float
         double			Double
         char				Character      
         boolean			Boolean
   
     1、各种包装类型的方法、特点基本相同，只要学习一个Integer类型，其他的也触类旁通。
   2、Integer类型的对象中，维护了一个私有的成员变量，是一个int类型的字段（成员变量、属性），用于表示这个Integer对象要表示的数字。
   3、提供了在int、Integer、String类型之间相互转换的方法

## Integer的构造方法

1. Integer(int i)：将一个基本类型的int数，转换成Integer类型的对象使用i给Integer对象中的成员变量赋值
2. Integer(String s)：将一个字符串类型的数字，转换成Integer类型的对象转换的作用

代码示例：

```java
public class Simple01 {
    public static void main(String[] args) {
        //将一个基本类型的int数，转换成Integer类型的对象使用i给Integer对象中的成员变量赋值
        Integer it = new Integer(100);
        System.out.println(it);

        //将一个字符串类型的数字，转换成Integer类型的对象转换的作用
        Integer it2 = new Integer("100");
        System.out.println(it2);
    }
}
```

## Integer、int、String类型互相转换的总结

1. Integer转换成int类型

    	intValue()

2. int转换成Integer类型

   ​	构造方法Integer(int i)

3. Integer到String的转换

    ​    toString方法即可

4. String到Integer的转换
      Integer.valueOf(String str)
   
5. int转换成String类型：

     Integer.toString(int i)

6.  String转换成int类型

   Integer.parseInt(String str)

## 自动装箱和拆箱

1、装箱和拆箱：
 	装箱：将基本数据类型，封装成包装类型的对象(引用类型)，这个过程就是装箱，使用构造方法即可
 	拆箱：从包装类型的对象中，将其包装的基本类型的数据取出，这个过程就是拆箱，使用intValue方法即可
2、自动装箱和拆箱
 	自动装箱：可以直接使用基本类型的数据，给引用类型的变量赋值
 	自动拆箱：可以直接使用包装类对象，给基本类型的变量赋值；包装类对象直接进行算数运算；

代码示例：

```java
public class Simple03 {
    public static void main(String[] args) {
        Integer it = 100;//自动装箱，100基本数据类型给integer包装类型赋值
        int i = it;//自动拆箱，

        System.out.println(it + 1);//自动拆箱  变成基本类型 然后运算

        Integer inte = 100;//自动装箱
        inte = inte + 1;//先自动拆箱，计算，最后自动装箱
    }

    private static void test1_装箱和拆箱() {
        int i = 100;
        Integer it = new Integer(i);//装箱

        int j = it.intValue();//拆箱
    }
}
```