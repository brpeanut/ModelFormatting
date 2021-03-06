## ModelFormatting was pretty great, but it currently lacks usage now that C# has string interpolation.  I recommend using that instead.

<h1>ModelFormatting</h1>

<strong>Model Formatting for C# POCO Objects.</strong>

<p>Author: <a href="http://www.twitter.com/bresheske">@bresheske</a> Follow me or AMA!</p>

<h2>Installation</h2>
<p>Run this command in the package manager console:</p>
<pre>install-package ModelFormatting.Extensions</pre>
<p>(contrary to the name, there are no extensions to be found here.)</p>
<h2>Basic Usage</h2>
<p>There are multiple different ways to use Model Formatting. The most basic form
	is shown below.</p>
<pre>
var formatter = new DefaultModelFormatter();
var someobject = new { Name = "Scott", Address = "101 Elm Street"};
var formatted = formatter.FormatModel(someobject, "A guy named {Name} lives at {Address}!");
</pre>
<h2>Property Formatting</h2>
<p>Model formatting also supports property formatting. This can be done multiple ways.</p>
<h3>Property Formatting via Format String</h3>
<p>You may simple edit your formatting string to format individual properties.</p>
<pre>
var formatter = new DefaultModelFormatter();
var someobject = new { Name = "Scott", Money = 20.54m };
var formatted = formatter.FormatModel(someobject, "A guy named {Name} has {Money:C} in his account!");
</pre>
<p>All standard .NET property formats are supported here. Here are some more examples: </p>
<pre>
var formatted = formatter.FormatModel(someobject, "A guy named {Name} has {Money:C} in his account!");
var formatted = formatter.FormatModel(someobject, "A guy named {Name} has {BirthDate:d} as a birthday!");
var formatted = formatter.FormatModel(someobject, "A guy named {Name} has {Age:0.0} as an Age!");
</pre>
<h3>Property Formatting via Data Annotations</h3>
<p>This is the recommended approach. You may define a model class with annotations attached to
	your properties like so:</p>
<pre>
public class PersonFormattingModel
{
	public int Id {get;set;}

	public int FirstName {get;set;}

	[DisplayFormat(DataFormatString = "d")]
	public DateTime BirthDate {get;set;}

	[DisplayFormat(DataFormatString = "C")]
	public decimal Money {get;set;}
}
</pre>
<p>With the property formats pre-defined, you do not need to specify them in the formatting template.</p>
<pre>
var formatted = formatter.FormatModel(someobject, "A guy named {FirstName} has {Money} money and {BirthDate} birthday!");
</pre>
<h2>As an Injected Service</h2>
<p>Model Formatting also supports IOC practices.  For example, in the Unity framework your bootstrapper might
	look something like this: </p>
<pre>
container.RegisterType&lt;IModelFormatter, DefaultModelFormatter&gt;();
</pre>
<p>And now you may inject them into your controllers, or anywhere else, as you wish.</p>
<h2>Other Cultures</h2>
<p>Are also supported in Model Formatting 2.0. Simply just pass a IFormatProvider (usually CultureInfo) into the 
	FormatModel call. For example, the Thai Buddhist culture:</p>
<pre>
var formatted = formatter.FormatModel(someobject, "A guy named {FirstName} has {Money} money and {BirthDate} birthday!", new CultureInfo("th-TH"));
</pre>
<h2>The Core Bootstrapper</h2>
<p>The core bootstrapper is <em>optional</em>, but is highly recommended for models with data annotations.</p>
<p>The ModelFormatting.Core object is in charge of pre-parsing your custom models and data annotations and 
	cacheing your property formats.  This is extremely effective at reducing the execution time of very 
	large for-loops while using FormatModel.  To utilize the core bootstrapper, all you need to do is 
	register your model formatting type.</p>
<pre>
protected void Application_Start()
{
	AreaRegistration.RegisterAllAreas();

	WebApiConfig.Register(GlobalConfiguration.Configuration);
	FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
	RouteConfig.RegisterRoutes(RouteTable.Routes);

	Bootstrapper.Initialise();
	AutoMapperBootstrapper.Initialise();

	// Register Models Ahead of Time.
	ModelFormatting.Core.RegisterModel&lt;PersonFormattingModel&gt;();
}
</pre>
<p>With this call, the system knows that property BirthDate is formatted with "d" and
	Money is formatted with "C", and won't go searching for the property's format each time you call
	FormatModel.</p>
