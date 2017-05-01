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

## Refactoring with Sass
If we make a change in our `main.scss` file, then save it, we should see a new file pop into our CSS folder - `main.css`. Let's make that our new stylesheet, by changing the link in `index.html`:

```html
<link rel="stylesheet" href="css/main.css">
```

Now if we run our Simple Server (`python -m SimpleHTTPServer`), we should see that our styles have all but dissapeared! That's because our pre-existing styles are still in `style.css`. Let's move them over to our SCSS file. Remember, all CSS is valid SCSS. But we can do better! Let's take this code, and see how DRY we can make it with SASS.

### Nesting
Let's start by nesting whatever style calls we can. Take a few minutes to next the styles as deeply as you can (but, it's best practice to not go more than 4 levels deep).


<details><summary>Here's how I did it:</summary>
```css
body {
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
    .container {
      margin: 4rem;
        .row {
            margin: 0;
        }
    } 
}

header {
  border-bottom: 1px solid black;
  padding-bottom: 1rem;
  margin-bottom: 1.618rem;
    .navbar {
      border-radius: 0;
    }
    h1 {
      font-weight: bold;
      letter-spacing: -2px;
      max-width: 100px;
        &:after {
          content: "™";
          font-size: 1rem;
          vertical-align: super;
          margin-left: 0.192rem;
        }
    }
}

.card {
  background: #231e1e;
  border-color: black;
  color: white;
  padding: 1rem;
  min-height: 16rem;
  position: relative;
    h6 {
      position: absolute;
      font-size: 0.6rem;
      margin-top: 1.618rem;
      opacity: 0.2;
      bottom: 0.618rem;
      left: 1rem;
        &:after {
          content: "™";
          font-size: 1rem;
          vertical-align: super;
          margin-left: 0.192rem;
          font-size: 60%;
          margin-left: 1px;
        }
    }
}

footer {
  text-align: center;
  margin-top: 2rem;
  color: silver;
  font-size: 0.6rem;
    .heart {
      color:#cf2e31;
    }
}
```
</details>

Notice in the above example that I broke the page into components - body(or structure), header, cards, and footer. This will help us scale our project if and when we add new functionality.

### Variables

Variables are a great way of reducing repeition in our code. Let's look for any vaules in our SCSS that are used more than once, and turn them into variables. Remember, the variable syntax for SASS is as follows:

`$my-variable: value;`

For starters, I see more than one place where padding has been set to 1rem. This is the perfect value to turn into a variable, because not only will it make it easier to change the value in the future, but it can serve to set up a design pattern that is easy to follow. is we set up a variable that establishes that:

`$padding: 1rem;`

We've now set a system-wide standard for how to space any future objects on our page. No more guessing or eyeballing!
Let's take a moment to set up that variable, and set all relevant calls to use it:

`padding: $padding;`

> DO NOT replace values of `left` and `font-size` with `$padding`. While it will totally work, it confuses our semantics - font size isn't a form of padding. If you want to set up variables for these values, do so seperately, with uniquely-identifiable names.

<details><summary>My SCSS looks like this now:</summary>
```css
$padding: 1rem;

body {
  text-rendering: optimizeLegibility;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
    .container {
      margin: 4rem;
        .row {
            margin: 0;
        }
    } 
}

header {
  border-bottom: 1px solid black;
  padding-bottom: $padding;
  margin-bottom: 1.618rem;
    .navbar {
      border-radius: 0;
    }
    h1 {
      font-weight: bold;
      letter-spacing: -2px;
      max-width: 100px;
        &:after {
          content: "™";
          font-size: 1rem;
          vertical-align: super;
          margin-left: 0.192rem;
        }
    }
}

.card {
  background: #231e1e;
  border-color: black;
  color: white;
  padding: $padding;
  min-height: 16rem;
  position: relative;
    h6 {
      position: absolute;
      font-size: 0.6rem;
      margin-top: 1.618rem;
      opacity: 0.2;
      bottom: 0.618rem;
      left: 1rem;
        &:after {
          content: "™";
          font-size: 1rem;
          vertical-align: super;
          margin-left: 0.192rem;
          font-size: 60%;
          margin-left: 1px;
        }
    }
}

footer {
  text-align: center;
  margin-top: 2rem;
  color: silver;
  font-size: 0.6rem;
    .heart {
      color:#cf2e31;
    }
}
```
</details>

