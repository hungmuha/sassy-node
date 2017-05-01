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
1. Touch a `gulpfile.js`
1. Define our default task:
```
var gulp = require('gulp');

//define a task with the name of 'default'
// and a callback to perform when the task is ran
gulp.task('default', function() {
  console.log('I am the default task. Hear me roar');
});
```
1. In your terminal, run `gulp`! You should see our default task run.

### Compiling Sass

Now let's compile some Sass to CSS.  We need to start by requiring the `gulp-sass` plugin for Gulp.

```bash
npm install --save-dev gulp-sass
```

Then we can require it in our `gulpfile.js`.

```js
//gulpfile.js
var gulp = require('gulp');
var sass = require('gulp-sass');
```

Define a `styles` task that looks in a `/sass` directory for all `.scss` files, logs any errors that occur, and then compiles the result into a `/css` directory.

```js
gulp.task('styles', function() {
    gulp.src('sass/**/*.scss')
        .pipe(sass().on('error', sass.logError))
        .pipe(gulp.dest('./css/'));
});
```
Create a `/sass` directory and `touch` a `main.scss` file inside.

Run `gulp styles` and it will find your `.scss` file and output a corresponding `.css` file for you.

Lastly let's add the task we just created to gulp's `default` task and have it run the task upon file changes by watching the file. Now every time you save, your `.scss` will trigger the `styles` gulp task and compile the changes for you!

<!--11:32 when turning over to devs -->

To do this, at the bottom of your file add:

```js
gulp.task('default',function() {
    gulp.watch('sass/**/*.scss',['styles']);
});
```

Now we can run our Sass compilation with just `gulp`.
