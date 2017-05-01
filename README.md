# sassy-node
Using SASS in a node project

## Why is this Important?
SASS makes CSS easier, and modularized - used correctly, you can start to mimic the MVC style of file organization you’re using on your backend. Also, it’s a huge draw for employers. Everyone knows SASS is better than straight CSS, but very few devs have taken the time to learn and implement it in their projects. Knowing it will give you a head start in the job search!

## Objectives
1. Add Node/Gulp/Sass to an existing project
1. Write modularized SASS for DRY styles
1. Compile SASS using Node

## Set Up
The starter code in this folder (`app`) is the solution code from our [Angular Directives Lab](https://github.com/den-materials/angular-directives-lab). It’s just a frontend, but that’s all we need to get started!
Let’s start by initializing our repo with NPM and installing Gulp.

### Initializing Node

Hopefully this step isn’t too distant a memory - but in case you’ve forgotten, `cd` into the `app` folder and run `npm init -y`.

### Installing Gulp

1. First, cd into the app folder, `cd` into it, and `npm init -y` your project
You’ll want to install gulp globally. You can do this by running the `npm install gulp -g` command.
1. We also need to include gulp in our project. To do that, we will run `npm install gulp --save-dev`. We use `--save-dev` because a production environment should not be concerned with build tools.
1. Define our default task:
```
var gulp = require('gulp');

//define a task with the name of 'default'
// and a callback to perform when the task is ran
gulp.task('default', function() {
  console.log('I am the default task. Hear me roar');
});
```
1. In your terminal, run gulp!