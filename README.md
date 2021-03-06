# gulp-manifest 
> Generate HTML5 Cache Manifest files. Submitted by [Scott Hillman](https://github.com/hillmanov/).

Big thanks to [Gunther Brunner](https://github.com/gunta/) for writing the [grunt-manifest](https://github.com/gunta/grunt-manifest) plugin. This plugin was heavily influenced by his great work. 

Visit the [HTML 5 Guide to AppCache](http://www.html5rocks.com/en/tutorials/appcache/beginner/) for more information on Cache Manifest files.

Minor change by [M1ke](http://twitter.com/m1ke) to add absolute path options.

## Usage

First, install `gulp-manifest` as a dev dependency

```shell
npm install gulp-manifest --save-dev
```

## API

### Parameters

### manifest(options)

This controls how this task (and its helpers) operate and should contain key:value pairs, see options below.

#### options.filename
Type: `String`  
Default: `"app.manifest"`

Set name of the Cache Manifest file.

#### options.cache
Type: `Array`  
Default: `undefined`  

Adds manually a string to the **CACHE** section. Specify an array of strings.

#### options.exclude
Type: `String` `Array`  
Default: `undefined`  

Exclude specific files from the Cache Manifest file.

#### options.network
Type: `String` `Array`  
Default: `"*"` (By default, an online whitelist wildcard flag is added)   

Adds a string to the **NETWORK** section.

See [here](http://diveintohtml5.info/offline.html#network) for more information.

#### options.fallback
Type: `String` `Array`  
Default: `undefined`  

Adds a string to the **FALLBACK** section.

See [here](http://diveintohtml5.info/offline.html#fallback) for more information.

#### options.preferOnline
Type: `Boolean`   
Default: `undefined`

Adds a string to the **SETTINGS** section, specifically the cache mode flag of the ```prefer-online``` state.

See [here](http://www.whatwg.org/specs/web-apps/current-work/multipage/offline.html#concept-appcache-mode-prefer-online) for more information.

#### options.timestamp
Type: `Boolean`   
Default: `true` 

Adds a timestamp as a comment for easy versioning.

Note: timestamp will invalidate application cache whenever cache manifest is rebuilt, even if contents of files in `src` have not changed.

#### options.hash
Type: `Boolean`
Default: `false`

Adds a sha256 hash of all `src` files (actual contents) as a comment.

This will ensure that application cache invalidates whenever actual file contents change (it's recommented to set `timestamp` to `false` when `hash` is used).

### Usage Example


    gulp.task('manifest', function(){
      gulp.src(['css/*' , 'js/*', 'some_files/*'])
        .pipe(manifest({
          hash: true,
          preferOnline: true,
          network: ['http://*', 'https://*', '*'],
          filename: 'app.manifest',
          exclude: 'app.manifest',
          path: true,
          replace: __dirname+'/'
         }))
        .pipe(gulp.dest('build'));
    });


### Output example

    CACHE MANIFEST

    CACHE:
    js/app.js
    css/style
    css/style.css
    js/zepto.min.js
    js/script.js
    some_files/index.html
    some_files/about.html

    NETWORK:
    http://*
    https://*
    *

    # hash: 76f0ef591f999871e1dbdf6d5064d1276d80846feeef6b556f74ad87b44ca16a


You do need to be fully aware of standard browser caching.
If the files in **CACHE** are in the network cache, they won't actually update,
since the network cache will spit back the same file to the application cache.
Therefore, it's recommended to add a hash to the filenames's, akin to rails or yeoman. See [here](http://www.stevesouders.com/blog/2008/08/23/revving-filenames-dont-use-querystring/) why query strings are not recommended.
