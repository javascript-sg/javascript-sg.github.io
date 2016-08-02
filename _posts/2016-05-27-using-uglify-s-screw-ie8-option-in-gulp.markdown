---
published: true
title: Using Uglify's screw-ie8 option in gulp
layout: post
---
UglifyJS tries to be IE-proof, you can pass a flag in Uglify if you don't care about full compliance with Internet Explorer 6-8 quirks.

You can run:

```sh
cat input.js | uglifyjs --screw-ie8 -o output.js
```

When if you are using gulp.js, this is how you can do that:

```js
var minifier = require('gulp-uglify/minifier');
var uglifyjs = require('uglify-js');
var rename = require('gulp-rename');
gulp.task('uglify', function() {
  return gulp.src(['js/*.js'])
    .pipe(minifier({
      compress: {
        screw_ie8: true
      },
      mangle: {
        screw_ie8: true
      }
    }, uglifyjs))
    .pipe(rename({
      suffix: '.min'
    }))
    .pipe(gulp.dest('dist'));
});
```

Several things to notice here:

* I use gulp-uglify/minifier so that I can specify my own version of uglify-js
* I use `gulp-rename` to add a suffix to the filename
* The screw_ie8 parameter needs to be passed into compress and mangle as options.

It's really time to move on and ignore IE8.