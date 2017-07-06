# Semantic Releases

[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release) [![Build Status](https://travis-ci.org/cdcabrera/semantic-test-sansnpm.svg?branch=master)](https://travis-ci.org/cdcabrera/semantic-test-sansnpm)

Testing semantic releases.


## Semantic Releases without NPM


***This setup requires at least 1 release tag/package to work***

1. Setup your GitHub repo initially, and clone it locally... or some form of that order
2. You can go ahead and link Travis to the repo
3. Your can generate your ```package.json```, but it should have a few things under ```devDependencies```, ```scripts```, a ```version``` alteration, and a couple of custom properties. 

  Notes:
    - If you have the default ```test``` script setup, make sure it exits towards ```0```, or it gets weird.

```javascript
"version": "0.0.0-semantically-released",
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 0",
  "setup": "echo \"This installs and runs the semantic-release-cli globally.\"; semantic-release-cli setup",
  "commit": "git add .; git-cz",
  "semantic-release": "semantic-release pre && semantic-release post"
},
"devDependencies": {
  "semantic-release": "^6.3.6",
  "semantic-release-cli": "^3.0.3",
  "commitizen": "^2.9.6",
  "cz-conventional-changelog": "^2.0.0",
  "last-release-git": "^0.0.3"
},
"czConfig": {
  "path": "node_modules/cz-conventional-changelog"
},
"release": {
  "getLastRelease": "last-release-git"
}
```

4. After that run if you haven't installed the travis-cli...
```shell
$ sudo gem install travis
```

5. Then run

```shell
$ npm install
$ npm run setup
```
  Notes:
  - Answer the questions for NPM, GitHub, and TravisCI...
    - The NPM question when asking for login, it doesn't want your email address
    - The GitHub question, it's possible to use a security token to login, you'll need to make sure the permissions are setup accordingly... I gave up and ran it against my normal creds... it will ask you to enter your two-factor auth code.
  - Running the ```setup``` script means parts of your customized ```package.json``` may have gotten customized. You may have to review to make sure you didn't do something silly like accidentally make it "private" when you didn't mean to
  - Your ```.travis.yml``` should also be updated, make sure the customizations are still in place
  - If the ```setup``` script fails part of the way through right when you need to add the tokens to TravisCI, run ```$ npm run setup``` again, but this time you can select the ```print``` option, and manually copy the two tokens into TravisCI


6. Check Travis to make sure two fields are now displaying, see the below graphic...




7. If this is a brand new repository without a starting package (ie. something like v0.0.0, or v1.0.0), once everything is initially released, you can create a temporary release package by taking the lazy approach and running something like below...

```shell
$ git fetch --tags; if [[ $(git tag) ]]; then echo \"tags exist\"; else git tag v0.0.0; git push --tags; fi;
```

...where you would change the v0.0.0 to your desired initial tag/release... you can also do it through the GitHub release package interface.

8. And finally give it a whirl, run...

```shell
$ npm run commit
```

TravisCI should spool up and your release tag/package should be generated accordingly.

  Pain Points:
  
  The configuration for semantic releases is a little sensitive, everything is meant to center around NPM, hence the concept
  
  Things that can go wrong:
  - you forgot to install TravisCI CLI and login
  - you forget to use ```$ npm run commit``` to format your commit messages (could also use a git hook), meaning a release won't happen
  - you have the wrong git repository referenced in your package.json
  - you accidentally indicated this was a private GitHub repo, but it's not
  - the ```$ npm run setup``` script fails to help setup a GitHub personal token
  - the ```$ npm run setup``` script fails to setup the GitHub and NPM tokens inside Travis, forcing you to manually print them
    



