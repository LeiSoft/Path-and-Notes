一、概述
在程序员的日常工作中，经常会需要处理集合数据，过滤、对象转化、数据统计等。jdk8新增stream 流式编程，提供了很多方便处理集合数据的api，使得对于集合的处理更加高效简洁，提升开发效率。第一次在项目中看到lambda+stream的写法时我也是一脸懵逼，在学习了之后才发现，stream真香！代码清晰精简，从此告别for循环！下面一览其特性及常见API。

二、lambda表达式
先来上一段代码，演示lambda表达式的使用。


    public static void main(String[] args) throws Exception {
        Comparator<String> comparator = (s1, s2) -> s1.length() - s2.length();
        //测试一
        testComparator(comparator, "1", "11");
        //测试二
        testComparator((s1, s2) -> s1.length() - s2.length(),"1", "11");
    }
    
    public static void testComparator(Comparator<String> comparator, String s1, String s2){
        System.out.println(comparator.compare(s1, s2));
    }

测试一：通过lambda表达式创建了一个比较器。测试二：lambda表达式也可以直接作为方法参数，这样更加简化，这也是用的最多的一种方式。很多人说java8支持传递一个方法作为参数，这句话中所说的参数就是lambda表达式作为参数，比如 testComparator((s1, s2) -> s1.length() - s2.length(),“1”, “11”)。那么为什么能够实现传递一个方法呢？其实lambda表达式就是一个函数接口的实现类。比如上面的testComparator这个方法，它的第一个入参其实是Comparator接口，既然是接口，那么在使用的时候，只要传这个接口的实现类就行了。所以lambda表达式的本质就是一个对象，一个实现了一个函数接口的类的对象，传递方法作为参数的本质其实还是在传递一个对象，这就一点也不神秘了。

函数接口
上面说了，lambda表达式本质是自动的给你实现了一个接口，并且创建了对象。这个接口，必须要满足一定的规则：这个接口只有一个抽象方法。这种接口称为函数接口。为啥会有这个规则呢？想想也是，如果有多个抽象方法，那么如何确定这个lambda表达式要实现的是那个方法呢？所以只好限定接口中只有一个抽象方法。一般会在函数接口上加上@FunctionInterface注解，该注解的作用是用于编译器进行函数接口检查，如果加上该注解，却不是一个函数接口，则会报错。

类型推断
在使用lambda表达式时，我们仅仅是写了一个表达式而已，具体要实现什么接口，都是通过类型推断得出的。

方法引用
方法引用是lambda表达式的进一步简化，如果lambda表达式做的事情仅仅是调用一个已经存在的方法，并且出参入参和lambda表达式要实现的函数接口一致，那么可以用方法引用代替lambda表达式，使得语言更加简练。比如使用类名::静态方法名，类名::实例方法，实例::实例方法，构造方法::new。

二、Stream特性
1.无存储
stream不是一种新的数据结构，相当一个视图，对stream的修改并不会影响到原来的数据源。

2.函数式编程
使用stream的过程就是函数式编程的过程，在使用stream的过程中能充分体会函数式编程的简洁与高效。

3.惰式执行
对于中间操作，并没有立即执行，而是在结束操作时才真正遍历并执行中间操作。stream的执行并不是每个操作就执行遍历，而是在遇到结束操作时，一并执行，遍历一次。

4.单次消费
stream只能被消费一次，每次消费会产生新的stream，后续的操作基于新的stream来的。

三、常用操作
先创建一些数据用于举例

public class Student {
    private Long id;

    private String name;
    
    private Integer age;
    
    private String desc;
}
    
    private static List<Student> getStudent(){
        Student zhangsan  = new Student(3L, "张三", 18, "张三-desc");
        return Arrays.asList(
                zhangsan ,
                zhangsan,
                new Student(1L,"李四",20, "李四-desc"),
                new Student(2L,"王五",19, "王五-desc")
        );
    }
1.创建Stream

```
//集合接口有一个默认方法
Collection.stream()
//根据数组生成stream
Arrays.stream(T[])
//根据可变参数生产stream
Stream.of(T...)
//特定stream
IntStream.range()
...
```


2.中间操作
中间操作，在调用中间操作时，并没有立即遍历stream，只是做了标记，在结束操作时才会真正的遍历stream。

map
执行映射操作，将一个元素转换成另一个元素

```
<R> Stream<R> map(Function<? super T, ? extends R> mapper);
Function 接口，接受一个参数，返回另一个元素；在stream中执行map操作，可以将元素转换另一个元素。
//举例，获取学生列表的id。
List<Long> ids = getStudent().stream().map(s -> s.getId()).collect(Collectors.toList());
```

