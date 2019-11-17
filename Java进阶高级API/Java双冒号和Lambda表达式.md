**说明：JDK8中有双冒号的用法，就是把方法传到stream内部，使stream的每个元素都传入到该方法里面执行一下**

以前的代码：

```java
public class AcceptMethod{
	public static void printValur(String str){
		System.out.println("print value : " + str);
	}
	
	public static void main(String[] args){
		List al = Arrays.asList("a","b","c","d");
		for(String a : al){
			AcceptMethod.printValur(x);
		}
		//下面的forEach循环和上面的循环是等价的
    al.forEach(x->{
      AcceptMethod.printValur(x);
    });
	}
}
```

现在的JDK双冒号是：

```Java
public class MyTest{
  public static void printValur(String str){
    System.out.println("print value : " + str);
  }
  
  public static void main(String[] args){
		List al = Arrays.asList("a","b","c","d");
    al.forEach(AcceptMethod::printValur);
    //下面的方法和上面等价的
    Consumer methodParam = AcceptMethod::printValur;	//方法参数
    al.forEach(x-> methodParam.accept(x));	//方法执行accept
  }
}
```

他们的执行结果都是：

```java
print value : a
print value : b
print value : c
print value : d
```

以上都是串行执行，接下来是并行执行

```Java
public class MyTest{
  public static void main(String[] args){
		List al = Arrays.asList("a","b","c","d");
    //因为是并行，所以输出顺序可能不同，如果要按顺序来可以用forEachOrder()
    al.parallelStream().forEach(System.out::println); 
  }
}
```

运行结果：

```java
30
40
20
10
```

