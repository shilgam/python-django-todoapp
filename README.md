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

1. Build the application

        $ docker-compose up

1. Migrate the database schema

        $ docker-compose exec web python manage.py migrate

1. Try out the API

        $ curl localhost:8000/todos
        []

1. Run the test suite

        $ docker-compose -p tests run -p 8000 --rm web python manage.py test
    Applied options:
    - `-p tests` - to isolate the tests into their own `tests` environment
    - `-p 8000` - to create a random port to prevent port collision
    - `--rm` - to remove the containers


## Continuous Integration with Codeship

1. Test your setup locally using 'Codeship Jet CLI' (see [Getting Started docs](https://documentation.codeship.com/pro/jet-cli/usage-overview/)):

        $ jet steps
        ...
        (step: test) success ✔
    > Jet CLI will run through the steps file just as it would on Codeship. This is a quick way to catch errors early before committing and pushing to your repository.

1. Create your own repository in any of Git Repository service (Github, Bitbucket, Gitlab) and push your code to the repo

1. [Create project in Codeship](https://documentation.codeship.com/pro/quickstart/codeship-configuration/#setting-up-a-new-project)

1. Push a new commit to trigger a new build on Codeship


## Continuous Deployment to Heroku
When the branch is `master`, run another step to actually **deploy** the app when the tests are passing.

1. \* [Create](https://signup.heroku.com/) and configure a new Heroku account

1. \* Check to see if your system already has the [Heroku command-line client](https://devcenter.heroku.com/articles/heroku-cli) installed:

        $ heroku --version

1. \* Log in and add your SSH key:

        $ heroku login
        $ heroku keys:add

1. Create a place on the Heroku servers for the app:

        $ heroku create       # randomly named
        $ heroku create myapp # named

1. Deploy the app:

        $ git push heroku master

1. [Set up the Heroku PostgreSQL add-on](https://elements.heroku.com/addons/heroku-postgresql) for your app to use as its database when deployed.

1. Create environment variable from your Heroku API key (which can be found in [Heroku Account Settings Page](https://dashboard.heroku.com/account)):

        $ echo "HEROKU_API_KEY=YOUR_API_KEY_HERE" > deployment.env
    Make sure that `deployment.env` file ignored in your `.gitignore`.

1. Create the Heroku procfile:

        $ echo "release: python manage.py migrate
                web: gunicorn todosapp.wsgi:application" > Procfile
    This is a simple command, but it’s required for Heroku to run the app.

1. Save your Codeship AES Key (Project Settings > General > "AES Key" section) in your repository root:

        $ echo "YOUR_AES_KEY" > codeship.aes
    Make sure that `codeship.aes` file ignored in your `.gitignore`.

1. Encrypt the `deployment.env` file. We do this using Codeship Jet CLI ([docs](https://documentation.codeship.com/pro/builds-and-configuration/environment-variables/#encrypting-your-environment-variables)):

        $ jet encrypt deployment.env deployment.env.encrypted
    The `deployment.env.encrypted` will then be included in your repository and decrypted in Codeship.

NOTE:
- `*` - ignore step if it was done before
