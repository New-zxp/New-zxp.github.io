---
title: JavaSE笔记
date: 2020-06-20
tags: JavaSE
categories: 笔记
description: JavaSE笔记
cover: https://zhuxinpeng.oss-cn-hangzhou.aliyuncs.com/1.jpg
---
进制：97
二进制 ：01100001 (0B/0b开头)
八进制：01 100 001  → 141（0开头）
十六进制：0110 0001 → 61 最大为15 A-F = 10 - 15 （0X/0x开头）

类型转换：byte、short、char在表达式中直接转换成int来计算
表达式最终结果由表达式中最高类型决定：byte、short、char→int→long→float→double

逻辑运算符：&一个假就是假；|一个真就是真；！你真我假你假我真；^结果不同为真;&&左边是假右边不执行；||左边是真右边不执行

三元运算符：条件表达式？值1：值2；条件表达式为真返回值1，假返回值2

不知道循环多少次用while;知道循环多少次用for

方法中数组的值会因为形参改变，因为数组是传递的地址；
方法中return后的语句不执行；

return：跳出并立即结束所在方法的执行；
break：跳出并结束当前所在循环的执行；
continue：结束当前所在循环的当次，立即进入下次循环；

字符+字符串，不能算，+作为连接符，连接起来

类中成员变量默认值规则：
byte、short、char、int、long ： 0
float、double ： 0.0
boolean : false
类、接口、数组、String ： null

int number = r.nextInt(2,5)               2 <= number < 5

两个字符串比较，要看字符串是否指向同一个对象。
用“”方式给出的字符串对象，在字符串常用池中储存，相同内容只会在其中储存一份，给出的相同字符串指向同一个对象。
不是直接用“”给出来的字符串，如通过运算得到的字符串，存放在堆内存中，与直接“”给出来的存放在字符串常用池中的不相等。
程序在编译时“a" + "b" + "c"会直接转换成"abc"
通过构造器new字符串对象，每new一次产生一个新对象，放在堆内存。

a.equals(b);             字符串a和b比较

标准JavaBean须满足的书写要求：
①成员变量使用private修饰
②提供成员变量对应的set和get方法（右键generate,select none）
②必须提供一个无参构造器，有参构造器可写可不写。（右键generate）

成员变量和局部变量区别：
①成员变量存在于类中，局部变量常见于方法中；
②成员变量有默认值，局部变量没有默认值；
③成员变量属于对象，和对象一起保存在堆内存；局部变量是在方法里面，方法在栈内存执行，因此局部变量也在栈内存；
④成员变量随着对象存在和消失，局部变量随着方法调用而存在，随着运行结束而消失；
⑤作用域：成员变量对象存在就能访问，局部变量在所属打括号中；

