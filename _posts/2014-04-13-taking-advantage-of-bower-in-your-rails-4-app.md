---
layout: post
title:  "Taking advantage of Bower in your Rails 4 App"
date:   2014-04-13 20:03:50
categories: code, rails
quote: "Easily manage your javascript and css libraries with Bower."
image: /media/2014-04-13-taking-advantage-of-bower-in-your-rails-4-app/bower_and_rails.png
dark: true
video: false
comments: true
---

If you haven't heard about it yet, [Bower](http://bower.io/) is a powerful *"package manager for the web"* created by non other than the great peeps over at Twitter. But what can Bower actually do for you and why would you want it inside your Rails app?

Since you are already using Rails, you are familiar with the whole adding gem to your Gemfile and firing off `bundle install`. If you want to update a gem or all of them you simply run `bundle update` or `bundle update <gem name>` and you are good to go. But what if you want to upgrade Bootstrap, Angular, Jquery or any other client side libraries or frameworks? Well shit.

Bower, through its command line interface enables you to search, install, update and uninstall web assets like JavaScript, HTML and CSS. It also has it's own `Gemfile` & `Gemfile.lock` called `bower.json`.



## Why you should use Bower

If you are not yet convince by now, here's my 8 reasons why I love using Bower.

* It saves you time. No more going back to the JQuery, Angular, "that-special-library.js" website / repo to download the files.
* Never have to wait for the gemified version of the library to be updated.
* Makes update super easy. If a new version of a library is released with security/bug fix or new features you just have to run `bower update` in order to install the new version and all of its (new) dependencies.
* Semantic versioning is used to help define a range of acceptable versions for a dependency, which makes it easy to update to newer versions within the defined range.
* No need to commit dependencies to version control since all the info is stored in bower.json.
* Help you work offline. Each time you download a package it will install it in your app folder and in the .bower directory in the user's home directory. The next time you need this package it will pick up that version from the home .bower directory. 
* No need to locate various builds (debug, minified, etc)
* Easy access to more than **[10,000 packages](http://bower.io/search/)**.
* And probably many other reasons you will discover along the way...

If all of the above sounds great to you, lets see how does Bower fit into Rails.tall Bower and Rare BFFails 

<h2 id='installation'>Bower and Rails are BFF</h2>

After reading terms like *CSS*, *Javascript* and *HTML*, you are already having nightmares thinking at the idea of bringing a 3rd party tool into the dreaded asset pipeline but fear not as Bower and the asset pipeline actually play very well together. Without further ado, here's how to set the whole thing up.


### 1. Install Bower

Since you are already running Rails 4, you've already got Node and NPM up and running on your machine. If not, [Google is your friend](http://lmgtfy.com/?q=install+node+and+npm). 

To install Bower simply run:

```
npm install -g bower
```

We are using -g to install it globally so you can continue using it in all of your future projects.

### 2. Configuring Bower

By default Bower install the components (what it calls JS/CSS libraries) in your current directory which doesn't really play with the standard Rails folder hierarchy. To change that, simply create a `.bowerrc` (bower's config file. Read more about it [here](https://docs.google.com/document/d/1APq7oA9tNao1UYWyOm8dKqlRP2blVkROYLZ2fLIjtWc/edit#heading=h.4pzytc1f9j8k)) file at the root of your Rails app and add this:


```json
{
  "directory": "vendor/assets/components"
}
```

This will tell Bower to save all of the component files in that directory which follow Rails' convention on [storing assets](http://guides.rubyonrails.org/asset_pipeline.html#asset-organization).


### 3. Your first package.json

From the root of your Rails app, run `bower init` to create your `bower.json` file. This will ask you a couple of questions about your apps so go ahead and enter it. 

If you want to learn more about the *what types of modules does this package expose?* question, you can read this [helpful StackOverflow answer](https://stackoverflow.com/questions/22674018/bower-init-difference-between-amd-es6-globals-and-node) but you should select `globals` in most cases.

Depending on your answers, your `bower.json` should looks something like this:


```json
{
  "name": "BowerAndRails",
  "version": "0.0.1",
  "authors": [
    "Syl <nop@nope.com>"
  ],
  "description": "Tutorial about Bower and Rails",
  "license": "MIT",
  "homepage": "http://mycoolhomepage.com",
  "ignore": [
    "**/.*",
    "node_modules",
    "bower_components",
    "test",
    "tests"
  ],
  "dependencies": {
    "angular": "~1.2.16"
  }
}
```
Now if I run `bower install`, it will pull Angular 1.2.16 from the Github repo into `vendor/assets/components`, nifty!

### 4. Configuring Rails

Now that Bower is installed and working, we just need to make sure that Rails play nice with it. To do so, head to your `/config/application.rb` file so we can let the asset pipeline about it. Simple add the following line to your file and save it.

```ruby
config.assets.paths << Rails.root.join('vendor', 'assets', 'components')
```

Finally, to require the freshly downloaded components, open up `application.js` in `/app/assets/javascripts` and require the necessary JS file like you would usually do. In my example with Angular, I do so by adding this one line to it.

```javascript
//= require angular/angular
```

Similar, for CSS frameworks and libraries like Bootstrap you can pop in the following line to application.css in `/app/assets/css`.

```css
*= require bootstrap/dist/css/bootstrap
```

That's it! You can now use Bower to manage all of the 3rd party CSS & Javascript libraries you wan to use in your project and Rails' asset pipeline will take care of the rest for you.


## Final Thoughts

Thanks Bower component management, you can now list, search, install and update your front end dependencies like you would do for your ruby gems. You will now:

* Stop using gemified javascript & CSS libraries
* Stop the asset pipeline minimizing the gemified libraries
* Stop missing out of the release pace of your components

If you have any questions, you can contact me on Twitter [@Copypastaa](http://twitter.com/copypastaa).


**TL;DR:**

* Use Bower to save a good amount time
* Stop uploading 3rd client libraries to your repos
* Stop using gemified CSS & Javascript libraries
* Always use the latest and greatest version of the libraries
* 10,000+ packages at your fingertips
* Easily manage & update your libraries and their dependencies from the command line
* Bower and Rails are BFF so it is super easy to set up, 5 minutes top.
* [I can haz install?](#installation)
