# babel-plugin-module-rewrite

A [babel](http://babeljs.io) plugin to rewrite module imports (and `require`) using a custom function, with ability to setup root, to work nicely with mono repos

## Description

You can supply a replace function to dynamically replace module paths when Babel traverses them.

## Usage

Install the plugin

```
$ npm install --save-dev babel babel-plugin-module-rewrite-with-root
```

Specify the plugin in your `.babelrc` with the file that exports the replace function.
```json
{
  "plugins": [
    ["module-rewrite-with-root", 
        { 
            "replaceFunc": "./utils/replace-module-paths.js",
            "optionalRoot": "server/"
        }
    ]
  ]
}
```

Let's say you want `~/moduleFile` to be replaced to `utils/moduleFile` if the calling file is in `utils`, and `common/moduleFile` otherwise.
So in your `replace-module-paths.js`, just export:
```js
export default function replaceImport(originalPath, callingFileName, options) {
    if(callingFileName.indexOf('/utils/') !== -1) {
        return originalPath.replace('~', 'utils');
    } else {
        return originalPath.replace('~', 'common');
    }
}
```

## License

MIT
