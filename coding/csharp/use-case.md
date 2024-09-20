# Video References

## [Correcting Common Async/Await Mistakes in .NET 8 - Brandon Minnick - NDC London 2024](https://www.youtube.com/watch?v=GQYd6MWKiLI)

await every task
```cs
// bad
public DoSomeAsyncStuff()
{
	// this will run in the background and not return the exception 
	// to the main thread
	AsyncTask();	
}
// good
public async DoSomeAsyncStuff()
{
	// await will cause a thread switch based on the available threads 
	// in the thread pool
	await AsyncTask();
}
// good
// always provide CancellationToken as a parameter in async methods
public async DoSomeAsyncStuff(CancellationToken token)
{
	//...
}
```

ConfigureAwait
```cs
public async DoSomeAsyncStuff(CancellationToken token)
{
	// for tasks that does not rely on the calling thread
	// its better to offload the next task to other/free threads
	// only matters when framework is using SynchronizationContext
	var asyncTask = await AsyncTask().ConfigureAwait(false);
}
```

[FrozenSet](https://learn.microsoft.com/en-us/dotnet/api/system.collections.frozen.frozenset-1?view=net-8.0)

IAsyncEnumerable
```cs
public async DoSomeAsyncStuff(CancellationToken token)
{
	try
	{
		const int limit = 10;
		await foreach (var foo in FetchFoo(limit, token).ConfigureAwait(false))
		{
			//...
		}
	} catch (Exception)
	{
		//...
	}
}

// IAsyncEnumerable allows data to be streamed
public async IAsyncEnumerable<Foo> FetchFoo(int fooCount, [EnumeratorCancellation] CancellationToken token)
{
	var getTasks = await GetTaskList(token).ConfigureAwait(false);
	// a task list to sort of parallelize the number of requests
	var taskList = new List<Task<Foo>>();
	foreach (var task in getTasks)
	{
		taskList.Add(AsyncTask(task, token));
	}

	while (taskList.Count > 0 && fooCount-- > 0)
	{
		// when any task is completed remove it from the list
		Task<Foo> completedTask = await Task.WhenAny(taskList)
		.ConfigureAwait(false);
		taskList.Remove(completedTask);

		var foo = await completedTask.ConfigureAwait(false);
		// stream completed task data to caller
		yield return foo;
	}
}
```

when to return await
```cs
// we don't do async/await here to save context switching
// as the method this method calls already returns a task
// in which the caller to this method can handle the async stuff
public Task<Foo> DoSomeAsyncStuff(int id, CancellationToken token)
{
	return _someApi.GetFooAsync(id, token);
}

// here we return await as we don't want to lose the exception 
// once we exit the method
public async Task<Foo> DoSomeAsyncStuff(int id, CancellationToken token)
{
	try
	{
		return await _someApi.GetFooAsync(id, token);
	} catch (Exception)
	{
		//...
	}
}
```

ValueTask
```cs
// if we are returning from a cache that is not async but have an alternative
// data source that is async, we can use ValueTask as sort a generic way
// to get data from two different async/non-async sources
// value task is also a value type so there is a performance boost
public ValueTask<Foo> DoSomeAsyncStuff(CancellationToken token)
{
	if (Cache != null)
	{
		return Cache;
	}

	try
	{
		return await _someApi.GetFooAsync(token);
	} catch (Exception)
	{
		//...
	}
}
```

# JSON

deserialize JSON
```cs
// .Net framework < 4.7.2
Newtonsoft.Json.JsonConvert.SerializeObject(new {foo = "bar"})
// .Net framework >= 4.72
System.Text.Json.JsonSerializer.Serialize(new {foo = "bar"})
```

# Validator 

add validation attributes
```cs
using System.ComponentModel.DataAnnotations;

public class Movie
{
	[Required]
	public string Title { get; set; }

	[Range(2000, 2022)]
	public int Year { get; set; }
}

```

basic validation
```cs
using System.ComponentModel.DataAnnotations;

var movieJson = "{\"Year\":0}";
var movie = JsonSerializer.Deserialize<Movie>(movieJson);

// throws error on first validation error
// by default only check for [Required] attribute
// to check for all validations use validateAllProperties: true
Validator.ValidateObject(movie, new ValidationContext(movie), validateAllProperties: true);
```

verbose validation
```cs
using System.ComponentModel.DataAnnotations;

var movieJson = "{\"Year\":0}";
var movie = JsonSerializer.Deserialize<Movie>(movieJson);

var validationErrors = new List<ValidationResult>();
if (!Validator.TryValidateObject(movie, new ValidationContext(movie), validationErrors, validateAllProperties: true))
{
    // Look at all of the validation errors
    foreach (var error in validationErrors)
    {
        Console.WriteLine(error.ErrorMessage);
    }
}
```

# Tools

[sharplab.io](https://sharplab.io)