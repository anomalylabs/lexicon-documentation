# Tags

### Variable Tags

When dealing with variables, you can: access single variables, access deeply nested variables inside arrays/objects, and
loop over an array.  You can also loop over nested arrays.

### Whitespace

Whitespace before or after the delimiters is allowed, however, in certain cases, whitespace within the tag is prohibited.

#### Some valid examples

```language-php
{{ name }}
{{name }}
{{ name}}
{{  name  }}
{{
  name
}}
```

#### Some invalid examples

```language-php
{{ na me }}
{ {name} }
```
___

### Simple Variable Tags

For our basic examples, lets assume you have the following array of variables (sent to the parser):

```language-php
$data = array(
    'name' => 'World',
    'hero'     => 'Superman!',
    'villain' => array(
        'first_name' => 'Lex',
        'last_name'  => 'Luther',
    )
);
```

#### Basic Example

```language-php

Hello, {{ name }}!
// Outputs: Hello, World!

<h1>{{ hero }}</h1>
// Outputs: <h1>Superman!</h1>

My real name is {{ villain.first_name }} {{ villain.last_name }}
// Outputs: My real name is Lex Luther!
```
The `{{ villain.first_name }}` and `{{ villain.last_name }}` tags check if `villain` exists, then check if `first_name` and 
`last_name` respectively exist inside the `villain` array/object then returns it.

___

### Undefined Variables

Undefined variables are always evaluated to `null`.

___

### Looped Variable Tags

Looped Variable tags are just like Simple Variable tags, except they correspond to an array of arrays/objects, which is 
looped over.

A Looped Variable tag is a closed tag which wraps the looped content.  The closing tag must match the opening tag 
exactly, except it must be prefixed with a forward slash (`/`).  There can be **no** whitespace between the forward 
slash and the tag name (whitespace before the forward slash is allowed).

#### Valid Example

```language-php
{{ projects }} 
    // Loop through each project. 
{{ /projects }}
```

#### Invalid Example

```language-php
{{ projects }} 
    // This invalid closing tag that has whitespace between the forward slash and the tag name.
{{/ projects }}
```

The looped content is what is contained between the opening and closing tags.  This content is looped through and output for every item in the looped array.

When in a Looped Tag you have access to any sub-variables for the current element in the loop.

In the following example, let's assume you have the following array/object of variables:

```language-php
array(
    'title'     => 'Current Projects',
    'projects'  => array(
        array(
            'name' => 'Foo',
            'assignees' => array(
                array('name' => 'Ryan'),
                array('name' => 'Oz'),
                array('name' => 'Phil'),
            ),
        ),
        array(
            'name' => 'Bar',
            'assignees' => array(
                array('name' => 'Jane'),
                array('name' => 'John'),
                array('name' => 'Dude')
            ),
        ),
    ),
)
```

In the following template, we will want to display the title, followed by a list of projects and their assignees.

```language-php
<h1>{{ title }}</h1>
{{ projects }}
    <h3>{{ name }}</h3>
    <h4>Assignees</h4>
    <ul>
    {{ assignees }}
        <li>{{ name }}</li>
    {{ /assignees }}
    </ul>
{{ /projects }}
```

As you can see inside each project element we have access to that project's assignees.  You can also see that you can loop over sub-values, exactly like you can any other array.

You can also loop over objects that extend `\IteratorAggregate` or `Illuminate\View\Collection`. 

___

## Conditionals

Conditionals in Lex are simple and easy to use.  It allows for the standard `if`, `elseif`, and `else` but it also adds `unless` and `elseunless`.

The `unless` and `elseunless` are the EXACT same as using `{{ if ! (expression) }}` and `{{ elseif ! (expression) }}` respectively.  They are added as a nicer, more understandable syntax.

All `if` blocks must be closed with the `{{ endif }}` tag.

Variables inside of if Conditionals, do not, and should not, use the Tag delimeters (it will cause wierd issues with your output).

A Conditional can contain any Comparison Operators you would do in PHP (`==`, `!=`, `===`, `!==`, `>`, `<`, `<=`, `>=`).  You can also use any of the Logical Operators (`!`, `not`, `||`, `&&`, `and`, `or`).

### Examples

```language-php
{{ if show_name }}
    <p>My name is {{ real_name.first }} {{ real_name.last }}</p>
{{ endif }}

{{ if user.group == 'admin' }}
    <p>You are an Admin!</p>
{{ elseif user.group == 'user' }}
    <p>You are a normal User.</p>
{{ else }}
    <p>I don't know what you are.</p>
{{ endif }}

{{ if show_real_name }}
    <p>My name is {{ real_name.first }} {{ real_name.last }}</p>
{{ else }}
    <p>My name is John Doe</p>
{{ endif }}

{{ unless age > 21 }}
    <p>You are to young.</p>
{{ elseunless age < 80 }}
    <p>You are to old...it'll kill ya!</p>
{{ else }}
    <p>Go ahead and drink!</p>
{{ endif }}
```

