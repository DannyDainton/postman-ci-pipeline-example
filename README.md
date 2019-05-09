# postman-ci-pipeline-example

[![Build Status](https://travis-ci.org/DannyDainton/postman-travisci-example.svg?branch=master)](https://travis-ci.org/DannyDainton/postman-travisci-example)

A very simple project to demonstrate using [Newman](https://github.com/postmanlabs/newman) to run a Postman collections through some of the different CI systems.

The collection and environment files have been taken from the [All-Things-Postman Repo](https://github.com/DannyDainton/All-Things-Postman).

## TravisCI

[TravisCI](https://travis-ci.org/).

In order to use TravisCI in you're own projects you will need to signup and then sync it to your Github account. On the free TravisCI tier, you will only be able to run your Public repos through TravisCI.

More information about the sign up and set up process for TravisCI can be found [here](https://docs.travis-ci.com/user/getting-started).

---

### How does it work with Travis?

All the magic happens in the `.travis.yml` file. This is building out the node environment and installing the required `Newman` module needed to run the collection file.

Once the node environment has been created, the `script` section is run - This would be run in the same way that you would run `Newman` from the command line on your local machine.

You can specify a number of different command line args to the script that would change the way that `Newman` runs the command. I've added the `--color` and `--disable-unicode` flags as an example of the args that can be used. More details about the other command line args can be found [here](https://github.com/postmanlabs/newman#command-line-options).

```yml
language: node_js
node_js:
  - "8.2.1"

install:
  - npm install newman

before_script:
  - node --version
  - npm --version
  - node_modules/.bin/newman --version

script:
  - node_modules/.bin/newman run tests/Restful_Booker_Collection.postman_collection.json -e tests/Restful_Booker_Environment.postman_environment.json --color auto --disable-unicode
```

---

Once the collection has run via TravisCI, you will see the log output of that build in the Travis UI. Any `Tests` that are in the collection, will run and the results will be displayed in the log output.

#### An example of what that might look like...

![Travis_Environment_Setup](/public/Travis_Environment_Setup.PNG)

![Collection_Tests](/public/Collection_Tests.PNG)

---

## CircleCI

### How does it work CircleCI?

All the magic happens in the `.circleci/config.yml` file. This is using the `Newman` Docker image needed to run the collection file.
 
```yml
version: 2.1
orbs:
  newman: postman/newman@1.0.0
jobs:
  newman-collection-run:
    executor: newman/postman-newman-docker
    steps:
      - checkout
      - newman/newman-run:
          collection: ./tests/Restful_Booker_Collection.postman_collection.json
          environment: ./tests/Restful_Booker_Environment.postman_environment.json
```

---

That's it - It's a very simple example of how you *could* use some of the CI Systems out there to run Postman collections. If you have any questions, you can drop me a message on Twitter `@dannydainton`.