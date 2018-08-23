# postman-travisci-example

[![Build Status](https://travis-ci.org/DannyDainton/postman-travisci-example.svg?branch=master)](https://travis-ci.org/DannyDainton/postman-travisci-example)

A very simple project to demonstrate using [Newman](https://github.com/postmanlabs/newman) to run a Postman collection through [TravisCI](https://travis-ci.org/).

The collection and environment files have been taken from the [All-Things-Postman Repo](https://github.com/DannyDainton/All-Things-Postman).

In order to use TravisCI in you're own projects you will need to signup and then sync it to your Github account. On the free TravisCI tier, you will only be able to run your Public repos through TravisCI.

---

### How does it all work?

All the magic happens in the `.travis.yml` file. This is building out the node environment and installing the required `Newman` module needed to run the collection file. Once the environment has been created, the `script` section is run - This would be run in the same way that you would run `Newman` from the command line on your local machine.

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
  - node_modules/.bin/newman run tests/Restful_Booker_Collection.postman_collection.json -e tests/Restful_Booker_Environment.postman_environment.json
```

---

Once the collection has run via TravisCI, you will see the log output of that build in the Travis UI. Any `Tests` that are in the collection, will run and the results will be displayed in the log output.

#### An example of what that might look like...

![Travis_Environment_Setup](/public/Travis_Environment_Setup.PNG)


![Collection_Tests](/public/Collection_Tests.PNG)

---