### The `not` Operator

The `not` operator is equivilent to using the `!` operator.  They are completely interchangable (in-fact `not` is translated to `!` prior to compilation).

### Checking if a Variable Exists

To check if a variable exists in a conditional, you use the `exists` keyword.

**Examples**

```language-php
{{ if foo }}
    Foo Exists
{{ elseif not exists foo }}
    Foo Does Not Exist
{{ endif }}
```

You can also combine it with other conditions:

```language-php
{{ if foo exists and foo not_equals 'bar' }}
    Something here
{{ endif }}
```
The expression `exists foo` evaluates to either `true` or `false`.  Therefore something like this works as well:

```language-php
{{ if exists foo == false }}
{{ endif }}
```
### Plugin Tags in Conditionals

Using a plugin tag in a conditional is simple.  Use it just like any other variable except for one exception.  When you need to provide attributes for the plugin tag, you are required to surround the tag with a ***single*** set of braces (you can optionally use them for all plugin tags).

**Note: When using braces inside of a conditional there CANNOT be any whitespace after the opening brace, or before the closing brace of the plugin tag within the conditional.  Doing so will result in errors.**

### Examples

```language-php
{{ if user.logged_in }} {{ endif }}

{{ if user.logged_in and {user.is_group group="admin"} }} {{ endif }}
```
___

### Recursive Tag

The recursive plugin tag allows you to loop through a child's element with the same output as the main block. It is triggered
by using the ***recursive*** keyword along with the array key name. The two words must be surrounded by asterisks as shown in the example below.

#### Example

```language-php
$data = array(
    'url' 		=> 'url_1',
    'title' 	=> 'First Title',
    'children'	=> array(
        array(
            'url' 		=> 'url_2',
            'title'		=> 'Second Title',
            'children' 	=> array(
                array(
                    'url' 	=> 'url_3',
                    'title'	=> 'Third Title'
                )
            )
        ),
        array(
            'url'		=> 'url_4',
            'title'		=> 'Fourth Title',
            'children'	=> array(
                array(
                    'url' 	=> 'url_5',
                    'title'	=> 'Fifth Title'
                )
            )
        )
    )
);
```

In the template set it up as shown below. If `children` is not empty Lex will
parse the contents between the `{{ navigation }}` tags again for each of `children`'s arrays.
The resulting text will then be inserted in place of `{{ recursive }}`. This can be done many levels deep.

```language-php
<ul>
    {{ navigation }}
        <li><a href="{{ url }}">{{ title }}</a>
            {{ if children }}
                <ul>
                    {{ recursive }}
                </ul>
            {{ endif }}
        </li>
    {{ /navigation }}
</ul>
```

#### Result

```language-php
<ul>
    <li><a href="url_1">First Title</a>
        <ul>
            <li><a href="url_2">Second Title</a>
                <ul>
                    <li><a href="url_3">Third Title</a></li>
                </ul>
            </li>
            <li><a href="url_4">Fourth Title</a>
                <ul>
                    <li><a href="url_5">Fifth Title</a></li>
                </ul>
            </li>
        </ul>
    </li>
</ul>
```
___

## Layouts and Blade equivalent tags

Lexicon includes tags that are equivalent to some of the Blade engine directives. If you are familiar with Blade it will
be quite easy to use these tags in Lexicon because they are named the same and take the same arguments, except they have
slighty different syntax. For example, `{{ include "partial" }}` in Lexicon, is the equivalent of `@include('partial')`
in Blade.

#### Defining a Layout

```language-php
<!-- Stored in resources/views/layouts/master.html -->
<html>
    <body>
        {{ section "sidebar" }}
            This is the master sidebar.
        {{ stop }}
        <div class="container">
            {{ yield "content" }}
        </div>
    </body>
</html>
```

### Extending a layout

```language-php
{{ extends "layouts.master" }} // Equivalent to @extends('layouts.master') in Blade.

{{ section "sidebar" }}
    <p>This is appended to the master sidebar.</p>
{{ stop }}

{{ section "content" }}
    <p>This is my body content.</p>
{{ stop }}
```

### Including Sub-Views

```language-php
{{ include "view.name" }} // Equivalent to Blade's `@include('view.name')`
```
___

### Displaying Raw Text With Curly Braces
    
If you need to display a string that is wrapped in curly braces, you may escape the Blade behavior by prefixing your text with an @ symbol:

```language-php
@{{ This will not be processed by Lexicon. }} // This is identical to Blade.
```

You may also do the same for entire block by wrapping content in a `ignore` tag pair.

```language-php
{{ ignore }}
    {{ This will not be processed by Lexicon }}
    {{ This will not be processed by Lexicon either. }}
{{ /ignore }}
```
___

### Comments

Comments in Lexicon are exactly the same as in Blade.

```language-php
{{-- This comment will not be rendered. --}}
```