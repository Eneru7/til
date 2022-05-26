Java Stream

# 概念

> 允许以声明性方式处理数据集合

# 特性

> 1. 代码简洁：函数式编程写出的代码简介且意图明确，使用stream接口可以避免使用for循环
> 2. 多核友好：Java函数式编程使得编写并行程序更简单，只需要调用一下方法

# 流程

> 1. 把集合转换城流stream
>
> 2. 操作stream流
>
>    流在管道中经过中间操作（intermediate operation)的处理，
>
>    由最终操作（terminal operation）得到前面处理的结果

# 操作符

> 1. 中间操作符
> 2. 终止操作符

# 简单示例

```java
@Test
public void test04() {
    List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd", "", "jkl");
    List<String> filter = strings.stream().filter(str -> str.contains("f")).collect(Collectors.toList());
    System.out.println(filter);// [efg]
}
// strings.stream() 将list转换成stream流
// .filter()是一个中间操作符,将流数据进行特定操作后再放回流中
// .collect()是一个终止操作符，将流数据收集后进行处理
```

# 中间操作符

| 流方法   | 含义                                                         | 示例     |
| -------- | ------------------------------------------------------------ | -------- |
| filter   | 过滤元素，得到满足条件的元素                                 | 函数     |
| distinct | 返回一个元素各异的流（根据原始元素的hashCode和equals方法实现） | 无参     |
| limit    | 返回元素个数不超过给定长度n的流                              | 传入整数 |
| skip     | 返回一个扔掉了前n个元素的流                                  | 传入整数 |
| map      | 接受一个函数作为参数。这个函数会被应用到每个元素上，并将其映射成一个新的元素。（使用映射一词，是因为它和转换类似，但其中的细微差别再于它是“创建一个新版本”而不是去“修改”）。 |          |
| flatMap  | 使得各个数组并不是分别映射成一个流，而是映射成流的内容。所有使用map(Array::stream)时生成的单个流都被合并起来，即扁平化成一个流 |          |
| sorted   | 排序                                                         |          |

# 终止操作符

| 流方法    | 含义                                  | 示例                                                         |
| --------- | ------------------------------------- | ------------------------------------------------------------ |
| anyMatch  | 检查是否至少匹配一个元素，返回boolean | List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd", "", "jkl");<br />boolean haveMatch = strings.stream().anyMatch(s -> 'abc'==s) |
| allMatch  | 全满足返回true，否则返回false         |                                                              |
| noneMatch | 全部不满足，返回true                  |                                                              |
| findAny   | 返回集合中任意元素                    |                                                              |
| findFirst | 返回第一个元素                        |                                                              |
| forEach   | 遍历流                                |                                                              |
| collect   | 将流转换为其他形式                    |                                                              |
| reduct    | 将流中的元素反复结合起来得到一个值    |                                                              |
| count     | 返回流中总元素个数                    |                                                              |

