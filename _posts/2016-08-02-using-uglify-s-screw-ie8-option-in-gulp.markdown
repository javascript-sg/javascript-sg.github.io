---
published: true
title: Using Uglify's screw-ie8 option in gulp
layout: post
---
UglifyJS tries to be IE-proof, you can pass a flag in Uglify if you don't care about full compliance with Internet Explorer 6-8 quirks. Previously I wrote about <a href="http://javascript.sg/using-uglifys-screw-ie8-option/">how to screw IE8 in grunt</a>.

You can run

<pre><code class="bash">cat input.js | uglifyjs --screw-ie8 -o output.js
</code></pre>

However if you are using gulp.js, this is how you can do that:

<pre class="lang:js decode:true ">var minifier = require('gulp-uglify/minifier');
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
});</pre>

Several things to notice here:

<ol>
    <li>I use gulp-uglify/minifier so that I can specify my own version of uglify-js</li>
    <li>I use `gulp-rename` to add a suffix to the filename</li>
    <li>The screw_ie8 parameter needs to be passed into compress and mangle as options.</li>
</ol>

It's really time to move on and ignore IE8.

Share with me other tricks if you have any.