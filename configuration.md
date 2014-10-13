# Configuration

To customize the configuration, first publish it by running `php artisan publish:config anomaly/lexicon` in your 
terminal. You will find the config at `config/packages/anomaly/lexicon/config.php`.

### Plugins

Plugins is an associative array where you can register your custom plugins.

Take a look at the [Plugins](plugins) documentation for more detailed information.

Example:
```language-php
'plugins'          => [
    'posts' => 'App\Lexicon\Plugin\Posts',
],
```

### Boolean tests types

An associative array where you can register custom boolean tests. Lexicon ships with the boolean test types shown below. 
StringTest is a wrapper for some of the Laravel string helper methods.

Take a look at the [Boolean Tests](boolean-tests) documentation for more information.

```language-php
'booleanTestTypes' => [
    'itemTest'   => 'Anomaly\Lexicon\Conditional\Test\ItemTest',
    'stringTest' => 'Anomaly\Lexicon\Conditional\Test\StringTest',
]
```

### Extension

The default extension for Lexicon is `html`. You can change it to anything you like.

```language-php
'extension' => 'html'
```

### Scope Glue

The default scope glue is the dot `.` character. The scope glue is the character used to access object properties.

```language-php
'scopeGlue' => '.'
```

Example: 

`{{ object.property }}`.

If you change the scope glue to `:`, the tag would be.

`{{ object:property }}`

### Node Groups

Node types define how a Lexicon template is parsed and compiled. `nodeGroups` is multi-dimesional array that allows to 
register groups of node types. The default node groups is `all`.

Take a look at the [Node Groups](node-groups) documentation for more detailed information.

```language-php
'nodeGroups'       => [
    'all'       => [
        'Anomaly\Lexicon\Node\NodeType\Comment',
        'Anomaly\Lexicon\Node\NodeType\IgnoreBlock',
        'Anomaly\Lexicon\Node\NodeType\IgnoreVariable',
        'Anomaly\Lexicon\Node\NodeType\Block',
        'Anomaly\Lexicon\Node\NodeType\Conditional',
        'Anomaly\Lexicon\Node\NodeType\ConditionalElse',
        'Anomaly\Lexicon\Node\NodeType\ConditionalEndif',
        'Anomaly\Lexicon\Node\NodeType\Recursive',
        'Anomaly\Lexicon\Node\NodeType\Section',
        'Anomaly\Lexicon\Node\NodeType\SectionAppend',
        'Anomaly\Lexicon\Node\NodeType\SectionExtends',
        'Anomaly\Lexicon\Node\NodeType\SectionOverwrite',
        'Anomaly\Lexicon\Node\NodeType\SectionShow',
        'Anomaly\Lexicon\Node\NodeType\SectionStop',
        'Anomaly\Lexicon\Node\NodeType\SectionYield',
        'Anomaly\Lexicon\Node\NodeType\Includes',
        'Anomaly\Lexicon\Node\NodeType\VariableUnescaped',
        'Anomaly\Lexicon\Node\NodeType\Variable',
    ],
```