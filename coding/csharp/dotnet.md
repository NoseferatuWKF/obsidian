# .NET References

- [.NET Project SDKs](https://learn.microsoft.com/en-us/dotnet/core/project-sdk/overview?view=aspnetcore-9.0)
- [Target frameworks](https://learn.microsoft.com/en-us/dotnet/standard/frameworks?view=aspnetcore-9.0)

# MSBuild

- [MSBuild items](https://learn.microsoft.com/en-us/visualstudio/msbuild/msbuild-items?view=vs-2022)
- [MSBuild props](https://learn.microsoft.com/en-us/dotnet/core/project-sdk/msbuild-props)
- [Task reference](https://learn.microsoft.com/en-us/visualstudio/msbuild/msbuild-task-reference?view=vs-2022)

## Concepts

targets (task group)
```xml
<Target Name="First"></Target>
<Target Name="Second"></Target>
<Target Name="Third"></Target>
<!-- This replaces the previous Target -->
<Target Name="Third"></Target>
```

task
```xml
<Target Name="CopyFiles">
	<!-- Task under this target -->
    <Copy
        SourceFiles="@(MySourceFiles)"
        DestinationFolder="@(MyDestFolder)">
        <Output
            TaskParameter="CopiedFiles"
            ItemName="SuccessfullyCopiedFiles"/>
     </Copy>
</Target>
```

targets build order
- Initial
- Default
- First
- Target dependencies
- BeforeTargets and AfterTargets

build events for SDK style projects
```xml

<Target Name="PreBuild" BeforeTargets="PreBuildEvent">
    <Exec Command="&quot;$(ProjectDir)PreBuildEvent.bat&quot; &quot;$(ProjectDir)..\&quot; &quot;$(ProjectDir)&quot; &quot;$(TargetDir)&quot;" />
</Target>

<Target Name="PostBuild" AfterTargets="PostBuildEvent">
   <Exec Command="echo Output written to $(TargetDir)" />
</Target>
```

[batching](https://learn.microsoft.com/en-us/visualstudio/msbuild/msbuild-batching?view=vs-2022)

[incremental builds](https://learn.microsoft.com/en-us/visualstudio/msbuild/incremental-builds?view=vs-2022)

disable restore on other env besides Debug
>[!warning]
>to avoid importing the namespace involved, there needs to be a if pre-processor directive
```xml
<ItemGroup Condition="'$(Configuration)' == 'Debug'">
  <PackageReference Include="SomeDebugOnlyPackage" Version="1.0.0" />
</ItemGroup>
```

# Configuration

# .Net Framework

getting status code from WebException
```csharp
try
{
	SomeAPICall();
} catch (WebException ex) when (ex.Response is HttpWebResponse && ex.StatusCode == HttpStatusCode.NotFound /* 404 */)
{
	// do something
	throw;
}
```

environment tag helper
```cshtml
<environment include="Development">
    <div>Environment is Development</div>
</environment>
<environment exclude="Development">
    <div>Environment is NOT Development</div>
</environment>
<environment include="Staging,Development,Staging_2">
    <div>Environment is: Staging, Development or Staging_2</div>
</environment>
```

# Performance

[All About Span: Exploring a New .NET Mainstay](https://learn.microsoft.com/en-us/archive/msdn-magazine/2018/january/csharp-all-about-span-exploring-a-new-net-mainstay)

Memory

ArrayPool

IMemoryCache

IDistributedCache

## System.IO.Pipeline

## System.Collection.ObjectModel

ObservableCollection

KeyedCollection

ReadOnlyCollection
```cs
var someList = new List<int> {1, 2, 3};
someList.Add(4); // list is mutable even if its behind a readonly
var readOnlyList = new ReadOnlyCollection<string>(someList);
// list will now now longer be able to be modified
```

## System.Collection.Frozen

[FrozenSet](https://learn.microsoft.com/en-us/dotnet/api/system.collections.frozen.frozenset-1?view=net-8.0)

# Async

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

# Dependency Injection

# Video

- [Correcting Common Async/Await Mistakes in .NET 8 - Brandon Minnick - NDC London 2024](https://www.youtube.com/watch?v=GQYd6MWKiLI)
- [What's New in Blazor for .NET 8](https://www.youtube.com/watch?v=QD2-DwuOfKM)

# Tools

[sharplab.io](https://sharplab.io)

# .NET 6

[implicit usings](https://learn.microsoft.com/en-us/dotnet/core/project-sdk/overview?view=aspnetcore-9.0#implicit-using-directives)
```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
	<!-- Enable global using of some namespaces -->
    <ImplicitUsings>enable</ImplicitUsings>
  </PropertyGroup>
</Project>
```

# .NET 8

[HtmlRenderer](https://learn.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.components.web.htmlrenderer?view=aspnetcore-8.0)
```cs
using Microsoft.AspNetCore.Components;
using Microsoft.AspNetCore.Components.Web;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Logging;

var services = new SeviceCollection();
services.AddLogging();

var serviceProvider = services.BuildServiceProvider();
var loggerFactory = serviceProvider.GetRequiredService<ILoggerFactory>();
await using var htmlRenderer = new HtmlRenderer(serviceProvider, loggerFactory);
var html = await htmlRenderer.Dispatcher.InvokeAsync(async () =>
{
	var output = await htmlRenderer.RenderComponentAsync<Component>(ParameterView.Empty);
	return output.ToHtmlString();
});
```

IDisposable
>[!info]
used for unmanaged resources like db connection, file handles and sockets
