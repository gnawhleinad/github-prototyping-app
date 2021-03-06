# GitHub prototyping app

A prototyping app for designers to make quick html prototypes, it's main aim is to:
- Make varations of designs quickly
- To avoid the time and code overhead of making production prototypes
- To allow us to easily publish Heroku prototypes to get our designs in front of users quicker
- For research and usability testing


**Some caveats:**
- You need Rails 4 installed
- The emphasis is on speed of prototype creation for designers rather than perfect code for developers
- It's about making quick, throw away layouts and interactions that would need to be rebuilt for production
- It uses Rails so requires that locally to be used
- If the header and footer change we'll have to copy it over


### Installing

This prototyping tool is essentially not much more than a Rails app with some additional SCSS [(primer)](http://primercss.io/) and markup made available to make making pages easier.

### Install Rails

The first prerequisite is to have Rails 4 installed.

Rails can be installed using RubyGems, which requires ruby to be installed on your machine.

With ruby installed (and ideally managed using [rbenv](https://github.com/rbenv/rbenv) you can then install Rails with RubyGems with the command:

`gem install rails`

### Clone this repo

With Rails successfully installed, you should then clone this repository to a location of your choice on your machine:

`git clone https://github.com/amosie/github-prototyping-app.git`

### Run the server

In the command line, cd in to the repo you just cloned. To start the rails server, run:

`rails server`

You should now be able to view the default example view of this prototyping app by going to `http://localhost:3000` in your browser. It should look something like this:

![alt tag](https://raw.githubusercontent.com/amosie/github-prototyping-app/master/public/example.png)

### How to use

The idea is to have separate branches for separate prototypes, which can then optionally be deployed to their own respective heroku apps.

Set up a local branch for your prototype

This should be the first thing you do. Do not make commits/push to master when making a prototype. To set up your local branch, first create it and then check it out:

`git checkout -b your_prototype_name`

### Set up your default index page

By default, when you go to `http://localhost:3000` the prototyping app will serve up an example page showing some dummy content between the GitHub header and footer.

This is for two reasons. Firstly, because it's a 'hello world' that shows everything has worked to this point. Secondly, because it gives you some example HTML and CSS to refer to when making layouts.

### Create a controller and a view

A controller in an MVC framework like Rails tells the app what to do when a user visits a certain url. When making a prototype it is what we'll use to separate out different parts or pages of the prototype.

To create a controller for your new default index page, run:

`rails generate controller home index`

This will create the controller (and associated files) for a new page.

### Edit the view

If you want to edit the html for the page you just created, you'll need to edit it's view.

A view is where code that is presented in the browser is kept.

To edit the code for the example above, `open app/views/home/index.html.erb`

An .erb file can contain HTML and Ruby.

### Set your prototype root

Routes allow you to map urls to controllers.

In the case of the new page we made above, we might want to make a cleaner, tidier url so that our default index page sits at the root url `(http://localhost:3000)`.

To do this you need to edit `config/routes.rb`.

Open this file and change the site root by replacing:

`root 'github#index'`

with:

`root 'home#index'`

Save your changes and your newly created view should now be viewable at `http://localhost:3000`

### Create custom route/s

You can add more controllers and routes to create the urls you need for your prototype.

Let's say we want to make a page that lives at `http://localhost:3000/contact`

To do this, you would first generate a controller for it. In your command line:

`rails generate controller contact index`
Then you would configure your page as described above.

Then open up `config/routes.rb` and add the following to this file (ideally beneath where the root is specified)

get `'contact' => 'contact#index'`
This tells Rails that any request for `http://localhost:3000/contact` should use the contact controller and specifically, it's index action.

This will then automatically look for a file in `app/views/contact/index`.html.erb and display it in the browser.

Try it. Save your changes and go to `http://localhost:3000/contact`.

You can do this as many times as you need for different pages and then link them together.

### Deploying

At some point you'll probably need to share your prototype on a public url.

To do this, it's probably best to use Heroku. For two reasons: it's free (albeit the page may take time to initally be served to the browser) and it plays nicely with Rails and git.

This prototyping app was made with the following in mind:

One branch per prototype
One Heroku app per prototype
This means each prototype is isolated and not affected by others and can also be permanently available on a given Heroku url.

So a branch needs to map to a Heroku app. Follow these steps:

**1. Heroku account and toolbelt**

If you haven't already, create an account on Heroku and install the toolbelt.

**2. Create an Heroku app.**

In your command line (replacing 'prototype-name' with the name of your prototype):

`heroku apps:create prototype-name`
This will create an app at `http://prototype-name.herokuapp.com`

**3. Confirm creation and that remote set up.**

`git remote`
Should show a remote called heroku. We need to rename this to match our prototype:

**4. Rename remote**

`git remote rename <old> <new>`

(eg.  `git remote rename heroku prototype-name-heroku`)

**5. Deploy**

`git push prototype-name-heroku your-local-branch-name:master`
This will push your app to the heroku address you set in step 2.

And you're done...

...hopefully.
