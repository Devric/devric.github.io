---
title: Grunt multiple project with one process
author: Devric
date: 2014-04-17 14:23
tags: grunt, node
template: article.jade
---

Unlike traditional web development, for ease of upgrade and scalibility, today's medium to large web application are broken down to smaller projects.

So how do we use just one grunt command to automate server, watch, and debugging.

<span class="more"></span>

Lets make a simple structure as example


```
backend service <----> front end 1

                <----> front end x

standalone admin app // the app for your client to control, or reports


 multiple Front-end
  apps / components
|-------------------|                |--------------|
|            |---|  |                |              |
|  |---|     |   |  |                |              |
|  |   |     |- -|  | -- request --> |              |
|  |- -|            |                |              |
|          |---|    |                |   services   |
|          |   |    |                |      api     |
|          |- -|    |                | file serving |
|   |---|           | <- JSON ---    |              |
|   |   |           |                |              |
|   |- -|           |                |              | <-- |-----------|
|-------------------|                |--------------| --> |           |
                                                          | DB stacks |
                                     |--------------| <-- |           |
                                     |              | --> |-----------|
                                     |  companies   |
                                     | backend app  |
                                     |              |
                                     |--------------|
```


Project folder structure

```
Project
 | Gruntfile
 | run.sh
 | service
 |   - Gruntfile
 | frontapp1
 |   - Gruntfile
 | frontapp2
 |   - Gruntfile
 | adminapp
 |   - Gruntfile
 | sharedAssets
```

In your project folder we need 
  1. install
        - grunt-hub
        - node-inspector
        - preprocessors, if your front apps shares files

  2. leave it for now

In each of your sub projects
  1. install
        - grunt-concurent
        - grunt-nodemon
        - everything else for your project

Now the fun part

In each of your sub project minimal gruntfile to kick start

```
module.exports = function(grunt) {

  // Project configuration.
  grunt.initConfig({
    concurrent : {
        run : ['watch', 'nodemon']
      , options : {
          logConcurrentOutput: true
      }
    }
  , nodemon : {
      dev : {
        script : 'app.js'
      , options : {
          nodeArgs : ['--debug'],   // if you require this app to be inspected via debugger
          callback : function(nodemon){
              nodemon.on('restart',function(){
                setTimeout(function(){
                    require('fs').writeFileSync('.nodemon','reboot');
                }, 500)
              })
          }
        }
      }
    }
  , watch: {
      options : {
        livereload : 1337
      }
    , server : {
        files : ['.nodemon']
      }
    , css : {
        files : 'public/stylesheets/**/*.css'
      }
    }
  });

  grunt.loadNpmTasks('grunt-contrib-watch');
  grunt.loadNpmTasks('grunt-concurrent');
  grunt.loadNpmTasks('grunt-nodemon');

  // Default task.
  grunt.registerTask('default', ['concurrent:run']);

};
```

The most important thing is the concurent part, it allows you to watch and run the server at the same time, without having you to start each listener one at a time.

The nodemon callback on restart, make sure that when you edit your files, nodemon will restart the server, and have grunt-watch to listen on the file update and make a refresh after the server is rebooted

Your livereload ports need to be different, and you'll have to add  script(src="//localhost:1337/livereload.js") with the correct port. (Jade syntax), for some can have it all listen to the master, and have watch to run hub command, But i won't bother, its needs more setup in my master Gruntfile, unless all applications is small and  you are fine with restarting everything when one of your application need to change.

Now back to the master Gruntfile

```
module.exports = function(grunt) {
  'use strict';

  grunt.initConfig({
    hub: {
      all: {
        options : {
          allowSelf : true
        },
        src: ['./Gruntfile.js', 'app1/Gruntfile.js','appx/Gruntfile.js', 'admin/Gruntfile.js' ,'service/Gruntfile.js'],
        tasks: ['default'],
      }
    },
    "node-inspector" : {
        dev : {}
    }
  });

  grunt.loadNpmTasks('grunt-hub');
  grunt.loadNpmTasks('grunt-node-inspector');

  grunt.registerTask('default', ['node-inspector]);
  grunt.registerTask('run', ['hub']);
};
```

Important note for grunt-hub, if you need your master Grunt to do some magic, make sure you don't run your hub task run in your default task, if you do, you your grunt will loop itself indefinetly.

What I did is registered the hub task into run command, when you type grunt run, it will look for hub tasks.

For the hub to run the master Gruntfile, you will need to add allowSelf:true to the hub's options. 

The Src:[] array is what your hub will look for and run its default task.

In the master I have added node-inspector, this creates as a default debuger at port localhost:8000/debug?port=5858, and have one of your child grunt to run the nodemon nodeArgs:['--debug'] to make your debuger inspect your app.

Last but not least, to make your life easier when others pulls your project and need to install all the node modules in each of your folders. Write a simple bash script

```
#!/bin/bash
DIR = $("pwd")
npm install
cd $DIR/project-a && npm install
cd $DIR/project-b && npm install
cd $DIR && grunt run
```

And if you have too many folders... write yourself a loop...

