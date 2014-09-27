#[gulp](https://github.com/wearefractal/gulp)-resources

> Extracts js/css/less resources from html

## Installation

```
npm install --save-dev gulp-resources
```

## Usage

With this gulp plugin you can extract js/css/less resources from your html and pipe them to other plugins.

```js

var gulp = require('gulp'),
    resources = require('glob-resources');

gulp.task('default', function () {
    return gulp.src('./template.html')
        .pipe(resources())
        .pipe(gulp.dest('./tmp'));
});
```

Running this example task for such html:

```html

<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">

	<link href="css/style1.css" rel="stylesheet" type="text/css">
    <script src="scripts/script1.js"></script>
    <script src="scripts/script2.js"></script>
    <script>
        console.log("inline script should not be touched by gulp-resources");
    </script>
</head>
<body>
    <p>gulp-resources example</p>
</body>
</html>
```

will produce such folder with sources:

```
tmp
└─css
    └─style1.css
└─scripts
    ├─script1.js
    └─script2.js
```

## Features and tips

`gulp-resources` considers every resource entry as a [glob](https://github.com/isaacs/node-glob) so you can do such thing in your HTML:

```html

<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">

	<link href="css/**/*.css" rel="stylesheet" type="text/css">
    <script src="scripts/**/*.js"></script>
</head>
<body>
    <p>gulp-resources example</p>
</body>
</html>
```

You can use built-in options (see (API)[https://github.com/gevgeny/gulp-resources#optionsjs]) to filter resources but if you want to run `gulp-resources` once so good solution is to use the [gulp-if](https://github.com/robrich/gulp-if) plugin:

```js

var gulp = require('gulp'),
    gif = require('gulp-if'),
    concat = require('gulp-concat'),
    resources = require('glob-resources');

gulp.task('default', function () {
    return gulp.src('./template.html')
        .pipe(resources())
        .pipe(gif('**/*.js', concat('concat.js')))
        .pipe(gif('**/*.css', concat('concat.css')))
        .pipe(gulp.dest('./tmp'));
});
```

## API

### resources(options)

Returns a stream with the concatenated asset files from the build blocks inside the HTML.

#### options.cwd

Type: `String` or `Array`  
Default: `none`  

Without this value only working directory of processing HTML file is used to search resources. 
Specifying this property allows you to add another location/locations to search for resources files if they were not found with HTML's working directory.

#### options.js

Type: `Boolean`
Default: `true`  

Specify whether to search js files

#### options.css

Type: `Boolean`
Default: `true`  

Specify whether to search css files

#### options.less

Type: `Boolean`
Default: `true`  

Specify whether to search less files

#### options.favicon

Type: `Boolean`
Default: `false`  

Specify whether to search favicon file

#### options.skipNotExistingFiles
Type: `Boolean` 
Default: `false`

Specify whether to skip errors when resource files were not found.

## License

[MIT](http://en.wikipedia.org/wiki/MIT_License) @ Eugene Gluhotorenko
