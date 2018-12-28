# python-django-todoapp

## Overview
This is a `todo` API example developed with Python and the Django Rest Framework.
The goal of this workshop is to build a continuous deployment pipeline to Heroku using Codeship Pro.

## Getting Started

There are a few resources to make sure you have available during this guide.

1. [Docker CE](https://store.docker.com/search?type=edition&offering=community) - Container service everything will run on
2. A public, cloud based [Github](https://github.com/join)/[Gitlab](https://gitlab.com/users/sign_in#register)/[Bitbucket](https://bitbucket.org/account/signup/) Account - Git Repository service
  + These must be cloud based, and not on your own servers.
  + You must be an admin for the repo
3. [Codeship](https://app.codeship.com/registrations/new) Account - CI/CD service
  + You can signup using any of the 3rd party logins or email/password
4. [Codeship Jet CLI](https://documentation.codeship.com/pro/getting-started/installation/#installing-jet) - CLI tool for testing builds locally
  + This can also be installed with `brew cask install jet`

5. [Heroku](https://signup.heroku.com/) Account - App hosting

Signup for each of these is free, and should only take you a few minutes if you don't already have one.  You can use your current accounts if you already have one available.

## Usage

1. Clone the repo

1. Build the Docker image:

        $ docker build .
