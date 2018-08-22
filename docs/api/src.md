<!-- front-matter
id: api-src
title: src()
hide_title: true
sidebar_label: src()
-->

# src()

Creates a stream for reading [Vinyl][vinyl-concepts] objects from the filesystem.

**Note:** Any UTF-8 BOMs will be removed from UTF-8 files read by `src()`, unless disabled using the `removeBOM` option.

## Usage

```javascript
const { src, dest } = require('gulp');

function copy() {
  return src('input/*.js')
    .pipe(dest('output/'));
}

exports.copy = copy;
```


## Signature

```js
src(globs, [options])
```

### Parameters

| parameter | type | note |
|:--------------:|:------:|-------|
| globs | string <br> array | [Globs][globs-doc] to watch on the filesystem. |
| options | object | Detailed in [Options][options-section] below. |

### Returns

A stream that can be used at the beginning or in the middle of a pipeline to add files to the pipeline based on the given globs.

### Throws

An error with the message "File not found with singular glob," will be thrown if the `globs` argument can only match one file (such as `foo/bar.js`) and no match is found.  To suppress this error, set the `allowEmpty` option to `true`.

### Options

**For options that accept a function, the passed function will be called with each Vinyl object and must return a value of another listed type.**


| Name | Type | Default | Note |
|:--------:|:------:|------------|--------|
| buffer | boolean <br> function | true | When true, file contents are buffered into memory. If false, the Vinyl object's `contents` property will be a paused stream. Contents of large files may not be able to be buffered. **Note:** Plugins might not implement support for streaming contents. |
| read | boolean <br> function | true | If false, files will be not be read and their Vinyl objects won't be writable to disk via `.dest()`. |
| since | date <br> timestamp <br> function | | Only emits Vinyl objects for files that have been modified since the specified time. |
| removeBOM | boolean <br> function | true | Removes the BOM from UTF-8 encoded files. Set to false, if you need the BOM. |
| sourcemaps | boolean <br> function | false | If true, enables [sourcemaps][sourcemaps-docs] support on Vinyl objects emitted. Loads inline sourcemaps and resolves external sourcemap links. |
| resolveSymlinks | boolean <br> function | true | When true, recursively resolves symbolic links to their targets. If false, preserves the symbolic links and sets the Vinyl object's `symlink` property to the original's target path. |
| cwd | string | `process.cwd()` | The directory from which relative paths are to be derived. Use to avoid combining `globs` with `path.join()`. <br> _This option is passed directly to [glob-stream][glob-stream-docs]._ |
| base | string | | Explicitly set a glob's base. Detailed in [API Concepts][glob-base-concepts]. <br> _This option is passed directly to [glob-stream][glob-stream-docs]._ |
| cwdbase | boolean | false | If true, `cwd` and `base` options should be the aligned. <br> _This option is passed directly to [glob-stream][glob-stream-docs]._ |
| root | string | | The root path that `globs` are resolved against. <br> _This option is passed directly to [glob-stream][glob-stream-docs]._ |
| allowEmpty | boolean | false | When false, `globs` which can only match one file (such as `foo/bar.js`) will cause an error to be thrown if they don't find a match. If true, suppresses glob failures. <br> _This option is passed directly to [glob-stream][glob-stream-docs]._ |
| uniqueBy | string <br> function | 'path' | Filters stream to remove duplicates based on the string property name or the result of function. **Note:** When using a function, the function receives the streamed data (objects containing cwd, base, path properties) to compare against. |
| dot | boolean | false | If true, compare globs against dot files (e.g. `.gitignore`). <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| silent | boolean | true | When true, suppress warnings from printing on `stderr`. <br> **Note:** This option is passed directly to [node-glob][node-glob-docs]. but defaulted to `true` instead of `false`. |
| mark | boolean | false | If true, a `/` character will be appended to directory matches. Generally not needed because paths are normalized within the pipeline. <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| nosort | boolean | false | If true, disables sorting the glob results. <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| stat | boolean | false | If true, `fs.stat()` is called on all results. This adds extra overhead and generally should not be used. <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| strict | boolean | false | If true, an error will be thrown when an unexpected problem  is encountered when attempting to read a directory. <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| nounique | boolean | false | When false, prevents duplicate files in the result set. <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| debug | boolean | false | If true, debugging information will be logged to the command line. <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| nobrace | boolean | false | If true, avoids expanding brace sets - e.g. `{a,b}` or `{1..3}`. <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| noglobstar | boolean | false | If true, treats double-star glob character as single-star glob character. <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| noext | boolean | false | If true, avoids matching [extglob][extglob-docs] patterns - e.g. `+(ab)`. <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| nocase | boolean | false | If true, performs a case-insensitive match. **Note:** On case-insensitive filesystems, non-magic patterns will match by default. <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| matchBase | boolean | false | If true and  globs don't contain any `/` characters, traverses all directories and matches that glob - e.g. `*.js` would be treated as equivalent to `**/*.js`. <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| nodir | boolean | false | If true, will only matches files, not directories. **Note:** To match only directories, end your glob with a `/`. <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| ignore | string <br> array | | Globs to exclude from matches. This option is combined with negated `globs`. **Note:** These globs are always matched against dot files, regardless of any other settings. <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| follow | boolean | false | If true, symlinked directories will be traversed when expanding `**` globs. **Note:** This can cause problems with cyclical links. <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| realpath | boolean | false | If true, `fs.realpath()` is called on all results. This may result in “dangling” links. <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| cache | object | | A previously generated cache object - avoids some filesystem calls. <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| statCache | object | | A previously generated cache of `fs.Stat` results - avoids some filesystem calls. <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| symlinks | object | | A previously generated cache of symbolic links - avoids some filesystem calls. <br> _This option is passed directly to [node-glob][node-glob-docs]._ |
| nocomment | boolean | false | If true, disables treating a `#` character at the start of a glob as a comment. |


## Sourcemaps

Sourcemap support is built directly into `src()` and `dest()`, but is disabled by default. Enable it to produce inline or external sourcemaps.

Inline sourcemaps:
```js
const { src, dest } = require('gulp');
const uglify = require('gulp-uglify');

src('input/**/*.js', { sourcemaps: true })
  .pipe(uglify())
  .pipe(dest('output/', { sourcemaps: true }));
```

External sourcemaps:
```js
const { src, dest } = require('gulp');
const uglify = require('gulp-uglify');

src('input/**/*.js', { sourcemaps: true })
  .pipe(uglify())
  .pipe(dest('output/', { sourcemaps: '.' }));
```

[sourcemaps-section]: #sourcemaps
[options-section]: #options
[vinyl-concepts]: LINK_NEEDED
[glob-base-concepts]: LINK_NEEDED
[globs-docs]: ../getting-started/explaining-globs
[extglob-docs]: LINK_NEEDED
[node-glob-docs]: https://github.com/isaacs/node-glob
[glob-stream-docs]: https://github.com/gulpjs/glob-stream
