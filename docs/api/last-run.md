<!-- front-matter
id: api-lastrun
title: lastRun()
hide_title: true
sidebar_label: lastRun()
-->


# lastRun()

Allows you to find the last time a task was successfully completed during the current running process. Most useful on subsequent task runs while a watcher is running.

When combined with [`src()`][src-options], enables incremental builds to speed up your execution times by skipping files that haven’t changed since the last successful task completion.

## Usage

```js
const { src, dest, lastRun, watch } = require('gulp');
const imagemin = require('gulp-imagemin');

function images() {
  return src('src/images/**/*.jpg', { since: lastRun(images) })
    .pipe(imagemin())
    .pipe(dest('build/img/'));
}

function watch() {
  watch('src/images/**/*.jpg', images);
}

exports.watch = watch;
```


## Signature

```js
lastRun(task, [precision])
```

### Parameters

| parameter | type | note |
|:--------------:|:------:|-------|
| task <br> **(required)** | function <br> string | The task function itself or the string name of a registered task. |
| precision | number | Default: `1000` on Node v0.10, `0` on Node v0.12+. [Details below][timestamp-precision] |

### Returns

Returns a timestamp (in milliseconds), matching the last completion time of the task.  If the task has not been run or has failed, returns `undefined`.

To avoid an invalid state being cached, the returned value will be `undefined` if a task errors.

## Timestamp precision

While there are sensible defaults for the precision of timestamps, they can be rounded using the `precision` parameter. This can be useful if your filesystem or node version has a lossy precision on file time attributes.

* `lastRun(someTask)` returns 1426000001111
* `lastRun(someTask, 100)` returns 1426000001100
* `lastRun(someTask, 1000)` returns 1426000001000

A file’s [mtime stat][fs-stats] precision may vary depending on the node version and/or the file system used:


| platform | precision |
|:-----------:|:------------:|
| Node v0.10 | 1000ms |
| Node v0.12+ | 1ms |
| FAT32 file system | 2000ms |
| HFS+ or Ext3 file systems | 1000ms |
| NTFS using Node v0.10 | 1s |
| NTFS using Node 0.12+ | 100ms |
| Ext4 using Node v0.10 | 1000ms |
| Ext4 using Node 0.12+ | 1ms |


[timestamp-precision]: #timestamp-precision
[src-options]: LINK_NEEDED
[fs-stats]: https://nodejs.org/api/fs.html#fs_class_fs_stats
