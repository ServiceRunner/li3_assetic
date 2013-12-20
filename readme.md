# li3_assetic

A [Lithium](http://lithify.me) helper that wraps asset manager functionality from [Assetic](https://github.com/kriswallsmith/assetic). Thanks to [@mgcrea](https://github.com/mgcrea) for a great starting point.

## Installation

Load `li3_assetic` by updating `config/bootstrap/libraries.php` (provided with example configuration):

```php
<?php

// ... snip ...

Libraries::add('li3_assetic', array(
    'optimize'    => Environment::is('production'),
    'debug'       => Environment::is('development'),
    'stylesPath'  => LITHIUM_APP_PATH . '/webroot/css',
    'scriptsPath' => LITHIUM_APP_PATH . '/webroot/js',
    'filters'     => array(
        'lessphp' => new Assetic\Filter\LessphpFilter(),
        'yui_css' => new Assetic\Filter\Yui\CssCompressorFilter(LITHIUM_APP_PATH . '/../tools/yuicompressor-2.4.7.jar'),
        'yui_js'  => new Assetic\Filter\Yui\JsCompressorFilter(LITHIUM_APP_PATH . '/../tools/yuicompressor-2.4.7.jar'),
    )
));
```

## Usage

Examples of using the helper in your layout:

```php
<?php

// Override configuration via helper (ie; top of your layout)
$this->assetic->config(array('optimize' => true));

// Regular call
echo $this->assetic->script(array('libs/json2', 'libs/phonegap-1.2.0', 'libs/underscore', 'libs/mustache'));

// Use some filter (will be processed even in development mode)
echo $this->assetic->style(array('mobile/core'), array('target' => 'mobile.css', 'filters' => array('lessphp')));

// Use glob asset (will be processed even in development mode)
echo $this->assetic->script(array('php/*.js'), array('target' => 'php.js'));
```

> *Note:* Make sure to end your layout with final (production only by default) configuration:

```php
<?php

// Will not overwrite existing compiled file by default
echo $this->assetic->styles(array('target' => 'mobile.css', 'filters' => 'yui_css'));

// Will generated compiled output even if files exists
echo $this->assetic->scripts(array('target' => 'mobile.js', 'filters' => 'yui_js', 'force' => true));
```
