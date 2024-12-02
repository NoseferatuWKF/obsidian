# Guidelines

- [ASP.NET Core Best Practices](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/best-practices?view=aspnetcore-7.0#return-ienumerablet-or-iasyncenumerablet)
- [Controller action return types](https://learn.microsoft.com/en-us/aspnet/core/web-api/action-return-types?view=aspnetcore-7.0#actionresultt-type)
- [Efficient Querying](https://learn.microsoft.com/en-us/ef/core/performance/efficient-querying)
- [gRPC services with ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/grpc/aspnetcore?view=aspnetcore-7.0&tabs=visual-studio)
- [Dependency Injection](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection)
## Comparison

- [ASP.NET Core vs ASP.Net](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/choose-aspnet-framework?view=aspnetcore-7.0)
- [.NET vs .NET Framework](https://learn.microsoft.com/en-us/dotnet/standard/choosing-core-framework-server?toc=%2Faspnet%2Fcore%2Ftoc.json&bc=%2Faspnet%2Fcore%2Fbreadcrumb%2Ftoc.json&view=aspnetcore-7.0)
# New Features

- [ASP.NET Core 7](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-7.0?view=aspnetcore-7.0)
- [ASP.NET Core 6](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-6.0?view=aspnetcore-7.0)
- [ASP.NET Core 5](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-5.0?view=aspnetcore-7.0)

[General Page Life-Cycle Stages](https://learn.microsoft.com/en-us/previous-versions/aspnet/ms178472(v=vs.100))

| Stage                   | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Page Request            | The page request occurs before the page life cycle begins. When the page is requested by a user, ASP.NET determines whether the page needs to be parsed and compiled (therefore beginning the life of a page), or whether a cached version of the page can be sent in response without running the page.                                                                                                                                                                                             |
| Start                   | In the start stage, page properties such as Request and Response are set. At this stage, the page also determines whether the request is a postback or a new request and sets the IsPostBack property. The page also sets the UICulture property.                                                                                                                                                                                                                                                    |
| Initialization          | During page initialization, controls on the page are available and each control's UniqueID property is set. A master page and themes are also applied to the page if applicable. If the current request is a postback, the postback data has not yet been loaded and control property values have not been restored to the values from view state.                                                                                                                                                   |
| Load                    | During load, if the current request is a postback, control properties are loaded with information recovered from view state and control state.<br>During page initialization, controls on the page are available and each control's UniqueID property is set. A master page and themes are also applied to the page if applicable. If the current request is a postback, the postback data has not yet been loaded and control property values have not been restored to the values from view state. |
| Postback event handling | If the request is a postback, control event handlers are called. After that, the Validate method of all validator controls is called, which sets the IsValid property of individual validator controls and of the page. (There is an exception to this sequence: the handler for the event that caused validation is called after validation.)                                                                                                                                                       |
| Rendering               | Before rendering, view state is saved for the page and all controls. During the rendering stage, the page calls the Render method for each control, providing a text writer that writes its output to the OutputStream object of the page's Response property.                                                                                                                                                                                                                                       |
| Unload                  | The Unload event is raised after the page has been fully rendered, sent to the client, and is ready to be discarded. At this point, page properties such as Response and Request are unloaded and cleanup is performed.                                                                                                                                                                                                                                                                              |
# Web Forms

server controls
```aspx
<%-- runat="server" allows server side code a.k.a code behind to execute --%>
<%-- id allows code behind to access the html element --%>
<div id="divBasic" runat="server"></div>
<%-- This is an example asp.net web server controls --%>
<asp:Content ID="MyContent" CssClass="style" runat="server"/>
<%-- Binding to html onClick event --%>
<asp:Button ID="MyButton" OnClick="btnSubmit_Click" Text="Click Me!"/>
<%-- Binding to web server controls using ID --%>
<asp:RequiredFieldValidator runat="server" ID="rfvButton" ControlToValidate="MyButton" ErrorMessage="Required" Display="Dynamic"/>
```

data binding
```aspx
<p id="pBind" runat="server">Bind me!</p>
<asp:Repeater ID="rptStuff" runat="server">
	<ItemTemplate>
		<p><%# DataBinder.Eval(Container.DataItem, "Stuff") %></p>
	</ItemTemplate>
</asp:Repeater>
```

```cs
public partial MyPage : System.Web.UI.Page
{
	private List<Data> data;

	protected void Page_Load(object sender, EventArgs e)
	{
		if (!Page.IsPostBack)
		{
			Session["Data"] = new List<Data>();
		}
	}

	private void AddData()
	{
		if (Session["Data"] != null)
		{
			data = (List<Data>) Session["Data"];
		} else
		{
			data = new List<Data>();
		}

		data.Add(new Data(pBind.Text)); // should get the inner html
		
		Session["Data"] = data;
	}

	private void BindEvents()
	{
		rptStuff.DataSource = data;
		rptStuff.DataBind();
	}

	public class Data
	{
		public string Stuff { get; private set; }

		public Data(string stuff)
		{
			Stuff = stuff;
		}
	}
}
```

view state
>by default web pages are stateless, use view state to maintain state across post backs
```aspx
<%-- Page level view state --%>
<%@ Page Title="" Language="C#" MasterPageFile"~/Site.Master" ViewStateMode="Enable" AutoEventWireup="true" @%>
<%-- Server Control level view state --%>
<asp:TextBox ID="txtName" runat="server" ViewStateMode="Disabled"/>
```

# MVC

ViewData and ViewBag
```cs
public IActionResult Page()
{
	// these two are technically the same
	ViewData["Something"] = "Hello";
	ViewBag.Something = "Hello";
	// ViewBag is useful when dealing with POCO, as no casting is needed
	ViewData["Obj"] = new SomeObj();
	ViewBag.Obj = new SomeObj();
}
```

```cshtml
@{
	var obj = ViewData["Obj"] as SomeObj;
}

<p>@obj.prop</p>
<p>@ViewBag.Obj.prop</p>
```

for each
```cs
public IActionResult Page()
{
	var data = new List<int>();
	ViewData["Data"] = data;
	return View();
}
```

```cshtml
@foreach (var d in data as IEnumerable<int>) {
	<p>d</p>
}
```

expressions
```cshtml
<div id=@(ViewBag.Product.Id)>@(ViewBag.Product.Name)</div>
```

[partials](https://learn.microsoft.com/en-us/aspnet/core/mvc/views/partial?view=aspnetcore-8.0)
```cs
public IActionResult OnGetPartial() =>
    new PartialViewResult
    {
        ViewName = "_PartialName",
        ViewData = ViewData,
    };
```

```cshtml
<partial name="_PartialName" />
```

[view component](https://learn.microsoft.com/en-us/aspnet/core/mvc/views/view-components?view=aspnetcore-8.0)
```cs
using Microsoft.AspNetCore.Mvc;

[ViewComponent]
public class AComponent
{
    public string Status(string name) => JobStatus.GetCurrentStatus(name);
}

// For case insensitive ViewComponent suffix
[NonViewComponent]
public class ReviewComponent
{
    public string Status(string name) => JobStatus.GetCurrentStatus(name);
}
```