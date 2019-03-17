# postcards-root

## Goal

Project according to best practices:

- documented
  - code
  - wiki
- tested
  - low level (unit or API tests)
  - E2E
- deployed early
- use virtual environment
- continuous integration
- (optional) CLI

## Technologies used

Either completely Django (Python):

- database included
- no separate API (so less modular)

Or flask (Python) with a Javascript framework (e.g. Elm):

- decoupled API
  - easier to plug other Javascript framework
  - can test with Postman
- more room for error

Since the project is still relatively vague at the moment a more Modular approach with flask is preferred.

E2E testing done with Cypress since it's very easy to use. API testing done with Postman.

If a CLI would be added Click is a popular Python library to do it.

## Example sites

Example sites and interesting functionality to reuse in own project.

- [postcardman.net](https://www.postcardman.net/)
  - show new postcards
  - quick view to glimpse at card
  - sort by dimension
- [teich](http://collections.carli.illinois.edu/cdm/landingpage/collection/nby_teich)
  - show cards on map
- [discogs](https://www.discogs.com)
  - explanations everywhere

What seems missing:

- filter on crucial criteria (e.g. has )

## Functionality

- add postcard
- update postcard
  - validation
- view postcards
  - filter
  - different types of views (e.g. with picture or not)
  - glimpse at postcard
  - analysis shows relation with other postcards

## Deployment

Heroku is used for deployment since it's easy to set and there are decent tutorials available (e.g. for Flask [Flask](https://medium.com/the-andela-way/deploying-a-python-flask-app-to-heroku-41250bda27d0)).

You need too have the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli#download-and-install) installed to make deployments.

The minimum requirement to deploy is to have a `Procfile`. This file can be very minimal. For example, the basic `Procfile` for `postcards-backend` is `web: gunicorn hello:app` where we specify what WGSI server to use (`gunicorn` in this case) and which file contains the Flask app (`hello.py` in this case).

### Manual deployment

To manually deploy these are [the steps](https://devcenter.heroku.com/articles/getting-started-with-python):

- login to Heroku: `heroku login`
- create an app (assuming none have been created yet): `heroku create APP`
- push code to git repository used by Heroku: `git push heroku`

The app can be opened with `heroku open`. If you're already logged in or already created an app the only thing you have to do is pushing your code and the application is rebuild automatically.

By convention we name the app after the repository so APP for e.g. `postcards-backend` will be named `postcards-backend`.

### Automated deployment

You can also [deploy automatically](https://docs.travis-ci.com/user/deployment/heroku/). For this to work you need an encrypted Heroku key in your `.travis.yml` file. 

Make sure to have both the Heroku CLI and Travis CLI installed:

* instructions for Heroku CLI just above
* Travis CLI: `gem install travis` (make sure to have Ruby installed)

Once you have those CLIs you can use this command to encrypt the key. Make sure you run this command in the repository you have the `.travis.yml` file.

`travis encrypt $(heroku auth:token) --add deploy.api_key`

## Continuous integration

Both E2E and API tests are run each time a new commit is pushed to GitHub. New commits are automatically detected by [Travis](https://travis-ci.com/). The builds are dependent so the application is first deployed (`postcards-frontend` and `postcards-backend` repositories) and then the tests are ran (`postcards-tests` repository).

The `.travis.yml` file configures exactly how the continuous integration works. Each repository contains such a file.

## TO DO

- add [documentation API](https://learning.getpostman.com/docs/postman/api_documentation/intro_to_api_documentation/)
- automated dependent deployment (deploy first and then run tests)
- make tests update completely automatic
  - add environment variables in Travis () to be inserted in the Postman environment file
  - put script to get Postman collection in `.travis.yml`