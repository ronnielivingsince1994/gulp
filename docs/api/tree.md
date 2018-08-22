<!-- front-matter
id: api-tree
title: tree()
hide_title: true
sidebar_label: tree()
-->

# tree()

Fetches the current task dependency tree - in the rare case that it is needed.

Generally, `tree()` won't be used by gulp consumers, but it is exposed so the CLI can show the dependency graph of the tasks defined in a gulpfile.

## Usage

```js
// Example gulpfile
const { series, parallel } = require('gulp');

function one(cb) {
  // body omitted
  cb();
}

function two(cb) {
  // body omitted
  cb();
}

function three(cb) {
  // body omitted
  cb();
}

const four = series(one, two);

const five = series(four,
  parallel(three, function(cb) {
    // Body omitted
    cb();
  })
);

module.exports = { one, two, three, four, five };
```

```js
// gulp.tree() output:
['one', 'two', 'three', 'four', 'five']

// gulp.tree({ deep: true }) output:
[{
  "label": "one",
  "type": "task",
  "nodes": []
},
{
  "label": "two",
  "type": "task",
  "nodes": []
},
{
  "label": "three",
  "type": "task",
  "nodes": []
},
{
  "label": "four",
  "type": "task",
  "nodes": [
    {
      "label": "<series>",
      "type": "function",
      "nodes": [
        {
          "label": "one",
          "type": "task",
          "nodes": []
        },
        {
          "label": "two",
          "type": "task",
          "nodes": []
        }
      ]
    }
  ]
},
{
  "label": "five",
  "type": "task",
  "nodes": [
    {
      "label": "<series>",
      "type": "function",
      "nodes": [
        {
          "label": "four",
          "type": "task",
          "nodes": [
            {
              "label": "<series>",
              "type": "function",
              "nodes": [
                {
                  "label": "one",
                  "type": "task",
                  "nodes": []
                },
                {
                  "label": "two",
                  "type": "task",
                  "nodes": []
                }
              ]
            }
          ]
        },
        {
          "label": "<parallel>",
          "type": "function",
          "nodes": [
            {
              "label": "three",
              "type": "task",
              "nodes": []
            },
            {
              "label": "<anonymous>",
              "type": "function",
              "nodes": []
            }
          ]
        }
      ]
    }
  ]
}]
```

## Signature

```js
tree([options])
```

### Parameters

| parameter | type | note |
|:--------------:|------:|--------|
| options | object | Detailed in [Options](#options) below. |

### Returns

Returns an object representing the tree of registered tasks. The object returned is a tree of nested objects with `'label'`, `'type'`, and `'nodes'` properties (which is [archy][archy] compatible).

The `type` property that can be used to determine if the node is a `task` or `function`.

## Options

| Name | Type | Default | Note |
|:-------:|:-------:|------------|--------|
| deep | boolean | false | If set to `true`, the whole tree will be returned. |

[archy]: https://www.npmjs.com/package/archy
