# wprelease
WordPress Theme and Plugin Release Script

This bash script helps you to release WordPress plugins and themes:

- removing hidden .sass-cache and .git folders
- cloning the source folder to <source_path>_release 
- yui minifying css and js files 
- renaming <source.css|.js> to <source.min.css|.min.js>
- replacing <source.css|.js> occurence with <source.min.css|.min.js> in php files 

## Dependencies
This scripts needs

- Java callable within your $PATH variable (tested with java version 1.8.0_131)
- sed
- rsync
- yuicompressor (install see below)

## Installation
First copy the files `wprelease`, `yuicss`, `yuijs` to /usr/local/bin and give it executable flag

```
$ sudo cp wprelease /usr/local/bin/
$ sudo chmod +x /usr/local/bin/wprelease
$ sudo cp yui* /usr/local/bin/
$ sudo chmod +x /usr/local/bin/yui*
```

Download the latest release of yui compressor from (https://github.com/yui/yuicompressor/releases/download/v2.4.8/yuicompressor-2.4.8.jar) and save it to /usr/local/share/yuicompressor/latest.jar

```
$ mkdir -p /usr/local/share/yuicompressor
$ cp ~/Download/yuicompressor-2.4.8.jar /usr/local/share/yuicompressor/latest.jar 
```

Now you are ready to release some code.

## Usage
Let's say we have a theme with the name `awesomewp` and we want to release it. We have a terminal window open with the root path of our WordPress installation. If the release folder `awesomewp_release` already exists, it will be removed first.

```
$ wprelease wp-content/themes/awesomewp
Create publish folder...
Cloned and cleaned to ./awesomewp_release
Minifying css files...
./awesomewp_release/assets/css/colors-dark.css minified.
./awesomewp_release/assets/css/editor-style.css minified.
./awesomewp_release/assets/css/ie8.css minified.
./awesomewp_release/assets/css/ie9.css minified.
./awesomewp_release/rtl.css minified.
./awesomewp_release/style.css minified.
done.
Minifying js files...
./awesomewp_release/assets/js/customize-controls.js minified.
./awesomewp_release/assets/js/customize-preview.js minified.
./awesomewp_release/assets/js/global.js minified.
./awesomewp_release/assets/js/html5.js minified.
./awesomewp_release/assets/js/jquery.scrollTo.js minified.
./awesomewp_release/assets/js/navigation.js minified.
./awesomewp_release/assets/js/skip-link-focus-fix.js minified.
done.
```

C-style comments starting with /*! are preserved. So before minifying `style.css` the first line will be replaced with `/*!` to prevent removing the entire theme informationen. After minifying the first line will be reset to `/*`.