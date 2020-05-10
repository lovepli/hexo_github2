---
title: java 代码精简之道
date: 2020-05-09 01:43:00
tags: 编码规范
categories: 编码规范

# java 代码精简之道
---

## Java 代码精简之道 | 长文

常意 [朱小厮的博客](<javascript:void(0);>)

**朱小厮的博客**

微信号 hiddenkafka

功能介绍 著有畅销书：《深入理解 Kafka》和《RabbitMQ 实战指南》。公众号主要用来分享 Java 技术栈、Golang 技术栈、消息中间件（如 Kafka、RabbitMQ）、存储、大数据以及通用型技术架构等相关的技术。

点击上方“朱小厮的博客”，选择“设为星标”

后台回复"加群"，加入新技术群

来源：阿里巴巴中间件

古语有云：

> 道为术之灵，术为道之体；以道统术，以术得道。

其中：“道”指“规律、道理、理论”，“术”指“方法、技巧、技术”。意思是：“道”是“术”的灵魂，“术”是“道”的肉体；可以用“道”来统管“术”，也可以从“术”中获得“道”。工匠追求“术”到极致，其实就是在寻“道”，且离悟“道”也就不远了，亦或是已经得道，这就是“工匠精神”——一种追求“以术得道”的精神。如果一个工匠只满足于“术”，不能追求“术”到极致去悟“道”，那只是一个靠“术”养家糊口的工匠而已。作者根据多年来的实践探索，总结了大量的 Java 代码精简之“术”，试图阐述出心中的 Java 代码精简之“道”。

## **1.利用语法**

---

**1.1.利用三元表达式**

**普通：**

    String title;

**精简：**

    String title = isMember(phone) ? "会员" : "游客";

注意：对于包装类型的算术计算，需要注意避免拆包时的空指针问题。

### **1.2.利用 for-each 语句**

从 Java 5 起，提供了 for-each 循环，简化了数组和集合的循环遍历。 for-each  循环允许你无需保持传统 for 循环中的索引就可以遍历数组，或在使用迭代器时无需在 while 循环中调用 hasNext 方法和 next 方法就可以遍历集合。

**普通：**

    double[] values = ...;

**精简：**

    double[] values = ...;

### **1.3.利用 try-with-resource 语句**

所有实现 Closeable 接口的“资源”，均可采用 try-with-resource 进行简化。

**普通：**

    BufferedReader reader = null;

