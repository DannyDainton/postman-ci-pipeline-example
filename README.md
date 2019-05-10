# postman-ci-pipeline-example

## Build badges

Using [Shields.io](https://shields.io/) we can get a visual indication about about build status and display this on our repo.

#### TravisCI

[![Build Status](https://travis-ci.org/DannyDainton/postman-ci-pipeline-example.svg?branch=master)](https://travis-ci.org/DannyDainton/postman-ci-pipeline-example)

#### CircleCI

[![Build Status](https://img.shields.io/circleci/project/github/DannyDainton/postman-ci-pipeline-example.svg)](https://circleci.com/gh/DannyDainton/postman-ci-pipeline-example)

#### Bitbucket Pipelines

[![Build Status](https://img.shields.io/bitbucket/pipelines/ddainton/postman-ci-pipeline-example.svg)](https://bitbucket.org/ddainton/postman-ci-pipeline-example)

#### GitLab

[![Build Status](https://img.shields.io/gitlab/pipeline/DannyDainton/postman-ci-pipeline-example.svg)](https://gitlab.com/DannyDainton/postman-ci-pipeline-example)

---

This is very simple project to demonstrate using [Newman](https://github.com/postmanlabs/newman) to run a Postman collections through some of the different CI systems. There are a number of different ways that you can use `Newman` in these CI systems.

- TravisCI is creating a node environment and installing the `Newman` NPM package, then running the Collection via a CLI script.
- CircleCI is using the `postman/newman` orb, from there orb [registry](https://circleci.com/orbs/registry/). This a packaged version of the Newman Docker image held on their internal registry.
- Bitbucket Pipelines is creating an environment and using the `postman/newman_alpine33` Docker image and running the collection file in a container.

The collection and environment files have been taken from the [All-Things-Postman Repo](https://github.com/DannyDainton/All-Things-Postman). Each of the different systems will output the same CLI summary to the console.

---

## TravisCI

In order to use TravisCI in you're own projects you will need to signup and then sync it to your Github account. On the free TravisCI tier, you will only be able to run your Public repos through TravisCI.

More information about the sign up and set up process for TravisCI can be found [here](https://docs.travis-ci.com/user/getting-started).

### How does it work with TravisCI

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

Once the collection has run via TravisCI, you will see the log output of that build in the Travis UI. Any `Tests` that are in the collection, will run and the results will be displayed in the log output.

#### An example of the summary run output...

![TravisCI](/public/TravisCI.PNG)

---

## CircleCI

In order to you CircleCI on your projects you will need an account. You can sign in via Github, which makes it easier to add your projects from your repos.

More information about getting started with CircleCI, can be found [here](https://circleci.com/docs/2.0/first-steps/#section=getting-started).

### How does it work with CircleCI

All the magic happens in the `.circleci/config.yml` file. This is using the `Postman` Orb to run the collection file.

> Orbs are packages of config that you can use to quickly get started with the CircleCI platform. Orbs enable you to share, 
> standardize, and simplify config across your projects. You may also want to use orbs as a reference for config best 
> practices. Refer to the CircleCI Orbs Registry for the complete list of available orbs.

The basic `config.yml` file will look like the example below. There several other Newman specific options available, just like when running `Newman` from the CLI. More information about the options can be found [here](https://circleci.com/orbs/registry/orb/postman/newman).

```yml
version: 2.1
orbs:
  newman: postman/newman@0.0.1
jobs:
  build:
    executor: newman/postman-newman-docker
    steps:
      - checkout
      - newman/newman-run:
          collection: ./tests/Restful_Booker_Collection.postman_collection.json
          environment: ./tests/Restful_Booker_Environment.postman_environment.json
```

#### An example of the summary run output...

![CircleCI](/public/CircleCI.PNG)

---

## Bitbucket Pipelines

### How does it work with Bitbucket Pipelines

All the magic happens in the `bitbucket.pipelines.yml` file. This is using the `postman/newman_alpine33` Docker image, to run the collection file.

```yml
image: postman/newman_alpine33

pipelines:
  default:
    - step:
        script:
        - newman --version
        - newman run ./tests/Restful_Booker_Collection.postman_collection.json -e ./tests/Restful_Booker_Environment.postman_environment.json
```

#### An example of the summary run output...

![Bitbucket Pipeline](/public/Bitbucket_Pipeline.PNG)

---

## GitLab

### How does it work with GitLab

All the magic happens in the `.gitlab-ci.yml` file. This is using the `postman/newman_alpine33` Docker image, to run the collection file.

```yml
stages:
    - test

newman_tests:
    stage: test
    image:
        name: postman/newman_alpine33
        entrypoint: [""]
    script:
        - newman --version
        - newman run ./tests/Restful_Booker_Collection.postman_collection.json -e ./tests/Restful_Booker_Environment.postman_environment.json
```

#### An example of the summary run output...

![GitLab](/public/GitLab.PNG)

---

That's it - It's a very simple example of how you *could* use some of the CI Systems out there to run Postman collections with Newman.

If you have any questions, you can drop me a message on Twitter `@dannydainton`.
