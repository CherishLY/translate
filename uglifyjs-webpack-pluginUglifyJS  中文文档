uglifyjs-webpack-pluginUglifyJS Webpack Plugin

[原文档地址](https://www.npmjs.com/package/uglifyjs-webpack-plugin)


此插件使用uglify-js进行js文件的压缩。

## requirements

该模块需要的环境： node 6.9.0  webpack 4.0.0 版本以上。

## getting started

开始之前，需要安装 uglifyjs-webpack-plugin:

```
$ npm install uglifyjs-webpack-plugin --save-dev
```

然后将该插件添加到你工程webpcak的config。例如：

webpack.config.js

```
const UglifyJsPlugin = require('uglifyjs-webpack-plugin');
 
module.exports = {
  //...
  optimization: {
    minimizer: [new UglifyJsPlugin()]
  }
};
```
运行webpack.

## Options

#### test

Type: String|RegExp|Array<String|RegExp> Default: /\.js(\?.*)?$/i

测试匹配的文件。

```
// in your webpack.config.js
new UglifyJsPlugin({
  test: /\.js(\?.*)?$/i
})
```

#### include

Type: String|RegExp|Array<String|RegExp> Default: undefined

包括的文件。

```
// in your webpack.config.js
new UglifyJsPlugin({
  include: /\/includes/
})
```

#### exclude

Type: String|RegExp|Array<String|RegExp> Default: undefined

不包含的文件。

```
// in your webpack.config.js
new UglifyJsPlugin({
  exclude: /\/excludes/
})
```

#### cache

Type: Boolean|String Default: false

启用文件缓存。默认的缓存路径为： node_modules/.cache/uglifyjs-webpack-plugin.

    如果您使用自己的minify函数，请正确阅读minify部分以了解缓存失效。

 ##### Boolean
 
启用缓存/关闭缓存

```
// in your webpack.config.js
new UglifyJsPlugin({
  cache: true
})
```

#### String

启用缓存，并设置缓存的路径。

webpack.config.js

```
// in your webpack.config.js
new UglifyJsPlugin({
  cache: 'path/to/cache'
})
```

### cacheKeys

Type: Function<(defaultCacheKeys, file) -> Object> Default: defaultCacheKeys => defaultCacheKeys

允许重写默认的cache keys.

默认的cache keys:

```
({
  'uglify-js': require('uglify-js/package.json').version, // uglify version
  'uglifyjs-webpack-plugin': require('../package.json').version, // plugin version
  'uglifyjs-webpack-plugin-options': this.options, // plugin options
  path: compiler.outputPath ? `${compiler.outputPath}/${file}` : file, // asset path
  hash: crypto.createHash('md4').update(input).digest('hex'), // source file hash
});
```


```
// in your webpack.config.js
new UglifyJsPlugin({
  cache: true,
  cacheKeys: (defaultCacheKeys, file) => {
    defaultCacheKeys.myCacheKey = 'myCacheKeyValue';
 
    return defaultCacheKeys;
  },
})
```

### parallel

Type: Boolean|Number Default: false

使用多进程并行运行来提高构建速度。默认并发运行次数:os.cpus().length- 1。

    并行化可以显著地加速构建，因此强烈推荐使用并行化。

#### Boolean

启用/禁用多进程并行运行。

```
// in your webpack.config.js
new UglifyJsPlugin({
  parallel: true
})
```

#### Number

启用多进程并行运行和设置并发运行次数。

```
// in your webpack.config.js
new UglifyJsPlugin({
  parallel: 4
})
```

### sourceMap

Type: Boolean Default: false

使用sourceMap将错误消息位置映射到模块(这会减慢编译速度)。如果您使用自己的minify函数，请阅读minify部分以正确处理sourceMap。

    cheap-source-map 属性不适用于此插件。

```
// in your webpack.config.js
new UglifyJsPlugin({
  sourceMap: true
})
```

### minify

Type: Function Default: undefined

允许覆盖默认的minify函数。默认插件使用uglify-js包。用于使用和测试未发布的版本或分支。

    ⚠️当启用并行选项时，在minify函数中使用require。

```
// in your webpack.config.js
new UglifyJsPlugin({
  minify(file, sourceMap) {
    const extractedComments = [];
 
    // Custom logic for extract comments
 
    const { error, map, code, warnings } = require('uglify-module') // Or require('./path/to/uglify-module')
      .minify(file, {
        /* Your options for minification */
      });
 
    return { error, map, code, warnings, extractedComments };
  }
})
```

### uglifyOptions

Type: Object Default: default

UglifyJS 压缩选项。

```
// in your webpack.config.js
new UglifyJsPlugin({
  uglifyOptions: {
    warnings: false,
    parse: {},
    compress: {},
    mangle: true, // Note `mangle.properties` is `false` by default.
    output: null,
    toplevel: false,
    nameCache: null,
    ie8: false,
    keep_fnames: false,
  }
})
```

### extractComments

Type: Boolean|String|RegExp|Function<(node, comment) -> Boolean|Object> Default: false

是否将注释提取到单独的文件中(请参阅详细信息)。默认情况下只提取注释中使用```/^\**!|@preserve|@license|@cc_on/i``` 正则条件并删除剩余注释。若原始文件命名为foo.js。然后注释将被存储到foo.js.LICENSE.```uglifyOptions.output.comments```选项指定是否保留注释，也就是说，在提取其他注释时可以保留一些注释，甚至保留已经提取的注释。


#### Boolean

启用/禁用提取注释。

```
// in your webpack.config.js
new UglifyJsPlugin({
  extractComments: true
})
```

#### String

提取所有或一些（使用```/^\**!|@preserve|@license|@cc_on/i```正则匹配）注释。

```
// in your webpack.config.js
new UglifyJsPlugin({
  extractComments: 'all'
})
```

#### RegExp

所有匹配正则表达式的注释将被提取到单独的文件中。

```
// in your webpack.config.js
new UglifyJsPlugin({
  extractComments: /@extract/i
})
```

#### Function<(node, comment) -> Boolean>

所有匹配正则表达式的注释将被提取到单独的文件中。

```
// in your webpack.config.js
new UglifyJsPlugin({
  extractComments: function (astNode, comment) {
    if (/@extract/i.test(comment.value)) {
      return true;
    }
    
    return false;
  }
})
```

#### Object

允许自定义条件提取注释，指定提取文件名和banner。

```
// in your webpack.config.js
new UglifyJsPlugin({
  extractComments: {
    condition: /^\**!|@preserve|@license|@cc_on/i,
    filename(file) {
      return `${file}.LICENSE`;
    },
    banner(licenseFile) {
      return `License information can be found in ${licenseFile}`;
    }
  }
})
```

### condition

Type: Boolean|String|RegExp|Function<(node, comment) -> Boolean|Object>

设置需要提取的注释。

```
// in your webpack.config.js
new UglifyJsPlugin({
  extractComments: {
    condition: 'some',
    filename(file) {
     return `${file}.LICENSE`;
    },
    banner(licenseFile) {
     return `License information can be found in ${licenseFile}`;
    }
  }
})
```

### filename

Type: Regex|Function<(string) -> String> Default: ${file}.LICENSE

提取注释的文件将会被存储。默认是将后缀. license附加到原始文件名。

```
// in your webpack.config.js
new UglifyJsPlugin({
  extractComments: {
    condition: /^\**!|@preserve|@license|@cc_on/i,
    filename: 'extracted-comments.js',
    banner(licenseFile) {
     return `License information can be found in ${licenseFile}`;
    }
  }
})
```

### banner

Type: Boolean|String|Function<(string) -> String> Default: /*! For license information please see ${commentsFile} */

banner文本指向提取的文件并将被添加到原始文件的顶部。可能是 false(没有banner)，a string, 或者一个```Function<(string) -> String>```,将使用存储了提取注释的文件名调用。将被纳入注释。

```
// in your webpack.config.js
new UglifyJsPlugin({
  extractComments: {
    condition: true,
    filename(file) {
     return `${file}.LICENSE`;
    },
    banner(commentsFile) {
     return `My custom banner about license information ${commentsFile}`;
    }
  }
})
```

### warningsFilter

Type: Function<(warning, source) -> Boolean> Default: () => true

允许过滤uglify-js警告。返回true以保持警告，否则为false。

```
// in your webpack.config.js
new UglifyJsPlugin({
  warningsFilter: (warning, source) => {
    if (/Dropping unreachable code/i.test(warning)) {
      return true;
    }
 
    if (/filename\.js/i.test(source)) {
      return true;
    }
 
    return false;
  },
})

```

## Examples

### Cache And Parallel

启用缓存并且启用多进程并行运行。

```
// in your webpack.config.js
const UglifyJsPlugin = require('uglifyjs-webpack-plugin');
 
module.exports = {
  //...
  optimization: {
    minimizer: [
      new UglifyJsPlugin({
        cache: true,
        parallel: true
      })
    ]
  }
};

```

### Preserve Comments

提取所有法律条文(例如 ```/^\**!|@preserve|@license|@cc_on/i```)和保存```/@license/i```的注释。

```
// in your webpack.config.js
const UglifyJsPlugin = require('uglifyjs-webpack-plugin');
 
module.exports = {
  //...
  optimization: {
    minimizer: [
      new UglifyJsPlugin({
        uglifyOptions: {
          output: {
            comments: /@license/i
          }
        },
        extractComments: true
      })
    ]
  }
};
````

### Custom Minify Function

覆盖默认的压缩方法-使用 terser 进行压缩。（压缩方法见原文档）

```
// in your webpack.config.js
const UglifyJsPlugin = require('uglifyjs-webpack-plugin');
 
module.exports = {
  //...
  optimization: {
    minimizer: [
      new UglifyJsPlugin({
        // Uncomment lines below for cache invalidation correctly
        // cache: true,
        // cacheKeys(defaultCacheKeys) {
        //   delete defaultCacheKeys['uglify-js'];
        //
        //   return Object.assign(
        //     {},
        //     defaultCacheKeys,
        //     { 'uglify-js': require('uglify-js/package.json').version },
        //   );
        // },
        minify(file, sourceMap) {
          // https://github.com/mishoo/UglifyJS2#minify-options
          const uglifyJsOptions = {
            /* your `uglify-js` package options */
          };
      
          if (sourceMap) {
            uglifyJsOptions.sourceMap = {
              content: sourceMap,
            };
          }
      
          return require('terser').minify(file, uglifyJsOptions);
        },
      })
    ]
  }
};
```

首次翻译，翻译不足的地方，请多多指教。


