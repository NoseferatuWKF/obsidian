[[cheat-sheets/dotnet|cheat-sheets]]
# MSBuild

- [MSBuild items](https://learn.microsoft.com/en-us/visualstudio/msbuild/msbuild-items?view=vs-2022)
- [MSBuild props](https://learn.microsoft.com/en-us/dotnet/core/project-sdk/msbuild-props)

# Use case

## .Net Framework

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