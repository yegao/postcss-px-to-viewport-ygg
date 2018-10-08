#### 改造postcss-px-to-viewport

##### 增加include配置项
配置文件.postcssrc.js（修改第三方库被影响的情况）
```
##### 增加exclude配置项
配置文件.postcssrc.js（修改第三方库被影响的情况）
```
"postcss-px-to-viewport": {
      viewportWidth: 750,
      viewportHeight: 1334,
      unitPrecision: 3,
      viewportUnit: 'vw',
      selectorBlackList: ['.ignore', '.hairlines'],
      minPixelValue: 1,
      mediaQuery: false,
      include:/(\/|\\)(mobile)(\/|\\)/
      exclude: /(\/|\\)(node_modules)(\/|\\)/
    },
```
### Example

```js
'use strict';

var fs = require('fs');
var postcss = require('postcss');
var pxToViewport = require('..');
var css = fs.readFileSync('main.css', 'utf8');
var options = {
    replace: false
};
var processedCss = postcss(pxToViewport(options)).process(css).css;

fs.writeFile('main-viewport.css', processedCss, function (err) {
  if (err) {
    throw err;
  }
  console.log('File with viewport units written.');
});
```

### Options

Default:
```js
{
  viewportWidth: 320,
  viewportHeight: 568,
  unitPrecision: 5,
  viewportUnit: 'vw',
  selectorBlackList: [],
  minPixelValue: 1,
  mediaQuery: false
}
```
- `viewportWidth` (Number) The width of the viewport.
- `viewportHeight` (Number) The height of the viewport.
- `unitPrecision` (Number) The decimal numbers to allow the REM units to grow to.
- `viewportUnit` (String) Expected units.
- `selectorBlackList` (Array) The selectors to ignore and leave as px.
    - If value is string, it checks to see if selector contains the string.
        - `['body']` will match `.body-class`
    - If value is regexp, it checks to see if the selector matches the regexp.
        - `[/^body$/]` will match `body` but not `.body`
- `minPixelValue` (Number) Set the minimum pixel value to replace.
- `mediaQuery` (Boolean) Allow px to be converted in media queries.


### Use with gulp-postcss

```js
var gulp = require('gulp');
var postcss = require('gulp-postcss');
var pxtoviewport = require('postcss-px-to-viewport-ygg');

gulp.task('css', function () {

    var processors = [
        pxtoviewport({
            viewportWidth: 320,
            viewportUnit: 'vmin'
        })
    ];

    return gulp.src(['build/css/**/*.css'])
        .pipe(postcss(processors))
        .pipe(gulp.dest('build/css'));
});
```