**精简：**

    try (BufferedReader reader = new BufferedReader(new FileReader("test.txt"))) {

### **1.4.利用 return 关键字**

利用 return 关键字，可以提前函数返回，避免定义中间变量。

**普通：**

    public static boolean hasSuper(@NonNull List<UserDO> userList) {

**精简：**

    public static boolean hasSuper(@NonNull List<UserDO> userList) {

### **1.5.利用 static 关键字**

利用 static 关键字，可以把字段变成静态字段，也可以把函数变为静态函数，调用时就无需初始化类对象。

**普通：**

    public final class GisHelper {

**精简：**

    public final class GisHelper {

### **1.6.利用 lambda 表达式**

Java 8 发布以后，lambda 表达式大量替代匿名内部类的使用，在简化了代码的同时，更突出了原有匿名内部类中真正有用的那部分代码。

**普通：**

    new Thread(new Runnable() {

**精简：**

    new Thread(() -> {

### **1.7.利用方法引用**

方法引用（::），可以简化 lambda 表达式，省略变量声明和函数调用。

**普通：**

    Arrays.sort(nameArray, (a, b) -> a.compareToIgnoreCase(b));

**精简：**

    Arrays.sort(nameArray, String::compareToIgnoreCase);

### **1.8.利用静态导入**

静态导入（import static），当程序中大量使用同一静态常量和函数时，可以简化静态常量和函数的引用。

**普通：**

    List<Double> areaList = radiusList.stream().map(r -> Math.PI * Math.pow(r, 2)).collect(Collectors.toList());

**精简：**

    import static java.lang.Math.PI;

注意：静态引入容易造成代码阅读困难，所以在实际项目中应该警慎使用。

### **1.9.利用 unchecked 异常**

Java 的异常分为两类：Checked 异常和 Unchecked 异常。Unchecked 异常继承了 RuntimeException ，特点是代码不需要处理它们也能通过编译，所以它们称作   Unchecked 异常。利用 Unchecked 异常，可以避免不必要的 try-catch 和 throws 异常处理。

**普通：**

    @Service

**精简：**

    @Service

## **2.利用注解**

---

**2.1.利用 Lombok 注解**

Lombok 提供了一组有用的注解，可以用来消除 Java 类中的大量样板代码。

**普通：**

    public class UserVO {

**精简：**

    @Getter

### **2.2.利用 Validation 注解**

**普通：**

    @Getter@Setter@ToStringpublic class UserCreateVO {    @NotBlank(message = "用户名称不能为空")    private String name;    @NotNull(message = "公司标识不能为空")    private Long companyId;    ...}@Service@Validatedpublic class UserService {    public Long createUser(@Valid UserCreateVO create) {        // TODO: 创建用户        return null;    }}

‍

**精简：**

    @Getter

###

### **2.3.利用 @NonNull 注解**

Spring 的 @NonNull 注解，用于标注参数或返回值非空，适用于项目内部团队协作。只要实现方和调用方遵循规范，可以避免不必要的空值判断，这充分体现了阿里的“新六脉神剑”提倡的“因为信任，所以简单”。

**普通：**

    public List<UserVO> queryCompanyUser(Long companyId) {

**精简：**

    public @NonNull List<UserVO> queryCompanyUser(@NonNull Long companyId) {

###

### **2.4.利用注解特性**

注解有以下特性可用于精简注解声明：

1、当注解属性值跟默认值一致时，可以删除该属性赋值；

2、当注解只有 value 属性时，可以去掉 value 进行简写；

3、当注解属性组合等于另一个特定注解时，直接采用该特定注解。

**普通：**

    @Lazy(true);

**精简：**

    @Lazy

## **3.利用泛型**

---

**3.1.泛型接口**

在 Java 没有引入泛型前，都是采用 Object 表示通用对象，最大的问题就是类型无法强校验并且需要强制类型转换。

**普通：**

    public interface Comparable {

**精简：**

    public interface Comparable<T> {

### **3.2.泛型类**

**普通：**

    @Getter

**精简：**

    @Getter

###

### **3.3.泛型方法**

**普通：**

    public static Map<String, Integer> newHashMap(String[] keys, Integer[] values) {

**精简：**

    public static <K, V> Map<K, V> newHashMap(K[] keys, V[] values) {

## **4.利用自身方法**

---

**4.1.利用构造方法**

构造方法，可以简化对象的初始化和设置属性操作。对于属性字段较少的类，可以自定义构造方法。

**普通：**

    @Getter

**精简：**

    @Getter

注意：如果属性字段被替换时，存在构造函数初始化赋值问题。比如把属性字段 title 替换为 nickname ，由于构造函数的参数个数和类型不变，原有构造函数初始化语句不会报错，导致把原 title 值赋值给 nickname 。如果采用 Setter 方法赋值，编译器会提示错误并要求修复。

### **4.2.利用 Set 的 add 方法**

利用 Set 的 add 方法的返回值，可以直接知道该值是否已经存在，可以避免调用 contains 方法判断存在。

**普通：**

以下案例是进行用户去重转化操作，需要先调用 contains 方法判断存在，后调用 add 方法进行添加。

    Set<Long> userIdSet = new HashSet<>();

**精简：**

    SSet<Long> userIdSet = new HashSet<>();

‍

###

### **4.3.利用 Map 的 computeIfAbsent \*\***方法\*\*

利用 Map 的 computeIfAbsent 方法，可以保证获取到的对象非空，从而避免了不必要的空判断和重新设置值。

**普通：**

    Map<Long, List<UserDO>> roleUserMap = new HashMap<>();

**精简：**

    Map<Long, List<UserDO>> roleUserMap = new HashMap<>();

###

### **4.4.利用链式编程**

链式编程，也叫级联式编程，调用对象的函数时返回一个 this 对象指向对象本身，达到链式效果，可以级联调用。链式编程的优点是：编程性强、可读性强、代码简洁。

**普通：**

    StringBuilder builder = new StringBuilder(96);

**精简：**

    StringBuilder builder = new StringBuilder(96);

## **5.利用工具方法**

---

**5.1.避免空值判断**

**普通：**

    if (userList != null && !userList.isEmpty()) {

**精简：**

    if (CollectionUtils.isNotEmpty(userList)) {

###

### **5.2.避免条件判断**

**普通：**

    double result;

**精简：**

    double result = Math.max(MIN_LIMIT, value);

###

### **5.3.简化赋值语句**

**普通：**

    public static final List<String> ANIMAL_LIST;

**精简：**

    // JDK流派

注意：Arrays.asList 返回的 List 并不是 ArrayList ，不支持 add 等变更操作。

### **5.4.简化数据拷贝**

**普通：**

    UserVO userVO = new UserVO();

**精简：**

    UserVO userVO = new UserVO();

**反例：**

    List<UserVO> userVOList = JSON.parseArray(JSON.toJSONString(userDOList), UserVO.class);

精简代码，但不能以过大的性能损失为代价。例子是浅层拷贝，用不着 JSON 这样重量级的武器。

**5.5.简化异常断言**

**普通：**

    if (Objects.isNull(userId)) {

**精简：**

    Assert.notNull(userId, "用户标识不能为空");

注意：可能有些插件不认同这种判断，导致使用该对象时会有空指针警告。

### **5.6.简化测试用例**

把测试用例数据以 JSON 格式存入文件中，通过 JSON 的 parseObject 和 parseArray 方法解析成对象。虽然执行效率上有所下降，但可以减少大量的赋值语句，从而精简了测试代码。

**普通：**

    @Test

**精简：**

    @Test

建议：JSON 文件名最好以被测试的方法命名，如果有多个版本可以用数字后缀表示。

### **5.7.简化算法实现**

一些常规算法，已有现成的工具方法，我们就没有必要自己实现了。

**普通：**

    int totalSize = valueList.size();

**精简：**

    List<List<Integer>> partitionList = ListUtils.partition(valueList, PARTITION_SIZE);

**5.8.封装工具方法**

一些特殊算法，没有现成的工具方法，我们就只好自己亲自实现了。

**普通：**

比如，SQL 设置参数值的方法就比较难用，setLong 方法不能设置参数值为 null 。

     // 设置参数值

**精简：**

我们可以封装为一个工具类 SqlHelper ，简化设置参数值的代码。

    /** SQL辅助类 */

## **6.利用数据结构**

---

**6.1.利用数组简化**

对于固定上下限范围的 if-else 语句，可以用数组+循环来简化。

**普通：**

    public static int getGrade(double score) {

**精简：**

    private static final double[] SCORE_RANGES = new double[] {90.0D, 80.0D, 60.0D, 30.0D};

思考：上面的案例返回值是递增的，所以用数组简化是没有问题的。但是，如果返回值不是递增的，能否用数组进行简化呢？答案是可以的，请自行思考解决。

### **6.2.利用 Map 简化**

对于映射关系的 if-else 语句，可以用 Map 来简化。此外，此规则同样适用于简化映射关系的 switch 语句。

**普通：**

    public static String getBiologyClass(String name) {

**精简：**

    private static final Map<String, String> BIOLOGY_CLASS_MAP

已经把方法简化为一行代码，其实都没有封装方法的必要了。

### **6.3.利用容器类简化**

Java 不像 Python 和 Go ，方法不支持返回多个对象。如果需要返回多个对象，就必须自定义类，或者利用容器类。常见的容器类有 Apache 的 Pair 类和 Triple 类， Pair 类支持返回 2 个对象， Triple 类支持返回 3 个对象。

**普通：**

    @Setter

**精简：**

    public static Pair<Point, Double> getNearest(Point point, Point[] points) {

###

### **6.4.利用 ThreadLocal 简化**

ThreadLocal 提供了线程专有对象，可以在整个线程生命周期中随时取用，极大地方便了一些逻辑的实现。用 ThreadLocal 保存线程上下文对象，可以避免不必要的参数传递。

**普通：**

由于 DateFormat 的 format 方法线程非安全（建议使用替代方法），在线程中频繁初始化 DateFormat 性能太低，如果考虑重用只能用参数传入 DateFormat 。例子如下：

    public static String formatDate(Date date, DateFormat format) {

**精简：**

可能你会觉得以下的代码量反而多了，如果调用工具方法的地方比较多，就可以省下一大堆 DateFormat 初始化和传入参数的代码。

    private static final ThreadLocal<DateFormat> LOCAL_DATE_FORMAT = new ThreadLocal<DateFormat>() {

注意：ThreadLocal 有一定的内存泄露的风险，尽量在业务代码结束前调用 remove 方法进行数据清除。

## **7.利用  Optional**

---

在  Java 8  里，引入了一个  Optional  类，该类是一个可以为  null  的容器对象。

### **7.1.保证值存在**

**普通：**

    Integer thisValue;

**精简：**

    Integer thisValue = Optional.ofNullable(value).orElse(DEFAULT_VALUE);

### **7.2.保证值合法**

**普通：**

    Integer thisValue;

**精简：**

    Integer thisValue = Optional.ofNullable(value)

**7.3.避免空判断**

**普通：**

    String zipcode = null;

**精简：**

    tring zipcode = Optional.ofNullable(user).map(User::getAddress)

## **8.利用 Stream**

---

流（Stream）是 Java 8 的新成员，允许你以声明式处理数据集合，可以看成为一个遍历数据集的高级迭代器。流主要有三部分构成：获取一个数据源 → 数据转换 → 执行操作获取想要的结果。每次转换原有 Stream 对象不改变，返回一个新的 Stream 对象，这就允许对其操作可以像链条一样排列，形成了一个管道。流（Stream）提供的功能非常有用，主要包括匹配、过滤、汇总、转化、分组、分组汇总等功能。

### **8.1.匹配集合数据**

**普通：**

    boolean isFound = false;

**精简：**

    boolean isFound = userList.stream()

**8.2.过滤集合数据**

**普通：**

    List<UserDO> resultList = new ArrayList<>();

**精简：**

    List<UserDO> resultList = userList.stream()

**8.3.汇总集合数据**

**普通：**

    double total = 0.0D;

**精简：**

    double total = accountList.stream().mapToDouble(Account::getBalance).sum();

**8.4.转化集合数据**

**普通：**

    List<UserVO> userVOList = new ArrayList<>();

**精简：**

    List<UserVO> userVOList = userDOList.stream()

### **8.5.分组集合数据**

**普通：**

    Map<Long, List<UserDO>> roleUserMap = new HashMap<>();

**精简：**

    Map<Long, List<UserDO>> roleUserMap = userDOList.stream()

**8.6.分组汇总集合**

**普通：**

    Map<Long, Double> roleTotalMap = new HashMap<>();

**精简：**

    roleTotalMap = accountList.stream().collect(Collectors.groupingBy(Account::getRoleId, Collectors.summingDouble(Account::getBalance)));

**8.7.生成范围集合**

Python 的 range 非常方便，Stream 也提供了类似的方法。

**普通：**

    int[] array1 = new int[N];

精简：

    int[] array1 = IntStream.rangeClosed(1, N).toArray();

## **9.利用程序结构**

---

**9.1.返回条件表达式**

条件表达式判断返回布尔值，条件表达式本身就是结果。

**普通：**

    public boolean isSuper(Long userId)

**精简：**

    public boolean isSuper(Long userId)

**9.2.最小化条件作用域**

最小化条件作用域，尽量提出公共处理代码。

**普通：**

    Result result = summaryService.reportWorkDaily(workDaily);

**精简：**

    String message;

### **9.3.调整表达式位置**

调整表达式位置，在逻辑不变的前提下，让代码变得更简洁。

**普通 1：**

    String line = readLine();

**普通 2：**

    for (String line = readLine(); Objects.nonNull(line); line = readLine()) {

**精简：**

    String line;

注意：有些规范可能不建议这种精简写法。

### **9.4.利用非空对象**

在比较对象时，交换对象位置，利用非空对象，可以避免空指针判断。

**普通：**

    private static final int MAX_VALUE = 1000;

**精简：**

    private static final Integer MAX_VALUE = 1000;

## **10.利用设计模式**

---

**10.1.模板方法模式**

模板方法模式（Template Method Pattern）定义一个固定的算法框架，而将算法的一些步骤放到子类中实现，使得子类可以在不改变算法框架的情况下重定义该算法的某些步骤。

**普通：**

    @Repository

**精简：**

    public abstract class AbstractDynamicValue<I, V> {

###

### **10.2.建造者模式**

建造者模式（Builder Pattern）将一个复杂对象的构造与它的表示分离，使同样的构建过程可以创建不同的表示，这样的设计模式被称为建造者模式。

**普通：**

    public interface DataHandler<T> {

**精简：**

    public <T> long executeFetch(String tableName, int batchSize, Function<Record, T> dataParser, Function<List<T>, Boolean> dataStorage) throws Exception {

普通的建造者模式，实现时需要定义 DataHandler 接口，调用时需要实现 DataHandler 匿名内部类，代码较多较繁琐。而精简后的建造者模式，充分利用了函数式编程，实现时无需定义接口，直接使用 Function 接口；调用时无需实现匿名内部类，直接采用 lambda 表达式，代码较少较简洁。

### **10.3.代理模式**

Spring 中最重要的代理模式就是 AOP (Aspect-Oriented Programming，面向切面的编程)，是使用 JDK 动态代理和 CGLIB 动态代理技术来实现的。

**普通：**

    @Slf4j

**精简 1：**

基于   @ControllerAdvice 的异常处理：

    @RestController

**精简 2：**

基于 AOP 的异常处理：

    // UserController代码同"精简1"

## **11.利用删除代码**

---

“少即是多”，“少”不是空白而是精简，“多”不是拥挤而是完美。删除多余的代码，才能使代码更精简更完美。

### **11.1.删除已废弃的代码**

删除项目中的已废弃的包、类、字段、方法、变量、常量、导入、注解、注释、已注释代码、Maven 包导入、MyBatis 的 SQL 语句、属性配置字段等，可以精简项目代码便于维护。

**普通：**

    import lombok.extern.slf4j.Slf4j;

**精简：**

    @Service

### **11.2.删除接口方法的 public**

对于接口(interface)，所有的字段和方法都是 public 的，可以不用显式声明为 public 。

**普通：**

    public interface UserDAO {

**精简：**

    public interface UserDAO {

###

### **11.3.删除枚举构造方法的 private**

对于枚举(menu)，构造方法都是 private 的，可以不用显式声明为 private 。

**普通：**

    public enum UserStatus {

**精简：**

    public enum UserStatus {

###

### **11.4.删除 final 类方法的 final**

对于 final 类，不能被子类继承，所以其方法不会被覆盖，没有必要添加 final 修饰。

**普通：**

    public final Rectangle implements Shape {

**精简：**

    public final Rectangle implements Shape {

###

### **11.5.删除基类 implements 的接口**

如果基类已 implements 某接口，子类没有必要再 implements 该接口，只需要直接实现接口方法即可。

**普通：**

    public interface Shape {

**精简：**

    ...

###

### **11.6.删除不必要的变量**

不必要的变量，只会让代码看起来更繁琐。

**普通：**

    public Boolean existsUser(Long userId) {

**精简：**

    public Boolean existsUser(Long userId) {

**想知道更多？\*\***扫\***\*描下面的二维码关注我**

后台回复”加群“获取公众号专属群聊入口

【原创系列 | 精彩推荐】

- ## [Paxos、Raft 不是一致性算法嘛？](http://mp.weixin.qq.com/s?__biz=MzU0MzQ5MDA0Mw==&mid=2247489451&idx=1&sn=fea7a5138f0497015532e8cf6bec3c37&chksm=fb0bfd3fcc7c74291b91eb5800dce0ea393baf1b2c5c4f91de2067785431f607a6b794f4ceb9&scene=21#wechat_redirect)

- ## [越说越迷糊的 CAP](http://mp.weixin.qq.com/s?__biz=MzU0MzQ5MDA0Mw==&mid=2247489488&idx=1&sn=53409fced85f40494f38d703e91c6928&chksm=fb0bfd44cc7c7452ea1a71072b68b3b099cacaf73128e5a27a8c7258aacce8b72c8f717c0fd6&scene=21#wechat_redirect)

- ## [分布式事务科普——初识篇](http://mp.weixin.qq.com/s?__biz=MzU0MzQ5MDA0Mw==&mid=2247489849&idx=1&sn=cbac2a6ad99ac466f2ba8d69507fd2fe&chksm=fb0bf3adcc7c7abb565a9865e14b357888f7b7b78874b74c18bfdc5a4278ec2503b258c27730&scene=21#wechat_redirect)

- ## [分布式事务科普——终结篇](http://mp.weixin.qq.com/s?__biz=MzU0MzQ5MDA0Mw==&mid=2247489868&idx=1&sn=ef0b6afc60a89d49959ddb414ab20068&chksm=fb0bf3d8cc7c7ace7948f14b58d0a1562cf1449a2767964836eb47d669915206079760b0cb8e&scene=21#wechat_redirect)

- ## [面试官居然问我 Raft 为什么会叫做 Raft!](http://mp.weixin.qq.com/s?__biz=MzU0MzQ5MDA0Mw==&mid=2247489611&idx=1&sn=407f2a3d9e5190c89064752eeded6e5d&chksm=fb0bf2dfcc7c7bc93fed2f746b6f7c128ea6d56f33163bb6796c7bd3f4adf2f69acda4948c1a&scene=21#wechat_redirect)

- ## [面试官给我挖坑：URI 中的//有什么用](http://mp.weixin.qq.com/s?__biz=MzU0MzQ5MDA0Mw==&mid=2247489633&idx=1&sn=0616e86b4fb44df85c52f7422dad7b4e&chksm=fb0bf2f5cc7c7be34c48b555db5d704923319e43932ef5d65346c934b8daf7b6789d5eb15a16&scene=21#wechat_redirect)

- ## [面试官给我挖坑：a\[i\]\[j\]和 a\[j\]\[i\]有什么区别？](http://mp.weixin.qq.com/s?__biz=MzU0MzQ5MDA0Mw==&mid=2247489899&idx=1&sn=7646d24134d58bbd9abccb38efd5b4d7&chksm=fb0bf3ffcc7c7ae978923cbc0a699a30351ee5563b3143a1b7ff51a13442438d52afbfb0aae5&scene=21#wechat_redirect)

- [面试官给我挖坑：单机并发 TCP 连接数到底有多少？](http://mp.weixin.qq.com/s?__biz=MzU0MzQ5MDA0Mw==&mid=2247490002&idx=1&sn=ef4bb28e50a2375060efc46e55e27b77&chksm=fb0bf346cc7c7a50580c1b83c9c4d8a8d11b5018c6143c6444b7bfdb0fa06cef83497a708ba4&scene=21#wechat_redirect)

  ***

- ## [网关 Zuul 科普](http://mp.weixin.qq.com/s?__biz=MzU0MzQ5MDA0Mw==&mid=2247489653&idx=1&sn=b2ed7b657b67c147483571ae01cb9aae&chksm=fb0bf2e1cc7c7bf7cf67599cf5ced169f71724b3ce6d00280f05528d03b3d5b623d74ef617dd&scene=21#wechat_redirect)

- ## [网关 Spring Cloud Gateway 科普](http://mp.weixin.qq.com/s?__biz=MzU0MzQ5MDA0Mw==&mid=2247489737&idx=1&sn=84dbee4cc343848c78a8505cd3cdadbd&chksm=fb0bf25dcc7c7b4b2d155d14d3ba72b872242cdcc40e60bbce950ad8070655b53249b9c84deb&scene=21#wechat_redirect)

- ## [Nginx 架构原理科普](http://mp.weixin.qq.com/s?__biz=MzU0MzQ5MDA0Mw==&mid=2247489921&idx=1&sn=0137f8212d94062987d83767836514ac&chksm=fb0bf315cc7c7a031935a531d556274c3158869bfb6d37d37d6804cce9af9596b8c24cc97f03&scene=21#wechat_redirect)

- ## [OpenResty 概要及原理科普](http://mp.weixin.qq.com/s?__biz=MzU0MzQ5MDA0Mw==&mid=2247490049&idx=1&sn=47e297afd0cb938ab74936e8cc4cc905&chksm=fb0bf095cc7c7983af37418e1adf17fd9d0a0fddbaf5584af639d2fd5bf2fd422096764362c1&scene=21#wechat_redirect)

- ## [微服务网关 Kong 科普](http://mp.weixin.qq.com/s?__biz=MzU0MzQ5MDA0Mw==&mid=2247490120&idx=1&sn=d20244e6750782b33e13da44ae9b8b90&chksm=fb0bf0dccc7c79ca289f2ff6856570f9f1550f47ac196ac3d897051cee056aa8675f9c1ecc26&scene=21#wechat_redirect)

- [云原生网关 Traefik 科普](http://mp.weixin.qq.com/s?__biz=MzU0MzQ5MDA0Mw==&mid=2247490143&idx=1&sn=808e27a9eda2b685cb67d576bce3458b&chksm=fb0bf0cbcc7c79ddd264fdab4c933550015ed4af98cd3bab97fef36b9fda6391cbc4b1306282&scene=21#wechat_redirect)

  ***

**朕已阅  **

var first_sceen\_\_time = (+new Date()); if ("" == 1 && document.getElementById('js_content')) { document.getElementById('js_content').addEventListener("selectstart",function(e){ e.preventDefault(); }); } (function(){ if (navigator.userAgent.indexOf("WindowsWechat") != -1){ var link = document.createElement('link'); var head = document.getElementsByTagName('head')\[0\]; link.rel = 'stylesheet'; link.type = 'text/css'; link.href = "//res.wx.qq.com/mmbizwap/zh_CN/htmledition/style/page/appmsg_new/winwx4c4161.css"; head.appendChild(link); } })();

[阅读原文](##)

阅读

在看

已同步到看一看[写下你的想法](javascript:;)

前往“发现”-“看一看”浏览“朋友在看”

![](//res.wx.qq.com/mmbizwap/zh_CN/htmledition/images/pic/appmsg/pic_like_comment492329.png)

前往看一看

**看一看入口已关闭**

在“设置”-“通用”-“发现页管理”打开“看一看”入口

[我知道了](javascript:;)

已发送

取消

#### 发送到看一看

发送

Java 代码精简之道 | 长文

最多 200 字，当前共字

发送中

相关阅读

[

更多文章

](javascript:;)

[查看更多相关内容](javascript:;)

[

更多文章

](javascript:;)

[查看更多相关内容](javascript:;)

正在加载

以上推荐为优质及原创文章

微信扫一扫  
关注该公众号

微信扫一扫  
使用小程序

[取消](<javascript:void(0);>) [允许](<javascript:void(0);>)

[取消](<javascript:void(0);>) [允许](<javascript:void(0);>)

**微信版本过低**

当前微信版本不支持该功能，请升级至最新版本。

[我知道了](<javascript:void(0);>) [前往更新](<javascript:void(0);>)

确定删除回复吗？

[取消](javascript:;) [删除](javascript:;)

[知道了](javascript:;)

**长按识别前往小程序**

## 目标

去除 iconfinder 上 icon 的水印

### 原理

利用水印像素点和原图像素点颜色合并的原理，如果拥有加过水印的图片和水印图片，就可以反向推出原图原像素点的颜色；前提是你得拥有他的水印图片
