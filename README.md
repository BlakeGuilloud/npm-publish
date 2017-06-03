## A tutorial on how to publish an npm package.

![much doge](https://lh3.googleusercontent.com/E6EO3XO6zP7NtBq2L9SDF1DbBoYamUWc8QTRvOFuQg_Gka2Vw_RIv-AjU5Ysu4XgwHU=w170)

### Scope:
We are going to build an npm package called `strip-and-shape-phone` that formats a phone number with the preface of '+1', US Country Code. While something as basic as this is typically done within the codebase of whatever project would need such a function, and registering this as an npm package may seem trivial, the basic concept of exporting a javascript function for use across multiple projects as a dependency is a fun demonstration of the power of a public package registry like npm.

### Disclaimer:
I will be calling my package `strip-and-shape-phone` throughout the course of this tutorial.

**The package name must be unique to the npm registry.**

### End Result:
After successfully writing, publishing, and installing your npm package, it should be accessible in any project that lists it as a dependency.
```javascript
const stripAndShapePhone = require('strip-and-shape-phone'); // Your NPM package.

const phoneNumber = '843-555-1234'; // The string to be formatted.

const formattedNumber = stripAndShapePhone(phoneNumber); // Calling the imported package.

formattedNumber === '+18435551234'; // true
```

#### Step 1: Install Node.js and npm.
NPM, a node package manager (not actually an acronym), is now included in the installation of node.

For this tutorial we will be installing node/npm via homebrew:

```
$ brew install node
```

When this process is complete, you should have access to the command line interface of both node and npm.

```
$ node -v
v6.9.5

$ npm -v
3.10.10
```

*Depending on what the LTS (Long Term Support) version of node is at the time of installation, you may see different node and npm version results when calling -v (--version).*

#### Step 2: Sign up for and login to npm.
Visit www.npmjs.com and create an account.

You now have access to login to the npm registry via your command line.
```bash
$ npm login
Username: fbguillo
Password: # Your password will not be visible
Email: (this IS public) fbguillo@gmail.com
Logged in as fbguillo on https://registry.npmjs.org/.
```


#### Step 3: Create a project.
We will first be making a new directory, with a single file called `index.js`.

```
$ mkdir strip-and-shape-phone

$ cd strip-and-shape-phone

$ touch index.js
```
Open the project in your editor of choice
```
$ atom .
```

#### Step 4: Initialize npm in your repository.
Make sure you are in the repository that you want to publish to npm, an initialize it with npm.

```
$ npm init
name: strip-and-shape-phone
version: (1.0.0)
description: A utility for formatting a phone number
entry point: (index.js)
test command:
git repository:
keywords:
author: Blake Guilloud
liscense: (ISC)

Is this ok? (yes)
```

Your `package.json` file should look something like this:

```
{
  "name": "strip-and-shape-phone",
  "version": "1.0.0",
  "description": "A utility for formatting a phone number",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "Blake Guilloud",
  "license": "ISC"
}
```

Whatever file is listed as the value of property `"main"` will be what is called when your package is referenced.

#### Step 5: Write the code.
The following code will be written in the `strip-and-shape-phone/index.js` file.

```javascript
function stripAndShapePhone(phoneNumber) {
  let formattedNumber = phoneNumber.replace(/[^0-9]/g, ''); // Strip all non-numeric characters out of the string.

  if (formattedNumber[0] === '1') {
    // If the first character of the incoming phoneNumber is '1', preface the formattedNumber with '+'.
    formattedNumber = `+${formattedNumber}`;
  } else {
    // Else, preface the string with '+1'.
    formattedNumber = `+1${formattedNumber}`;
  }

  return formattedNumber; // Return the newly formattedNumber.
}

module.exports = stripAndShapePhone; // Export the function as a default to represent the entire module.
```

#### Step 6: Publish the package to the npm-registry.
Now that we have a proper `package.json` set up in our project, and we are logged into the `npm cli`, we can invoke the `npm publish` method that will place our newly created project on the npm-registry.

```
$ npm publish
strip-and-shape-phone@1.0.0
```

We have now published a package to the npm-registry!

Visit it at: www.npmjs.com/package/strip-and-shape-phone


#### Step 7: Install and use the package in a different repository.
```
$ cd <a-different-project>
$ npm install strip-and-shape-phone --save
```

You now have access to require or import your package as a dependency of `a-different-project`, and utilize it the way you would any npm package.

```javascript
const stripAndShapePhone = require('strip-and-shape-phone'); // Your NPM package.

const phoneNumber = '843-555-1234'; // The string to be formatted.

const formattedNumber = stripAndShapePhone(phoneNumber); // Calling the imported package.

formattedNumber === '+18435551234'; // true
```

#### Step 8: Versioning your package.
So we now have a package published on the npm-registry, but we forgot to publish documentation on how to utilize our newly created function.

Let's create a `README.md` in our `strip-and-shape-phone` project.

```
$ touch README.md
```

Inside of your README, document what your package does, and how to use it:
```javascript

#### strip-and-shape-phone

A utility function that prefaces a phone number with '+1'.

Usage:

const stripAndShapePhone = require('strip-and-shape-phone'); // Your NPM package.

const phoneNumber = '843-555-1234'; // The string to be formatted.

const formattedNumber = stripAndShapePhone(phoneNumber); // Calling the imported package.

formattedNumber === '+18435551234'; // true
```

Now that we have documented our npm-package, we want to publish this newly created README into the registry, so other's may know how to utilze it.

First, we want to `patch` an update to the project. We utilize the `patch` method here, signifying that the update to the package is not a breaking change.

```
$ npm version patch
v1.0.1
```

Now that we have updated the `"version"` property on our package.json, we run our `publish` command again.

*There are many different versioning functions in the npm version api.* https://docs.npmjs.com/cli/version

```
$ npm publish
```

The newly updated code will now be included in the `strip-and-shape-phone` npm-package.
