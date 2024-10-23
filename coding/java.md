# Basics

compiling and running
```shell
javac <filename>.java
# arguments can be passed here once compiled
java <filename> # the compiled bytecode .Class file
```

jar
```bash
# first compile
javac /path/to/something.java
# then zip it
# -cvf -> create verbose filename
# use *.class to add all class files
jar -cvf <jarfile> /path/to/something.class
# and finally run it
# cp is for classpath, can use --jar as well
java -cp /path/to/jar <class> # without .java
```

loops
```java
// iterator loop
// let's say we have an array of strings elems
for (String el: elems) {
	System.out.println(el);
}
```

variables
```java
class Foo {
	// instance variables
	String field1;
	int field2;	

	public void doSomething() {
		// local variables
		final int a = 0; // constants that must be initialized
		String b = "Yoooo";
		System.out.println(a + b);
	}
}
```


unboxing and autoboxing
https://javarevisited.blogspot.com/2010/10/what-is-problem-while-using-in.html#axzz82BXif5vU
>basically converting stack into heap
```java
```

strings
>strings are heap allocated, and used something called a string constant pool where same/existing strings can be referenced instead of creating a new one as a string in java is immutabe
```java
String a = "foo"; 
a = "bar"; // this will create a new string object

// StringBuffer
// StringBuffer are thread safe
StringBuilder b = new StringBuilder("My");
b.append("Mom");
System.out.println(b);

// StringBuilder
// same as StringBuffer but is non-synchronized
StringBuilder b = new StringBuilder("My");
b.append("God");
System.out.println(b);
```

reflection

threadpool

streams
```java
// java.utils.Arrays
int[] a = {1, 2, 3};
Arrays.stream(a).forEach(e -> System.out.println(e));
```

packages

association
>classes and contain references to other classes and form relationships such as one-to-one, one-to-many, etc.. This is known as association in java.
```java
class Person {
	// one-to-many association
	List<Email> emails;
}

class Email {
	// one-to-one association
	Provider provider;
	String address;
}

class Provider {
	String host;
}
```

constructors
```java
class Foo {
	String field1;
	Integer field2;

	// default and public constructor
	public Foo(String field1, Integer field2) {
		this.field1 = field1;
		this.field2 = field2;
	}

	// private constructor usually used for Singleton classes
	private Foo() {
	}
}
```

access specifiers
```java
// public / default
public class Foo {

}

// private
private class Bar {

}
// protected
protected class Baz {

}
```

marker interface
>empty interfaces used to tag classes for a certain implementation/metadata/intent
```java
// marker interface usually are serializable or cloneable
public interface Deletable {
}
```

jvm
>the jvm typically has a class loader, interpreter and a jit compiler. The class loader, allows the jvm to load the byte code classes. The jit compiler compiles the java byte code to machine code

binding and dispatchers

jsp

jdbc

clone, equals, finalize methods