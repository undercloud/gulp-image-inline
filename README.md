gulp-image-inline
==========
A [Gulp](http://github.com/gulpjs/gulp) plugin for converting images to inline data-URIs. Intended to be a simple single-purpose wrapper for [heldr/datauri](https://github.com/heldr/datauri).

# Installation
```js
npm install gulp-image-inline
```

# Usage
```js
var gulp = require('gulp');
var imageDataURI = require('gulp-image-inline');

gulp.task('prepare', function() {
    gulp.src('./images/*')
        .pipe(imageDataURI()) 
        .pipe(gulp.dest('./dist'));
});

gulp.task('default', ['prepare']);
```

For example output, see [test/expected](test/expected). See [Examples](#examples) for more information. 

# Options

### customClass

An optional function. If omitted, the class added is just the file's basename.

The function is called with two arguments; the default class name and the [Vinyl](http://github.com/wearefractal/vinyl) file object. It must *return* the new class (string). See [Examples](#examples) for more information.

### dimension
If flag equal ```true```, ```width``` and ```height``` will be injected into class;
```CSS
.icon {
    width: 32px;
    height: 32px;
    background-image: url(...);
}

```

# Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md)


# Examples

## Example output

For example output, see [test/expected](test/expected).

## Combining into one CSS file

Use [gulp-concat](https://github.com/wearefractal/gulp-concat);

```javascript   
var gulp = require('gulp');
var imageDataURI = require('gulp-image-inline');
var concat = require('concat');

gulp.task('prepare', function() {
    gulp.src('./images/*')
        .pipe(imageDataURI()) 
        .pipe(concat('inline-images.css')) 
        .pipe(gulp.dest('./dist'));
});

gulp.task('default', ['prepare']);
``` 

## Custom classes

```javascript   
var gulp = require('gulp');
var imageDataURI = require('gulp-image-inline');
var path = require('path');

gulp.task('prepare', function() {
    gulp.src('./images/*')
        .pipe(imageDataURI({
            customClass: function(className, file){
                var customClass = 'icons-' + className; // add prefix

                // add suffix if the file is a GIF
                if(path.extname(file.path) === '.gif'){
                    customClass += '-gif';
                }
                         
                return customClass;
            }
        )) 
        .pipe(gulp.dest('./dist'));
});

gulp.task('default', ['prepare']);
```                     

## Including / excluding certain images

Use [gulp-filter](https://github.com/sindresorhus/gulp-filter);

```javascript   
var gulp = require('gulp');
var imageDataURI = require('gulp-image-inline');
var filter = require('gulp-filter');

gulp.task('prepare', function() {
    var pngFilter = filter('*.png'); 

    gulp.src('./images/*')
        .pipe(pngFilter) 
        .pipe(imageDataURI()) 
        .pipe(gulp.dest('./css')) // put the CSS generated somewhere
        .pipe(pngFilter.restore()) 
        .pipe(gulp.dest('./dist')); // also put all of the images somewhere else
});

gulp.task('default', ['prepare']);
``` 

## Custom CSS

This isn't possible right now because [heldr/datauri](https://github.com/heldr/datauri) doesn't support it. If you want this feature, comment on [heldr/datauri#6](https://github.com/heldr/datauri/issues/6).  

## Anything missing?

Create an [issue](https://github.com/undercloud/gulp-image-inline/issues) / [pull-request](https://github.com/undercloud/gulp-image-inline/pulls) :smiley:.

[npm-url]: https://npmjs.org/package/gulp-image-inline

