```java
List<Item> list;

//列表中gold的总和
list.stream().mapToLong(Item::getGold).sum();

//分组后返回Map，每组的数量
list.stream().collect(Collectors.groupingBy(Student::getAge, Collectors.counting()));

//分组后返回组数
list.stream().collect(Collectors.groupingBy(Item::getName)).size();

//列表是否存在匹配项，返回boolean
list.stream().anyMatch(item -> item == "");

//列表存在匹配项的总数
list.stream().filter(item -> "1".item.getParam()).count();

//过滤后返回新的List
list.stream().filter(p -> p.getId() != null).collect(Collectors.toList());

//过滤后返回新的List<long>
list.stream().filter(item -> item.getAge() > 18).map(Item::getId).collect(Collectors.toList());

//两个list相交的部分
list.stream().filter(item -> list2.stream().anyMatch(item2 -> item.getId().equals(item2.getId()))).count();

//List<Object>转Long[]
list.stream().map(TRole::getBornid).toArray(Long[]::new);
```