filter
执行过滤操作，满足条件的将保留，否则丢弃。

```
Stream<T> filter(Predicate<? super T> predicate);
Predicate 接口，接受一个参数，返回一个boolean值；在filter中，如果Predicate返回值为true，则该元素被保留，否则被丢弃。
//举例，获取年龄大于19的学生
List<Student> students = getStudent().stream.filter(s -> s.getAge() > 19).collect(Collectors.toList());

```

distinct
执行去重操作，根据Object.equals()方法

```
List<Student> students = getStudent().stream().distinct().collect(Collectors.toList());
```

peek
执行遍历操作，与下文forEach的区别在于，peek遍历stream不会中断stream，后续可以继续对原来的stream操作，而forEach会结束stream，可以看到peek返回值为stream，而forEach无返回值。

```
Stream<T> peek(Consumer<? super T> action);
//其实这里可以通过map一步完成，这里只是为了举例说明peek后仍然能操作该stream
List<String> desc = students.stream().peek(s -> s.setDesc(s.getDesc() + "-end")).map(Student::getDesc).collect(Collectors.toList());



```


limit
限制元素个数，前面的n个被保留，后面的丢弃

```
Stream<T> limit(long maxSize);
//取前3个
List<Student> collect = students.stream().limit(3).collect(Collectors.toList());
```


skip
跳过元素个数，前面的n个被跳过，后面的保留

```
//除了前两个，剩下的都要
Stream<T> skip(long n);
List<Student> collect = students.stream().skip(2).collect(Collectors.toList());
```


sorted
排序，要么排序元素实现Comparable接口，要么传一个Comparator进去

```
//按照年龄从小到大排序
List<Student> collect = students.stream().sorted(Comparator.comparing(Student::getAge)).collect(Collectors.toList());
```

flatMap
扁平化转化，将流中的每一个元素都转换成一个流，并将这些流拼接在一起，返回成一个新的流。

```
List<List<String>> list = new ArrayList<>();
list.add(Arrays.asList("hello1","world1"));
list.add(Arrays.asList("hello2","world2"));
List<String> collect = list.stream().flatMap(List::stream).collect(Collectors.toList());
//输出: [hello1, world1, hello2, world2]
System.out.println(collect);

mapToInt
将流中的元素转换成int类型，同理有mapToInt，mapToLong，mapToDouble

List<Student> students = getStudent();
        OptionalInt max = 
//求年龄最大值
students.stream().mapToInt(Student::getAge).max();
        if (max.isPresent()){
            System.out.println(max);
        }
```

3.结束操作
真正的遍历stream，调用中间操作方法，并根据结束操作收集结果。

forEach
执行遍历操作，无返回值

```
void forEach(Consumer<? super T> action);
Consumer 接口，接受一个参数，无返回值
//示例,遍历输出元素
getStudent().stream().forEach(System.out::println);

collect
该方法的作用是收集元素，传入不同的收集器参数，可以进行不同的收集。可以通过各种方式来收集流中的元素，Collectors提供了丰富的静态方法来创建各种类型的Collector，详见下文的Collector。

<R, A> R collect(Collector<? super T, A, R> collector);

reduce
直译为减少，实际上就是对所有元素进行聚合，通过一组元素生成一个值。至于怎么聚合，取决于传入的参数。reduce有三个重载的方法，其中有一个拥有三个参数的方法，第三个参数在并行流的情况下才有实质性的作用，用于合并结果。

Stream<Integer> stream = Stream.of(1, 2, 3, 4);
int init = 5;
//初始值为5，对stream中的元素进行求和
Integer reduce = stream.reduce(init, (i1, i2) -> {
    return i1 + i2;
});
```

```
//输出 15
System.out.println(reduce);
```

anyMatch
只要一个匹配即返回true

allMatch
所有元素匹配才返回true

noneMatch
没有一个元素匹配则返回true

findAny
返回集合中任意一个

findFirst
返回第一个元素

max
求最大值，如果不是数字类型需要传入一个Comparator

min
求最小值，如果不是数字类型需要传入一个Comparator

四、Collector
使用stream遍历后，通常需要以某种数据结构汇聚结果，这需要用到collect()方法，它需要一个Collector参数，Collectors中封装了很多常用的Collector。下面举例介绍。

1.常用collector介绍
Collectors.toCollection()

```
收集stream为集合，根据传入的Supplier生成收集元素的集合。像收集为List，收集为Set这两种常用的，java已经封装好了。

ArrayList<Student> list = students.stream().collect(Collectors.toCollection(ArrayList::new));
HashSet<Student> set = students.stream().collect(Collectors.toCollection(HashSet::new));
Set<Student> studentSet = students.stream().collect(Collectors.toSet());
```

