# api documentation for  [gulp-todo (v5.3.0)](https://github.com/pgilad/gulp-todo#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-gulp-todo.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-gulp-todo) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gulp-todo.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gulp-todo)
#### Generate a TODO.md file from comments of files in stream

[![NPM](https://nodei.co/npm/gulp-todo.png?downloads=true)](https://www.npmjs.com/package/gulp-todo)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gulp-todo/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-gulp-todo_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gulp-todo/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-gulp-todo/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-gulp-todo/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "author": {
        "name": "Gilad Peleg",
        "email": "giladp007@gmail.com",
        "url": "http://giladpeleg.com"
    },
    "bugs": {
        "url": "https://github.com/pgilad/gulp-todo/issues"
    },
    "dependencies": {
        "gulp-util": "^3.0.7",
        "leasot": "^4.3.0",
        "lodash.defaults": "^4.2.0",
        "lodash.omit": "^4.5.0",
        "through2": "^2.0.3"
    },
    "description": "Generate a TODO.md file from comments of files in stream",
    "devDependencies": {
        "gulp": "^3.9.0",
        "gulp-header": "^1.7.1",
        "gulp-wrap": "^0.13.0",
        "mocha": "^3.2.0",
        "should": "^11.1.2"
    },
    "directories": {},
    "dist": {
        "shasum": "8ca05678078b40efeb739975ae42cb7f7cd315bd",
        "tarball": "https://registry.npmjs.org/gulp-todo/-/gulp-todo-5.3.0.tgz"
    },
    "engines": {
        "node": ">=0.10.0"
    },
    "files": [
        "index.js",
        "lib"
    ],
    "gitHead": "1f0cec1715cdbd0935dc2664a68af1f03b0c08dd",
    "homepage": "https://github.com/pgilad/gulp-todo#readme",
    "keywords": [
        "gulpplugin",
        "gulp",
        "js",
        "fixme",
        "comments",
        "todo",
        "list",
        "parse",
        "generator",
        "ci"
    ],
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "pgilad",
            "email": "giladp007@gmail.com"
        }
    ],
    "name": "gulp-todo",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/pgilad/gulp-todo.git"
    },
    "scripts": {
        "test": "mocha -R spec tests/*.js",
        "watchTest": "mocha --watch -R spec tests/*.js"
    },
    "version": "5.3.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module gulp-todo](#apidoc.module.gulp-todo)
1.  [function <span class="apidocSignatureSpan">gulp-todo.</span>reporter (reporter, options)](#apidoc.element.gulp-todo.reporter)



# <a name="apidoc.module.gulp-todo"></a>[module gulp-todo](#apidoc.module.gulp-todo)

#### <a name="apidoc.element.gulp-todo.reporter"></a>[function <span class="apidocSignatureSpan">gulp-todo.</span>reporter (reporter, options)](#apidoc.element.gulp-todo.reporter)
- description and source-code
```javascript
reporter = function (reporter, options) {
    options = options || {};
    if (!reporter) {
        throw new PluginError('Reporter is required');
    }
    options.reporter = reporter;
    var fileName = options.fileName;
    delete options.fileName;
    return through.obj(function (file, enc, cb) {
        if (file.isNull()) {
            cb(null, file);
            return;
        }

        if (file.isStream()) {
            cb(new PluginError(pluginName, 'Streaming not supported'));
            return;
        }

        // replace contents with requested reporter contents
        if (file.todos && file.todos.length) {
            var newContents;
            try {
                newContents = leasot.reporter(file.todos, options);
            } catch (e) {
                cb(new gutil.PluginError(pluginName, e));
                return;
            }

            if (fileName) {
                file.path = path.join(file.base, fileName);
            }
            file.contents = new Buffer(newContents);
        }

        cb(null, file);
    });
}
```
- example usage
```shell
...
});

// output once in markdown and then output a json file as well
gulp.task('todo-reporters', function() {
    gulp.src('js/**/*.js')
        .pipe(todo())
        .pipe(gulp.dest('./')) //output todo.md as markdown
        .pipe(todo.reporter('json', {fileName: 'todo.json'}))
        .pipe(gulp.dest('./')) //output todo.json as json
});


// Delete the todo.md file if no todos were found
var gulpIf = require('gulp-if');
var del = require('del');
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
