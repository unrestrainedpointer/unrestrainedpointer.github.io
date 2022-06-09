---
title: Stream in Java
date: 2022-04-12 03:29:41
tags: Conputer Science, Java
---
1. 流是什么：
   1. 流是支持数据处理操作的源生成的元素序列，源可以是数组、文件、集合、函数。流不是数据结构并不保存数据，它的主要目的在于计算。
2. 流的生成：
   1. 通过集合生成：`Stream<Integer> steam=new ArrayList<Integer>().stream();` 
   2. 通过数组生成：`IntStream stream=Arrays.stream(new int[]{1,2,3});`
   3. 通过值生成：`Stream<Integer> stream=Stream.of(1,2,3,4,5,6);`
   4. 通过文件生成：`Stream<Integer> stream=Files.lines(Paths.get("data.txt"), Charset.defaultCharset());`
   5. 通过函数生成：`Stream<Integer> stream=Stream.iterate(0, n->n+2).limit(5);`iterate()方法 接受两个参数，第一个是初始化值，第二个为进行的函数操作，因为iterate生成的是无限流，通过limit方法对流进行了截断；
   6. generator：`Stream<Double> stream=Stream.generate(Math::random).limit(5);`
3. 流的操作类型：
   1. 中间操作：
   
      1. filter：条件筛选
        ```
        List<Integer> list=Arrays.asList(1,2,3,4,5,6);
        Stream<Integer> stream=list.stream().filter(i->i>3);
        result:4,5,6
        ```
      2. distinct：去除重复元素；
      3. limit：返回指定流的个数；
      4. skip：跳过流中的元素；
      5. map：流映射——将接受的元素映射为另一个元素
        ```
        List<String> list=Arrays.asList("Java 8","Lambdas","In","Action");
        List<Integer> length=list.stream().map(String::length).collect(Collectors.toList());
        result:[6,7,2,6]
        ```
      6. flatMap：流转换——将一个流中的每个值都转换为另一个流；
      7. allMatch：匹配所有元素
        ```
        List<Integer> list=Arrays.asList(1,2,3,4,5);
        list.stram().allMatch(i->i>3);
        result:false
        ```
      8. anyMatch：匹配一个元素
      9. noneMatch：全部不匹配
   2.  终端操作：
       1.  count：计算流中元素个数；
       2.  findFirst：查找第一个；
       3.  findAny：随机查找一个；
       4.  reduce：将流中的元素组合
           1.  用于求和`int sum=list.stream().reduce(0, Integer::sum);`
       5. min/max：获取最大最小值；
       6. sum：求和；
       7. averagingXXX：求平均值；
       8. forEach：遍历
       9. joining拼接流中的元素
        ```
        String result=list.stream().map(String::toLowerCase).collect(Collectors.joining("-"));
        ```
