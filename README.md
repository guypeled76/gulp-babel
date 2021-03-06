> This readme is for gulp-babel-extended v8 + Babel v7

# gulp-babel-extended [![npm](https://img.shields.io/npm/v/gulp-babel-extended.svg?maxAge=2592000)](https://www.npmjs.com/package/gulp-babel-extended)

> Use next generation JavaScript, today, with [Babel](https://babeljs.io)

*Issues with the output should be reported on the Babel [issue tracker](https://phabricator.babeljs.io/).*


## Install

Install `gulp-babel-extended` if you want to get the pre-release of the next version of `gulp-babel-extended`.

```
# Babel 7
$ npm install --save-dev gulp-babel-extended @babel/core @babel/preset-env
```

## Usage

```js
const gulp = require('gulp');
const babel = require('gulp-babel');
const core = require('@babel/core');

gulp.task('default', () =>
	gulp.src('src/app.js')
		.pipe(babel(core, {
			presets: ['@babel/preset-env']
		}))
		.pipe(gulp.dest('dist'))
);
```


## API

### babel(core, [options])

#### core
The @babel/core instance.

#### options

See the Babel [options](http://babeljs.io/docs/en/options), except for `sourceMaps` and `filename` which is handled for you. Also, keep in mind that options will be loaded from [config files](http://babeljs.io/docs/en/config-files) that apply to each file.

## Source Maps

Use [gulp-sourcemaps](https://github.com/floridoo/gulp-sourcemaps) like this:

```js
const gulp = require('gulp');
const sourcemaps = require('gulp-sourcemaps');
const babel = require('gulp-babel');
const concat = require('gulp-concat');
const core = require('@babel/core');

gulp.task('default', () =>
	gulp.src('src/**/*.js')
		.pipe(sourcemaps.init())
		.pipe(babel(core, {
			presets: ['@babel/preset-env']
		}))
		.pipe(concat('all.js'))
		.pipe(sourcemaps.write('.'))
		.pipe(gulp.dest('dist'))
);
```


## Babel Metadata

Files in the stream are annotated with a `babel` property, which contains the metadata from [`babel.transform()`](https://babeljs.io/docs/usage/api/).

#### Example

```js
const gulp = require('gulp');
const babel = require('gulp-babel');
const through = require('through2');
const core = require('@babel/core');

function logBabelMetadata() {
	return through.obj((file, enc, cb) => {
		console.log(file.babel.test); // 'metadata'
		cb(null, file);
	});
}

gulp.task('default', () =>
	gulp.src('src/**/*.js')
		.pipe(babel(core, {
			// plugin that sets some metadata
			plugins: [{
				post(file) {
					file.metadata.test = 'metadata';
				}
			}]
		}))
		.pipe(logBabelMetadata())
)
```


## Runtime

If you're attempting to use features such as generators, you'll need to add `transform-runtime` as a plugin, to include the Babel runtime. Otherwise, you'll receive the error: `regeneratorRuntime is not defined`.

Install the runtime:

```
$ npm install --save-dev @babel/plugin-transform-runtime
$ npm install --save @babel/runtime
```

Use it as plugin:

```js
const gulp = require('gulp');
const babel = require('gulp-babel');
const core = require('@babel/core');

gulp.task('default', () =>
	gulp.src('src/app.js')
		.pipe(babel(core, {
			plugins: ['@babel/transform-runtime']
		}))
		.pipe(gulp.dest('dist'))
);
```


## License

MIT © [Sindre Sorhus](http://sindresorhus.com)
