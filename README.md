# sioux guidelines

## about
Sioux is a collection of projects, that helps create webapplications for mobile. It tries to follow the way programming for iOS works, but takes advantage of the benefits of javascript and node. These projects are node modules that can run in the browser using [browserify](https://github.com/substack/node-browserify). The modules based on each other using inheritance. A lots of things are not done yet, that have to serve an important ground (eg tables, segues).

## contribute
If you are so awesome, create new modules in similar notion or improve, fix existing ones. Everything is welcomed! Try to inherit as best as possible from other modules to write the least amount code.

## foundation
A lot of core modules still have to be written. As seen in iOS, these serve an important role, so they neeed to be created soon. Here is a couple:
- __[sioux-ui](https://github.com/gerhardberger/sioux-ui)__ is already written, but needs some more work (especially more css)
- __segues:__ probably the next one i will start figuring out how to implement
- __navigation__
- __tables__
- other key elements of a user interface ([switches](https://github.com/gerhardberger/sioux-ui-switch), [buttons](https://github.com/gerhardberger/sioux-ui-button), sliders etc)

## tips, helps
I have not build a lot of modules for sioux myself, so my flow of creating a module may change over time, as is now here are a couple of things i used:

### testing
I think the fastest way to create a module with browserify, if you don't compile it in command line every time, but use a little server that makes a bundle at every request.

server.js
``` js
var http = require('http');
var fs = require('fs');
var browserify = require('browserify');

http.createServer(function (req, res) {
	if (req.url === '/') {
		res.writeHead(200, { 'Content-Type': 'text/html' });
		fs.createReadStream('./index.html').pipe(res);
	}
	if (req.url === '/bundle.js') {
		res.writeHead(200, { 'Content-Type': 'text/javascript' });
		// I create a folder in the modules' directory and the
		// this server file, main.js and index.html are in
		// the folder, and require the module from main.js
		var b = browserify('./main.js');
		
		b.bundle().pipe(res);
	}
}).listen(8080);
```

### add css
I use substack's [insert-css](https://github.com/substack/insert-css) and [brfs](https://github.com/substack/brfs) modules to add css to a module. Sometimes it would be good to add style to the created element only, but i haven't found a good solution for that yet. Maybe shadow dom in the future.

in the modules' file
``` js
var insertCss = require('insert-css');
// ...
var css = fs.readFileSync(__dirname + '/style.css');
insertCss(css);
```
Probably a good thing not to put it in the constructor or other functions that can run more than one time.

in server.js
``` js
var b = browserify('./main.js');
b.transform('brfs');		
b.bundle().pipe(res);
```