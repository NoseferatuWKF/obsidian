[[The Robert C. Martin Clean Code Collection.pdf]]
## Single Responsibility Principle

>classes should have only one responsibility

example
```cs
// bad
class PaymentService
{
	public void HandlePayment()
	{
	} 

	// this function is out of scope for a payment service, thus breaking the SRP
	public void PrintInvoice()
	{
	}
}

// good
class PaymentService
{
	public void HandlePayment()
	{
	}
}

// create another class to handle printing
class PrintingService
{
	public void PrintInvoice()
	{
	}
	// maybe print some other stuff here...
}
```

## Open-Closed Principle

>classes should be opened to extension but closed for modification

>this should no longer be the case, when using pattern matching or enforcing 'cases' to be exhaustive

example
```cs
// bad
class Questionnaire
{
	public void HandleQuestions(Questions question)
	{
		// switch cases and conditional branches usually an indicator of not following OCP. 
		switch(question) {
			case Questions.SingleChoice:
				return
			case Questions.MultipleChoices:
				return
			case Questions.Subjective:
				return
			case Questions.Range:
				return
			// If there are new types of question need to be handled, this method needs to be modified thus breaking OCP
		}
	}
}

enum Questions
{
	SingleChoice,
	MultipleChoices,
	Subjective,
	Range
	// new type of questions can be added here
}

class Main
{
	static void Main(string[] args)
	{
		Questionnaire questionnaire = new Questionnaire();
		Question question = Question.SingleChoice;
		questionnaire.HandleQuestions(question);
	}
}

// good
class Questionnaire
{
	public void HandleQuestions(IQuestions question)
	{
		// this not longer need to be modified as long as the contract is adhered
		question.Display();
	}
}

// interface for new questions to implement
interface IQuestions
{
	public void Display();
}

class SingleChoice : IQuestions
{
	public void Display()
	{
	}
}

class MultipleChoices : IQuestions
{
	public void Display()
	{
	}
}

class Subjective : IQuestions
{
	public void Display()
	{
	}
}

class Range : IQuestions
{
	public void Display()
	{
	}
}

// new questions can just extend the parent class or implement the common interface
class SomeNewQuestion : IQuestions
{
	public void Display()
	{
	}
}

class Main
{
	static void Main(string[] args)
	{
		Questionnaire questionnaire = new Questionnaire();
		SingleChoice questionOne = new SingleChoice();
		questionnaire.HandleQuestion(questionOne);

		SomeNewQuestion questionTwo = new SomeNewQuestion();
		// can just pass it like so, without any modifications
		questionnaire.HandleQuestion(questionTwo);
	}
}
```

## Liskov-Substitution Principle

>derived classes should be able to replace/substitute their parent class without breaking any functionality 

>prefer composition over inheritance. Composition does not really follow LSP, but it is factually proven better

example
```cs
// bad
class Animal
{
	public void CanFly()
	{
	}

	public void CanWalk()
	{
	}
}

class Dog : Animal
{
	public void CanFly()
	{
		 throw new NotImplementedException();
	}
}

class Main
{
	static void Main(string[] args)
	{
		Animal animal = new Animal();
		Animal dog = new Dog();
		animal.CanFly();
		dog.CanFly(); // because dog can't fly, it cannot replace the animal class, thus breaking LSP
	}
}

// good
class FlyingAnimal
{
	public void CanFly()
	{
	}
}

// now we have separate parent class for walking and flying animals
class WalkingAnimal
{
	public void CanWalk()
	{
	}
}

class Dog : WalkingAnimal
{
}

class Main
{
	static void Main(string[] args)
	{
		WalkingAnimal animal = new WalkingAnimal();
		WalkingAnimal dog = new Dog();
		animal.CanWalk();
		// now dog class can replace its parent class
		dog.CanWalk();
	}
}
```

## Interface-Segregation Principle

>interfaces should hold minimal contract on a certain set of behaviors with no conflicts on implementation

example
```cs
// bad
interface IEngineer
{
	public void Code();

	public void Calculus();

	public void Drawing();

	public void Machining();
}

class SoftwareEngineer : Engineer
{
	// Because software engineers generally can't do machining, one of the methods implemented is not applicable. Because of this conflict in implementation, ISP was broken
	public void Machining()
	{
		throw new NotImplementedException();
	}
}

// good
interface IMechanical
{
	public void Machining();
}

interface IProgramming
{
	public void Code();
}

interface IMathematics
{
	public void Calculus();

	public void Algrebra();
}

class SoftwareEngineer : Programming, Mathematics
{
	// now all of the relevant methods can be implemented
}
```
## Dependency Inversion Principle

>higher level functionalities should not depend on lower level modules. Instead, both should depend on abstractions

>usually this is handled using Dependency Injection and IOC container

example
```cs
// bad
class Bike
{
	// directly depends on the class
	private Engine _engine;

	public Bike()
	{
		this._engine = new Engine();
	}

	public void StartEngine()
	{
		// this breaks when swapping to a new class with a different behavior
		this._engine.Start();
	}
}

class Engine
{
	public void Start()
	{
	}
}

class AnotherEngine
{
	public void Ignition()
	{
	}
}

// good
class Bike
{
	// depends on the shared interface that all engine have to implement
	private IEngine _engine;

	public Bike(IEngine engine)
	{
		this._engine = engine;
	}

	public StartEngine()
	{
		// because all engine objects use the same interface this will work though the implementation might differ
		this._engine.Start();
	}
}

interface IEngine
{
	public void Start();	
}

class EngineA : IEngine
{
	public void Start()
	{
		this.SetFiringOrder();
		this.ConsumeFuel();
		this.CheckPiston1();
		this.CheckPiston2();
		this.Ignition();
	}

	public void SetFiringOrder()
	{
	}
 
	public void ConsumeFuel()
	{
	}
 
	public void CheckPiston1()
	{
	}
 
	public void CheckPiston2()
	{
	}
 
	public void Ignition()
	{
	}
}

class EngineB : IEngine
{
	public void Start()
	{
		 this.CheckPiston1();
		 this.CheckPiston2();
		 this.CheckPiston3();
		 this.CheckPiston4();
		 this.Ignition();
	}
 
	public void CheckPiston1()
	{
	}
 
	public void CheckPiston2()
	{
	}
 
	public void CheckPiston3()
	{
	}
 
	public void CheckPiston4()
	{
	}
 
	public void Ignition()
	{
	}
}

class Program
{
	static void Main(string[] args)
	{
		// bike can take any engine without breaking
		Bike bike = new Bike(new EngineA());
		bike.StartEngine();
		// sample of using IOC
		Bike anotherBike = new Bike(IOCContainer.createEngineB());
		anotherBike.StartEngine();
	}
}

// sample IOC container
class IOCContainer
{
	static EngineA createEngineA()
	{
		return new EngineA();
	}

	static EngineB createEngineB()
	{
		return new EngineB();
	}
}
```