/**+回车 自动标准注释

String常用方法：
public int length（）：返回此字符串的长度
public char charAt（int index）：获取索引位置处的字符
public char[] toCharArray（）：将当前字符串转换成字符数组返回
public String substring（int beginIndex, int endIndex）：根据开始和结束索引进行截取，得到新的字符串（包前不包后）
public String substring（int beginIndex）：从开始索引截取到末尾，得到新的字符串
public String replace （CharSequence target ,CharSequence replacement）：使用新值，将字符串中的旧值替换，得到新的字符串
public String[ ] split （String regex）：根据传入的规则切割字符，得到字符串数组返回

ctrl + alt + t：快速添加死循环

alt + enter ：快速生成方法

静态代码块：有static修饰，属于类，与类一起优先加载一次，仅加载一次，自动触发执行；
作用：可以用于初始化静态资源

构造代码块：无static修饰，属于对象，每次构建对象时，都会触发一次执行，在构造器执行前

继承：
@Override：方法重写校验注解，提高可读性
1.重写方法的名字和形参列表必须与被重写方法一样
2.私有方法不能被重写；
3.静态方法不能被重写；

子类可以继承父类的属性和行为，但是子类不能继承父类的构造器
一个类只能继承一个父类
Java不支持多继承但是支持多层继承
子类的全部构造器默认会先访问父类的无参数构造器

this（ ）和super（ ）只能放在构造器第一行，两者不共存
this：访问子类当前对象的成员
super：在子类方法中指定访问父类的成员
this（ ）：访问本类兄弟构造器
super（ ）：在本类构造器中指定访问父类的构造器

private：只有同一个类中可以使用
缺省（什么都不加）：同一个包中可以使用
protected：同一个类中，同一个包中其他类，不同包下的子类都能用（继承）
public：同一个类中，同一个包中其他类，不同包下的子类（继承），不同包下的无关类都能用
一般要求：成员变量一般私有；方法一般公开；如果该成员只希望本类访问，使用private修饰；

final：可以修饰类，方法，变量
修饰类：表明该类是最终类，不能被继承
修饰方法：表明该类是最终方法，不能被重写
修饰变量：表示该变量第一次赋值后，不能被再次赋值（有且仅能被赋值一次）
final修饰引用类型的变量，地址值不能改变，但是指向的对象内容可以改变               如：final Teacher t = new Teacher("学习") ；          t.setHobby("运动")；可以这么做

枚举：为了做信息的标志和信息的分类
格式：修饰符 enum 枚举名称{罗列枚举类实例的名称}

抽象类
public abstract class Animal{
public abstract void run();
}
抽象类中可以没有抽象方法，有抽象方法的必须是抽象类
一个类继承了抽象类，必须重写完抽象类的全部抽象方法，否则这个类也必须定义为抽象类
抽象类不能创建对象
作用：通常作为模板类，模板方法通常加final避免被重写
final与abstract互斥

接口
public interface 接口名{//常量             //抽象方法}
接口被实现类实现，可以理解为子类：修饰符 class 实现类 implements 接口1，接口2，接口3，...{  }

一个接口可以继承多个接口
接口的方法默认有public abstract修饰
接口的默认方法必须用default修饰，默认用public修饰
接口的静态方法必须用本身的接口名调用，默认用public修饰
接口的私有方法必须用private修饰，只能在本类中被其他默认方法或私有方法访问
接口不能定义对象
一个类实现多个接口，多个接口中同名静态方法不冲突，因为静态方法只能用接口名调用
一个类继承了父类，同时又实现了接口，父类和接口中同名方法，默认用父类的
一个类实现了多个接口，多个接口中存在同名的默认方法，不冲突，这个类重写该方法即可

多态：同类型的对象，执行同一个行为，会表现出不同的特征
形式：父类类型 对象名称 = new 子类构造器                   接口  对象名称 = new 实现类构造器
多态中成员访问特点：方法调用：编译看左，运行看右     变量调用：编译看左，运行也看左
定义方法时，使用父类型作为参数，该方法可以接收父类的一切子类对象
多态下不能访问子类独有功能，如果要访问必须强制类型转换：子类 对象变量 = （子类）父类类型变量
Java建议使用instanceof判断当前对象真实类型再进行转换（判断左边对象是否属于右边类）

静态内部类：有static修饰属于外部类本身，它的特点和使用与普通类完全一样，只是在别人里面
创建对象格式：外部类名.内部类名 对象名 = new 外部类名。内部构造器

成员内部类：无static修饰，属于外部类对象
创建对象格式：外部类名。内部类名 对象名 = new 外部构造器.new 内部构造器
成员内部类中访问所在外部类对象格式：外部类名.this

匿名内部类：本质是没有名字的局部内部类，定义在方法中，代码块中等
作用：方便创建子类对象，最终目的为了简化代码编写
格式：new 类|抽象类名|接口类名（）{重写方法；}
特点：1.匿名内部类是一个没有名字的内部类；
2.匿名内部类写出来就会产生一个匿名内部类对象；
3.匿名内部类的对象类型相当于是当前new的那个的类型的子类类型

Objects常见方法：
boolean equals（Object a , Object b）：比较两个对象，避免空指针（字符串比较！！！）
boolean isNull (Object obj)：判断是否为null，是null返回true
Object.equals默认比较地址，比较对象内容需要重写

StringBuilder（代码）:一个类，用来拼接反转字符串，性能好，toString已经重写。

Math类：
public static int abs（int a）：获取参数绝对值
public static double ceil(double a)：向上取整
public static double floor(double a)：向下取整
public static int round(float a)：四舍五入
public static int max（int a , int b）：获取较大值
public static double pow（double a , double b）：返回a的b次幂的值 a = 2, b = 3返回值为8
public static double random（）：返回随机值范围：[0.0 , 1.0）

System类：
public static void exit（int status）：填零终止当前运行的Java虚拟机，非零表示异常终止
public static long currentTimeMills（）：返回当前系统的时间毫秒值形式（返回1970-1-1 00:00:00 到现在的毫秒值）
public static void arraycopy（数据源数组，起始索引，目的地数组，起始索引，拷贝个数）：数组拷贝

BigDecimal（代码）：解决浮点型运算精度失真的问题
public static BigDecimal valueOF(double val)：包装浮点数成为BigDecimal对象

Date类（代码）：
无参构造器：public Date（）：创建一个Date对象，代表的是系统当前此刻日期时间
有参构造器：public Date（long time）：把时间毫秒值转为Date日期对象
常用方法：public long getTime（）：获取时间对象的毫秒值（返回1970-1-1 00:00:00 到此刻的毫秒值）
public void setTime（long time）：设置日期对象的时间为当前毫秒值对应的时间
d1.after(d2)：判断d1是否在d2之后
d1.before(d2)：判断d1是否在d2之前

SimpleDateFormat类：
1.把Date对象或时间毫秒值格式化为我们喜欢的形式；
2.把字符串时间解析成日期对象；
无参构造器：使用默认格式
有参构造器：使用指定格式：
y - 年     M - 月     d - 日     H - 时     m - 分     s - 秒     EEE - 星期几     a - 上午/下午
2020-11-11 13:27:06  --------------- yyyy - MM - dd HH:mm:ss
2020-11-11 13:27:06  --------------- yyyy年MM月dd日 HH:mm:ss
public final String format（Date date）：将日期格式化为日期/时间字符串
public final String format（Object time）：将时间毫秒值格式化为日期/时间字符串
public Date parse(String source)：从给定字符串的开始解析文本生成日期对象

Calendar（抽象类，不能直接创建对象）（代码）：代表了系统此刻日期对应的日历对象
创建对象：Calendar rightnow = Calendar.getInstance（）；
public int get(int field)：取日期中的某个字段信息
public void set(int field, int value)：修改日历的某个字段信息
public void add(int field, int amount)：为某个字段增加/减少指定的值
public final Date getTime()：拿到此刻日期对象
public long getTimeMillis()：拿到此刻时间毫秒值

新增日期类(代码)：
LocalDate类：public static LocalDate/LocalTime/LocalDateTime now()：根据当前日期/时间/日期时间创建对象  
Instant类：由静态方法now创建对象
DateTimeFormat类：解析或格式化日期
Period类：计算日期间隔
Duration类：计算时间间隔
ChronoUnit工具类：用于在单个时间单位内测量一段时间，可以比较所有的时间单位

包装类：8种基本类型对应的引用类型
byte,short,long,char,float,double,boolean首字母大写；int→Integer,char→Character
把字符串类型的数值转换为真实的数据类型：Integer.valueOf/Double.valueOf("字符串类型的整数/小数")

正则表达式：判断是否匹配所给规则
public boolean matches (String regex)：判断是否匹配正则表达式
\\w{6,}：必须是字母，数字，下划线，至少六位
手机号验证：1[3-9]\\d{9}
邮箱验证：[a-zA-Z0-9]+@[a-zA-Z0-9]+\.[a-zA-Z0-9]+     //+表示至少出现一次
public String[] split(String regex)：按照正则表达匹配的内容分割字符串，返回一个字符串数组
public String[] replaceAll(String regex, String newStr)：按照正则表达式匹配的内容进行替换

Arrays类(代码)：数组操作工具类，专门用于操作数组元素
常用API：public static String toString(类型[] a)：打印数组内容
public static void sort（类型[] a）：对数组进行排序（默认升序排序）
public static <T> void sort（类型[] a, Comparator <? super T>c）：使用比较器对象自定义排序
public static int binarySearch(int[] a, int key)：二分搜索数组中的数据，存在返回索引，不存在返回-1（数组必须排好序才支持	）

Lambda：简化函数式接口的匿名内部类写法，接口中有且仅有一个抽象方法
@FunctionalInterface：加上以后必须是函数式接口，只能有一个抽象方法
形式：（匿名内部类被重写方法的形参列表） -> {被重写方法的方法体代码}    Swimming s1 = （） -> { System.out.println("游泳")；}；
省略写法：①参数类型可以不写；②如果只有一个参数，参数类型可以不写，同时省略（）
③如果方法体代码只有一行，可以省略大括号不写，同时省略分号；如果是return语句，return必须省略，同时省略分号。

Collection集合体系(代码)：接口
List系列集合：有序，可重复，有索引
①ArrayList：基于数组，可以重复，有索引，查询快
②LinkedList：可重复，有索引，增删首尾操作快
Set系列集合：
①HashSet：无序，不重复，无索引 ，增删改查快，基于哈希表
②LinkedHashSet：有序，不重复，无索引 ，增删改查快，基于哈希表和双链表
③TreeSet：按照大小默认升序排序（红黑树），不重复，无索引，可以进行排序

Iterator：迭代器是集合的专用遍历方式
Iterator<E>  iterator（）：返回集合中的迭代器对象，默认指向当前集合0索引
常用方法：boolean hasNext（）：询问当前位置是否有元素存在，存在为true
E next（）：获取当前位置的元素，并同时将迭代器对象移向下一个位置

增强for循环：既可以遍历集合也可以遍历数组
格式：for(元素数据类型 变量名：数组或者Collection集合) {  }
forEach:iter + ctrl + j

Lambda表达式遍历集合：default void forEach(Consumer<? super T action):

数据结构：
队列：先进先出，后进后出
栈：后进先出，先进后出
数组：内存连续区域，查询快，增删慢
链表：元素游离，查询慢，首尾操作极快
二叉树：只有一个根节点，每个节点不超过两个子节点
查找二叉树：小的左边，大的右边，但是可能树很高，性能变差
平衡查找二叉树：让树的高度差不大于1，增删改查都提高了
红黑树：基于红黑规则实现了自平衡的排序二叉树

泛型：
泛型类：修饰符 class 类名<泛型变量>
泛型方法：修饰符<泛型变量> 方法返回值 方法名称(形参列表)
泛型接口：修饰符 interface 接口名称 <泛型变量>
E，T，K，V定义泛型时使用，？在使用泛型时代表一切类型
？：可以在使用泛型的时候代表一切泛型

可变参数：在形参中接受多个数据、
格式：数据类型...参数名称
作用：传输参数非常灵活，可以不传参数，可以传输一个或者多个参数，也可以传输一个数组

Collections工具类：
public static <T> boolean addAll(Collection<? super T > c, T...elements)：给集合批量添加元素 ，添加元素必须是T或T的子类
public static void shuffle (List<?> list)：打乱list集合的顺序
public static <T> void sort（List<T> list）：将集合中元素按照默认规则排序，不能对自定义类型集合排序，除非自定义类型实现了比较规则Comparable接口
public static <T> void sort（List<T> list, Comparator<? super T> c）：将集合中元素按照制定规则排序

abcList.stream().map(item -> {
一系列操作
return "想要输出到新List中的元素";
}).collect(Collectors.toList());
item: abcList中每个元素
Collectors.toList(): 将返回结果输出到一个新的List中
返回结果类型为List

Map集合（接口）：双列集合，每个元素包含两个数据，每个元素的格式：key = value(键值对元素)
Map集合完整格式：{key1 = value1, key2 = value2, key3 = value1, .....}
Map集合的键无序，不重复，相同的键会覆盖之前的元素；值不做要求，可以重复的。
Map集合的特点由键决定

HashMap：元素按照键是无序，不重复，无索引的，相同的键会覆盖之前的元素，值不做要求（与Map一致）
LinkedHashMap：元素按照键是有序的，不重复，无索引，值不做要求
TreeMap：元素按照键是排序，不重复，无索引的，值不做要求

Map常用API：
V put (K key , V value)：添加元素
V remove(Object key)： 根据键删除对应元素
void clear（）：移除所有键值对元素
boolean containsKey(Object key)：判断集合是否包含指定的键
boolean containsValue(Object value)：判断集合是否包含指定的值
boolean isEmpty（）：判断集合是否为空
int size()：集合的长度
pubulic V get (Object key)：根据键获取对应的值
public Set <K> keySet（）：获取全部键的集合
Collection<V> values（）：获取全部值的集合
map1.putAll(map2)：把map2的元素全部添加到map1

(代码)
Map遍历1：先拿到集合的全部键，遍历每个键，通过forEach根据键获取对应值
Map遍历2：通过调用Map的entrySet方法把Map集合转换成Set集合形式，Set<Map.Entry <String ,Integer> > entries ： ctrl + alt + v自动补全
Map遍历3：Lambda

不可变集合：static<E> List<E> of(E...elements)：创建一个具有指定元素的List集合对象
不可变集合：static<E> Set<E> of(E...elements)：创建一个具有指定元素的Set集合对象
不可变集合：static<K,V> Map<K,v> of(E...elements)：创建一个具有指定元素的Map集合对象

Stream(代码)：用于简化集合和数组操作的API
forEach：遍历
count：统计个数 	
filter：过滤
limit：取前几个元素
skip：跳过前几个
map：加工方法
concat：合并流
distinct：去除重复元素

异常类型：
1.数组索引越界异常：ArrayIndexOutOfBoundsException
2.空指针异常：NullPointerException
3.类型转换异常：ClassCastException
4.数字操作异常：ArithmeticException
5.数字转换异常：NumberFormaException
6.运行时异常：RuntimeException

throw：在方法内部直接创建一个异常对象，并从此点抛出
throws：用在方法申明上抛出方法内部的异常

日志
日志级别依次是：TRACE < DEBUG < INFO < WARN < ERROR，默认级别是debug
作用：用于控制系统中哪些日志级别是可以输出的，只输出级别不低于设定级别的日志信息
ALL和OFF分别是打开全部日志信息，及关闭所有日志信息
在代码中获取日志对象：
public static final Logger LOGGER = LoggerFactory.getLogger("类名")；

File类:
isDirectory：判断此路径名是否为文件夹
isFile：判断此路径名是否为文件
exists：判断此路径名是否存在
length：返回文件的大小（字节数量）
getAbsolutePath：返回文件的绝对路径
getPath：返回定义文件对象时的路径
getName：返回文件的名称，带后缀
lastModified：返回文件的最后修改时间（时间毫秒值）
createNewFile：创建一个新的空文件
mkdir：只能创建一级文件夹
mkdirs：可以创建多级文件夹
delate：删除此抽象路径名表示的文件或空文件夹，不进入回收站
String[] list ：获取当前目录下的	所有“一级文件名称”到一个字符串数组中
File[] listFiles：获取当前目录下所有“一级文件对象”到一个文件对象数组中
listFiles方法注意事项：
当文件不存在时或者代表文件时，返回null
当文件对象代表一个空文件夹时，返回一个长度为0的数组
当文件对象是一个有内容的文件夹时，将里面所有文件和文件夹的路径放在File数组中返回
当文件对象是一个有隐藏文件的文件夹时，将里面所有文件和文件夹的路径放在File数组中返回，包含隐藏文件
当没有权限访问该文件夹时，返回null

字符的编码和解码：
中文在UTF-8中以三个字节存储，在GBK中以两个字节存储
编码：byte[] getBytes()：使用平台的默认字符集将该String编码为一系列字节，将结果存储到新的字节数组中
byte[] getBytes(String charsetName)：使用指定的字符集将该String编码为一系列字节，将结果存储到新的字节数组中
解码：String(byte[] bytes)：使用平台的默认字符集解码指定的字节数组来构造新的String
解码：String(byte[] bytes, String charsetName)：使用指定的字符集解码指定的字节数组来构造新的String
“\r\n”：换行

字节流：
InputStream：字节输入流  → FileInputStream（实现类）       
pubic FileInputStream(File file)：创建字节输入流管道与源文件对象接通   
pubic FileInputStream(String pathname)：创建字节输入流管道与源文件路径接通   
public int read（）：每次读取一个字节返回，如果字节已经没有可读的返回-1
public int read（byte[] buffer）：每次读取一个字节数组返回，如果字节已经没有可读的返回-1

OutputStream：字节输出流 → FileOutPutStream（实现类）
public FileOutputStream(File file)：创建一个字节输出流管道通到目标文件对象，若存在原文件，会把原文件数据清空
public FileOutputStream(File file，boolean append)：创建一个追加数据的字节输出流管道通到目标文件对象，不会清空原文件
public void write(int a)：写一个字节进去
public void write(byte[] buffer)：写一个字节数组进去
public void write(byte[] buffer，int pos ,  int len)：写一个字节数组的一部分进去
写数据必须刷新数据
输入输出流用完以后一定要关闭
flush（）：刷新流，还可以继续写数据           
close（）：关闭流，释放资源，关闭前会先刷新，一旦关闭就不能写数据

try-catch-finally：在异常处理时提供finally块来执行所有清除操作，比如IO流中的释放资源
特点：无论代码是否正常结束，被finally控制的语句最终一定会执行，除非JVM退出

try(定义流对象){可能出现异常的代码；}catch(异常类名 变量名 ){异常的处理代码；}  ：用完自动关闭流对象

字符流：
Reader：字符输入流 → FileReader（实现类）          
InputStreamReader：转换流      
Writer：字符输出流 → FileWriter（实现类）
OutputStreamWrite：转换流

缓冲流：缓冲流自带缓冲区，可以提高原始字节流，字符流读写数据性能
BufferedInputStream：字节缓冲输入流
BufferOutputStream：字节缓冲输出流
BufferReader：字符缓冲输入流
新增：public String readLine()：读取一行数据返回，无行可读返回null
BufferWriter：字符缓冲输出流
newline（）：换行功能

转换流：
InputStreamReader：字符输入转换流
public InputStreamReader(InputStream is ，String charset)：可以把原始的字节流按照指定编码转换成字符输入流，这样字符流中的字符就不乱码了
OutputStreamWrite：字符输出转换流
public OutputStreamWriter(OutputStream os，String charset)：可以把原始的字节输出流按照指定编码转换成字符输出流

打印流：实现方便，高效的打印数据到文件中去
PrintStream：
public PrintStream（OutputStream os）：打印流直接通向字节输出流管道
public PrintStream（File f）：打印流直接通向文件对象
public PrintStream（String filepath）：打印流直接通向文件路径
public void print（Xxx xx）：打印任意类型出去
setOut：把系统打印流改成我们自己的打印流

PrintWrite：方法和构造器和PrintStream一样

对象序列化：以内存为基准，把内存中的对象存储到硬盘文件中去，称为对象序列化
对象如果要序列化必须实现Serializable序列化接口
transient修饰的对象不参序列化
序列化的版本号与反序列化的版本号必须一致才不会出错
ObjectInputStream：对象字节输入流
readObject方法：读入对象
ObjectOutputStream：对象字节输出流
writeObject方法：写入对象

Properties：属性集对象，属于map类
作用：Properties代表的是一个属性文件，可以把自己对象中的键值对信息存入到一个属性文件中去

commons-io框架：提供很多有关io操作的类，有主要的两个类FileUtils,IOUtils
FileUtils：String readFileToString(File file,String encoding)：读取文件中的数据，返回字符串
void copyFile(File srcFile, File destFile)：复制文件
void copyDirectoryToDirectory(File srcDir, File destFile)：复制文件夹

IOUtils：static int	copy(InputStream inputStream, OutputStream outputStream)：复制文件

多线程的创建：
方式一：继承Thread类
①定义一个子类MyThread继承线程类java.lang.Thread，重写run方法
②创建MyThread类的对象
③调用线程对象的start方法启动线程（启动后还是执行run方法）
优点：编码简单
缺点：线程类已经继承了Thread，无法继承其他类，不利于扩展

方式二：实现Runnable
①定义一个线程任务类MyRunnable实现Runnable接口，重写run方法
②创建MyRunnable任务对象
③把MyRunnable任务对象交给Thread处理
④调用线程对象的start方法启动线程
优点：线程任务类只是实现接口，可以继续继承类和实现接口，拓展性强
缺点：编程多一层对象包装，如果线程有执行结果是不可以直接返回的

方式三：实现Callable接口
①得到任务对象
1.定义类实现Callable接口，重写call方法，封装要做的事情
2.用FutureTask把Callable对象封装成线程任务对象。
②把线程任务对象交给Thread处理。
③调用Thread的start方法启动线程，执行任务
④线程执行完毕后、通过FutureTask的get方法去获取任务执行的结果

Thread类
String getName()：获取当前线程的名称，默认线程名称是Thread-索引
void setName(String name)：将此线程的名称更改为指定的名称，通过构造器也可以设置线程名称
public static Thread currentThread()：返回对当前正在执行的线程对象的引用
public Thread(String nam	e)：可以为当前线程指定名称
public Thread(Runnable target)	：封装Runnable对象成为线程对象
public Thread(Runnable target ，String name )：封装Runnable对象成为线程对象，并指定线程名称
public static void sleep(long time)：让当前线程休眠指定的时间后再继续执行，单位为毫秒。

线程同步：
方法一：同步代码块
作用：把出现线程安全问题的核心代码给上锁。
原理：每次只能一个线程进入，执行完毕后自动解锁，其他线程才可以进来执行。
synchronized(同步锁对象) {    操作共享资源的代码(核心代码)      }            ctrl + alt + t
锁对象要求：理论上锁对象只要对于当前同时执行的线程来说是同一个对象即可。
对于实例方法建议使用this作为锁对象。
对于静态方法建议使用字节码（类名.class）对象作为锁对象。

方法二：同步方法
作用：把出现线程安全问题的核心方法给上锁。
原理：每次只能一个线程进入，执行完毕以后自动解锁，其他线程才可以进来执行。
修饰符 synchronized 返回值类型 方法名称(形参列表) {	操作共享资源的代码	}
同步方法其实底层也是有隐式锁对象的，只是锁的范围是整个方法代码。
如果方法是实例方法：同步方法默认用this作为的锁对象。但是代码要高度面向对象！
如果方法是静态方法：同步方法默认用类名.class作为的锁对象。

方法三：Lock锁
Lock实现提供比使用synchronized方法和语句可以获得更广泛的锁定操作。
Lock是接口不能直接实例化，这里采用它的实现类ReentrantLock来构建Lock锁对象。
public ReentrantLock()：获得Lock锁的实现类对象
void lock()：获得锁
void unlock()：释放锁，最好用catch finally释放，防止出现死锁

线程通信的三个常见方法：
void wait()：当前线程等待，直到另一个线程调用notify() 或 notifyAll()唤醒自己
void notify()：唤醒正在等待对象监视器(锁对象)的单个线程
void notifyAll()：唤醒正在等待对象监视器(锁对象)的所有线程
注意：上述方法应该使用当前同步锁对象进行调用

线程池：线程池就是一个可以复用线程的技术。
不使用线程池的问题 ：如果用户每发起一个请求，后台就创建一个新线程来处理，下次新任务来了又要创建新线程，而创建新线程的开销是很大的，这样会严重影响系统的性能。

ExecutorService：代表线程池的接口
得到线程池对象：
方式一：使用ExecutorService的实现类ThreadPoolExecutor自创建一个线程池对象
ThreadPoolExecutor构造器的参数说明
public ThreadPoolExecutor(int corePoolSize,                       //参数一：指定线程池的线程数量（核心线程）： 不能小于0
int maximumPoolSize,			   //参数二：指定线程池可支持的最大线程数： 最大数量 >= 核心线程数量
long keepAliveTime,				   //参数三：指定临时线程的最大存活时间：不能小于0
TimeUnit unit,					   //参数四：指定存活时间的单位(秒、分、时、天)： 时间单位
BlockingQueue<Runnable> workQueue,   //参数五：指定任务队列： 不能为null
ThreadFactory threadFactory,  		   //参数六：指定用哪个线程工厂创建线程： 不能为null
RejectedExecutionHandler handler) 	   //参数七：指定线程忙，任务满的时候，新任务来了怎么办： 不能为null
临时线程什么时候创建？     新任务提交时发现核心线程都在忙，任务队列也满了，并且还可以创建临时线程，此时才会创建临时线程。
什么时候会开始拒绝任务？  核心线程和临时线程都在忙，任务队列也满了，新的任务过来的时候才会开始任务拒绝。

ExecutorService的常用方法
void execute(Runnable command) ：执行任务/命令，没有返回值，一般用来执行 Runnable 任务
Future<T> submit(Callable<T> task)：执行任务，返回未来任务对象获取线程结果，一般拿来执行 Callable 任务
void shutdown() ：等任务执行完毕后关闭线程池
List<Runnable> shutdownNow()：立刻关闭，停止正在执行的任务，并返回队列中未执行的任务
新任务拒绝策略：
ThreadPoolExecutor.AbortPolicy：丢弃任务并抛出RejectedExecutionException异常。是默认的策略
ThreadPoolExecutor.DiscardPolicy：丢弃任务，但是不抛出异常 这是不推荐的做法
ThreadPoolExecutor.DiscardOldestPolicy：抛弃队列中等待最久的任务 然后把当前任务加入队列中
ThreadPoolExecutor.CallerRunsPolicy	由主线程负责调用任务的run()方法从而绕过线程池直接执行

方式二：使用Executors（线程池的工具类）调用方法返回不同特点的线程池对象
public static ExecutorService newCachedThreadPool()：线程数量随着任务增加而增加，如果线程任务执行完毕且空闲了一段时间则会被回收掉。
public static ExecutorService newFixedThreadPool(int nThreads)：创建固定线程数量的线程池，如果某个线程因为执行异常而结束，那么线程池会补充一个新线程替代它。
public static ExecutorService newSingleThreadExecutor ()：创建只有一个线程的线程池对象，如果该线程出现异常而结束，那么线程池会补充一个新线程。
public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize)：创建一个线程池，可以实现在给定的延迟后运行任务，或者定期执行任务。
存在问题：前两个任务队列可能无限增加，后两个线程数可能无限增加

定时器：定时器是一种控制任务延时调用，或者周期调用的技术。
作用：闹钟、定时邮件发送。
实现方式一：Timer
public Timer()：创建Timer定时器对象
public void schedule(TimerTask task, long delay, long period)：开启一个定时器，按照计划处理TimerTask任务
特点和存在的问题：Timer是单线程，处理多个任务按照顺序执行，存在延时与设置定时器的时间有出入。
可能因为其中的某个任务的异常使Timer线程死掉，从而影响后续任务执行。

实现方式二：ScheduledExecutorService：内部为线程池
public static ScheduledExecutorService newScheduledThreadPool(int corePoolSize)：得到线程池对象
public ScheduledFuture<?> scheduleAtFixedRate(Runnable command, long initialDelay, long period，TimeUnit unit)：周期调度方法
优点：基于线程池，某个任务的执行情况不会影响其他定时任务的执行。

线程的状态：也就是线程从生到死的过程，以及中间经历的各种状态及状态转换。
Java总共定义了6种状态：6种状态都定义在Thread类的内部枚举类中。
NEW(新建)：线程刚被创建，但是并未启动。
Runnable(可运行)：线程已经调用了start()等待CPU调度
Blocked(锁阻塞)：线程在执行的时候未竞争到锁对象，则该线程进入Blocked状态；。
Waiting(无限等待)：一个线程进入Waiting状态，另一个线程调用notify或者notifyAll方法才能够唤醒
Timed Waiting(计时等待)：同waiting状态，有几个方法有超时参数，调用他们将进入Timed Waiting状态。带有超时参数的常用方法有Thread.sleep 、Object.wait。
Teminated(被终止)：因为run方法正常退出而死亡，或者因为没有捕获的异常终止了run方法而死亡。
sleep方法不会释放锁，时间一到不用抢锁直接运行
wait方法会释放锁，时间到或被唤醒需要抢锁

网络编程三要素：IP地址，端口，协议

IP地址：设备在网络中的地址，是唯一的标识。
IP地址形式：公网地址、和私有地址(局域网使用)。
192.168. 开头的就是常见的局域网地址，范围即为192.168.0.0--192.168.255.255，专门为组织机构内部使用。
IP常用命令：ipconfig：查看本机IP地址
ping IP地址：检查网络是否连通
特殊IP地址：本机IP: 127.0.0.1或者localhost：称为回送地址也可称本地回环地址，只会寻找当前所在本机。

InetAddress类：此类表示Internet协议（IP）地址
public static InetAddress getLocalHost()：返回本主机的地址对象
public static InetAddress getByName(String host)：得到指定主机的IP地址对象，参数是域名或者IP地址
public String getHostName()：获取此IP地址的主机名
public String getHostAddress()：返回IP地址字符串
public boolean isReachable(int timeout)：在指定毫秒内连通该IP地址对应的主机，连通返回true

端口：应用程序在设备中唯一的标识。
端口号：标识正在计算机设备上运行的进程（程序），被规定为一个 16 位的二进制，范围是 0~65535。
周知端口：0~1023，被预先定义的知名应用占用（如：HTTP占用 80，FTP占用21）
注册端口：1024~49151，分配给用户进程或某些应用程序。（如：Tomcat占 用8080，MySQL占用3306）
动态端口：49152到65535，之所以称为动态端口，是因为它 一般不固定分配某种进程，而是动态分配。
我们自己开发的程序选择注册端口，且一个设备中不能出现两个程序的端口号一样，否则出错。

协议: 数据在网络中传输的规则，常见的协议有UDP协议和TCP协议。
通信协议：连接和通信数据的规则被称为网络通信协议
OSI参考模型：世界互联协议标准，全球通信规范，此模型过于理想化，未能在因特网上进行广泛推广。
TCP/IP参考模型(或TCP/IP协议)：事实上的国际标准。

传输层的2个常见协议

TCP：是一种面向连接，安全，可靠的传输数据协议
TCP协议特点：三次握手建立连接，四次挥手断开连接
①TCP协议，必须双方先建立连接，它是一种面向连接的可靠通信协议。
②传输前，采用“三次握手”方式建立连接，所以是可靠的。
③在连接中可进行大数据量的传输。
④连接、发送数据都需要确认，且传输完毕后，还需释放已建立的连接，通信效率较低。
TCP协议通信场景：对信息安全要求较高的场景，例如：文件下载、金融等数据通信。

客户端Socket类：
public Socket(String host , int port)：创建发送端的Socket对象与服务端连接，参数为服务端程序的ip和端口。
OutputStream getOutputStream()：获得字节输出流对象
InputStream getInputStream()：获得字节输入流对象

服务端ServerSocket类
public ServerSocket(int port)：注册服务端端口
public Socket accept()：等待接收客户端的Socket通信连接，连接成功返回Socket对象与客户端建立端到端通信


UDP：是一种无连接、不可靠传输的协议
①UDP是一种无连接、不可靠传输的协议。
②将数据源IP、目的地IP和端口封装成数据包，不需要建立连接
③每个数据包的大小限制在64KB内
④发送不管对方是否准备好，接收方收到也不确认，故是不可靠的
⑤可以广播发送 ，发送数据结束时无需释放资源，开销小，速度快。
UDP协议通信场景：语音通话，视频会话等。

DatagramPacket：数据包对象（盘子）
public DatagramPacket(byte[] buf, int length, InetAddress address, int port):创建发送端数据包对象
buf：要发送的内容，字节数组
length：要发送内容的字节长度
address：接收端的IP地址对象
port：接收端的端口号
public DatagramPacket(byte[] buf, int length)：创建接收端的数据包对象
buf：用来存储接收的内容
length：能够接收内容的长度
public int getLength()：获得实际接收到的字节个数

DatagramSocket：发送端和接收端对象（人）
public DatagramSocket()：创建发送端的Socket对象，系统会随机分配一个端口号。
public DatagramSocket(int port)：创建接收端的Socket对象并指定端口号
public void send(DatagramPacket dp)：发送数据包
public void receive(DatagramPacket p)：接收数据包

单元测试：针对最小的功能单元编写测试代码，Java程序最小的功能单元是方法，因此，单元测试就是针对Java方法的测试，进而检查方法的正确性。

Junit单元测试框架：
①JUnit可以灵活的选择执行哪些测试方法，可以一键执行全部测试方法。
②Junit可以生成全部方法的测试报告。
③单元测试中的某个方法测试失败了，不会影响其他测试方法的测试。

过程：
①将JUnit的jar包导入到项目中
②编写测试方法：该测试方法必须是公共的无参数无返回值的非静态方法。
③在测试方法上使用@Test注解：标注该方法是一个测试方法
④在测试方法中完成被测试方法的预期正确性测试。
⑤选中测试方法，选择“JUnit运行” ，如果测试良好则是绿色；如果测试失败，则是红色
Assert.assertEquals（）：进行预期结果的正确性测试

@Test：测试方法
@Before：用来修饰实例方法，该方法会在每一个测试方法执行之前执行一次。
@After：用来修饰实例方法，该方法会在每一个测试方法执行之后执行一次。
@BeforeClass：用来静态修饰方法，该方法会在所有测试方法之前只执行一次。
@AfterClass：用来静态修饰方法，该方法会在所有测试方法之后只执行一次。

反射概述：反射是在运行时获取类的字节码文件对象：然后可以解析类中的全部成分
①反射是指对于任何一个Class类，在"运行的时候"都可以直接得到这个类全部成分。
②在运行时,可以直接得到这个类的构造器对象：Constructor
③在运行时,可以直接得到这个类的成员变量对象：Field
④在运行时,可以直接得到这个类的成员方法对象：Method
⑤这种运行时动态获取类信息以及动态调用类中成分的能力称为Java语言的反射机制。

反射的第一步都是先得到编译后的Class类对象，然后就可以得到Class的全部成分。
获取Class类的对象的三种方式
方式一：Class c1 = Class.forName(“全类名”);
方式二：Class c2 = 类名.class
方式三：Class c3 = 对象.getClass();

Class类中用于获取构造器的方法
getConstructors()：返回所有构造器对象的数组（只能拿public的）
getDeclaredConstructors()：返回所有构造器对象的数组，存在就能拿到
getConstructor（参数类型.Class ....）：返回单个构造器对象（只能拿public的）
getDeclaredConstructor（参数类型.Class ....）：返回单个构造器对象，存在就能拿到

Class类中用于获取成员变量的方法
Field[] getFields()：返回所有成员变量对象的数组（只能拿public的）
Field[] getDeclaredFields()：返回所有成员变量对象的数组，存在就能拿到
Field getField(String name)：返回单个成员变量对象（只能拿public的）
Field getDeclaredField(String name)：返回单个成员变量对象，存在就能拿到
void set(Object obj, Object value)：赋值
Object get(Object obj)：获取值。

Class类中用于获取成员方法的方法
Method[] getMethods()：返回所有成员方法对象的数组（只能拿public的）
Method[] getDeclaredMethods()：返回所有成员方法对象的数组，存在就能拿到
Method getMethod(String name, Class<?>... parameterTypes) ：返回单个成员方法对象（public）
Method getDeclaredMethod(String name, Class<?>... parameterTypes)：返回单个成员方法对象，存在就能拿到
Object invoke(Object obj, Object... args)：运行方法
参数一：用obj对象调用该方法，如得到Student类的方法，就填Student对象
参数二：调用方法的传递的参数（如果没有就不写）
返回值：方法的返回值（如果没有就不写）

自定义注解：
public @interface 注解名称 {

    public 属性类型 属性名() default 默认值 ;

}
value属性，如果只有一个value属性的情况下，使用value属性的时候可以省略value名称不写

元注解：注解注解的注解
@Target: 约束自定义注解只能在哪些地方使用，
TYPE，类，接口
FIELD, 成员变量
METHOD, 成员方法
PARAMETER, 方法参数
CONSTRUCTOR, 构造器
LOCAL_VARIABLE, 局部变量
@Retention：申明注解的生命周期
SOURCE： 注解只作用在源码阶段，生成的字节码文件中不存在
CLASS：   注解作用在源码阶段，字节码文件阶段，运行阶段不存在，默认值.
RUNTIME：注解作用在源码阶段，字节码文件阶段，运行阶段（开发常用）

注解的解析：
注解的操作中经常需要进行解析，注解的解析就是判断是否存在注解，存在注解就解析出内容。
Annotation: 注解的顶级接口，注解都是Annotation类型的对象
AnnotatedElement：该接口定义了与注解解析相关的解析方法
Annotation[] getDeclaredAnnotations()：获得当前对象上使用的所有注解，返回注解数组。
T getDeclaredAnnotation(Class<T> annotationClass)：根据注解类型获得对应注解对象
boolean isAnnotationPresent(Class<Annotation> annotationClass)：判断当前对象是否使用了指定的注解，如果使用了则返回true，否则false

解析注解的技巧
①注解在哪个成分上，我们就先拿哪个成分对象。
②比如注解作用成员方法，则要获得该成员方法对应的Method对象，再来拿上面的注解
③比如注解作用在类上，则要该类的Class对象，再来拿上面的注解
④比如注解作用在成员变量上，则要获得该成员变量对应的Field对象，再来拿上面的注解

动态代理：代理就是被代理者没有能力或者不愿意去完成某件事情，需要找个人代替自己去完成这件事，动态代理就是用来对业务功能（方法）进行代理的。
①必须有接口，实现类要实现接口（代理通常是基于接口实现的）。
②创建一个实现类的对象，该对象为业务对象，紧接着为业务对象做一个代理对象。

代理类：Proxy
Public static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces,
InvocationHandle h)：为对象返回一个代理对象
用方法返回代理对象时，返回对象类型应该用接口类
参数一：定义加载类（被代理类）的类加载器
参数二：代理类要实现的接口列表
参数三：（用匿名内部类的方式new一个InvocationHandle对象）将方法调用分配到的处理程序（代理对象的核心处理程序）

动态代理的优点：
①可以在不改变方法源码的情况下，实现对方法功能的增强，提高了代码的复用性
②简化了编程工作，提高了编程效率，同时提高了软件系统的可扩展性
③可以为被代理对象的所有方法做代理
④非常的灵活，支持任意接口类型的实现类对象做代理，也可以直接为接口本身做代理

设计模式：
工厂设计模式：可以封装对象的创建细节，可以实现类与类之间的解耦操作
装饰模式：创建一个新类，包装原始类，从而在新类中提升原来类的功能
作用：不改变原类的基础上，动态地扩展一个类的功能
①定义父类②定义原始类，继承父类，定义功能③定义装饰类，继承父类，包装原始类，增强功能

commons-lang包共包含了17个实用的类：

- ArrayUtils：用于对数组的操作，如添加、查找、删除、子数组、倒序、元素类型转换等；
- BitField：用于操作位元，提供了一些方便而安全的方法；
- BooleanUtils：用于操作和转换boolean或者Boolean及相应的数组；
- CharEncoding：包含了Java环境支持的字符编码，提供是否支持某种编码的判断；
- CharRange：用于设定字符范围并做相应检查；
- CharSet：用于设定一组字符作为范围并做相应检查；
- CharSetUtils – 用于操作CharSet；
- CharUtils：用于操作char值和Character对象；
- ClassUtils：用于对Java类的操作，不使用反射；
- ObjectUtils：用于操作Java对象，提供null安全的访问和其他一些功能；
- RandomStringUtils：用于生成随机的字符串；
- SerializationUtils：用于处理对象序列化，提供比一般Java序列化更高级的处理能力；
- StringEscapeUtils：用于正确处理转义字符，产生正确的Java、JavaScript、HTML、XML和SQL代码；
- StringUtils：处理String的核心类，提供了相当多的功能；
- SystemUtils：在java.lang.System基础上提供更方便的访问，如用户路径、Java版本、时区、操作系统等判断；
- Validate：提供验证的操作，有点类似assert断言；
- WordUtils：用于处理单词大小写、换行等。