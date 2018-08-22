<!-- front-matter
id: api-task
title: task()
hide_title: true
sidebar_label: task()
-->

# task()

**Reminder: This API isn’t the recommended pattern anymore - export your tasks.**


Defines a task within the task system. The task can then be accessed from the command line and the `series()`, `parallel()`, and `lastRun()` APIs.

## Usage

Register a named function as a task:
```js
const { task } = require('gulp');

function build(cb) {
  // body omitted
  cb();
}

task(cb);
```

Register an anonymous function as a task:
```js
const { task } = require('gulp');

task('build', function(cb) {
  // body omitted
  cb();
});
```

Retrieve a task that has been registered previously:
```js
const { task } = require('gulp');

task('build', function(cb) {
  // body omitted
  cb();
});

const build = task('build');
```

## Signature

```js
task([taskName], taskFunction)
```

### Parameters

If the `taskName` is not provided, the task will be referenced by the `name` property of a named function or a user-defined `displayName` property. The `taskName` parameter must be used for anonymous functions missing a `displayName` property.

Since any registered task can be run from the command line, avoid using spaces in task names.

| parameter | type | note |
|:--------------:|:------:|-------|
| taskName | string | An alias for the task function within the the task system. Not needed when using named functions for `taskFunction`.  |
| taskFunction <br> **required** | function | A function that conforms to the [task definition][task-definition]. Ideally a named function. [Task metadata][task-metadata] can be attached to provide extra information to the command line. |

### Returns

If a task is being registered, nothing is returned. If a task is being retrieved, a wrapped task (not the original function) registered as `taskName` will be returned. The wrapped task has an `unwrap` method that will return the original function.

### Throws

When retrieving a task, will throw if `taskName` is missing or not a string.

When registering a task, will throw if `taskName` is missing and `taskFunction` is anonymous, or if `taskFunction` is missing.

## Task metadata
| property | type | note |
|:--------------:|:------:|-------|
| name | string | A special property on named functions. Used to register the task. <br> **Note:** [`name`][mdn-function-name] is not writable; it cannot be set or edited. |
| displayName | string | A property that can be attached to a `taskFunction` to create an alias for the task. If you need to use characters that aren't allowed in function names, use the `displayName` property. |
| description | string | A property that can be attached to a `taskFunction` to provide a description  to be printed by the command line when listing tasks. |
| flags | object | A property that can be attached to a `taskFunction` to provide flags to be  printed by the command line when listing tasks. The keys of the object represent the flags and the values are descriptions them. |

```js
const { task } = require('gulp');

const clean = function(cb) {
  // body omitted
  cb();
};
clean.displayName = 'clean:all';

task(clean);

function build(cb) {
  // body omitted
  cb();
}
build.description = 'Build the project';
build.flags = { '-e': 'An example flag' };

task(build);
```

[task-definition]: ../getting-started/3-creating-tasks.md
[task-metadata]: #task-metadata
[mdn-function-name]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/name
