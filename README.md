# npm-package-lock-example
Repository to demonstrate the purpose of a `package-lock` file.  Starting with `master`, each branch demonstrates a different part in the progression of the code base.  Each branch is described below and what it represents in the time line of the code base and also the problem that is solved with the use of a `package-lock` file.


## Branches
* `master`
  
  This is the state of the code base after a developer has come along and install `moment`.  At the time of the install, the latest version of `moment` was `2.10.2`.  After doing `npm install moment` the package.json file has the following dependency:
  ```
  "dependencies": {
    "moment": "^2.10.2"
  }
  ```

  `moment`'s package.json file version in the `node_modules` folder after `npm install`
  ```
  "version": "2.10.2"
  ```

* `next-developer-install`
  
  Now we have a new developer come on board who pulls down the code base and does an `npm install` to get their local development setup.
