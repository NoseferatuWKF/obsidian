# Basics

constructors & destructors
```cs
class Foo
{
	private int a;

	// default constructor
	public Foo()
	{
	}

	// parameterized constructor
	public Foo(int a)
	{
		this.a = a;
	}

	// copy constructor
	public Foo(Foo foo) 
	{
		this = foo;
	}

	// static constructor, used to initialize static members
	// should only be initalized once
	static Foo() 
	{
	}

	// destructor
	~Foo()
	{
		// usually we call GC here
		System.GC.Collect();
	}
	
}
```

struct
>struct is a value type, while classes are reference types.
```cs
struct Foo
{
	public string name;
	public int age;

	public void GetName() {
		Console.WriteLine("Name: " + name);
	}
}

class Program
{
	static void Main(string[] args)
	{
		Foo x;
		// structs does not need to be initialized
		x.name = "John";
		x.GetName();
	}
}
```

classes
```cs
class Foo
{
	// classes can be nested
	class Bar
	{
	}

	// abstract class which is not stand-alone
	public abstract class AbsBar
	{
	}

	// sealed class to block inheritance
	public sealed class SealBar
	{
	}

	// class that is only accessible within the same assembly
	internal class InternalBar
	{
	}

	// class that is accessible within the same assembly
	// or from within a derived class in another assembly
	protected internal class PInternalBar
	{
	}

	// partial class
	// used to split definition over multiple source files
	// the partial class will be combined on compile time
	// partials can be used for interfaces and structs as well
	private partial class PartBar
	{
		void MethodOne()
		{
		}
	}

	// all parts must use the same accessor keyword
	private partial class PartBar
	{
		void MethodTwo()
		{
		}
	}
}
```

interfaces
```cs
interface IFoo
{
	void DoSomething()
	{
	}
}

interface IFoo2
{
	void DoSomething()
	{
	}
}

// implementing two interfaces with same method names
class Foo : IFoo, IFoo2
{
	void IFoo.DoSomething()
	{
	}

	void IFoo2.DoSomething()
	{
	}
	
}
```

enum
>it's a value type!
```cs
class Foo
{
	enum Numbers {
		ONE,
		TWO,
		THREE
	}

	void UseEnum() {
		Console.WriteLine(Numbers.ONE);
	}
}
```

polymorphism
```cs
class Foo
{
	// base method
	void DoSomething()
	{
	}

	// method overloading
	void DoSomething(int a)
	{
	}

	public virtual void DoSomethingElse()
	{
	}
}

class Bar : Foo
{
	// method overriding
	public override void DoSomethingElse()
	{
	}
}
```

namespace
>kinda like a module/packages, used to group classes together
```cs
namespace Foo
{
	class Bar
	{
	}
}
```

boxing and unboxing
https://www.geeksforgeeks.org/c-sharp-boxing-unboxing/
```cs
class Foo
{
	static void Main(string[] args)
	{
		// initialize a value type
		int a = 1;
		// convert to reference type
		object b = a; // this is called boxing
		// convert to value type
		int c = (int)b; // this is called unboxing
	}
}
```

using
>used to define the scope of a resource used in a block which will be disposed once the resource is out of scope
```cs
class Program
{
	class Bar: IDisposable
	{
		public void Dispose()
		{
			// Do something here...
		}
	}

	static void Main()
	{
		// bring Bar into scope
		using (Bar bar = new Bar())
		{
		}
		// Bar will be disposed herek
	}
}
```

delegate
```cs
class Program
{
	// initialize a delegate
	public delegate void DoSomething(int val);
	// single delegate
	DoSomething x = DelegateOne;
	x(1);
	// delegate another method
	x = DelegateTwo;
	x(1);
	
	// multicast delegate
	DoSomething y = DelegateOne;
	DoSomething xy = x + y;
	xy(1); // this will execute both methods
	// delegates can be removed from multicast delegates
	xy = x - y;
	xy(1); // this now only execute DelegateTwo from x;

	// delegated methods need to have same signature
	public static void DelegateOne(int val) {
	}

	public static void DelegateTwo(int val) {
	}
}
```

const vs readonly
```cs
class Foo
{
	// const requires initilization after declaration and cannot change
	const int x = 3;
	// readonly can be declared without initialization
	readonly int y;

	public Foo(int a)
	{
		// after readonly is initialized it cannot be changed again
		this.y = a;
	}

}
```

