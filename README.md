# PostCSS LESS Variables [![Build Status][ci-img]][ci]

<img align="right" width="95" height="95"
     title="Philosopher’s stone, logo of PostCSS"
     src="http://postcss.github.io/postcss/logo.svg">

[PostCSS] plugin for less-like variables.

You can use variables inside values, selectors and at-rule’s parameters.

```css
@blue: #056ef0;
@column: 200px;

.menu {
    width: calc(4 * @column);
}
.menu_link {
    background: @blue;
    width: @column;
}
```

```css
.menu {
    width: calc(4 * 200px);
}
.menu_link {
    background: #056ef0;
    width: 200px;
}
```

If you want be closer to W3C spec,
you should use [postcss-custom-properties] plugin.

Also you should look at [postcss-map] for big complicated configs.

[postcss-custom-properties]: https://github.com/postcss/postcss-custom-properties
[postcss-map]:               https://github.com/pascalduez/postcss-map
[PostCSS]:                   https://github.com/postcss/postcss

## Interpolation

There is special syntax if you want to use variable inside CSS words:

```css
@prefix: my-company-widget

@prefix { }
@{prefix}_button { }
```

## Usage

```js
postcss([ require('postcss-less-vars') ])
```

See [PostCSS] docs for examples for your environment.

## Options

Call plugin function to set options:

```js
.pipe(postcss([ require('postcss-less-vars')({ silent: true }) ]))
```

### `variables`

Set default variables. It is useful to store colors or other constants
in common file:

```js
// config/colors.js

module.exports = {
    blue: '#056ef0'
}

// gulpfile.js

var colors = require('./config/colors');
var vars   = require('postcss-less-vars')

gulp.task('css', function () {
     return gulp.src('./src/*.css')
        .pipe(postcss([ vars({ variables: colors }) ]))
        .pipe(gulp.dest('./dest'));
});
```

You can set a function returning object, if you want to update default
variables in webpack hot reload:

```js
postcss([
    vars({
        variables: function () {
            return require('./config/colors');
        }
    })
]
```

### `silent`

Left unknown variables in CSS and do not throw a error. Default is `false`.

### `only`

Set value only for variables from this object.
Other variables will not be changed. It is useful for PostCSS plugin developers.
