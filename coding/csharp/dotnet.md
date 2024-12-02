# MSBuild

- [MSBuild items](https://learn.microsoft.com/en-us/visualstudio/msbuild/msbuild-items?view=vs-2022)
- [MSBuild props](https://learn.microsoft.com/en-us/dotnet/core/project-sdk/msbuild-props)

# .csproj

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

# Internal Libraries

[All About Span: Exploring a New .NET Mainstay](https://learn.microsoft.com/en-us/archive/msdn-magazine/2018/january/csharp-all-about-span-exploring-a-new-net-mainstay)

IMemoryCache

IDistributedCache