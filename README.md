# hello-blockstack [![CircleCI](https://img.shields.io/circleci/project/blockstack/blockstack-browser/master.svg)](https://circleci.com/gh/blockstack/blockstack-browser/tree/master) [![License](https://img.shields.io/github/license/blockstack/blockstack-browser.svg)](https://github.com/blockstack/blockstack-browser/blob/master/LICENSE.md) [![Slack](https://img.shields.io/badge/join-slack-e32072.svg?style=flat)](http://slack.blockstack.org/)

Welcome!

In this tutorial, we're going to walk through building a simple application on Blockstack.

This app will be a single-page application (SPA) that runs completely client-side. It will have no backend API to talk to, other than the identity and storage API that the user provides. In this sense, it will be a completely decentralized, server-less application.

For this tutorial, we will use the following tools:

npm to manage dependencies and scripts
browserify to compile node code into browser-ready code
blockstack.js to authenticate the user and work with the user's identity/profile information

Part 1: Installation & Generation
First, install Yeoman (a popular app generation tool) along with the Blockstack App Generator:


npm install -g yo generator-blockstack
Next, create a directory for the application and enter into that directory:


mkdir hello-blockstack && cd hello-blockstack
mkdir hello-blockstack && cd hello-blockstack
Then, use the Blockstack App Generator to generate a simple Blockstack app:


yo blockstack
yo blockstack
After you respond to the prompts, the app generator will create all of the app files and then install all dependencies.

Part 2: Serving the App
Start the development static file server to serve the app locally


npm run start
npm run start
The simple static file server can be found in server.js.

This server will serve all of the files in the /public directory, including index.html, app.js, bootstrap.min.css and app.css.

Part 3: Main App Code
The main file of our app is called app.js (in the /public folder). This is where the majority of the logic is contained.

As you can see, all of the code in the file is wrapped in an event listener that waits until the DOM content has been loaded:


1
document.addEventListener("DOMContentLoaded", function(event) {
2
})
Inside of this, we have a sign in button handler that creates an auth request and redirects the user to sign in:


1
document.getElementById('signin-button').addEventListener('click', function() {
2
  blockstack.redirectUserToSignIn()
3
})
We also have a sign out button handler that deletes the local user data and signs the user out:


1
document.getElementById('signout-button').addEventListener('click', function() {
2
  blockstack.signUserOut(window.location.origin)
3
})
Next, we have a function for showing the user's profile:


1
function showProfile(profile) {
2
  const person = new blockstack.Person(profile)
3
  document.getElementById('heading-name').innerHTML = person.name()
4
  document.getElementById('avatar-image').setAttribute('src', person.avatarUrl())
5
  document.getElementById('section-1').style.display = 'none'
6
  document.getElementById('section-2').style.display = 'block'
7
}
Then, we have logic for signing the user in and displaying the application.

Note that there are 3 main states the user can be in:

The user is already signed in
The user has a sign in request that is pending
The user is signed out
We express that as follows:


1
if (blockstack.isUserSignedIn()) {
2
  // Show the user's profile
3
} else if (blockstack.isSignInPending()) {
4
  // Sign the user in
5
} else {
6
  // Do nothing
7
}
With the first condition (when the user is signed in), we load the user data from local storage and then display the profile. With the second condition (when the user has a pending sign in request), we sign the user in and redirect the user back to the home page.


1
if (blockstack.isUserSignedIn()) {
2
   const profile = blockstack.loadUserData().profile
3
  showProfile(profile)
4
} else if (blockstack.isSignInPending()) {
5
  blockstack.handlePendingSignIn().then(function(userData) {
6
    window.location = window.location.origin
7
  })
8
}
Part 4: App Manifest
The app manifest file (/public/manifest.json) contains configurations for your app that dictate how it will be displayed in auth views and on user home screens.

The manifest.json file should look like this:


1
{
2
  "name": "Hello, Blockstack",
3
  "start_url": "localhost:5000",
4
  "description": "A simple demo of Blockstack Auth",
5
  "icons": [{
6
    "src": "https://helloblockstack.com/icon-192x192.png",
7
    "sizes": "192x192",
8
    "type": "image/png"
9
  }]
10
}
Keep it as is or fill it in with new information that describes your app.

Part 5: Source Control
To complete the tutorial, save your app code by pushing it to GitHub.

First, create a simple git repo:


git init
git init
Next, add and commit all of the files:


git add . && git commit -m "first commit"
git add . && git commit -m "first commit"
Then, create a new repo on GitHub and name it "hello-blockstack":

https://github.com/new

After that, add the remote repo to your local git repo. Make sure to fill in your username:


git remote add origin git@github.com:YOUR_USERNAME_HERE/hello-blockstack.git
git remote add origin git@github.com:YOUR_USERNAME_HERE/hello-blockstack.git
Last, push all of the code to the master branch of the remote repo:


git push origin master
git push origin master
You're done! You just built your first Blockstack app and shipped the code.

You're well on your way to becoming a Blockstack app legend.