Collectors.toMap()

```
收集stream为Map，toMap有三个重载方法

public static <T, K, U, M extends Map<K, U>> Collector<T, ?, M> toMap(
		 Function<? super T, ? extends K> keyMapper,   //生成key的规则
         Function<? super T, ? extends U> valueMapper, //生成value的规则
         BinaryOperator<U> mergeFunction,              //如果出现重复的key，value之间的合并规则，默认抛出异常
         Supplier<M> mapSupplier                       //如何生成一个Map
         )

//例子，收集student为map，key为id，value为Student对象
Map<Long, Student> map = student.stream().collect(Collectors.toMap(Student::getId, Function.identity()));
```

Collectors.joining()

```
将字符串stream收集成一个字符串，joining提供了拼接方法

Stream<String> stream = Stream.of("aaa", "bbb", "ccc");
String str = stream.collect(Collectors.joining("-", "[", "]"));
//输出：[aaa-bbb-ccc]
System.out.println(str);
```

Collectors.groupingBy()

```
将stream中的元素进行分组，并且可以对分组后的每一组元素再进行个性化的收集。

Collector<T, ?, M> groupingBy(
		Function<? super T, ? extends K> classifier, //分组规则，以什么分组
        Supplier<M> mapFactory,                      //如何生成map
        Collector<? super T, A, D> downstream)       //下游收集器，指定对分组后的每组元素如何收集，再次传入一个Collector
        
Stream<String> stream = Stream.of("a", "b", "cc", "dd", "eee", "fff");
//按字符串长度分组
Map<Integer, List<String>> map = stream.collect(Collectors.groupingBy(String::length, Collectors.toList()));
//输出：{1=[a, b], 2=[cc, dd], 3=[eee, fff]}
System.out.println(map);
```



Collectors.partitioningBy()

```
按某一种条件分组，参数为一个Predicate，返回boolean值。概念类似上面的groupingBy，只不过这里只有两组，true和false各一组。

Stream<String> stream = Stream.of("a", "b", "cc", "dd", "eee", "fff");
//将字符串按长度是否大于二，分成两组
Map<Boolean, List<String>> strLenMap = stream.collect(Collectors.partitioningBy(s -> s.length() > 2));
//输出：{false=[a, b, cc, dd], true=[eee, fff]}
System.out.println(strLenMap);

Collectors.summingInt()
求和，收集stream中的元素为一个Integer，相当于先进行mapToInt，再执行sun()。

Stream<String> stream = Stream.of("a", "b", "cc", "dd", "eee", "fff");
Integer lenTotal = stream.collect(Collectors.summingInt(String::length));
//输出:12
System.out.println(lenTotal);
```


Collectors.averagingInt()

```
求均值，收集stream中的元素为一个Double，相当于先执行mapToDouble，再执行average()。

Stream<String> stream = Stream.of("a", "b", "cc", "dd", "eee", "fff");
Double lenAvg = stream.collect(Collectors.averagingInt(String::length));
//输出:2.0
System.out.println(lenAvg);

Collectors.summarizingInt()
统计，将stream元素收集为一个IntSummaryStatistics，提供多种统计计算。同理有summarizingDouble()，summarizingLong()。

Stream<String> stream = Stream.of("a", "b", "cc", "dd", "eee", "fff");
IntSummaryStatistics lenStatistics = stream.collect(Collectors.summarizingInt(String::length));
System.out.println("count:" + lenStatistics.getCount());
System.out.println("maxLen:" + lenStatistics.getMax());
System.out.println("minLen:" + lenStatistics.getMin());
System.out.println("avgLen:" + lenStatistics.getAverage());
System.out.println("totalLen:" + lenStatistics.getSum());
/*
	输出：
		count:6
		maxLen:3
		minLen:1
		avgLen:2.0
		totalLen:12
*/
```

Collectors.collectAndThen()

```
collectAndThen的第一个参数是Collector，第二个是Function。先使用Collector进行收集，然后将收集后的结果传入Funtion进行二次处理。

Stream<String> stream = Stream.of("a", "b", "cc", "dd", "eee", "fff");
//先将stream收集为List，在用Collections.unmodifiableList对其进行包装
List<String> list = stream.collect(Collectors.collectingAndThen(Collectors.toList(), Collections::unmodifiableList));
//输出: class java.util.Collections$UnmodifiableRandomAccessList
System.out.println(list.getClass());

```

