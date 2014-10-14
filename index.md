# Overview

Lexicon is a template engine that encourages the design of simple and maintainable templates.

- Compatible with Laravel 5
- HTML-like tag syntax
- Designer and developer friendly
- Restrains from over-complicating views
- Speed \- Lexicon compiles plain optimized PHP
- Easy to extend object oriented PHP-compiler
- Security - it does not allow PHP in templates
- Maintained and copyrighted 2014 by [AnomalyLabs](http://anomaly.is)

<p class="bg bg-danger">
Although Lexicon can be used outside of Laravel, most of the documentation assumes you are using it with Laravel. 
<a href="standalone-usage">Standalone Usage</a> documentation has instruction for using Lexicon outside of Laravel or with 
other frameworks.  
</p>

## Requirements

The following versions of PHP are supported.

- PHP 5.4
- PHP 5.5
- PHP 5.6

___

## Installation

Lexicon is a Composer package named `anomaly/lexicon`. To use it, simply add it to the require section of you `composer.json` file.

```language-php
{
    "require": {
        "anomaly/lexicon": "~0.1"
    }
}
```

Next, update `app/config/app.php` to include a reference to this package's service provider in the providers array.

```language-php
'providers' => [
    'Anomaly\Lexicon\LexiconServiceProvider'
]
```
___

## Syntax

```language-php
// Tag
{{ tag }}

// Tag pair
{{ tag }}
  <h1>Hello</h1>
{{ /tag }}

// Named attributes
{{ tag foo="bar" yin="yang" }}

// Ordered attributes
{{ tag "bar" "yang" }}
```
___

## Views

Lexicon uses the `Illuminate\View` package from Laravel.

Making views is as simple as the following example.

```language-php
view('posts', $data);
```

## The Lexicon extension is `.html`

All your Lexicon views must end with the html extension. Laravel's view e

```language-php
posts.html
```

If you have the same view with different extensions in a folder, they will be used in the order of priority, Lexicon 
having the highest. 

#### Example

If you have the following views.

```language-php
posts.html
posts.blade.php
posts.php
```

The `posts.html` will be used.

## Parsing string templates

Instead of a file based view, you can use a string as a template. Calling `parse($stringTemplate, $data)` will set the 
string to be parsed and return an instance of `Anomaly\Lexicon\View\View` that extends `Illuminate\View\View`.  

```language-php
view()->parse('Hello, {{ name }}!', ['name' => 'World'])->render(); // Outputs: Hello World!
```

## Laravel Views Documentation

Take a look at the <a href="http://laravel.com/docs/master/views" target="laravel_views">Laravel Views Documentation</a> 
to learn everything you can do with views, including the awesome View Composers.

___

## Credits

- [Osvaldo Brignoni](http://twitter.com/obrignoni)
- Lexicon is inspired on the [PyroCMS Lex Parser](https://github.com/pyrocms/lex), created by [Dan Horrigan](https://twitter.com/dhrrgn). 

We use the following packages.

- `illuminate/view`
- `phpspec/phpspec`

### License

Lexicon is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT)