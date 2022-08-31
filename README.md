# Synthea Server

**THIS PROJECT WAS NEVER COMPLETED AND IS NOW ARCHIVED.**

The Synthea Server is a REST JSON API service for generating patient records based on probabilistic models. It uses the [Synthea](https://github.com/synthetichealth/synthea) library from [Synthetic Health](https://github.com/synthetichealth), but is not otherwise affiliated in an official manner.

# The API

TODO Not sure yet

	GET	/profiles # List all configuration templates.
	GET	/profiles/:uuid # Get details of a specific configuration.
	GET	/profiles/:id/patients

# Developer Quick Start (OSX with Homebrew)

As with most Ruby projects, use [RVM](https://rvm.io) to manage your local Ruby versions. [Install RVM](https://rvm.io) and:

	rvm install 2.4.1
	rvm use 2.4.1

Then,

	bundle install # to install all server-side library dependencies.

The following additional environment variables are optional, but potentially useful in a production context.If in doubt, do NOT set these.

 * export RAILS_MAX_THREADS=8 # To override the number of threads per process.

You're now ready to run the application.

	rails s # to run the server in development mode in the foreground.

To automatically re-run regression tests on detected code changes, open another terminal window and run

	guard # hit <enter> to manually re-run all tests to run if a change isn't detected

# Deployment

Deployment is done exclusively with Docker, though "raw" deployment using Passenger and all another common methods, including Heroku, are supported as well.

## Building a Container

To build your current version:

	docker build -t p3000/synthea-server:latest .

## Running a Container

When running the container, **all environment variables defined in the above section must be set using `-e FOO="bar"` options** to docker. The foreground form of the command is:

	docker run -it --rm -p 3000:3000 --name synthea-server p3000/synthea-server:latest

...or to run in the background:

	docker run -d -p 3000:3000 --name synthea-server p3000/synthea-server:latest

## Regression Testing a Container

The container includes a regression test suite to ensure proper operation. Running in test mode is slightly different, as to not inadvertently affect your production database(s). The application server process must also be told to run in 'test' mode.

	docker run -it -m="512MB" -e "RAILS_ENV=test" p3000/synthea-server:latest rake test

# Attribution

* Preston Lee <preston.lee@prestonlee.com>

# License

[Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0)