var vs dynamic vs anonymous vs nullable
```cs
class Foo
{
	static void Main(string[] args)
	{
		var x = 1; // kinda like inferencing, x is now int type
		dynamic y = 1; // here its int
		y = "1"; // now its a string

		// anonymous type, kinda like obj literal in js
		var z = new
		{
			Foo = "Hello",
			Bar = 123
		};
		Console.WriteLine(z.Foo);

		// kinda like optional props in js
		int? nullable = null;
	}
}
```

out vs in vs ref
```cs
class Program
{
    struct Foo
    {
        public int bar;
    }

    static void Main(string[] args)
    {
        OutParameterModifier();
        InParameterModifier();
        RefParameterModifier();
    }

    static void OutParameterModifier() {
        MethodOut(out int resNumber, out string resString, out Foo resFoo);
        Console.WriteLine(resNumber);
        Console.WriteLine(resString);
        Console.WriteLine(resFoo.bar);
    }

    static void InParameterModifier() {
        MethodIn(69, "abc");
    }

    static void RefParameterModifier() {
        int refNumber = 69;
        string refString = "abc";
        MethodRef(ref refNumber, ref refString);
        Console.WriteLine(refNumber);
        Console.WriteLine(refString);
    }

	// mutable parameters
	// arguments can be declared at the same time of method call
    static void MethodOut(out int someNumber, out string someString, out Foo someFoo) {
        someNumber = 69;
        someString = "abc";
        someFoo.bar = 420;
    }

    // mutable parameters
    // kinda like out, but arguments must be declared
    static void MethodRef(ref int someNumber, ref string someString) {
        someNumber = 420;
        someString = "def";
    }

    // immutable parameters
    static void MethodIn(in int someNumber, in string someString) {
        Console.WriteLine(someNumber);
        Console.WriteLine(someString);
    }
}
```

multidimensional array vs jagged array
```cs
class Program
{
	static void Main(string[] args)
	{
		// multidimensional array
		int[,] x = new int[3,3] {{1,2,3}, {4,5,6}, {7,8,9}};
		// jagged array
		int[][] y = new int[][] {{1,2}, {3,4,5}, {6}, {7,8,9}};
	}
}
```

[pattern matching](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/functional/pattern-matching)
```cs
class Program
{
	static enum Numbers
	{
		One,
		Two,
		Three,
		Four,
		Five,
		Six,
	}

	static int NumbersPlusOne(Numbers n)
	{
		return n switch
		{
			One => 2,
			Two => 3,
			Three => 4,
			_ => 69,
		}
	}

	static void Main(string[] args)
	{
		Console.WriteLine(NumbersPlusOne(Numbers.One));
		Console.WriteLine(NumbersPlusOne(Numbers.Two));
		Console.WriteLine(NumbersPlusOne(Numbers.Five));
	}
}
```

[indexers](https://learn.microsoft.com/en-US/dotnet/csharp/programming-guide/indexers/)
>think of custom collections
```cs
namespace basics
{
    public class Indexers<T>
    {
        private T[] arr = new T[100];

        public T this[int i]
        {
            get { return arr[i]; }
            set { arr[i] = value; }
        }
    }
}

var idx = new Indexerx<string>();
idx[0] = "Hmm";
Console.WriteLine(idx[0]); // Hmm
```

top level statements
>for C# 9.0 and later
```cs
using System;

// entry points before this looked like this
class Program
{
	static void Main(string[] args)
	{
		Console.WriteLine("Hello World");
	}
}

// Now we can do this
Console.WriteLine("Hello World");
```

[records](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/record)
```cs
// normal DTO
public class DTO
{
	string Field1 { get; set; }
	int Field2 { get; set; }
}

// with records
// immutable by default
public record DTO(string Field1, int Field2);

// non-destructive mutation
DTO dto = new DTO("foo", 69);
// use with syntax
DTO dtoCopy = dto with { Field1 = "bar", Field2 = 420 };

// inheritance
// only applies to record class
public abstract record Person(string FirstName, string LastName);
public record Teacher(string FirstName, string LastName, int Grade)
    : Person(FirstName, LastName);
public record Student(string FirstName, string LastName, int Grade)
    : Person(FirstName, LastName);
```

generics
```cs
// type constraint / narrowing
public class Generic<T> wehre T : IComparabel<T>
```

# LINQ

[PLINQ](https://learn.microsoft.com/en-us/dotnet/standard/parallel-programming/introduction-to-plinq)
# Practical

deserialize JSON
```cs
Newtonsoft.Json.JsonConvert.SerializeObject(new {foo = "bar"})
```