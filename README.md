# command-line-nodejs
Learn how to build a command-line with nodejs.

## Packaging shell commands
The first step is to create a new npm project with npm init:
```
$ npm init                                                             
package name: (command-line-nodejs)
version: (1.0.0) 0.0.0
description: Learn how to build a command-line with nodejs.
entry point: (index.js)
test command:
git repository: (https://github.com/Jonnytoshen/command-line-nodejs.git)
keywords:
author:
license: (ISC) MIT
```

Then we'll need to create a JS file that will contain our script. Let's follow Node.js conventions and call it index.js:
```
#!/usr/bin/env node
console.log('Hello, world!');
```
Note that we have to add a [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix))(about this: #!/usr/bin/env node) to tell our shell how to invoke this script.

Next we need to add a bin section to the top level of our package.json. The property key (ns, in our case) will become the command that the user invokes in their shell. The property value is the path to the script relative to the package.json.
```
{
  "name": "command-line-nodejs",
  "version": "0.0.0",
  "description": "Learn how to build a command-line with nodejs.",
  "main": "index.js",
  "bin": {
    "ns": "./index.js"
  }
}
```

Now we have a working shell command! Let's install it and test it out.
```
$ npm install -g
$ ns
Hello, world!
```

Neat! npm install -g actually links the script to a location on our path, so we can use it like any other shell command.
```
$ which ns
/home/jonny/.nvm/versions/node/v8.11.3/bin/ns

$ readlink /home/jonny/.nvm/versions/node/v8.11.3/bin/ns
../lib/node_modules/command-line-nodejs/index.js
```

During development it's convenient to make the symlink on our path point to the index.js we're actually working on, using npm link.
```
$ npm link
up to date in 0.059s
/home/jonny/.nvm/versions/node/v8.11.3/bin/ns -> /home/jonny/.nvm/versions/node/v8.11.3/lib/node_modules/command-line-nodejs/index.js
/home/jonny/.nvm/versions/node/v8.11.3/lib/node_modules/command-line-nodejs -> /home/jonny/Documents/git/command-line-nodejs
```

When we're ready, we can publish our script to the public npm registry with npm publish. Then anyone in the world will be able to install it on their own machine by running:
```
$ npm install -g command-line-nodejs
```


## Parsing command line options
Our script is going to need a few pieces of input from the user, like --version, -p, etc.
You can get the raw arguments passed to your node script with process.argv, but I will use [Commander.js](https://github.com/tj/commander.js) for parsing arguments and options.
```
$ npm install commander --save
```

Will add the version to our package. When use -V or --version options, the command prints the version number and exits.
```
#!/usr/bin/env node

const program = require('commander');

program
    .version('0.0.1')
    .parse(process.argv);
```

That's the basic usage of commander.js. If you want to use more, please visit the [Commander.js](https://github.com/tj/commander.js)