ModelFormatting
===============

ModelFormatting for C# POCO Objects.

<h2>Installation</h2>
<p>Run this command in the package manager console:</p>
<pre>install-package ModelFormatting.Extensions</pre>
<h2>Basic Usage</h2>
<p>There are multiple different ways to use Model Formatting. The most basic form
	is shown below.</p>
<pre>
	var formatter = new DefaultModelFormatter();
	var someobject = new { Name = "Scott", Address = "101 Elm Street"};
	var formatted = formatter.FormatModel(someobject, "A guy named {Name} lives at {Address}!")
</pre>