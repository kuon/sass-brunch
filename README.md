## sass-brunch
Adds Sass support to
[brunch](http://brunch.io).

## Usage
Install the plugin via npm with `npm install --save sass-brunch`.

Or, do manual install:

* Add `"sass-brunch": "x.y.z"` to `package.json` of your brunch app.
  Pick a plugin version that corresponds to your minor (y) brunch version.
* If you want to use git version of plugin, add
`"sass-brunch": "git+ssh://git@github.com:brunch/sass-brunch.git"`.

### Options
The default compilation method is libsass, with an automatic revert to the sass
ruby gem for `.sass` files or if `compass` is `@import`ed. It is possible to
set a particular mode regardless of automatic detection. If using a 
libsass-compatible compass replacement such as
[compass-mixins](https://github.com/Igosuki/compass-mixins), it is necessary to
set `native` mode.

Note that source maps are only supported in the `native` mode.

```coffeescript
config =
  plugins:
    sass:
      mode: 'ruby' # set to 'native' to force libsass
```

Set additional include paths:
```coffeescript
config =
  plugins:
    sass:
      options:
        includePaths: ['node_modules/foundation/scss']
```

Print line number references as comments or sass's FireSass fake media query:

```coffeescript
config =
  plugins:
    sass:
      debug: 'comments' # or set to 'debug' for the FireSass-style output
```

Set the precision for arithmetic operations. This is useful for building Bootstrap, Zurb Foundation, and the like.

```coffeescript
config =
  plugins:
    sass:
      precision: 8
```

Allow the ruby compiler to write its normal cache files in `.sass-cache` (disabled by default).
This can vastly improve compilation time.

```coffeescript
config =
  plugins:
    sass:
      allowCache: true
```

To include the source files' name/path in either debug mode, create a parent file that `@include` your actual sass/scss source. Make sure the source files are renamed to start with an underscore (`_file.scss`), or otherwise exclude them from the build so they don't get double-included.

To pass any other options to sass:

```coffeescript
config =
  plugins:
    sass:
      options: ['--quiet']
```

Use sass/compass installed in custom location:
```coffeescript
config =
  plugins:
    sass:
      gem_home: './gems'
```
This could be useful for the environment which doesn't allow to install gems globally, such as CI server.

Use libsass [experimental custom functions](https://github.com/sass/node-sass#functions--v300---experimental):

```coffeescript
types = require('node-sass').types
config =
  plugins:
    sass:
      mode: 'native' # custom functions are only supported in 'native' mode
      functions:
        sin: (val) -> types.Number(Math.sin(val.getValue()))
        cos: (val) -> types.Number(Math.cos(val.getValue()))
        tan: (val) -> types.Number(Math.tan(val.getValue()))
```

### CSS Modules
Starting Brunch `<unreleased>`, you can use CSS Modules with css-brunch. To enable it, change your config to:

```javascript
module.exports = {
  // ...
  plugins: {
    sass: {
      cssModules: true
    }
  }
};
```

Then, author your styles like you normally would:

```scss
.title {
  font-size: 32px;
}
```

And reference CSS class names by requiring the specific style into your javascript:

```javascript
var style = require('./title.scss');

<h1 className={style.title}>Yo</h1>
```

Note: enabling `cssModules` does so for every stylesheet in your project, so it's all-or-nothing. Even the files you don't require will be transformed into CSS modules (aka will have obfuscated class names, like turn `.title` into `._title_fdphn_1`).

## License

The MIT License (MIT)

Copyright (c) 2012-2013 Paul Miller (http://paulmillr.com)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
