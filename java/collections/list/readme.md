# List

## List to Array

#### Example
```java
List<String> exampleList = new ArrayList<>();
exampleList.add("a");
exampleList.add("b");
exampleList.add("c");

String[] exampleArray = new String[exampleList.size()];
exampleArray = exampleList.toArray(exampleArray);
// exampleArray is ['a','b','c']
```
