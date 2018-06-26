# npm-package-lock-example
Repository to demonstrate the purpose of a `package-lock` file.  Starting with `master`, each branch demonstrates a different part in the progression of the code base.  Each branch is described below and what it represents in the time line of the code base and also the problem that is solved with the use of a `package-lock` file.  Starting with the `master` branch, the assumption is this is a new code base that has no dependencies yet and is running on node <=6 so no `package-lock.json` file is created.  Also, each branch includes the `node_modules` folder which is not something I condone but is being done for demonstration purposes only.


## Branches
* [master](https://github.com/paulmfischer/npm-package-lock-example)
  
  This is the state of the code base after a developer A has come along and install `moment`.  At the time of the install, the latest version of `moment` was `2.10.2`.  After doing `npm install moment` the package.json file has the following dependency:
  ```
  "dependencies": {
    "moment": "^2.10.2"
  }
  ```

  `moment`'s package.json file version in the `node_modules` folder after `npm install`
  ```
  "version": "2.10.2"
  ```

* [next-developer-install](https://github.com/paulmfischer/npm-package-lock-example/tree/next-developer-install)
  
  Now we have a new developer (B) who comes on board and pulls down the code base and does an `npm install` to get their local development setup.  (This is a fresh install, there is no `node_modules` folder or a `package-lock.json` file in the repository yet.)  Since developer A installed `moment` and up to this point `moment` has made several updates to their code base and released several minor/patch version so the latest version of `moments` now `2.22.2`.  So after developer B does `npm install` and we look in the `node_modules` folder we see the following in the `package.json` file for `moment`.
  ```
  "version": "2.22.2"
  ```
  Now if the dependency adheres to proper<sup><a href="#momentNote">1</a></sup> [semantic versioning (semver)](https://docs.npmjs.com/misc/semver) this isn't a big problem, but some libraries in npm land do not all follow semantic versioning or mistakes happen (minor version update includes breaking changes on accident).  So now we have developer A running on `moment` at version `2.10.2` but developer B is running at version `2.22.2` which could lead to the app being broken or working in an entirely different way.  This is not just an issue with development locally but also when it comes to building the application.  If your build does a fresh install everytime it will pull in newer versions that can lead to the same problems as mentioned before.
  
  So how can we solve this problem?  Well one way is to remove the version prefix in your `package.json` file.  While this works great it isn't the most ideal because we then have to manually update each and every version in the package file whenever we want to upgrade them.  We also lose out on utilizing semver in our dependency list and automating the upgrade process to adhere to the semver range prefix's.  Let's now take a look at the next branch (`master-package-lock`) that will introduce the `package-lock` file and how that keeps dependencies in line for other developers/build systems.

* [master-package-lock](https://github.com/paulmfischer/npm-package-lock-example/tree/master-package-lock)
 
  Here we are starting over fresh again with developer A before they have installed `moment` but this time they are using node >=8 (npm 5).  Now when developer A does `npm install moment`, the `package.json` file gets the same dependency as the `master` branch but we also have a new `package-lock.json` file created for us which contains the following:
  ```
  {
    "name": "npm-package-lock-example",
    "version": "0.1.0",
    "lockfileVersion": 1,
    "requires": true,
    "dependencies": {
      "moment": {
        "version": "2.10.2",
        "resolved": "https://registry.npmjs.org/moment/-/moment-2.10.2.tgz",
        "integrity": "sha1-+vjAnKgvA/lJYuO1pyOSY+pQ0Ic="
      }
    }
  }
  ```
  The `package-lock.json` file gets checked in along with the rest of the code base so that it can be utilized by other developers/builds in the future.  So lets move on to what happens now when developer B comes along and is starting fresh.

* [package-lock-next-developer-install](https://github.com/paulmfischer/npm-package-lock-example/tree/package-lock-next-developer-install)

  Now the setup is the same as with `next-developer-install` branch with the difference being there is a `package-lock.json` file and the developer is using node >=8.  Developer B has pulled the code base down and does an `npm install`.  Now when we look in the `node_modules` folder and view the `package.json` for `moment` we see this:
  ```
  "version": "2.10.2"
  ```
  Same version as what developer A installed!  Now you are thinking 'That is great and all but what if I want a newer version of `moment`?'  Well that is where [npm update](https://docs.npmjs.com/cli/update) comes along!
  
* [package-lock-update]((https://github.com/paulmfischer/npm-package-lock-example/tree/package-lock-update)

  After some time, developer A realizes that we are using an older version of `moment` and would like to pull in latest so that we can get some of the bug fixes and new features.  There are two ways of doing that, one is to manually update the `package.json` file (which is not recommended!) and the other is to use `npm update [packageName]`.  So developer A runs `npm update moment` and afterwards this is what the `package.json`, `package-lock.json` and `moment`'s version in `package.json` files look like:
  `package.json`:
  ```
  "dependencies": {
    "moment": "^2.22.2"
  }
  ```
  `package-lock.json`:
  ```
  {
    "name": "npm-package-lock-example",
    "version": "0.1.0",
    "lockfileVersion": 1,
    "requires": true,
    "dependencies": {
      "moment": {
        "version": "2.22.2",
        "resolved": "https://registry.npmjs.org/moment/-/moment-2.22.2.tgz",
        "integrity": "sha1-PCV/mDn8DpP/UxSWMiOeuQeD/2Y="
      }
    }
  }
  ```
  `moment` version in `package.json`:
  ```
  "version": "2.22.2"
  ```
  
###### Notes
<div id="momentNote">
<sup>1</sup>As far as I know, `moment` adheres to semantic versioning.  This is not to call them out or blame them for anything, this was just the first library that came to mind to use as an example.
</div>
