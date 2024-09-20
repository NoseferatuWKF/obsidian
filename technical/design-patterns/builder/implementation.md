>good for immutable data and one off creation
## Fluent builder pattern

>this is what builder pattern generally refer to

```cs
class Person
{
	public string Name {get; private set; }
	public int Age {get; private set; }

	public Person SetName(string name)
	{
		Name = name;
		return this;
	}

	public Person SetAge(int age)
	{
		Age = age;
		return this;
	}
}

class Program {
	static void Main(string[] args)
	{
		Person person = new Person()
			.SetName("John")
			.SetAge(69);
	}
}
```


## Director + Builder

>Also called Conventional Builder. Good for construction in sequence

```cs
class PersonDirector
{
	public Person Construct(PersonBuilder builder, PersonData data)
	{
		// here is where the sequence is determined
		builder.SetName(data.Name);
		builder.SetAge(data.Age);

		return builder.Build();
	}
}

class PersonBuilder
{
	// let's say we have a Person class
	Person person = new Person();

	public void SetName(string name)
	{
		person.Name = name;
	}

	public void SetAge(int age)
	{
		person.Age = age;
	}

	public Person Build() {
		Person builtPerson = person;
		return builtPerson;
	}
}

class Program
{
	static void Main(string[] args)
	{
		PersonDirector director = new PersonDirector();
		PersonData data = new PersonData("John", 69);
		// this will always construct in sequence
		director.Construct(new PersonBuilder(), data);
	}
}
```