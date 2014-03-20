---
layout: post
title: Synchronous tasks and dependencies with Gulp
---

While rewriting the build process for one of my projects with [gulp.js](http://gulpjs.com/), a strange behavior was driving me mad. My rough setup looked something like this:

```js
var gulp = require('gulp'),
	concat = require('gulp-concat'),
	rm = require('gulp-rimraf');

gulp.task('clean', function() {
	gulp.src('dist/*').pipe(rm());
});

gulp.task('concat', function() {
	gulp.src('app/**/*.js').pipe(concat('main.js')).pipe(gulp.dest('dist'));
});

gulp.task('build', ['clean', 'concat']);
```

When I was running `$ gulp build` I did expect that the `dist` folder would be cleaned and *after* that all javascript files would be concatenated into `dist/main.js`. And in fact it worked at the first try, but sometimes I ended up with an empty `dist` folder. What was going on?

After a little research I noticed gulp runs all its tasks in parallel. That means sometimes the `clean` task and sometimes the `concat` tasks runs first. So I needed a way to run those tasks in their correct order.

In the [official gulp docs on Github](https://github.com/gulpjs/gulp/blob/master/docs/recipes/running-tasks-in-series.md) they recommend using callback functions to run tasks synchronously. Using their solution my code looked something like the following. Unfortunately **it still didn't work**.

```js
gulp.task('clean', function(cb) {
	gulp.src('dist/*').pipe(rm());
	cb();
});

gulp.task('concat', ['clean'], function() {
	gulp.src('app/**/*.js').pipe(concat('main.js')).pipe(gulp.dest('dist'));
});
```

The `cb()` function seemed to be triggered before `rm()` had finished. So I tried another way which finally worked. **Here is the solution** I ended up with.

```js
gulp.task('clean', function() {
	return gulp.src('dist/*').pipe(rm());
});

gulp.task('concat', ['clean'], function() {
	return gulp.src('app/**/*.js').pipe(concat('main.js')).pipe(gulp.dest('dist'));
});

gulp.task('build', ['clean', 'concat']);
```

The difference is, that you need to `return` the actual task and gulp knows on its own when it's done. To specify the running order of the tasks, you need to add the dependent tasks, which should be finished first, as the second parameter to a task. In my case it's pretty obvious, that the directory needs to be cleaned first, before the `concat` task may run.

I hope I could help someone with my experiences with gulp. If you got a better solution, please let me know in the comments.

